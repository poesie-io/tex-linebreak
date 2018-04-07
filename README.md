# tex-linebreak

_tex-linebreak_ is a JavaScript library for laying out justified text as you
would find in a newspaper, book or technical paper.

It implements the Knuth-Plass line-breaking algorithm, as used by TeX. Compared
to standard methods of justifying text (eg.  `text-align: justify` in CSS) this
method produces more optimal spacing with fewer lines having spacing between
words that is too tight or too loose, both of which are difficult to read.

_tex-linebreak_ has no dependencies on a particular JS environment (browser,
Node) or render target (`<canvas>`, HTML elements, PDF).

<table>
  <tr>
    <td>CSS `text-align: justify` (Firefox 61)</td>
    <td>_tex-linebreak_ output rendered</td>
  </tr>
  <tr>
    <td><img width="400" src="images/css-text-align-output.png"></td>
    <td><img width="400" src="images/tex-linebreak-output.png"></td>
  </tr>
  <tr>
    <td>Text justified using the browser's `text-align` option
        can have a large amount of inter-word spacing in places.
        This depends on the browser's implementation, but it affects
        Safari, Firefox and Chrome to varying degrees.</td>
    <td>The TeX algorithm in contrast minimizes the amount of
        stretching/shrinking of spaces required over the whole paragraph,
        producing fewer large gaps and making the output more comfortable
        to read.</td>
  </tr>
</table>

## Usage

First, add the _tex-linebreak_ package to your dependencies.

```sh
npm install tex-linebreak
```

Then use the APIs to lay out and render text:

```js
import { layoutItemsFromString, breakLines, positionItems } from 'tex-linebreak';

// Convert your text to a set of "box", "glue" and "penalty" items used by the
// line-breaking process.
//
// "Box" items are things (typically words) to typeset.
// "Glue" items are spaces that can stretch or shrink or be a breakpoint.
// "Penalty" items are possible breakpoints (hyphens, end of a paragraph etc.).
//
// `layoutItemsFromString` is a helper that takes a string and a function to
// measure the width of a piece of that string and returns a suitable set of
// items.
const measureText = text => text.length * 5;
const items = layoutItemsFromString(yourText, measureText);

// Find where to insert line-breaks in order to optimally lay out the text.
const lineWidth = 200;
const breakpoints = breakLines(items, lineWidth)

// Compute the (xOffset, line number) at which to draw each box item.
const positionedItems = positionItems(items, lineWidth, breakpoints);

positionedItems.forEach(pi => {
  const item = items[pi.item];

  // Add code to draw `item.text` at `(box.xOffset, box.line)` to whatever output
  // you want, eg. `<canvas>`, HTML elements with spacing created using CSS,
  // WebGL, ...
});
```

For working code showing different ways to use this library, see [the
demos](src/demos/).

## API

See [src/layout.ts](src/layout.ts) for API documentation.

## References

[1] D. E. Knuth and M. F. Plass, “[Breaking paragraphs into lines](http://www.eprg.org/G53DOC/pdfs/knuth-plass-breaking.pdf),” Softw. Pract. Exp., vol. 11, no. 11, pp. 1119–1184, Nov. 1981.
