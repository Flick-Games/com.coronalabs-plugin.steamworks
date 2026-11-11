# steamworks.showBindingPanel()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Boolean][api.type.Boolean]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, remap, bindings, showBindingPanel
> __See also__          [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Opens Steam's controller configuration screen for your game in the Steam overlay, where the player can remap controls for one controller. Returns `true` if the screen was shown.

Games using Steam Input should offer this (for example, from a "Controller settings" button) rather than building their own remapping screen.


## Gotchas

* It needs the Steam overlay. It returns `false` when the overlay is unavailable, such as in the Corona Simulator unless it was launched from Steam. See the [Overlay Troubleshooting][plugin.steamworks.guide.overlays] guide.
* When the screen closes, the player's bindings may have changed. Query action origins again before showing button prompts.


## Syntax

	steamworks.showBindingPanel( controllerHandle )

##### controllerHandle ~^(required)^~
_[String][api.type.String]._ A handle from [steamworks.getConnectedControllers()][plugin.steamworks.getConnectedControllers].


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local controller = steamworks.getConnectedControllers()[1]
if ( controller and not steamworks.showBindingPanel( controller ) ) then
	print( "Steam overlay unavailable" )
end
``````
