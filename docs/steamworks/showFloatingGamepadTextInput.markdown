# steamworks.showFloatingGamepadTextInput()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Boolean][api.type.Boolean]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, keyboard, text input, steam deck, showFloatingGamepadTextInput
> __See also__          [steamworks.showGamepadTextInput()][plugin.steamworks.showGamepadTextInput]
>                       [floatingGamepadTextInputDismissed][plugin.steamworks.event.floatingGamepadTextInputDismissed]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Opens Steam's non-modal floating keyboard, mainly used on Steam Deck. Unlike [steamworks.showGamepadTextInput()][plugin.steamworks.showGamepadTextInput], the text is typed straight into the focused text field as ordinary key events, so nothing needs to be read back. Give the field keyboard focus first.

When the keyboard closes, a [floatingGamepadTextInputDismissed][plugin.steamworks.event.floatingGamepadTextInputDismissed] event is dispatched.

Returns `true` if the keyboard was shown.


## Syntax

	steamworks.showFloatingGamepadTextInput( [mode] [, x] [, y] [, width] [, height] )

##### mode ~^(optional)^~
_[String][api.type.String]._ `"singleLine"`, `"multipleLines"`, `"email"` or `"numeric"`. Defaults to `"singleLine"`. Every mode except `"multipleLines"` closes the keyboard when Enter is pressed.

##### x, y, width, height ~^(optional)^~
_[Number][api.type.Number]._ The text field's rectangle in window pixels, so Steam can avoid covering it. Each defaults to `0`.
