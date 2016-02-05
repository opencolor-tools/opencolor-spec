# opencolor tools specification version 1.0

This tries to document the current status

## File format

The format is based on YAML, but has a pelicularity that might prevent parsing it with specific YAML parsers: 
We're using ordered keys. This is explicitly not a YAML feature and only works in a few languages and probably 
not with all YAML parsers.

## Structure

Colors can be defined in two ways:

```yaml
colorName: <color definition>

# or

colorName:
  value: <color definition>
```

This is a hack to allow tacking arbitrary meta data onto a color.

Colors can be grouped into named groups:

```yaml
groupName:
  colorName: <color definition>
```

These groups can be nested. The format does not oppose a limit on nesting depth

## Meta Data

If a key contains a slash, the key is considered meta data. For example, this defines a palette author:

```yaml
/author: Jan Krutisch
red: #f00
```

As the position of the slash is irrelevant, we can use the slash to namespace metadata, for example to define 
palette styles for the open color tools picker:

```yaml
reds:
  ocp/style: compact
  dark red: #f00
```

## References

To allow to construct complex palettes with reusable colors, a color can be defined as a reference to another color.

References use dot-path syntax to traverse the group trees. If no dot is found, the reference is meant to be local to 
the current group (TODO: Is this correct? Or does the current implementation try to resolve the dot path from within the 
current group upwards the tree?)

```yaml
reds:
  dark red: '#800'
  light red: '#f44'
brand colors:
  brand red: =reds.dark red
  secondary red: =brand red
```

## Color Formats

Colors can be defined as hex colors (3 or 6 hex digits) or as rgb() colors in css syntax. Since the hash symbol in YAML 
means "comment", hex colors need to be quoted.

```yaml
short hex color: '#abc'
long hex color: '#aabbcc'
rgb color: rgb(255,0,0)
```

## Shortcomings

- The value syntax is confusing
- The quoted hex strings add superfluous noise. Could be fixed by allowing hex digits without the hash, but that will 
  confuse people as well
- meaningful indentation is a potential source of confusion and anger, especially if people start to mix tabs and spaces


