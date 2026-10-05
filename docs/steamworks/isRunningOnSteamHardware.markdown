# steamworks.isRunningOnSteamHardware()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [String][api.type.String]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, hardware, Steam Deck, Steam Machine, Steam Frame, isRunningOnSteamHardware
> __See also__          [steamworks.getSteamHardwareDefaultConfig()][plugin.steamworks.getSteamHardwareDefaultConfig]
>                       [steamworks.isRunningUnderProton()][plugin.steamworks.isRunningUnderProton]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns which Steam hardware the game is running on, so you can adapt layout, text size or default settings to the device.

Returns one of:

* `"steamDeck"`
* `"steamMachine"`
* `"steamFrame"`
* `"none"`: not running on Steam hardware, or the Steam client is unavailable.

This replaces `steamworks.isSteamDeck()`, which only recognises the Steam Deck.


## Gotchas

* Only use this for presentation and defaults. Controller glyphs should come from `steamworks.getGlyphForActionOrigin()`, which already matches the controller in use.


## Syntax

	steamworks.isRunningOnSteamHardware()


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local hardware = steamworks.isRunningOnSteamHardware()
if ( hardware ~= "none" ) then
	print( "Running on " .. hardware )
end
``````
