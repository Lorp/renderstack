# RenderStack

[**Try RenderStack now**](https://lorp.github.io/renderstack/dist/)

RenderStack is a web app that renders a font using multiple renderers. It’s useful for finding visual differences in rendering and layout. It also demonstrates how to make each renderer work on the web. Its font menu links to several open-source fonts, and it also allows drag & drop. [Samsa](https://github.com/Lorp/samsa), [Fontkit](https://github.com/foliojs/fontkit) and [Harfbuzz](https://github.com/harfbuzz/harfbuzz) are currently supported. Their renderings appear alongside the browser’s own rendering.

## User guide

* Select a font, or upload a font (font data does not leave the browser’s memory).
* Adjust variation axes.
* Type some text.
* Adjust font size.
* Adjust foreground colour (not yet).
* Inspect the renderings!
* Download the renderings to inspect them in more detail (not yet).

The project seeks to add additional renderers. If your favourite renderer can be built in JavaScript or WASM, then it is welcome to be added to RenderStack. A [FreeType](https://freetype.org) renderer would be particularly welcome.

## Running RenderStack on your own server

The repo includes a ready-to-run version of RenderStack in the `dist` folder. To run it on your own server:

1. Clone this GitHub repository.
2. Place the contents of the `dist` folder somewhere on your server.
3. With a browser, visit the `dist` folder on your server.

## Building RenderStack

To develop RenderStack, or even to edit the default list of fonts, you will need to build it. We use [Parcel](https://parceljs.org) to bundle all the code into a single file, and the following instructions assume this:

1. Install [node and npm](https://nodejs.org/en/download).
2. Install [parcel](https://parceljs.org).
3. Clone this GitHub repository.
4. `cd` to the project folder.
5. Run `npm install` to install dependencies.
6. Run `npm run build` to build the project.

Notes:
* Samsa and Harfbuzz.js are not currently available via npm, so they are included already in node_modules
* Parcel uses [SWC](https://swc.rs) to minify the code
* The command `npm run build` runs the `build` script in `package.json`. This copies the `fonts` folder and `hb.wasm` from `src` to `dist`, and runs Parcel.

## Renderer comparisons

| Renderer         | kB   | Variations | OTL | COLRv0 | COLRv1 | avar2 |
|---               |---   |----        |---  |---     |---    |--- |
| Safari           | –    | ✓          | ✓   | ✓      | ✗     | ✓  |
| Chrome (macOS)   | –    | ✓          | ✓   | ✓      | ✓     | ✓  |
| Chrome (Windows) | –    | ✓          | ✓   | ✓      | ✓     | ✗  |
| Samsa            | 46¹  | ✓          | ✗   | ✓      | ✓     | ✓  |
| Fontkit          | 353¹ | ✓          | ✓   | ✗      | ✗     | ✗  |
| Harfbuzz         | 355² | ✓          | ✓   | ✗      | ✗³    | ✓  |


¹ measured after minification by SWC
² this is the size of `hb.wasm`, compiled with support for avar2 and COLRv1  
³ although Harfbuzz parses the COLRv1 paint tables, JS bindings for hbjs.js have yet to be written


## Implementation notes

### Samsa

Uses Samsa Core v2, a major update on Samsa Core v1. Samsa Core v2 includes handling for:

* avar version 2
* COLRv1 (partial)
* basic layout

Samsa Core v2 is in development, and is not currently available on npm, nor in the [Samsa repo](https://github.com/Lorp/samsa) itself.

### Fontkit

> *Fontkit is an advanced font engine for Node and the browser, used by PDFKit. It supports many font formats, advanced glyph substitution and layout features, glyph path extraction, color emoji glyphs, font subsetting, and more.*

— From the [Fontkit GitHub repo](https://github.com/foliojs/fontkit)


### Harfbuzz 

> *HarfBuzz is a text-shaping engine. If you give HarfBuzz a font and a string containing a sequence of Unicode codepoints, HarfBuzz selects and positions the corresponding glyphs from the font, applying all of the necessary layout rules and font features. HarfBuzz then returns the string to you in the form that is correctly arranged for the language and writing system. HarfBuzz can properly shape all of the world's major writing systems. It runs on all major operating systems and software platforms and it supports the major font formats in use today.*

— From [What is Harfbuzz?](https://harfbuzz.github.io/what-is-harfbuzz.html)


Note that Harfbuzz is not primarily designed for font rendering, but offers SVG-compatible primitives so that glyphs can be displayed as well as laid out. More commonly, Harfbuzz is used by a text library to transform a string of text into a set of glyph IDs and positions, where a renderer such as FreeType generates the glyph images, and the text library positions them using the glyph positions provided by Harfbuzz.

### Building hb.wasm

You may want to update the WebAssembly (Wasm) version of Harfbuzz, for example to try variable components or other new features in development. To do this, clone the Harfbuzz repo, tune Harfbuzz compiler flags, and build a new `hb.wasm`.


