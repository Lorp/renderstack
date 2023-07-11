# RenderRater

**Try RenderRater now**

RenderRater is a web app that renders fonts using multiple renderers, primarily for the purpose of testing for visual differences in rendering and layout. It also serves as a demo of how to make these renderers work on the web.

It currently supports [Samsa](https://github.com/Lorp/samsa), [Fontkit](https://github.com/foliojs/fontkit) and [Harfbuzz](https://github.com/harfbuzz/harfbuzz), displaying their renderings alongside the browser’s own rendering.

## User guide

* Select a font, or upload a font.
* Adjust variation axes.
* Type some text.
* Adjust font size.
* Adjust foreground colour.
* Download renderings to inspect them in detail.

which is immediately rendered not only by browser in the normal way, but also by Samsa, Fontkit and Harfbuzz.

The project is actively seeking to add additional renderers. If your favrouite renderer can be built in JavaScript or WASM, then it is welcome to be added to RenderRater.

## Legal notice

When using the upload feature, no font data is transferred beyond the browser.



## Implementation notes

### Samsa

Uses Samsa Core v2, a major update on Samsa Core v1. Samsa Core v2 includes handling for:

* avar version 2
* COLRv1 (partial)
* basic layout

The Samsa library uses 46 kB (minified).

### Fontkit

The Fontkit library uses 353 kB (minified).

### Harfbuzz 

Harfbuzz
Harfbuzz.js

Building a custom Harfbuzz.

The Samsa WASM uses 355 kB.

## How to build RenderRater



## Renderer comparisons

| Renderer         | kB  | Variations | OTL | COLRv0 | COLRv1 | avar2 |
|---               |---  |----        |---  |---     |---    |--- |
| Safari           | –   | ✓          | ✓   | ✓      | ✗     | ✓  |
| Chrome (macOS)   | –   | ✓          | ✓   | ✓      | ✓     | ✓  |
| Chrome (Windows) | –   | ✓          | ✓   | ✓      | ✓     | ✗  |
| Samsa            | 46  | ✓          | ✗   | ✓      | ✓     | ✓  |
| Fontkit          | 353 | ✓          | ✓   | ✗      | ✗     | ✗  |
| Harfbuzz         | 355 | ✓          | ✓   | ✗      | ✗     | ✓  |


\* Minifications performed using [swc](https://swc.rs)