# Getting Started with AirPPT

Welcome to AirPPT. Easier to say AirPowerPoint :)

Powerpoint's simple drag and drop interface makes it easy to prototype and demonstrate UIs. However, we have to startover in "real" code when we begin implementing our UI.

AirPPT lets you go from slideshow canvas to functional UI using Powerpoint as a WYSISYG editor. It's also designed to be extensible!

This guide shows you how to take a slideshow and convert a slide to html. You'll use the [AirPPT CLI tool](https://www.npmjs.com/package/airppt).

## Prerequisites

Before you begin, make sure your development environment includes `Node.js®` and an npm package manager.

### Node.js

Airppt requires `Node.js`

- To check your version, run `node -v` in a terminal/console window.

- To get `Node.js`, go to [nodejs.org](https://nodejs.org "Nodejs.org").

### npm package manager

Airppt CLI depends on features and functionality provided by libraries that are available as [npm packages](https://docs.npmjs.com/getting-started/what-is-npm). To download and install npm packages, you must have an npm package manager.

This Quick Start uses the [npm client](https://docs.npmjs.com/cli/install) command line interface, which is installed with `Node.js` by default.

To check that you have the npm client installed, run `npm -v` in a terminal/console window.

## Step 1: Install the AirPPT CLI

You use the AirPPT CLI
to point to a slideshow and generate a UI or debug the output of a renderer.

Install the AirPPT CLI globally.

To install the CLI using `npm`, open a terminal/console window and enter the following command:

```
  npm install -g airppt
```

## Step 2: Create a sample Powerpoint

You can download a sample powerpoint with some basic slides [here](https://github.com/airpptx/samples/blob/master/apps.pptx).

Not all powerpoint shapes are supported yet. A list of supported elements can be found [here](#limitations). However, you can also extend the functionality and add support for your own [shape renderers](#shape-renderer-1)

## Step 3: Generate the application

You can generate the HTML and CSS using the CLI tool

1. Navigate to the same directory as the powerpount (`apps.pptx`).

2. Run the command

```
airppt -i ./sample.pptx --slide 1 -o ./mypage
```

3. Navigate to the `mypage` directory and open `index.html`

#### Bonus

5. You can also reuse the output to build a working application.

Read the getting started with `Electron` to get started.

## Limitations

AirPPT is a work in progress. Not all shapes nor cases are accounted for. I've tried my best to call out a few of the limitations here and the shapes that are supported.

### Extending AirPPT

The framework is designed to be extensible so that you can add back [your own shapes and custom elements](#shape-renderer-1).
