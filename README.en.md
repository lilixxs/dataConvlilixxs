# STM32 Data Type Converter

[简体中文](./README.md) | English

An online converter for STM32 embedded data types.
The interface follows the Claude Code light minimal style with square corners.
Live demo: <https://lilixxs.github.io/dataConvlilixxs/>

## Overview & Live Demo

This project converts a value between 10 standard data types at the same time.
It runs as a static web page. It needs no build step and no external library.
Open the live demo at <https://lilixxs.github.io/dataConvlilixxs/>.

## Core Features

1. **10 standard data types in one view**
   - Signed integers: `int8`, `int16`, `int32`, `int64`.
   - Unsigned integers: `uint8`, `uint16`, `uint32`, `uint64`.
   - IEEE 754 floats: `float` (32-bit single precision) and `double` (64-bit double precision).
2. **Per-row endianness reordering**
   - 16-bit rows support `AB` (big-endian) and `BA` (little-endian).
   - 32-bit rows support `ABCD` (big-endian), `DCBA` (little-endian), `BADC` (byte swap) and `CDAB` (word swap).
   - 64-bit rows support `ABCDEFGH` (big-endian), `HGFEDCBA` (little-endian), `BADCFEHG` (byte swap), `GHEFCDAB` (word swap) and `EFGHABCD` (double-word swap).
3. **Multi-radix input and switching**
   - The tool accepts decimal input, scientific notation, `0x` hex input and `0b` binary input.
   - One click switches every output to decimal, hex or binary.
4. **Visual byte spacing and space-free copy**
   - In hex and binary mode, the page shows each byte as a separate block. It uses CSS margins only.
   - The copy button and the selection copy action both put a continuous string on the clipboard. The string contains no space, for example `0x12345678`. You can paste it into C code.
5. **Chinese and English interface**
   - UI strings live in `i18n/zh-CN.json` and `i18n/en-US.json`.
   - Help documents live in `i18n/help.zh-CN.md` and `i18n/help.en-US.md`.

## Directory Structure

- `lilixxsDataConv.html`: The main application page. It holds the UI layout, the conversion logic and the i18n loader. The page reads the external language files and help documents from the `i18n/` directory over HTTP `fetch`.
- `index.html`: The redirect entry for the GitHub Pages root path.
- `i18n/zh-CN.json`: The Simplified Chinese UI language file.
- `i18n/en-US.json`: The English UI language file.
- `i18n/help.zh-CN.md`: The Simplified Chinese help document in Markdown format.
- `i18n/help.en-US.md`: The English help document in Markdown format.
- `claude_code_light_ui_style_guide.md`: The UI style reference document.
- `README.md`: The Chinese project readme.
- `README.en.md`: The English project readme.
- `CONTRIBUTING.md`: The guide for contributions and i18n maintenance.
- `LICENSE`: The MIT license text.

## Local Development

The page reads the external language files and the Markdown help documents from the `i18n/` directory over HTTP `fetch`.
Start a local static server before you preview the page.

```bash
python -m http.server 8000
```

Then open `http://localhost:8000/lilixxsDataConv.html` in a browser.

## Contributing & i18n Maintenance

When you add a feature or a new language, update the files in `i18n/*.json` and `i18n/help.*.md`. Read [CONTRIBUTING.md](./CONTRIBUTING.md) for the field specification.

## Author & License

Author: [lilixxs](https://github.com/lilixxs/dataConvlilixxs)

This project uses the [MIT License](./LICENSE).
