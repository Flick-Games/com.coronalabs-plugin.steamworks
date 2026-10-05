# steamworks.getSteamHardwareDefaultConfig()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [String][api.type.String]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, hardware, settings, getSteamHardwareDefaultConfig
> __See also__          [steamworks.isRunningOnSteamHardware()][plugin.steamworks.isRunningOnSteamHardware]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the default settings configuration Steam suggests for the current device. The value for each device can be changed on the Steamworks partner site without rebuilding the game, so use it to pick a tuned preset when the player first launches.

Returns one of:

* `"low"`, `"medium"`, `"high"`, `"max"`: a general preset.
* `"steamDeck"`, `"steamMachine"`, `"steamFrame"`: a preset tuned for that device.
* `"none"`: no default configured, or the Steam client is unavailable.


## Syntax

	steamworks.getSteamHardwareDefaultConfig()


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local config = steamworks.getSteamHardwareDefaultConfig()
if ( config == "steamDeck" ) then
	-- apply the settings tuned for Steam Deck
elseif ( config == "low" ) then
	-- apply the low preset
end
``````
