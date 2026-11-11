# steamworks.activateActionSet()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, action set, activateActionSet
> __See also__          [steamworks.getActionSetHandle()][plugin.steamworks.getActionSetHandle]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Switches a controller to an action set, for example from a menu set to an in-game set. While a set is active, only its actions report data, and Steam applies the player's bindings for that set.


## Gotchas

* A newly connected controller has no action set until you activate one, and until then its actions report `bActive = false`. Activate your current set on every controller handle you have not seen before (see [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers]).
* Never pass an action set handle of `0`. See [steamworks.getActionSetHandle()][plugin.steamworks.getActionSetHandle].
* To apply a set to every controller at once, pass the string `"18446744073709551615"` (Steam's all-controllers handle) as `controllerHandle`.


## Syntax

	steamworks.activateActionSet( controllerHandle, actionSetHandle )

##### controllerHandle ~^(required)^~
_[String][api.type.String]._ A handle from [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers], or `"18446744073709551615"` for all controllers.

##### actionSetHandle ~^(required)^~
_[Number][api.type.Number]._ A non-zero handle from [steamworks.getActionSetHandle()][plugin.steamworks.getActionSetHandle].


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local ALL_CONTROLLERS = "18446744073709551615"

local gameplaySet = steamworks.getActionSetHandle( "GameplayControls" )
if ( gameplaySet ~= 0 ) then
	steamworks.activateActionSet( ALL_CONTROLLERS, gameplaySet )
end
``````
