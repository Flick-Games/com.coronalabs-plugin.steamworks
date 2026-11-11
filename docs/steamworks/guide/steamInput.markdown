# Steam Input (Controllers)

This guide explains how to add controller support to a Solar2D game with Steam Input, Steam's controller API. Steam Input supports Xbox, PlayStation, Nintendo Switch and generic controllers, the Steam Controller and the built-in controls of Steam Deck and other Steam hardware. Players can remap any control in Steam, and Steam supplies button art that matches their controller.

<div class="guides-toc">

* [How Steam Input works](#concepts)
* [1. Write an action manifest](#manifest)
* [2. Load it and read input](#code)
* [3. Create default layouts](#layouts)
* [4. Ship the files](#shipping)
* [Button prompts (glyphs)](#glyphs)
* [Text entry on Steam Deck](#text)
* [Gotchas](#gotchas)

</div>


<a id="concepts"></a>

## How Steam Input works

Your game never reads buttons directly. Instead it declares __actions__ ("select", "jump", "move") and asks Steam whether each action is active. Which physical control triggers each action is decided by a __layout__, which you supply as a default per controller type and the player can change.

* __Action set__: a group of actions that are available together, such as `MenuControls` and `GameplayControls`. One set is active per controller at a time, so the same button can mean different things in a menu and in game.
* __Digital action__: an on/off action, like a button press.
* __Analog action__: a stick, trackpad, gyro or trigger, reported as `x` and `y` values.
* __Action manifest__: a VDF text file that lists your action sets and actions, their display names, and the default layout file for each controller type.
* __Layout__ (also called a configuration): a VDF file that binds the manifest's actions to one controller type's controls.


<a id="manifest"></a>

## 1. Write an action manifest

Create a `controller_config` folder in your project and add `action_manifest.vdf`. This sample declares a menu set and a gameplay set:

``````
"In Game Actions"
{
	"actions"
	{
		"MenuControls"
		{
			"title"		"#set_menu"
			"StickPadGyro"
			{
				"cursor_move"
				{
					"title"		"#action_cursor_move"
					"input_mode"	"joystick_move"
				}
			}
			"Button"
			{
				"select"	"#action_select"
				"cancel"	"#action_cancel"
			}
		}
		"GameplayControls"
		{
			"title"		"#set_gameplay"
			"StickPadGyro"
			{
				"move"
				{
					"title"		"#action_move"
					"input_mode"	"joystick_move"
				}
			}
			"AnalogTrigger"
			{
				"throttle"	"#action_throttle"
			}
			"Button"
			{
				"jump"		"#action_jump"
				"pause"		"#action_pause"
			}
		}
	}
	"localization"
	{
		"english"
		{
			"set_menu"		"Menu"
			"set_gameplay"		"Gameplay"
			"action_cursor_move"	"Move Cursor"
			"action_select"		"Select"
			"action_cancel"		"Back"
			"action_move"		"Move"
			"action_throttle"	"Throttle"
			"action_jump"		"Jump"
			"action_pause"		"Pause"
		}
	}
	"configurations"
	{
		"controller_xboxone"
		{
			"0"
			{
				"path"	"config_YOUR_APP_ID_controller_xboxone.vdf"
			}
		}
		"controller_ps5"
		{
			"0"
			{
				"path"	"config_YOUR_APP_ID_controller_ps5.vdf"
			}
		}
		"controller_neptune"
		{
			"0"
			{
				"path"	"config_YOUR_APP_ID_controller_neptune.vdf"
			}
		}
	}
}
``````

Notes on the format:

* Action names (`select`, `move`) are what your code uses. Titles starting with `#` are looked up in the `localization` block and shown to players in Steam's configurator. Add a block per language using Steam's language names (`"french"`, `"german"`, `"schinese"` and so on).
* `joystick_move` suits sticks and cursors. Valve's [action manifest documentation](https://partner.steamgames.com/doc/features/steam_controller/iga_file) lists the other input modes.
* Add new actions at the __end__ of their block, and avoid reordering or removing actions in a manifest that has shipped. See [Gotchas](#gotchas).
* The `configurations` block names the default layout for each controller type. You create these files in [step 3](#layouts); leave the block out until then.

These are the controller type names Steam uses in `configurations`:

------------------------------------  ------------------------------------------
`controller_xboxone`                  Xbox One and Xbox Series controllers
`controller_xbox360`                  Xbox 360 controller
`controller_xboxelite`                Xbox Elite controller
`controller_ps4`                      PlayStation 4 (DualShock 4)
`controller_ps5`                      PlayStation 5 (DualSense)
`controller_ps5_edge`                 DualSense Edge
`controller_switch_pro`               Nintendo Switch Pro controller
`controller_switch2_pro`              Nintendo Switch 2 Pro controller
`controller_switch_joycon_pair`       Pair of Joy-Con
`controller_generic`                  Other gamepads
`controller_neptune`                  Steam Deck
`controller_steamcontroller_gordon`   Steam Controller (2015)
`controller_triton`                   Steam Controller (2026)
`controller_steamframe_pair`          Steam Frame controllers
`controller_steamos_handheld`         Other handhelds running SteamOS
------------------------------------  ------------------------------------------

A controller type without a layout falls back to Steam's own mapping for it, so start with the controllers you can test and add the rest over time.


<a id="code"></a>

## 2. Load it and read input

The plugin initialises Steam Input when it loads. Point Steam at the manifest, then read actions every frame:

``````lua
local steamworks = require( "plugin.steamworks" )

local ALL_CONTROLLERS = "18446744073709551615"
local DEAD_ZONE = 0.1

local setHandles = { MenuControls = 0, GameplayControls = 0 }
local digitalHandles = { select = 0, cancel = 0, jump = 0, pause = 0 }
local analogHandles = { cursor_move = 0, move = 0, throttle = 0 }
local activeSet = "MenuControls"
local seenControllers = {}
local wasDown = {}

-- 1. Point Steam at the manifest as early as possible
local manifestPath = system.pathForFile( "controller_config/action_manifest.vdf", system.ResourceDirectory )
if ( system.getInfo( "platform" ) == "win32" ) then
	manifestPath = manifestPath:gsub( "/", "\\" )
end
steamworks.setInputActionManifestFilePath( manifestPath )

-- 2. Look up any handle that is still 0. Steam can take a few frames to read
--    the manifest, so this runs every frame until everything resolves.
local function resolveHandles()
	for name, handle in pairs( setHandles ) do
		if ( handle == 0 ) then
			setHandles[name] = steamworks.getActionSetHandle( name )
			if ( setHandles[name] ~= 0 ) then seenControllers = {} end
		end
	end
	for name, handle in pairs( digitalHandles ) do
		if ( handle == 0 ) then digitalHandles[name] = steamworks.getDigitalActionHandle( name ) end
	end
	for name, handle in pairs( analogHandles ) do
		if ( handle == 0 ) then analogHandles[name] = steamworks.getAnalogActionHandle( name ) end
	end
end

-- Switch every controller to another action set, e.g. when gameplay starts
local function setActionSet( name )
	activeSet = name
	if ( setHandles[name] ~= 0 ) then
		steamworks.activateActionSet( ALL_CONTROLLERS, setHandles[name] )
	end
end

local function onPressed( controller, action )
	print( action .. " pressed on " .. controller )
end

-- 3. Every frame: update Steam Input, activate the set on new controllers, read actions
local function pollControllers()
	steamworks.runFrame()
	resolveHandles()

	local setHandle = setHandles[activeSet]
	for _, controller in ipairs( steamworks.getConnectedControllers() ) do
		if ( not seenControllers[controller] and setHandle ~= 0 ) then
			steamworks.activateActionSet( controller, setHandle )
			seenControllers[controller] = true
		end

		wasDown[controller] = wasDown[controller] or {}
		for name, handle in pairs( digitalHandles ) do
			if ( handle ~= 0 ) then
				local data = steamworks.getDigitalActionData( controller, handle )
				local isDown = data.bActive and data.bState
				if ( isDown and not wasDown[controller][name] ) then
					onPressed( controller, name )
				end
				wasDown[controller][name] = isDown
			end
		end

		if ( activeSet == "GameplayControls" and analogHandles.move ~= 0 ) then
			local stick = steamworks.getAnalogActionData( controller, analogHandles.move )
			if ( stick.bActive and ( math.abs( stick.x ) > DEAD_ZONE or math.abs( stick.y ) > DEAD_ZONE ) ) then
				player.x = player.x + stick.x * 10
				player.y = player.y - stick.y * 10   -- Steam's y is positive up
			end
		end
	end
end
Runtime:addEventListener( "enterFrame", pollControllers )
``````

For the Corona Simulator, the Steam client must be running and your project needs the `appId` setting described on the [main plugin page][plugin.steamworks#project-settings].


<a id="layouts"></a>

## 3. Create default layouts

Default layouts are easiest to make in Steam's own configurator, which then saves them as VDF files.

1. __Turn on Steam Input Layout Dev Mode.__ In Steam, open __Settings__ › __Developer__ and turn on __Steam Input Layout Dev Mode__, then restart Steam. Turning on __Steam Input Layout Error Messages__ on the same page is also useful: Steam then reports any layout file it cannot read.
	* On Windows the __Developer__ page is hidden at first. Run `steam://devmode/enable` (press Win+R, enter it, press Enter), open Big Picture mode, and turn on __Settings__ › __System__ › __Enable Developer Mode__.
	* On SteamOS (Steam Deck and other Steam hardware), __Enable Developer Mode__ is under __Settings__ › __System__.
2. __Run the game with a controller connected.__ Your build must load the manifest from step 1. The Corona Simulator works, as does a build installed through Steam.
3. __Open the configurator__ from the Steam overlay (Shift+Tab, then __Controller settings__), or call [steamworks.showBindingPanel()][plugin.steamworks.showBindingPanel]. Your action sets and actions appear with the titles from the manifest's `localization` block. Bind every action in every set.
4. __Save the layout.__ With Layout Dev Mode on, choose __Export Layout__ from the layout's menu and save it as a personal layout.
5. __Find the file.__ Steam writes it to `steamapps/common/Steam Controller Configs/<account number>/config/<app id>-test/` inside the Steam folder, for example `C:\Program Files (x86)\Steam\steamapps\common\Steam Controller Configs\123456789\config\480-test\` on Windows. The file is named after the controller type, for example `controller_xboxone.vdf`. You can also run `steam://dumpcontrollerconfig?appid=YOUR_APP_ID` while the game is running to export what Steam currently has loaded.
6. __Copy it into `controller_config`__ and rename it to the `path` you gave that controller type in the manifest's `configurations` block.
7. __Tidy the header.__ A personal save records where it came from. Edit these fields at the top of the file so it reads as an official layout:

	``````
	"title"		"#Library_ControllerSaveDefaultTitle"
	"description"	"#Library_ControllerSaveDefaultDescription"
	"progenitor"	""
	``````

	Then delete the `"url"` line, which holds the path of your personal save, and set `"export_type"` to `"unknown"` in place of `"personal_local"`.
8. __Check it.__ Restart the game. The configurator should now show your layout as the game's official layout for that controller.

Repeat for each controller type you can test. A layout for one type can be a starting point for a similar type (for example `controller_ps4` from `controller_ps5`), but change its `"controller_type"` field to match, and test it on that controller if you can.


<a id="shipping"></a>

## 4. Ship the files

* __Include the folder in your build.__ `controller_config` is copied into the build with the rest of your project, so `system.pathForFile( ..., system.ResourceDirectory )` finds it at runtime. Make sure no `excludeFiles` rule in `build.settings` removes `.vdf` files.
* __Steamworks settings.__ On the Steamworks partner site, open your app's __App Admin__ page and go to __Application__ › __Steam Input Settings__:
	1. Under __Opt Controllers into Steam Input__, tick the controller families your game supports (Xbox, PlayStation, Nintendo Switch, Generic, and Any Future Devices). Steam then routes those controllers through Steam Input instead of passing them to the game directly.
	2. Under __Steam Input Default Controller Configuration__, choose __Custom Configuration__. This is required for games that use the Steam Input API.
	3. Enter the path to the action manifest, relative to the game's install folder. A Solar2D Windows build puts your project's files in a `Resources` folder, so for the layout in this guide the path is `Resources/controller_config/action_manifest.vdf`.
	4. Save, then publish the change from the __Publish__ tab.

	Your game should still call [steamworks.setInputActionManifestFilePath()][plugin.steamworks.setInputActionManifestFilePath] as shown above. That makes the manifest load in the Corona Simulator too, which does not run from your game's Steam install folder. Read Valve's [Steam Input documentation](https://partner.steamgames.com/doc/features/steam_controller) for the other settings on that page, including Steam Deck's touchscreen mode.
* __Keep your installed build current.__ If you have the game installed through Steam on your development machine, Steam may also read that build's manifest. Upload a new build whenever the manifest changes. See [Gotchas](#gotchas).


<a id="glyphs"></a>

## Button prompts (glyphs)

To show "Press A" style prompts that match the player's controller and bindings:

1. Get the controls bound to an action with [steamworks.getDigitalActionOrigins()][plugin.steamworks.getDigitalActionOrigins] or [steamworks.getAnalogActionOrigins()][plugin.steamworks.getAnalogActionOrigins].
2. Pass the first origin to [steamworks.getGlyphForActionOrigin()][plugin.steamworks.getGlyphForActionOrigin] for Steam's button image, or [steamworks.getStringForActionOrigin()][plugin.steamworks.getStringForActionOrigin] for its name.
3. Query again every so often (for example every half second), since the player can remap controls at any time.

``````lua
local function getPromptImage( controller, setName, actionName )
	local origins = steamworks.getDigitalActionOrigins( controller, setHandles[setName], digitalHandles[actionName] )
	if ( not origins[1] ) then return nil end

	local sourcePath = steamworks.getGlyphForActionOrigin( origins[1], "medium" )
	if ( not sourcePath ) then return nil end

	-- the glyph lives in Steam's folder: copy it somewhere Solar2D can load from
	local fileName = "glyph_" .. origins[1] .. ".png"
	local destPath = system.pathForFile( fileName, system.CachesDirectory )
	local source = io.open( sourcePath, "rb" )
	if ( not source ) then return nil end
	local dest = io.open( destPath, "wb" )
	dest:write( source:read( "*a" ) )
	source:close()
	dest:close()
	return fileName
end

local fileName = getPromptImage( controller, "MenuControls", "select" )
if ( fileName ) then
	display.newImageRect( fileName, system.CachesDirectory, 64, 64 )
end
``````


<a id="text"></a>

## Text entry on Steam Deck

Steam Deck and other Steam hardware have no physical keyboard, and Steam Deck compatibility requires that every text field can be filled in.

* When the player selects a text field with the controller, open Steam's keyboard with [steamworks.showGamepadTextInput()][plugin.steamworks.showGamepadTextInput] and copy the text from the [gamepadTextInputDismissed][plugin.steamworks.event.gamepadTextInputDismissed] event into your field.
* When the player taps a text field on the touchscreen, or clicks it with a pointer, call [steamworks.showFloatingGamepadTextInput()][plugin.steamworks.showFloatingGamepadTextInput] from the field's `"began"` phase. Steam's floating keyboard then types straight into the focused field.

Both calls return `false` outside Big Picture mode and Steam hardware, so fall back to your normal text input.

To adjust your game for the device it is running on, see [steamworks.isRunningOnSteamHardware()][plugin.steamworks.isRunningOnSteamHardware], [steamworks.getSteamHardwareDefaultConfig()][plugin.steamworks.getSteamHardwareDefaultConfig] and [steamworks.isRunningUnderProton()][plugin.steamworks.isRunningUnderProton].


<a id="gotchas"></a>

## Gotchas

* __Set the manifest path early.__ Call [steamworks.setInputActionManifestFilePath()][plugin.steamworks.setInputActionManifestFilePath] straight after requiring the plugin. You do not need [steamworks.initSteamInput()][plugin.steamworks.initSteamInput]: the plugin initialises Steam Input when it loads.
* __Handles can start as `0`.__ Steam may take a few frames to read the manifest, and until then some action handles (often all but the first one or two) come back as `0`. Keep asking for any `0` handle each frame, as in the sample, and never pass a `0` action set handle to [steamworks.activateActionSet()][plugin.steamworks.activateActionSet]. `0` is truthy in Lua, so test `handle ~= 0`.
* __Call [steamworks.runFrame()][plugin.steamworks.runFrame] every frame__ before reading actions or origins, or the results can be stale.
* __Controller handles are strings.__ They are 64-bit numbers, which a Lua number cannot hold exactly. Pass them back unchanged.
* __Activate the action set on every new controller.__ A controller connected mid-game has no active set, so its actions report `bActive = false` until you activate one.
* __Steam's `y` axis points up.__ Negate it for Solar2D's screen coordinates.
* __Add actions at the end.__ Steam can match actions by their position in the manifest. If your development machine also has the game installed through Steam, Steam may read that build's manifest as well as your project's, and actions can then appear swapped (for example, a D-pad direction triggering "pause"). Appending new actions, rather than inserting or reordering, avoids this; restarting the simulator clears it.
* __The Steam overlay and keyboard need a Steam-launched process.__ In the Corona Simulator, Steam may accept a keyboard or binding panel request but be unable to draw it, unless the simulator was launched from Steam. Test these with a build launched through Steam. See also the [Overlay Troubleshooting][plugin.steamworks.guide.overlays] guide.
