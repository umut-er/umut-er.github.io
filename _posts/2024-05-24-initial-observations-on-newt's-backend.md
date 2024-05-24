---
title: Initial Observations on Newt's Backend
date: 2024-05-24 23:47:00 +0300
categories: # [TOP, SUB]
tags: gsoc
author: uue
description: In this blog post, I explain my initial insights into the projects that power Newt.
toc: true # Table of Contents
comments: false
math: true
mermaid: true 
---

## Introduction
As a first step into my work, I am trying to understand how [ivis-cytoscape](https://github.com/iVis-at-Bilkent/cytoscape.js) and [sbgnviz](https://github.com/iVis-at-Bilkent/sbgnviz.js) extend the functionality of the original [cytoscape](https://github.com/cytoscape/cytoscape.js) project. I will now try to explain what these extensions are, what they do and why they do what they do.  
Just a heads up, the information here might not be accurate / outdated. Please take a look at the documentation / the source code for yourself to confirm if anything looks wrong.

## Motivation
Cytoscape.js is a well rounded project for many use cases. Many companies / projects have been using this project for many reasons (you can see some of them [here](https://js.cytoscape.org/#introduction/who-uses-cytoscape.js)). However, if a project is good for a very broad scenarios, it is very often the case that for very specific scenarios it needs to be extended to fit particular needs. This is the case here. See the picture below:  
![SBGN Process Description Symbols](/assets/img/PD.svg)    
This is taken from [this page](https://sbgn.github.io/referencecards). These symbols are the ones used in SBGN Process Description notation. These would be what we want to use as nodes / edges in Newt. However, in order to use these, we need specialized ways to render these symbols and the interactions between objects of these types. This is the motivation of the extension of cytoscape.js.

## How It Is Done
In this section I will explain how the original cytoscape.js got extended.

### What Can the Original Do?
Let's first of all note what cytoscape.js can do in order to talk about what it can't do / how it is extended. By default, cytoscape.js seems to support a lot of shapes, here are some examples:
- Polygon (more on this later!)
- Ellipse
- Round Rectangle
- Cut Rectangle
- Barrel
- Bottom Round Rectangle
- ...

See [this](https://js.cytoscape.org/#style/node-body) for all possibilities. In the source code, these shapes are defined in the `/src/core/extensions/renderer/base/node-shapes.js` file. If we look carefully in the source code, we can see that there are only six "drawing strategies", which are the ones listed above. The last five are pretty straight-forward, but there is more to Polygon than meets the eye. Below is a sample from the source code:
```js
BRp.generatePolygon = function( name, points ){
  return ( this.nodeShapes[ name ] = {
    renderer: this,

    name: name,

    points: points,

    draw: function( context, centerX, centerY, width, height ){
      this.renderer.nodeShapeImpl( 'polygon', context, centerX, centerY, width, height, this.points );
    },

    intersectLine: function( nodeX, nodeY, width, height, x, y, padding ){
      return math.polygonIntersectLine(
          x, y,
          this.points,
          nodeX,
          nodeY,
          width / 2, height / 2,
          padding )
        ;
    },

    checkPoint: function( x, y, padding, width, height, centerX, centerY ){
      return math.pointInsidePolygon( x, y, this.points,
        centerX, centerY, width, height, [0, -1], padding )
      ;
    }
  } );
};
``` 
Polygon is defined as an array of points on the unit square (defined as $$ [-1,1] \times [-1,1] $$) which are connected one by one (i.e. p1 connects to p2, p2 connects to p3...). All other shapes are defined using the Polygon drawing strategy (for example, the rhombus). As you can see in the source code, the `generatePolygon()` function returns an object which has a draw function which uses the 'polygon' drawing strategy. Keep this in mind for the following section.

### The Problem / The Solution
An important (and limiting) feature of the polygon drawing strategy is that there is no way of defining [arcs](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) between points. The other drawing strategies are too specific to be used elsewhere, too. So, the problem is that there is no way we can get the shapes we want doing limited modifications / additions to the source code. A complicating factor is that many of these are compound shapes, meaning they contain more than one shape.  
The way that this issue got resolved is somewhat interesting. The first step is relatively straightforward. We register our new types (which are `source and sink`, `nucleic acid feature`, `complex`, `macromolecule`, `simple chemical`, `biological activity` and `compartment`) into the base cytoscape.js system. Then, we create wrapper functions called `nodeFcnCaller.draw()`, `nodeFcnCaller.intersectLine()` and `nodeFcnCaller.checkPoint()` (See the source code above, exactly the same three functions which are returned from this function!). We modify the source code such that any time we want to call the regular `draw()`, `intersectLine()` or `checkPoint()` methods, we call the wrapper instead. The wrapper functions check that whether these are sbgn shapes, and if they are, calls a different function (which takes in something called an `imgObj`, see `cy-modifications.txt`) to deal with that case.  
I then investigated where these shapes and their methods are defined. And I found ... nothing. All clues pointed to `src/sbgn.js`, which was just an empty file with a comment:
```js
// sbgn shapes not supported by cytoscape.js this object will be exposed in cytoscape.js
// and will be filled in sbgnviz.js

// TODO: consider filling this object here and remove related things from sbgnviz
```
Upon reading this, I investigated sbgnviz.js. From what I gathered, it seems to function as a frontend to this cytoscape.js fork, allowing it to work with higher level sbgn representations, such as sbgnml (stands for sbgn markup language).  
What is relevant to us, however, understanding that sbgnviz.js fills in these required functionalities (such as defining sbgn types). This is problematic for the following reasons. Without these functions, this cytoscape.js fork cannot function as a standalone project and always has to be used in conjunction with sbgnviz.js. Another issue is that it is impossible to test some features of the base project, which is what I encountered when I first ran the tests. For this reason, I agreed with the developer who wrote the TODO, and tried to actually do it. I found the required functions and their implementations in [this file](https://github.com/iVis-at-Bilkent/sbgnviz.js/blob/master/src/sbgn-extensions/sbgn-cy-renderer.js) and moved them over to sbgn.js.

## Closing Remarks / Future Work
I would say that the small amount of work I did was relatively successful, and I felt like I learned a lot about the backend of the project. One thing that I am yet to figure out is something called `sbgn.isActive(node)`, which is required in some places (see first code sample below), but is not actually in the `sbgn-cy-renderer.js` file. I added a temporary function with the same name that always returns false (until I figure out what it actually is), and I am happy to report that the fork actually passes all 514 tests! However, I don't think there are specific tests for the sbgn features, which one might in the future look to add.  

```zsh
~/projects/clones/cytoscape.js (unstable ✔) jsgrep "isActive"
./src/collection/dimensions/bounds.js:469:      if(sbgn.isActive(ele)) {
./src/sbgn.js:1370:sbgn.isActive = function(node) {
```

Note: `jsgrep` is a alias around the `grep` command that I came up with to speed up my investigation of these huge repositories. It excludes `./dist`, `./documentation` and `./node_modules`. If you are interested, add the snippet below to your terminal source file:

```zsh
# javascript project grep command
jsgrep(){
	grep --color=auto -r -n $1 --exclude-dir="./node_modules" --exclude-dir="./dist" --exclude-dir="./documentation" --exclude-dir={.bzr,CVS,.git,.hg,.svn,.idea,.tox}
}
```
