# steamworks.showGamepadTextInput()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Boolean][api.type.Boolean]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, keyboard, text input, steam deck, showGamepadTextInput
> __See also__          [steamworks.getEnteredGamepadTextInput()][plugin.steamworks.getEnteredGamepadTextInput]
>                       [steamworks.showFloatingGamepadTextInput()][plugin.steamworks.showFloatingGamepadTextInput]
>                       [gamepadTextInputDismissed][plugin.steamworks.event.gamepadTextInputDismissed]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Opens Steam's modal gamepad keyboard, as used in Big Picture mode and on Steam Deck. When the player submits or cancels, a [gamepadTextInputDismissed][plugin.steamworks.event.gamepadTextInputDismissed] event is dispatched to listeners added via [steamworks.addEventListener()][plugin.steamworks.addEventListener].

Returns `true` if the keyboard was shown.


## Gotchas

* On a desktop PC the keyboard is generally only shown while Steam is in Big Picture mode. Expect `false` otherwise and fall back to the normal text field.
* Only one keyboard can be open at a time.


## Syntax

	steamworks.showGamepadTextInput( [mode] [, lineMode] [, description] [, maxChars] [, existingText] )

##### mode ~^(optional)^~
_[String][api.type.String]._ `"normal"` or `"password"`. Defaults to `"normal"`.

##### lineMode ~^(optional)^~
_[String][api.type.String]._ `"singleLine"` or `"multipleLines"`. Defaults to `"singleLine"`.

##### description ~^(optional)^~
_[String][api.type.String]._ Prompt shown above the keyboard. Defaults to an empty string.

##### maxChars ~^(optional)^~
_[Number][api.type.Number]._ Maximum number of characters. Defaults to `256`.

##### existingText ~^(optional)^~
_[String][api.type.String]._ Text to pre-fill. Defaults to an empty string.


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local function onGamepadTextDismissed( event )
	if event.submitted then
		myTextField.text = event.text
	end
end
steamworks.addEventListener( "gamepadTextInputDismissed", onGamepadTextDismissed )

local wasShown = steamworks.showGamepadTextInput( "normal", "singleLine", "Email address", 254, myTextField.text )
if not wasShown then
	native.setKeyboardFocus( myTextField )
end
``````
