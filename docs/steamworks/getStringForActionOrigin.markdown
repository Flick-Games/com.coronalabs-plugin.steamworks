# steamworks.getStringForActionOrigin()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [String][api.type.String]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, origin, getStringForActionOrigin
> __See also__          [steamworks.getGlyphForActionOrigin()][plugin.steamworks.getGlyphForActionOrigin]
>                       [steamworks.getDigitalActionOrigins()][plugin.steamworks.getDigitalActionOrigins]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Returns the name of an action origin, such as `"A Button"` or `"Cross"`, in the language of the player's Steam client. Returns `nil` if Steam has no name for the origin or Steam Input is unavailable.

Useful as a text fallback when [steamworks.getGlyphForActionOrigin()][plugin.steamworks.getGlyphForActionOrigin] returns `nil`.


## Syntax

	steamworks.getStringForActionOrigin( origin )

##### origin ~^(required)^~
_[Number][api.type.Number]._ An `EInputActionOrigin` value, from [steamworks.getDigitalActionOrigins()][plugin.steamworks.getDigitalActionOrigins] or [steamworks.getAnalogActionOrigins()][plugin.steamworks.getAnalogActionOrigins].


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local name = steamworks.getStringForActionOrigin( origin )
if ( name ) then
	hintText.text = "Press " .. name
end
``````
