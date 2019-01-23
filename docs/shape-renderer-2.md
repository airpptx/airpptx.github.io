# Creating a Shape Renderer

After you've understood how [parsing a shape](aaa) works and the attributes that make up a [PowerpointElement](a). You are ready to make a renderer to show .

Continuing from our past steps, we are writing the renderer for an Octagon.

## Prerequisites

Before you begin make sure you have cloned the `airppt-renderer` library. You will be doing your work in Typescript.

```
npm install
tsc
cd tests
node test.js
```

### Creating a Renderer

1. In order to create a renderer, you have to implement the `ElementRenderer` interface. You must implement both the `getCSS` and `getHTML` methods.

- In addition to the abstract methods, the base class provides a few helper methods that are mostly universal for all shapes.

- Conveniently, JQuery (`$`) is a provided variable for easy HTML manipulation.

2. Start by creating a file with the same name as the powerpoint preset shape [here](http://officeopenxml.com/drwSp-prstGeom.php). Our file will be named as `octagon.ts`

3) Name the class as the same as the corresponding shape in Powerpoint. Set up the constructor, and call the base class. It's ok, if you don't know what each propery has.

```
export default class Triangle extends ElementRenderer {
	constructor(scaler: GridScaler, element: PowerpointElement, pptDetails: PowerpointDetails, rendererOptions: RendererOptions) {
		super(scaler, element, pptDetails, rendererOptions);
	}

```

4. Import the `string-template` package as `format`. It should work after you NPM install. This will help with string manipulations.

`import * as format from "string-template";`

It's usually easier to copy over the imports and function headers from a previously implemented renderer. Here is the [triangle](https://github.com/airpptx/airppt-renderer/blob/master/ts/renderers/shapes/triangle.ts) one.

### Implementing getCSS()

5. Let's implement the `getCSS()` method. First, we'll build the CSS to get the right shape.

The typical naming convention for a CSS class for a shape is `#{name}.shape`, each individual element has it's own style class.

We can create a CSS class as a string and use the `format` method to fill the variable values as per the `PowerpointElement` properties

```javascript
let elementCSS = [];  //we'll push our CSS strings to this array

let shapeCSS = format(
	#{name}.shape{
    width:{width}px;
    height:{height}px;
    background: {background};
    -moz-border-radius: 50%;
    -webkit-border-radius: 50%;
    border-radius: 50%;
	display: table;
}`,
{
	name: this.element.name,
	width: this.scaler.getScaledValue(this.element.elementOffsetPosition.cx),
	height: this.scaler.getScaledValue(this.element.elementOffsetPosition.cy),
	background: this.element.shape.fill.fillType == FillType.Solid ? "#" + this.element.shape.fill.fillColor : "transparent"
}
	elementCSS.push(shapeCSS);

```

Recall how the `PowerpointElement` has it's coordinates in EMU units. We use the `scaler` class to get a converted pixel value. The offset positions describe how far out from a StartX, StartY coordinate a shape goes out too.

6. If an element has text or a border, we also need to format those with correct fonts and colors on a shape. Standrd helpers have already been implemented to help with this.

```javascript
if (this.element.paragraph) {
	let fontCSS = GenerateParagraphCSS(this.element.paragraph, this.element.name);
	elementCSS.push(fontCSS);
}

if (this.element.shape.border) {
	let borderCSS = GenerateBorderCSS(this.element.shape.border, this.element.name);
	elementCSS.push(borderCSS);
}
```

7. Lastly, we want to position the element correctly. However, the base class has a convenient helper to determine this.

```javascript
elementCSS.push(this.getPositionCSS());
```

8. Lastly, we want to return one giant string with our CSS. We make use of the `beautify` function.

```javascript
return this.beautify(elementCSS.join(""), { format: "css" });
```

### Implementing getHTML()

The getHTML() method returns the corresponding div elements with the associated class tags implemented. In a custom element, you could even return an IFrame to place into the final DOM.

For example, a rectangle with text means that the a `<p>` must live within a `<div>` - you need to set this structure in the `getHTML()` method.

Most shapes follow a similar structure. Here is an [example](https://github.com/airpptx/airppt-renderer/blob/master/ts/renderers/shapes/rectangle.ts#L56)

9. Create the initial `div` for the shape.

```javascript
let shapeDiv = format('<div id="{0}" class="{1}"> </div>', this.element.name, "position shape");
```

10. Append the div to the `body` using JQuery. This is just a blank HTML DOM that we can manipulate to build our HTML.

```javascript
this.$("body").append(shapeDiv); //add the shapediv initially
```

11. Add HTML elements for paragraphs and the necessary CSS class names

```javascript
if (this.element.paragraph) {
	let paragraphHTML = format('<p class="font">{0}</p>', this.element.paragraph.text);

	this.$("#" + this.element.name).append(paragraphHTML); //add the paragraph div within the div
}
```

12. Return the final HTML from the JQuery DOM

```javascript
return this.$("#" + this.element.name)[0].outerHTML;
```

### Expose your Renderer

13. Lastly, make your renderer visible so that it can be called by the main thread. You will have to add a reference to it in the [index file](https://github.com/airpptx/airppt-renderer/blob/master/ts/renderers/index.ts)

14. Confirm that your renderer works as expected by running the [test file](https://github.com/airpptx/airppt-renderer/tree/master/tests). Be sure to update the code as needed to point to the right powerpoint file.

Woo-hoo! You made it. Open an issue, or reach out to [me](mailto:rlingineni@live.com) with any questions.
