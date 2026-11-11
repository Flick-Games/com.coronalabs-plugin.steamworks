# steamworks.getInputTypeForHandle()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [String][api.type.String]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, controller type, getInputTypeForHandle
> __See also__          [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers]
>                       [steamworks.getGlyphForActionOrigin()][plugin.steamworks.getGlyphForActionOrigin]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the type of controller behind a controller handle, as one of these strings:

* `"SteamController"`: the original Steam Controller
* `"SteamController2026"`: the 2026 Steam Controller
* `"SteamDeck"`: Steam Deck's built-in controls
* `"SteamFrameController"`: Steam Frame controllers
* `"SteamOSHandheld"`: the built-in controls of another handheld running SteamOS
* `"XBox360"`, `"XBoxOne"`
* `"PS3"`, `"PS4"`, `"PS5"`
* `"SwitchPro"`, `"Switch2Pro"`
* `"GenericGamepad"`
* `"Unknown"`: any other type, an invalid handle, or Steam Input unavailable


## Gotchas

* To show the right button art, use [steamworks.getGlyphForActionOrigin()][plugin.steamworks.getGlyphForActionOrigin] rather than choosing art by controller type. Players can remap buttons, and Steam's glyphs always match the player's bindings.


## Syntax

	steamworks.getInputTypeForHandle( controllerHandle )

##### controllerHandle ~^(required)^~
_[String][api.type.String]._ A handle from [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers].


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local controllers = steamworks.getConnectedControllers()
if ( controllers[1] ) then
	print( "First controller is a " .. steamworks.getInputTypeForHandle( controllers[1] ) )
end
``````
