# steamworks.shutdownSteamInput()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, shutdownSteamInput
> __See also__          [steamworks.initSteamInput()][plugin.steamworks.initSteamInput]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Shuts down Steam Input. Does nothing if Steam Input is unavailable.

The plugin shuts Steam Input down automatically when it unloads, so you only need this to stop controller input early. Call [steamworks.initSteamInput()][plugin.steamworks.initSteamInput] to start it again.


## Syntax

	steamworks.shutdownSteamInput()


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

-- stop reading controllers, e.g. while a native video plays
Runtime:removeEventListener( "enterFrame", pollControllers )
steamworks.shutdownSteamInput()
``````
