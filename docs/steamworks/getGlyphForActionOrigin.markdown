# steamworks.getGlyphForActionOrigin()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [String][api.type.String]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, glyph, getGlyphForActionOrigin
> __See also__          [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the absolute path to a PNG image of the button or control for an action origin, as returned by `steamworks.getDigitalActionOrigins()` or `steamworks.getAnalogActionOrigins()`. The art matches the controller that produced the origin (Xbox, PlayStation, Steam Controller, Steam Deck and so on).

Returns `nil` if Steam Input is unavailable or Steam has no glyph for the origin.


## Gotchas

* The path points into the Steam client's install folder. Solar2D display APIs expect a filename relative to a base directory, so copy the file into `system.CachesDirectory` (or similar) before loading it.
* Do not cache origins for long. Players can remap buttons mid-session, so query origins again regularly and fetch a new glyph when an origin changes.


## Syntax

	steamworks.getGlyphForActionOrigin( origin [, size] [, styleFlags] )

##### origin ~^(required)^~
_[Number][api.type.Number]._ An `EInputActionOrigin` value.

##### size ~^(optional)^~
_[String][api.type.String]._ `"small"` (32&nbsp;px), `"medium"` (128&nbsp;px) or `"large"` (256&nbsp;px). Defaults to `"medium"`.

##### styleFlags ~^(optional)^~
_[Number][api.type.Number]._ `ESteamInputGlyphStyle` flags. Defaults to `0` (Knockout). Choose one base style and optionally add modifiers:

* `0x0`: Knockout. Face buttons have coloured labels or outlines on a knocked-out background.
* `0x1`: Light. Black detail on a white background.
* `0x2`: Dark. White detail on a black background.
* `0x10`: NeutralColorABXY modifier. Face buttons use the base style colour instead of their usual colours.
* `0x20`: SolidABXY modifier. Face buttons have a solid fill.


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

-- Dark style with neutral face button colours, 128 px
local path = steamworks.getGlyphForActionOrigin( origin, "medium", 0x2 + 0x10 )
if path then
	print( "Glyph at: " .. path )
end
``````
