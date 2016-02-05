# Design Goals

## Must

* The format *must* be readable and editable by humans
* The format *must* be machine readable
* Colors can be defined in any of the usual css notations:
  - hex codes in 3 and 6 char notation (#08F and #0088FF)
  - rgb() and rgba() (rgb(0, 128, 255) and rgba(0, 128, 255, 0.7)
  - Do we need to support color names? are they used anywhere? Do we hurt people when we leave this out?
  - CSS4 supports hex codes with alpha. Currently only supported in webkit, so technically possible but probably not important.
* Colors have names
* Colors can be sorted into named groups

## Should

* The format *should* allow to arbitrary meta data to groups and colors
* The format *should* be as concise as possible without sacrificing readability by either humans and machines


