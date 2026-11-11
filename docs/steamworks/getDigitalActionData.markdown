# steamworks.getDigitalActionData()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Table][api.type.Table]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, button, getDigitalActionData
> __See also__          [steamworks.getDigitalActionHandle()][plugin.steamworks.getDigitalActionHandle]
>                       [steamworks.runFrame()][plugin.steamworks.runFrame]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the current state of a digital action on one controller, as a table with two boolean fields:

* `bState`: `true` while the action is pressed.
* `bActive`: `true` if the action is available in the controller's active action set and bound to something. When `false`, ignore `bState`.

Steam reports state, not events. To detect a press or release, compare `bState` with its value on the previous frame.


## Gotchas

* Call [steamworks.runFrame()][plugin.steamworks.runFrame] once per frame before reading action data, or the state can be stale.
* If a controller always reports `bActive = false`, check that you have activated an action set on it with [steamworks.activateActionSet()][plugin.steamworks.activateActionSet].


## Syntax

	steamworks.getDigitalActionData( controllerHandle, actionHandle )

##### controllerHandle ~^(required)^~
_[String][api.type.String]._ A handle from [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers].

##### actionHandle ~^(required)^~
_[Number][api.type.Number]._ A non-zero handle from [steamworks.getDigitalActionHandle()][plugin.steamworks.getDigitalActionHandle].


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local selectAction = steamworks.getDigitalActionHandle( "select" )
local wasDown = {}

local function pollControllers()
	steamworks.runFrame()
	for _, controller in ipairs( steamworks.getConnectedControllers() ) do
		local data = steamworks.getDigitalActionData( controller, selectAction )
		local isDown = data.bActive and data.bState
		if ( isDown and not wasDown[controller] ) then
			print( "Select pressed on " .. controller )
		end
		wasDown[controller] = isDown
	end
end
Runtime:addEventListener( "enterFrame", pollControllers )
``````
