# steamworks.isRunningUnderProton()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Boolean][api.type.Boolean]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, proton, linux, isRunningUnderProton
> __See also__          [steamworks.isRunningOnSteamHardware()][plugin.steamworks.isRunningOnSteamHardware]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns `true` when a Windows build of the game is running under Valve's Proton compatibility layer, which is how Windows builds run on Steam Deck, Steam Machine and Linux desktops. Returns `false` on native Windows and macOS, or when the Steam client is unavailable.


## Syntax

	steamworks.isRunningUnderProton()


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

if ( steamworks.isRunningUnderProton() ) then
	print( "Running under Proton" )
end
``````
