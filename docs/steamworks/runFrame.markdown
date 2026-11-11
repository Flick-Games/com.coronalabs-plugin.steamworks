# steamworks.runFrame()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, runFrame
> __See also__          [steamworks.getDigitalActionData()][plugin.steamworks.getDigitalActionData]
>                       [steamworks.getAnalogActionData()][plugin.steamworks.getAnalogActionData]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Updates Steam Input's controller state for the current frame. Call it once per frame, before reading any action data or origins, typically at the top of an `"enterFrame"` listener.


## Gotchas

* Without this call, [steamworks.getDigitalActionData()][plugin.steamworks.getDigitalActionData], [steamworks.getAnalogActionData()][plugin.steamworks.getAnalogActionData] and the action origin functions can return stale or inactive results, whatever the player's bindings are.


## Syntax

	steamworks.runFrame()


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local function pollControllers()
	steamworks.runFrame()
	-- read action data here
end
Runtime:addEventListener( "enterFrame", pollControllers )
``````
