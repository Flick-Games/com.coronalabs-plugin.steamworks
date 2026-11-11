# steamworks.getAnalogActionHandle()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Number][api.type.Number]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, stick, trigger, getAnalogActionHandle
> __See also__          [steamworks.getAnalogActionData()][plugin.steamworks.getAnalogActionData]
>                       [steamworks.getAnalogActionOrigins()][plugin.steamworks.getAnalogActionOrigins]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the handle for an analog action declared in the `"StickPadGyro"` or `"AnalogTrigger"` block of your action manifest, such as "move" or "camera". Returns `0` if the action is not found or Steam Input is unavailable.


## Gotchas

* As with [steamworks.getDigitalActionHandle()][plugin.steamworks.getDigitalActionHandle], valid actions can briefly return `0` at start-up. Ask again on later frames until the handle is non-zero, then cache it.


## Syntax

	steamworks.getAnalogActionHandle( actionName )

##### actionName ~^(required)^~
_[String][api.type.String]._ The action's name as declared in the action manifest, for example `"cursor_move"`.


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local moveAction = steamworks.getAnalogActionHandle( "cursor_move" )
``````
