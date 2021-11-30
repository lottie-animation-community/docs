# Property types

Lottie has multiple properties depending on what type of layer, content and
attribute they are targeting. Below is the list of different types.

## Number

**Type**: number

A number property is a single floating point value. Depending on the attribute,
they can have valid ranges that will be specified when necessary.

## Array

**Type: **array[Number]

Some properties can have a single dimensional array containing the corresponding
value.

## String

**Type: **string

## Boolean

**Type: **boolean

## Shape

**Type: **object

Shapes are defined by four properties that describe the collection of bezier
curves that represent each path segment of the shape.

### Vertices

**Property Name**: *v*

**Property Type**: Array of Tuples of length 2 and type Number

**Required**

Vertices describe the collection of vertices needed to draw the bezier curves.

### In Points

**Property Name**: *i*

**Property Type**: Array of Tuples of length 2 and type Number

**Required**

In Points describe the collection of incoming control points needed to draw the
bezier curve. The number of in points should match the number of vertices of the
shape.

### Out Points

**Property Name**: *o*

**Property Type**: Array of Tuples of length 2 and type Number

**Required**

Out Points describe the collection of outgoing control points needed to draw the
bezier curve.  The number of out points should match the number of vertices of
the shape.

### Is Closed

**Property Name**: *c*

**Property Type**: Boolean

If true, the last vertex should connect to the first vertex of the list using
the out points of the last vertex and in points of the first  to draw a bezier
curve that closes the shape.

If false, the shape stroke between the first and last vertex should not be drawn
and the shape fill should describe a straight line between them.

## Color

**Type: **Tuples of length 3 and type Number

A Color is expressed as a 3-dimensional tuple where the first index is the red
component, the second is blue, and the third is green. All three values range
from 0 to 1.

Color is a special case of an Array.

## Gradient

**Type: **object

In order to express gradients in a concise way, gradients are described by two
properties.

### Points

**Property Name**: *p*

**Property Type**: Number

**Required**

Points describe the number of stops the gradient has.

### Colors

**Required**

**Property Name**: *k*

**Property Type**: Array[Number]

Each color stop has 4 values (color stop location, red, green, blue), and they
are all appended to the same array sequentially.

If the gradient has opacity, since opacity stops can differ from color stops,
they will be appended to the same array. All opacity values will be grouped at
the end of the array expressed as a pair of floating point numbers (opacity
color stop, opacity value).

Although opacity can have different color stop values, the total number of
opacity stops should be equal to the number of color stops.

In order to identify whether the array is representing only color values or also
opacity values, the Points property must be used.

## Text Document

### Font Family

**Property name:** *f*

**Property type**: string

**Required**

The font family of the rendered text

### Font Size

**Property name:** *s*

**Property type**: Number

**Required**

The font size of the text

### Font Caps

**Property name:** *ca*

**Property type**: Number

**Required**

Caps accepts 3 values. 

* 0 for regular, 

* 1 for All Caps, 

* 2 for Small Caps

### Paragraph Justification

**Property name:** *j*

**Property type**: Number

**Required**

Justification accepts 6 values.

* 0 for left justification

* 1 for right justification

* 2 for center justification

* 3 for full justification with last line aligned left

* 4 for full justification with last line aligned right

* 5 for full justification with last line aligned center

* 6 for full justification

### Text Tracking

**Property name:** *tr*

**Property type**: Number

**Required**

Tracking of the text

### Paragraph Line Height

**Property name:** *lh*

**Property type**: Number

**Required**

Line height of each text line

### Baseline Shift

**Property name:** *ls*

**Property type**: Number

**Required**

Distance from the text to its baseline. It can be a positive or negative value.

### Fill Color

**Property name:** *fc*

**Property type**: Color

**Optional**

Fill color of the text

### Stroke Color

**Property name:** *sc*

**Property type**: Color

**Optional**

Stroke color of the text

### Stroke Width

**Property name:** *sw*

**Property type**: Number

**Optional**

Stroke width of the text

### Box size

**Property name:** *sz*

**Property type**: Number

**Optional**

Text layers can have a text box that defines the boundaries of the container
where text would be rendered. Text lines should wrap around the box. If text
exceeds the box, it should clip it.

### Box position

**Property name:** *ps*

**Property type**: Coordinate

**Optional**

If the text layer has a box size defined, this property defines the position of
the box relative to the layer.
