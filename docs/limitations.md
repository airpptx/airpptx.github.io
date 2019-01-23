# AirPPT Limitations

Powerpoint stores information in a series of complex XML mappings. Checkout the [OpenXML Spec](https://www.ecma-international.org/news/TC45_current_work/OpenXML%20White%20Paper.pdf) to get an idea of how [complex](http://officeopenxml.com/anatomyofOOXML-pptx.php) it really is.

A complete list of all the preset Powerpoint shape types are available [here](http://officeopenxml.com/drwSp-prstGeom.php).

Here are the supported elements:

| PPT Element       | Supported   | Attributes                                 |
| ----------------- | ----------- | ------------------------------------------ |
| Rectangle         | Yes         | Borders, Lines, ColorFill, ImageFill       |
| Triangle          | Yes         | Text, Borders, Lines, ColorFill, ImageFill |
| Ellipse           | Yes         | Text, Borders, Lines, ColorFill, ImageFill |
| Images            | Yes         | Local Images, Copy+Paste Images            |
| Lines             | In Progress |                                            |
| Grouped Objects   | No          |                                            |
| Charts            | No          |                                            |
| Slide Backgrounds | Planned     |                                            |

Keep in mind that this project is meant to be extensible. So I highly encourage you to follow the [building a renderer tutorial](#shape-renderer-1) and add a shape of your own. You can also write custom elements to embed custom elements such as Google Maps or Youtube using a similar methodology.

## Read Me !

Here are a few things to keep in mind:

- Unsupported shapes are always rendered as rectangles

- Images will always appear on top of shapes. To get around this, pick a rectangle and use the [picture fill](https://support.office.com/en-us/article/insert-a-picture-into-an-autoshape-7bb2abbb-561f-4f40-9762-d86df823d305) option.

- Color fills that are part of the slideshow "Theme" will probably not be respected. A workaround is to explicitly [set the color](http://www.ladybugsteacherfiles.com/2016/08/using-precise-colors-in-powerpoint.html) with the picker.

* Slide elements that belong to the ["master" slide](https://support.office.com/en-us/article/customize-a-slide-master-036d317b-3251-4237-8ddc-22f4668e2b56) will not be shown. Some more work has to be done to support this.

* Try to avoid overlapping elements as best practice. Grid behavior can also vary across various devices.

* You can rename elements individually using [the selection pane](https://support.office.com/en-us/article/manage-objects-with-the-selection-pane-a6b2fd3e-d769-46c1-9b9c-b94e04a72550). This will add an "ID" attribute on the elements that can be used to select specific elements (e.g. buttons and textboxes). Follow the [alphabetter tutorial](a).
