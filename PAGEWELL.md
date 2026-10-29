# libmspub as used in Pagewell

This is [libmspub](https://git.libreoffice.org/libmspub) (MPL-2.0) at upstream commit
66701376e9eedcdb85f87bb78bb0ce102a213280, with the changes below. Pagewell, a Microsoft Store app by Cobaltline
Software that opens Microsoft Publisher (.pub) files, links it statically into its file engine. Every modified file is
published here under its original license (MPL-2.0); the changes are offered upstream.

Branch `pagewell`, tag `pagewell-1.0.0`.

## Changes

- **Lists:** the bullet character and numbering type were read from the wrong record (`info` instead of
  `listSubInfo`). List information is now emitted as `libmspub:list-type`, `list-bullet`, `list-numbering`,
  `list-start` and `list-delimiter` paragraph properties.
- **Linked text boxes:** text objects carry `libmspub:text-id` (the story) and `libmspub:text-chain-index` (the box's
  place in the chain, from shape chunk field 0x28; absent on the first box), so a consumer can flow one story box to box.
- **Custom geometry from the file:**
  - `parseGuides` was not implemented; the MS-ODRAW guide records (pGuides) are now read, so shapes whose vertices
    are formulas no longer collapse to a point.
  - `geoLeft` / `geoTop` are honoured (geometry spaces such as -32000..32000), including the geometry special values.
  - A vertex refers to a guide only when its high 16 bits are 0x8000 (MS-ODRAW); other values with the top bit set are
    negative coordinates.
  - `msosgfIfElse` tests `param1 > 0` (it tested non-zero), and `msosgfMod` is `sqrt(a² + b² + c²)` (it was `a·b·c`).

## Building

Pagewell builds libmspub and librevenge (unmodified) from source with MSYS2 UCRT64 g++ (C++17, static), with ICU
replaced by a small Win32 shim; see the Pagewell notices for the full list of components and licenses.
