# Open Color Format Specification

## Version/Author

V1.0 (2015-04-06)
Author: [Jan Krutisch](mailto:jan@krutisch.de)

## File format

The format is loosely based on the ideas of YAML, but since we've diverted from valid YAML for a few special cases, it is not parseable by YAML parsers anymore.

The character encoding is assumed to be UTF-8 at all times.

## Structure

Colors can be defined in two ways:

```
colorName: <color definition>, <color definition>

# or

colorName:
  <color definition>
  <color definition>
```

See [Color Definitions](#color-definitions) for an explanation of the format used to specify color values.

Colors can be grouped into so called Palettes:

```
paletteName:
  colorName: <color definition>
```

These palettes can be nested. The format does not currently impose a limit on nesting depth.

## Nesting of Groups

In theory, the format allows deep nesting of groups into groups into groups. In practice, it usually doesn't
make any sense to nest a lot deeper than 1-2 levels.

## Meta Data

If a key contains a slash, the key is considered meta data. For example, this defines a palette author:

```
/author: Jan Krutisch
red: #f00

```

As the position of the slash is irrelevant, we can use the slash to namespace metadata, for example to define
palette styles for the open color tools picker:

```
reds:
  ocp/style: compact
  dark red: #f00
```

Metadata can be grouped as well:

```
  meta/:
    author: Astrid Lindgren
```

Parser implementations are expected to flatten the structure of grouped metadata into a simple Hash like structure with
the keys joined by one (and only one) slash, so that the above meta group would look like this(expressed in JSON):

```json
{"meta/author": "Astrid Lindgren"}
```
:exclamation: We're thinking about allowing a short form that omits the colon for grouped metadata, but this is not decided yet.

Metadata can happen on Palettes and Colors:

```
palette:
  meta/author: Jan Krutisch

  color:
    #ff0
    meta/author: Jan Krutisch
```

## References

To allow to construct complex palettes with reusable colors, a color or palette can be defined as a reference to another color.

References use dot-path syntax to traverse the group trees. If no dot is found, the reference is meant to be local to
the current group. The resolver should traverse the tree upwards until the path fully matches. If a path does not match anywhere, the parser should throw an error message.

```
reds:
  dark red: '#800'
  light red: '#f44'
brand colors:
  brand red: =reds.dark red
  secondary red: =brand red

shades: =reds // referencing groups
```

### References as URLs

:exclamation: Currently not implemented by the [reference parser](https://github.com/opencolor-tools/opencolor-js.

Ideally, References could also be resolved as URLs, in a similar way to URL references in stylesheets. Since this is largely implementation dependent (and, in the case of JavaScript, even runtime dependent), the URL resolving is not part of this spec and will not be implemented in the first version of the reference parser. The syntax will look like this:

```
reds: =url(http://design.google.com/colors.oco#reds) // referencing groups via url
blues: =url(file://./pastells.oco#blues) // referencing groups via url, file protocol
```

### Special Metadata values

As metadata is meant to be used to configure views (for example, for our Mac OS X color picker), it makes sense to treat some metadata as typed values. These are:

- Booleans (if 'true' or 'false')
- Numbers (treated as floats)
- Color values (in any color space)
- References

## Color Definitions

Colors can be defined as hex colors (3, 4, 6 or 8 hex digits) or in a generic color space format. Interpretation of these formats is up to the consumer. The hex code, as widely used in web technology and beyond, is treated specially as it doesn't need the "function" syntax.

A color can have multiple definitions, but they must be non-ambiguous. A parser should just parse them all into a map like structure with the color space names as keys and the full color definition as value.

We assume that for the most cases, rgb(a), hsl(a) or hex colors will be used and expect most consuming libraries to implement special handling and validation of these color definitions. For many cases it might be useful to document additional color definitions such as PANTONE names, RAL numbers or even just CYMK values. as long as you define your own color space and keep the needed values in parentheses, you can know yourself out and define what ever you want.

```
short hex color: '#abc'
long hex color: '#aabbcc'
rgb color: rgb(255,0,0)

color: rgb(200,0,0), rgba(255,0,0,0.2), rgb-highcontrast(), CMYK(), RAL(12030),

phantasierot:
  rgb(200,0,0)
  rgba(255,0,0,0.2)
  rgb-highcontrast(255, 0, 0)
  cMYK()
  RAL(12030)
  mixing-rule(Yellow: 765 g, Red 032: 26 g, Black: 11 g, transp. White: 198 g)  
```

## Comments

Comments are using the JavaScript line comment syntax, like this:

```
a group: // this is a comment on a group
  a color: #ffe // colors can be commented as well
// comments can appear on their own lines as well.
```

Comments are meant to completely ignored by any parser implementation. If you want annotate OCO data, please use meta data namespaces.
