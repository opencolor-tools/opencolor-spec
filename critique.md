# Critique of the "original" Open Color Format

## Benefits
- Very close to YAML, readability is good
- Not a lot of stuff you can get wrong


## Shortcomings

- The value:-syntax is confusing (but, if single colors need metadata, unavoidable in any format, I guess)
- The quoted hex strings add superfluous noise. Could be fixed by allowing hex digits without the hash, but that will confuse people as well.
- meaningful indentation is a potential source of confusion and anger, especially if people start to mix tabs and spaces


## Possibilities to fix the current format

- Allow for unquoted colors by preprocessing the YAML (How error prone is this?) - This would probably mean that we'll need to use a different file extension, because it's not exactly valid YAML anymore. (One could  argue that this is not strictly true, as YAML allows for empty hash values)
- Allow for unhashed color values. I find this too Confusing.

## Alternatives

### TOML

- Looks like INI files, but has nesting.
- Nesting is done with dot notation which has redundancies, but at least it uses the same stuff as refs do anyway
- Is easy to read
- Does not allow for slashes in bare keys, would make us rethink the metadata idea
- Needs quoted strings, so no improvement over the YAML format in that regard.

```toml
name = "My Colors"
"/author" = "Marcel Jackwert"
"oco/styles" = """
  background: #FFF
"""
red = '#ff0000'
[blues]
  "ms/priority" = 100
  100 = '#00ff00'
  200 = '#00ff00'
[Subtle Greys]
  "Dark Grey" = '#CCC'
  "Lighter Grey" = '#DDD'
  "Lightest Grey": 'RGB(220, 220, 0)'
[Primary UI Colors]
  textBackground = '=Subtle Greys.Dark Grey'
  textHightlight = '=red'

```

### CSON (Coffeescript JSON)

- Used by Atom Editor
- Also uses significant whitespace
- not so many parsers
- I find the syntax not very compelling

```cson
/name: My Colors
/author: Marcel Jackwert
oco/styles: '''
  background: #FFF
  ''''
red: '#ff0000'
blues:
  ms/priority: 100
  100: '#00ff00'
  200: '#00ff00'
Subtle Greys:
  Dark Grey: '#CCC'
  Lighter Grey: '#DDD'
  Lightest Grey: 'RGB(220, 220, 0)'
Primary UI Colors:
  textBackground: '=Subtle Greys.Dark Grey'
  textHightlight: '=red'

```


### JSON

- Hard to write and read for humans, lots of reasons why JSON is not valid, like missing or superfluous commas
- Not possible to comment
