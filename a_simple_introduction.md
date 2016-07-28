# A simple introduction to the OCO format

Author: jan krutisch <jan@krutisch.de>

This text is meant as a less rigid, more human friendly introduction to the OCO format than the spec which obviously focuses on clarity and detail.

## Colors

Colors are the most basic atom of an OCO file. Colors have a name and one or more color values. Let's start with the easiest case:

```
a dubious red: #fd0505
```

The name is `a dubious red` and the color value is, well, a dubious red. I've specified the color value in the html/css hex color style but open color implementations should (or rather: must) parse all css/html ways of defining colors, including things like `hsl()`. Apart from that, you can define colors in whatever color space you like, as long as you write it a similar way. For example if you would like to specify the red used on german traffic signs (called "Verkehrsrot"), you could do it as RAL(3020). Most parts of Open Color Tools don't really know what to do with that information, but it can be neat to document these things in one place. That's why you can actually specify more than one color value per color. It would look like this:

```
Verkehrsrot:
  #CC0605
  RAL(3020)
```

*Currently, most parts of Open Color Tools ignore alpha channels on colors, so if you write something like `rgba(255,0,0, 0.5)` you'll end up with effectively `#ff0000`. While the [open color library](https://github.com/opencolor-tools/opencolor-js) handles alpha values just fine, we haven't yet decided on how to expose that in the palette renderer code or in exporters.*

## Palettes/ Groups

You can group colors into palettes just by starting with a name and indenting the colors below:

```
Verkehrsfarben:
  Verkehrsrot: #CC0605
  Verkehrsblau: #063971
  Verkehrsgelb: #FAD201
```

In Theory, you can also have groups in groups, but again, while the library handles this fine internally, we haven't figured out a way to consistently render potentially infinitely nested palettes.

You can have any number of Groups in your OCO file.

## Metadata

Sometimes you may want to attach metadata to a palette or to a color. For that, you can use a name that contains a slash somewhere:

```
Verkehrsfarben:
  /source: de.wikipedia.org/wiki/RAL-Farbe#Rot
```

Some metadata may have special meaning. For example, to configure the palette renderer in the Open Color Tools Companion App and the Color Picker, we use the oct/ prefix and a set of options. As the slash is such a natural namespace divider, you can use it in a special way in a case where you have to specify several options at once:

```
Verkehrsfarben:
  oct/
    view: fan
```

You are totally free to define your own namespace for your own set of tools, we'd just ask you to not overload the oct namespace, of course.

## References

When defining palettes for, say, an application we want to build, we often start out with a basic set of colors, maybe some are brand related, maybe some are platform specific (Material design comes to mind). We call those the "visual" colors and an example could look like this:

```
Brand Colors:
  Deep Blue: #1A237E
  Funky Orange: #FF7043
Blues:
  50: #E3F2FD
  800: #1565C0
```

Now, when working on a design for our application, we start systemizing the color usage (as one should) and some patterns emerge. There's a common background, a primary text color and so on. To work with these colors, we can now add them to our OCO file and use more sematic names. We can use an additional palette for that:

```
Night Theme:
  Background: #1565C0
  Header Background: #1A237E
  Primary Text: #E3F2FD
  Primary Text Pressed: #FF7043
```

But this is very inconvenient. We now have all colors duplicated in our file. If we would like to adjust, say, the `funky orange`, we would have to search and replace all occurrences. Each time we change something. Luckily, in Open Color, we have References, which allow you to now build your semantic palettes and simply reference the corresponding visual color names:

```
Night Theme:
  Background: =Blues.800
  Header Background: =Brand Colors.Deep Blue
  Primary Text: =Blues.50
  Primary Text Pressed: =Brand Colors.Funky Orange
```

We think this distinction between visual and sematic colors is a pretty good idea and one of the main reasons we've created Open Color in the first place, but of course, you can use color names and references in any way you would like.

## References in Metadata

Sometimes it can be necessary to use, say, colors in metadata. For example, in the Color Picker, you can choose a background color to render your palette on, so that you can see if the colors work in a different context. Again, using references keeps you from having to update metadata whenever you change the original background color:

```
Night Theme:
  oct/backgroundColor: =Background
  Background: =Blues.800
  Header Background: =Brand Colors.Deep Blue
  Primary Text: =Blues.50
  Primary Text Pressed: =Brand Colors.Funky Orange
```

## Where to go next:

* [Christophe's Article on Medium](https://medium.com/sketch-app-sources/designing-with-meaningful-color-28edd86240a7#.dujash3by) does a pretty good job of explaining why Open Color exists and what kind of workflows we're imagining.
* The example palettes in the Companion App show you how to customize the palette views.
* If you want to build technical workflows involving Open Color, the [opencolor npm module](https://www.npmjs.com/package/opencolor) is probably a good start.
