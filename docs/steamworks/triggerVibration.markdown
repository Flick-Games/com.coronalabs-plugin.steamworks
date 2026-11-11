# steamworks.triggerVibration()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, rumble, vibration, triggerVibration
> __See also__          [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Sets a controller's rumble motors. The motors keep running at the given speeds until you call this again, so stop them with speeds of `0`.

Controllers without rumble motors ignore the call, and the player can turn vibration off in Steam.


## Syntax

	steamworks.triggerVibration( controllerHandle, leftSpeed, rightSpeed )

##### controllerHandle ~^(required)^~
_[String][api.type.String]._ A handle from [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers].

##### leftSpeed ~^(required)^~
_[Number][api.type.Number]._ Left (low-frequency) motor speed, from `0` to `65535`.

##### rightSpeed ~^(required)^~
_[Number][api.type.Number]._ Right (high-frequency) motor speed, from `0` to `65535`.


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

-- a short rumble on every connected controller
for _, controller in ipairs( steamworks.getConnectedControllers() ) do
	steamworks.triggerVibration( controller, 30000, 30000 )
	timer.performWithDelay( 150, function()
		steamworks.triggerVibration( controller, 0, 0 )
	end )
end
``````
