# steamworks.initSteamInput()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Boolean][api.type.Boolean]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, initSteamInput
> __See also__          [steamworks.setInputActionManifestFilePath()][plugin.steamworks.setInputActionManifestFilePath]
>                       [steamworks.runFrame()][plugin.steamworks.runFrame]
>                       [steamworks.shutdownSteamInput()][plugin.steamworks.shutdownSteamInput]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Initialises Steam Input, Steam's controller API. Returns `true` on success, or `false` if Steam Input is unavailable (for example, when the Steam client is not running).

The plugin already initialises Steam Input when it loads, so you do not need to call this before using the other Steam Input functions. Calling it anyway is harmless. It is mainly for starting Steam Input again after [steamworks.shutdownSteamInput()][plugin.steamworks.shutdownSteamInput].

See the [Steam Input (Controllers)][plugin.steamworks.guide.steamInput] guide for the full setup.


## Syntax

	steamworks.initSteamInput()


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

-- start controller input again after shutting it down
if ( steamworks.initSteamInput() ) then
	print( "Steam Input ready" )
end
``````
