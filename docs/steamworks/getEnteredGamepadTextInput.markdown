# steamworks.getEnteredGamepadTextInput()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [String][api.type.String]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, keyboard, text input, getEnteredGamepadTextInput
> __See also__          [steamworks.showGamepadTextInput()][plugin.steamworks.showGamepadTextInput]
>                       [gamepadTextInputDismissed][plugin.steamworks.event.gamepadTextInputDismissed]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the text most recently submitted through [steamworks.showGamepadTextInput()][plugin.steamworks.showGamepadTextInput]. Returns an empty string if nothing has been submitted, or `nil` if Steam is unavailable.

The [gamepadTextInputDismissed][plugin.steamworks.event.gamepadTextInputDismissed] event already carries this text as `event.text`, so most games never need to call this.


## Syntax

	steamworks.getEnteredGamepadTextInput()
