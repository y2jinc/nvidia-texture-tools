## General Issues ##

  * does nvtt have state? should we have a context? Don't use globals/statics.

## InputOptions ##

  * ~~Default mipmap filter should be Box instead of Kaiser.~~ (Fixed)
  * Remove `Format_LATC`, stop adding more aliases.

### Alpha transparency ###

  * `alphaTransparency` should not be specified in setFormat.
  * Add `setAlphaMode(AlphaMode mode)`

Where `AlphaMode` could be:
  * AlphaMode\_None
  * AlphaMode\_Transparency
  * AlphaMode\_Premultiplied

None is the default (treats alpha as a regular channel) while Transparency and Premultiplied are hints. Currently there are some compressors that only support None, others that only support Transparency.

### Mipmapping options ###

~~Option 1:~~

  * Deprecate setMipmapping.
  * Add `setMipmapFilter(MipmapFilter filter)`
  * ~~Add maxLevel argument to `setTextureLayout`~~
  * To disable mipmap output set maxLevel = 0.

Option 2:

  * Deprecate setMipmapping.
  * Add `setMipmapFilter(MipmapFilter filter)`
  * Add `enableMipmaps(bool enable)`
  * Add `setMipmapMaxLevel(int maxLevel)`

Option 3:

  * Add `setMipmapFilter(MipmapFilter filter)`
  * Remove filter argument from `setMipmapping`: `setMipmapping(bool enabled, int maxLevel)`

Option 4:

  * Leave things as is.


## CompressionOptions ##

  * `enableHardwareCompressor`, should be called `enableCudaCompressor` or `enableGpuCompressor`.
  * `Quantization` should be a compression option.
  * remove error threshold from setQuality, it's not used.
  * `setColorWeights` should take an alpha weight, not used right now, but useful for future formats.

## OutputOptions ##

  * Should be a pimpl like `InputOptions` and `CompressionOptions`. Member attributes should be hidden.

  * Add `setFileName()` member to output dds file directly instead of using output handler.

### Output Handling ###

  * `OutputHandler::writeData` should return false to signal a write error and stop processing.
  * The `mipmap` method of `OutputHandler` should have been called `image` or `beginImage`.


### Error Handling ###

  * Error\_Unknown should be 0
  * Error\_UserInterruption is not being used and could be removed.