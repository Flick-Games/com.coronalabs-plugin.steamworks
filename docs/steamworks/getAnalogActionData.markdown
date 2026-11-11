# steamworks.getAnalogActionData()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Table][api.type.Table]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, stick, trigger, getAnalogActionData
> __See also__          [steamworks.getAnalogActionHandle()][plugin.steamworks.getAnalogActionHandle]
>                       [steamworks.runFrame()][plugin.steamworks.runFrame]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the current state of an analog action on one controller, as a table with these fields:

* `x`, `y`: the action's position. For a `joystick_move` action each runs from `-1` to `1`, with `y` positive __up__ (the opposite of Solar2D's screen coordinates). For a trigger, `x` runs from `0` to `1`.
* `bActive`: `true` if the action is available in the controller's active action set and bound to something. When `false`, ignore `x` and `y`.

The values depend on the action's `input_mode` in the action manifest. See Valve's [action manifest documentation](https://partner.steamgames.com/doc/features/steam_controller/iga_file) for the other modes.


## Gotchas

* Call [steamworks.runFrame()][plugin.steamworks.runFrame] once per frame before reading action data.
* Apply your own dead zone: a resting stick rarely reports exactly `0`.


## Syntax

	steamworks.getAnalogActionData( controllerHandle, actionHandle )

##### controllerHandle ~^(required)^~
_[String][api.type.String]._ A handle from [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers].

##### actionHandle ~^(required)^~
_[Number][api.type.Number]._ A non-zero handle from [steamworks.getAnalogActionHandle()][plugin.steamworks.getAnalogActionHandle].


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local moveAction = steamworks.getAnalogActionHandle( "cursor_move" )
local DEAD_ZONE = 0.1

local function pollControllers( event )
	steamworks.runFrame()
	for _, controller in ipairs( steamworks.getConnectedControllers() ) do
		local data = steamworks.getAnalogActionData( controller, moveAction )
		if ( data.bActive and ( math.abs( data.x ) > DEAD_ZONE or math.abs( data.y ) > DEAD_ZONE ) ) then
			player.x = player.x + data.x * 10
			player.y = player.y - data.y * 10   -- Steam's y is positive up
		end
	end
end
Runtime:addEventListener( "enterFrame", pollControllers )
``````
