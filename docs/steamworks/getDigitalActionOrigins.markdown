# steamworks.getDigitalActionOrigins()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Table][api.type.Table]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, glyph, origin, getDigitalActionOrigins
> __See also__          [steamworks.getGlyphForActionOrigin()][plugin.steamworks.getGlyphForActionOrigin]
>                       [steamworks.getStringForActionOrigin()][plugin.steamworks.getStringForActionOrigin]
>                       [steamworks.getAnalogActionOrigins()][plugin.steamworks.getAnalogActionOrigins]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns an array of the physical controls (action origins) currently bound to a digital action, for one controller and action set. Each entry is a number, an `EInputActionOrigin` value. The array is empty if the action is unbound or Steam Input is unavailable.

Use the origins to show the player which button to press, with [steamworks.getGlyphForActionOrigin()][plugin.steamworks.getGlyphForActionOrigin] (button art) or [steamworks.getStringForActionOrigin()][plugin.steamworks.getStringForActionOrigin] (a name such as "A Button").


## Gotchas

* Players can remap controls at any time, including mid-game. Query origins again regularly (for example every half second) rather than once at start-up.
* An action can be bound to more than one control. Most games show the first origin.


## Syntax

	steamworks.getDigitalActionOrigins( controllerHandle, actionSetHandle, actionHandle )

##### controllerHandle ~^(required)^~
_[String][api.type.String]._ A handle from [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers].

##### actionSetHandle ~^(required)^~
_[Number][api.type.Number]._ The action set to look in, from [steamworks.getActionSetHandle()][plugin.steamworks.getActionSetHandle].

##### actionHandle ~^(required)^~
_[Number][api.type.Number]._ A handle from [steamworks.getDigitalActionHandle()][plugin.steamworks.getDigitalActionHandle].


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local controller = steamworks.getConnectedControllers()[1]
local menuSet = steamworks.getActionSetHandle( "MenuControls" )
local selectAction = steamworks.getDigitalActionHandle( "select" )

local origins = steamworks.getDigitalActionOrigins( controller, menuSet, selectAction )
if ( origins[1] ) then
	print( "Press " .. ( steamworks.getStringForActionOrigin( origins[1] ) or "?" ) .. " to select" )
end
``````
