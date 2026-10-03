---
title: Design Guide
sidebar_position: 1
slug: /design-guide
---

# Design Guide

Creating a JSCAD design starts by writing a small script that calls functions to
create and manipulate shapes. Designs are written in JavaScript.

*[Find out more about JavaScript at W3Schools](https://www.w3schools.com/js/default.asp)*

The JSCAD applications execute the script, generate shapes, apply transforms, perform
operations, and render the result for viewing.

```js jscad
import { colorize, cylinder, subtract, translate, union, TAU } from '@jscad/modeling'

export const main = () => {
  const body = cylinder({ radius: 20, height: 8 })
  const spindle = cylinder({ radius: 4, height: 14 })
  const bolts = [0, 1, 2, 3].map((i) =>
    translate(
      [Math.cos(i * TAU / 4) * 13, Math.sin(i * TAU / 4) * 13, 0],
      cylinder({ radius: 2, height: 10 })
    )
  )

  return colorize([0.3, 0.6, 0.9], subtract(body, union(spindle, ...bolts)))
}
```

## How the Guide is Organised

| Page | What it covers |
| --- | --- |
| [Anatomy of a Design](./anatomy.md) | The structure every design shares |
| [Design Parameters](./parameters.md) | Making designs interactive |
| [Projects](./projects.md) | Splitting a design across several files |
| [3D Primitives](./3d-primitives.md) | Cuboids, spheres, cylinders, tori, polyhedra |
| [2D Primitives](./2d-primitives.md) | Rectangles, ellipses, polygons, stars |
| [Paths and Text](./paths-and-text.md) | Lines, curves, and text as geometry |
| [Transforms](./transforms.md) | Moving, rotating, scaling and aligning shapes |
| [Operations](./operations.md) | Combining shapes with booleans and hulls |
| [Extrusions](./extrusions.md) | Turning 2D shapes into 3D ones |
| [Curves and Slices](./curves-and-slices.md) | Bézier curves and slice-by-slice extrusion |
| [Offsets](./offsets.md) | Growing and shrinking shapes by a fixed distance |
| [Colors](./colors.md) | Applying color, and converting between color spaces |
| [Measurements](./measurements.md) | Asking a shape about its size and position |

## Importing the Modeling Library

All modeling functions are exported from `@jscad/modeling`,
allowing designs to import only the functions being used.
This keeps designs small and compact.

```js
import { cuboid, rotateZ, subtract } from '@jscad/modeling'
```

:::info[Changed in v3]

v2 designs used `require()` and reached into namespaces:

```js
// v2
const { cuboid } = require('@jscad/modeling').primitives
const { subtract } = require('@jscad/modeling').booleans
```

The v2 namespaces still exist in v3 for compatibility, but should be considered obsolete.

:::

## Shapes are Never Modified

One key point to remember is that operations always **return a new shape**
and leave the orginal shape untouched.

```js
const mycircle = circle({ radius: 5 })
const newshape = scale([2, 2], mycircle) // mycircle is unchanged
```

If the old shape is unnecessary then assign to the same variable:

```js
let myshape = circle({ radius: 5 })
myshape = scale([2, 2], myshape)
```
