# steamworks.isSteamDeck()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Boolean][api.type.Boolean]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, Steam Deck, isSteamDeck
> __See also__          [steamworks.isRunningOnSteamHardware()][plugin.steamworks.isRunningOnSteamHardware]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns `true` when the game is running on a Steam Deck, `false` otherwise or when the Steam client is unavailable.

Prefer [steamworks.isRunningOnSteamHardware()][plugin.steamworks.isRunningOnSteamHardware] in new code: it also recognises Steam Machine and Steam Frame.


## Syntax

	steamworks.isSteamDeck()


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

if ( steamworks.isSteamDeck() ) then
	print( "Running on Steam Deck" )
end
``````
