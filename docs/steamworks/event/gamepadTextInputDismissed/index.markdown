# gamepadTextInputDismissed

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Event][api.type.event]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, keyboard, text input, gamepadTextInputDismissed
> __See also__          [steamworks.showGamepadTextInput()][plugin.steamworks.showGamepadTextInput]
>                       [steamworks.addEventListener()][plugin.steamworks.addEventListener]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------

## Overview

This event occurs when the keyboard opened by [steamworks.showGamepadTextInput()][plugin.steamworks.showGamepadTextInput] is closed, whether the player submitted text or cancelled.

You can receive these events by adding a [listener][api.type.Listener] to the plugin via the [steamworks.addEventListener()][plugin.steamworks.addEventListener] function.


## Properties

##### event.name
_[String][api.type.String]._ Always `"gamepadTextInputDismissed"`.

##### event.submitted
_[Boolean][api.type.Boolean]._ `true` if the player submitted text, `false` if they cancelled.

##### event.length
_[Number][api.type.Number]._ Buffer length as reported by Steam, which counts the terminating null: `"hello"` reports `6`, and a cancel typically reports `1`. Use `event.text` (or `#event.text`) rather than this for the text itself.

##### event.text
_[String][api.type.String]._ The submitted text. An empty string when cancelled.


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local function onGamepadTextDismissed( event )
	if event.submitted then
		print( "Player entered: " .. event.text )
	else
		print( "Player cancelled" )
	end
end

steamworks.addEventListener( "gamepadTextInputDismissed", onGamepadTextDismissed )
``````
