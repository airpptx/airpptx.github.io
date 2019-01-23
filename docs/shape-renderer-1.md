# Debug and Understand Renderers

Chances are you might want to use a shape that's not supported yet. Checkout the changing [list of supported elements](limitations) to see what we support out of the box.

In this example, we'll build the shape renderer for an Octagon.

### Understanding Powerpoint Element Parsing

1. Drag an Octagon shape onto your powerpoint slide and save the slideshow

2) Run the following `airppt` CLI command. You will add an `--element` parameter that prints DEBUG output to understand the powerpoint shape

```
airppt --input {slideshowpath} --slide {slideNumber} --element
```

3. Copy the JSON output and paste into a [JSON Editor](https://jsoneditoronline.org/). This will make it easier to understand. Paste the JSON and click the ▶ arrow to parse the output.

4. You will see an attribute labeled `PowerpointElements` - this is a list of the all the PowerpointElements on a slide. The definitions for a powerpoint Element class can be found [here](PPTElement).

5. Find the element that corresponds to the Octagon, you'll realize that the ShapeType has already been prefilled according to the [preset ppt shapes](http://officeopenxml.com/drwSp-prstGeom.php).

6. Expand the `elementPosition` and `elementOffsetPosition` values. Powerpoints provides coordinates as [EMU coordinates](https://startbigthinksmall.wordpress.com/2010/01/04/points-inches-and-emus-measuring-units-in-office-open-xml/), you will need to keep this in mind when we write our renderer.

7. Expand the `raw` attribute. This shows the powerpoint element directly translated from XML. When the parsed output such as `fill` or `font` don't make sense, it's a good idea to check the raw output to see the original Powerpoint element.

You can checkout the `PowerpointElement` interface [here](aaaa) to learn more.

### Writing a Renderer

You are now ready to write a renderer to convert a PPT Octagon to HTMl.
