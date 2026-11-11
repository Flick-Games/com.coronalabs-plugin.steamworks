# steamworks.getAnalogActionOrigins()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Table][api.type.Table]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, glyph, origin, getAnalogActionOrigins
> __See also__          [steamworks.getGlyphForActionOrigin()][plugin.steamworks.getGlyphForActionOrigin]
>                       [steamworks.getDigitalActionOrigins()][plugin.steamworks.getDigitalActionOrigins]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns an array of the physical controls (action origins) currently bound to an analog action, such as a stick or trackpad, for one controller and action set. Each entry is a number, an `EInputActionOrigin` value. The array is empty if the action is unbound or Steam Input is unavailable.

Works like [steamworks.getDigitalActionOrigins()][plugin.steamworks.getDigitalActionOrigins]; see that page for the gotchas.


## Syntax

	steamworks.getAnalogActionOrigins( controllerHandle, actionSetHandle, actionHandle )

##### controllerHandle ~^(required)^~
_[String][api.type.String]._ A handle from [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers].

##### actionSetHandle ~^(required)^~
_[Number][api.type.Number]._ The action set to look in, from [steamworks.getActionSetHandle()][plugin.steamworks.getActionSetHandle].

##### actionHandle ~^(required)^~
_[Number][api.type.Number]._ A handle from [steamworks.getAnalogActionHandle()][plugin.steamworks.getAnalogActionHandle].


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local controller = steamworks.getConnectedControllers()[1]
local gameplaySet = steamworks.getActionSetHandle( "GameplayControls" )
local moveAction = steamworks.getAnalogActionHandle( "cursor_move" )

local origins = steamworks.getAnalogActionOrigins( controller, gameplaySet, moveAction )
local glyphPath = origins[1] and steamworks.getGlyphForActionOrigin( origins[1], "small" )
``````
