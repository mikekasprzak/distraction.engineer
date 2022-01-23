+++
title = "Web Fonts"
+++

## Glossary
* [TrueType](https://en.wikipedia.org/wiki/TrueType) (TTF) - 80's font standard from Apple, freely licensed to Microsoft.
* [OpenType](https://en.wikipedia.org/wiki/OpenType) (OTF) - Mid 90's collaboration from Microsoft and Adobe. Backwards compatible with TrueType.
* Open Font Format (OFF) - Mid 2000's open ISO/IEC version of the OpenType standard. Weirdly it's identical to the standard defined by Microsoft/Adobe. It's often neglected in conversation in lieu of OpenType.
* [SFNT](https://en.wikipedia.org/wiki/SFNT) - The underlying file structure of `.ttf` and `.otf` files (chunked?). I'm finding mixed messageds about glyph support. Wikipedia suggests `.ttf` supported 65536 glyphs, and with the OpenType Collections extension it can support multiple sets of 65536 glyphs. Other random websites suggested OpenType actually introduced the 65536 upper limit.
* [Font Hinting](https://en.wikipedia.org/wiki/Font_hinting) - From Apple, a method of aligning font data to rasterize sharply, ideal for low resolution displays. Patent expired in 2010.
* [ClearType](https://en.wikipedia.org/wiki/ClearType) - From Microsoft, Subpixel rendering of fonts. Patents expired in 2019.
* [FreeType](https://en.wikipedia.org/wiki/FreeType) - Open Source implementation of TTF and OTF typography. Was crippled by patents (Font Hinting, ClearType) until 2019.
* Font Variations - 2016 collaboration between Microsoft, Adobe, Apple, and Google, based on Adobe's work. Fonts Variations allow you to make a single font that blends between different weights. Browsers can use any weight from 1-999.
* Color Fonts - Doesn't seem very standardized, but each of the four (Microsoft, Adobe, Apple, Google) decided on how they'd support colors in fonts.
* [Compact Font Format, Type 2](https://en.wikipedia.org/wiki/PostScript_fonts#Compact_Font_Format) (CFF2) - Adobe Postscript data format, supported since OpenType 1.8. Outline data is either encoded in "TrueType style" (TTF) or "Postscript style" (CFF2).
* [Web Open Font Format](https://en.wikipedia.org/wiki/Web_Open_Font_Format) (WOFF) - Compressed font formats. WOFF uses zlib, WOFF2 uses Brotli.

[Reference](https://creativemarket.com/blog/the-missing-guide-to-font-formats)


## OpenType is the standard
I'm annoyed by this, but it turns out TrueType was not the standard I thought it was. Microsoft used the TrueType branding for Windows 3.1 and Windows 95, but that was because they hadn't named their efforts. OpenType was announced shortly after in 1996, but it seems this was a non-event I was completely oblivious to it.

SFNT appears to be the general "video container" of fonts, except unlike `.mkv` it was defined decades ago, and continues to be used today. My mistake was assuming, like every other format, the switch from `.ttf` to `.otf` was a disposal, throwing away of the old format. Apparently not. That's pretty amazing on one hand, but a perfect example of what I call the OpenGL problem: most information and tutorials you find for are dated, and not in line with the current specification.

It pains me, but it sound like I need to read the OpenType spec myself to get the whole story.


## Further confusion
OpenType is based on TrueType, and TrueType continued to be used to describe many aspects of OpenType.

It's been suggested that `.ttf` uses quadratic splines, where `.otf` uses cubic splines (cubic can represent circles more accurately with [less nodes](http://cutlings.wasbo.net/ttf-vs-otf-cutting-plotting/)).

After my conversation on Twitter, it's clear I should disregard my previous thinking, and most of what I can find with a quick Google search.

Best I can tell there's little practical difference between `.ttf` and `.otf`, but you don't use CFF data in a `.ttf`... Except I'm lost at exactly what CFF data constitutes (Postscript?). Font Variations seem legal in `.ttf` files even though they came decades after the standard. 


## Using WOFF2
```bash
# Installing
sudo apt install woff2

# compressing a font with WOFF2
woff2_compress font-name.otf
# outputs: font-name.woff2

# decompressing a WOFF2 font
woff2_decompress font-name.woff
# outputs: font-name.otf
```

NOTE: In my example above, my input and outputs were `.ttf` files. It's unclear what is used to decide one versus the other.


## Using WOFF (Legacy)
```bash
# Installing
sudo apt install woff-tools

# compressing a font with WOFF
sfnt2woff font-name.otf
# outputs: font-name.woff

# uncompress a WOFF font
woff2sfnt font-name.woff > out.otf
# output is sent to stdout, and should be captured
```
