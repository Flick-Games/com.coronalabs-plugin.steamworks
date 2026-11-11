# steamworks.getActionSetHandle()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Number][api.type.Number]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, action set, getActionSetHandle
> __See also__          [steamworks.activateActionSet()][plugin.steamworks.activateActionSet]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the handle for an action set declared in your action manifest, for use with [steamworks.activateActionSet()][plugin.steamworks.activateActionSet] and the action origin functions. Returns `0` if the set is not found or Steam Input is unavailable.


## Gotchas

* Shortly after start-up, Steam may not have finished reading the manifest, so a valid set can briefly return `0`. Treat `0` as "not ready yet": never pass it to [steamworks.activateActionSet()][plugin.steamworks.activateActionSet], and ask again on a later frame until you get a non-zero handle. Once non-zero, a handle never changes, so you can cache it.
* `0` is truthy in Lua, so test `handle ~= 0`, not just `handle`.


## Syntax

	steamworks.getActionSetHandle( actionSetName )

##### actionSetName ~^(required)^~
_[String][api.type.String]._ The action set's name as declared in the action manifest, for example `"MenuControls"`.


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local menuSet = steamworks.getActionSetHandle( "MenuControls" )
if ( menuSet ~= 0 ) then
	print( "MenuControls handle: " .. menuSet )
end
``````
