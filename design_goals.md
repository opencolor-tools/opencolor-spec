# Design Goals

## Must

* The format *must* be readable and editable by humans
* The format *must* be machine readable
* Colors can be defined in any of the usual css notations and other, arbitrary formats:
  - hex codes in 3 and 6 char notation (#08F and #0088FF)
  - hex codes in 4 and 8 char notation (#08F8 and #0088FF88) where the last part is the alpha value (0-F or 00-FF matching 0.0 - 1.0 in rgba notation)
  - rgb() and rgba() (rgb(0, 128, 255) and rgba(0, 128, 255, 0.7)
  - cymk(0 50 100 0)
  - RAL(2000)
  - PANTONE(Serenity)
  - PANTONE(15-3919)
* Colors have names
* Colors can be sorted into named groups or palettes

## Should

* The format *should* allow to arbitrary meta data to groups and colors
* The format *should* be as concise as possible without sacrificing readability by either humans and machines
