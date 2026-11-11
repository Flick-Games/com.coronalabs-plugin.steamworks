# steamworks.setInputActionManifestFilePath()

> --------------------- ------------------------------------------------------------------------------------------
> __Type__              [Function][api.type.Function]
> __Return value__      [Boolean][api.type.Boolean]
> __Revision__          [REVISION_LABEL](REVISION_URL)
> __Keywords__          steam, steamworks, input, controller, action manifest, setInputActionManifestFilePath
> __See also__          [steamworks.getDigitalActionHandle()][plugin.steamworks.getDigitalActionHandle]
>                       [Steam Input (Controllers)][plugin.steamworks.guide.steamInput]
>                       [steamworks.*][plugin.steamworks]
> --------------------- ------------------------------------------------------------------------------------------


## Overview

Tells Steam Input where to find your game's action manifest: the VDF file that declares the game's action sets and actions, and lists the default controller layouts shipped with the game. Returns `true` if Steam accepted the path.

This lets you ship the manifest and layouts inside your game's files instead of uploading them to Steamworks. See the [Steam Input (Controllers)][plugin.steamworks.guide.steamInput] guide for the file format and a sample.


## Gotchas

* Call this as early as possible, straight after requiring the plugin. Steam reads the manifest over the next few frames, so action and action set handles requested soon after can come back as `0`. Keep asking for any `0` handle on later frames until it is non-zero; see [steamworks.getDigitalActionHandle()][plugin.steamworks.getDigitalActionHandle].
* The path must be absolute. Use `system.pathForFile()` with `system.ResourceDirectory` for a manifest bundled with the game. On Windows, converting forward slashes to backslashes avoids problems with mixed separators.
* The layout files listed in the manifest's `configurations` block are looked up relative to the manifest, so keep them in the same folder.


## Syntax

	steamworks.setInputActionManifestFilePath( path )

##### path ~^(required)^~
_[String][api.type.String]._ Absolute path to the action manifest VDF file.


## Example

``````lua
local steamworks = require( "plugin.steamworks" )

local manifestPath = system.pathForFile( "controller_config/action_manifest.vdf", system.ResourceDirectory )
if ( system.getInfo( "platform" ) == "win32" ) then
	manifestPath = manifestPath:gsub( "/", "\\" )
end

steamworks.setInputActionManifestFilePath( manifestPath )
``````
