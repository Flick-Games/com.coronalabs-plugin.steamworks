# steamworks.getDigitalActionHandle()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Number][api.type.Number]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, button, getDigitalActionHandle
> __See also__          [steamworks.getDigitalActionData()][plugin.steamworks.getDigitalActionData]
>                       [steamworks.getDigitalActionOrigins()][plugin.steamworks.getDigitalActionOrigins]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the handle for a digital (on/off) action declared in the `"Button"` block of your action manifest, such as "jump" or "confirm". Returns `0` if the action is not found or Steam Input is unavailable.


## Gotchas

* Shortly after start-up, Steam may not have finished reading the manifest, so valid actions can briefly return `0` (often every action except the first one or two). Ask again on later frames until every handle is non-zero. Once non-zero, a handle never changes, so you can cache it.
* `0` is truthy in Lua, so test `handle ~= 0`, not just `handle`.


## Syntax

	steamworks.getDigitalActionHandle( actionName )

##### actionName ~^(required)^~
_[String][api.type.String]._ The action's name as declared in the action manifest, for example `"select"`.


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local selectAction = steamworks.getDigitalActionHandle( "select" )
if ( selectAction == 0 ) then
	-- not ready yet: try again on a later frame
end
``````
