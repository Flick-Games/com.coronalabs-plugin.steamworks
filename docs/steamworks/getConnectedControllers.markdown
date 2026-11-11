# steamworks.getConnectedControllers()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Table][api.type.Table]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, getConnectedControllers
> __See also__          [steamworks.getInputTypeForHandle()][plugin.steamworks.getInputTypeForHandle]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns an array of handles for the controllers currently connected. The array is empty when no controller is connected or Steam Input is unavailable.

Pass these handles to the other Steam Input functions, such as [steamworks.getDigitalActionData()][plugin.steamworks.getDigitalActionData].


## Gotchas

* Each handle is a __string__, not a number. Controller handles are 64-bit values, which a Lua number cannot hold exactly. Keep them as strings and pass them back unchanged. They also work as table keys.
* Controllers can be connected and disconnected at any time. Call this every frame (after [steamworks.runFrame()][plugin.steamworks.runFrame]) rather than caching the list, and activate your current action set on any controller you have not seen before.


## Syntax

	steamworks.getConnectedControllers()


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local controllers = steamworks.getConnectedControllers()
for i = 1, #controllers do
	print( "Controller " .. i .. ": " .. controllers[i] .. " (" .. steamworks.getInputTypeForHandle( controllers[i] ) .. ")" )
end
``````
