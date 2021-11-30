# Text Data

In order to render text, two different solutions are provided by the format.
Text can be exported as glyphs or as regular text data.

## Glyphs

Glyphs allows Lottie to detach itself from any external font file to render
text. By including every character as a shape described by bezier curves, it can
render text independently from the original font.

### Chars

**Property name:** *chars*

**Property type:** Array[Char]

The chars array contains the list of all characters available to be rendered.

#### Char Object

**Property type:** Object

A character is described by a set of properties to identify its font family,
shape and other necessary properties to be rendered.

##### Character

**Property name:** *ch*

**Property type:** String

**Required**

The character string. It should be used to map the character requested by a text
layer.

##### Style

**Property name:** *style*

**Property type:** String

**Required**

The font style. It should be used to map the character requested by a text
layer.

##### Font Family

**Property name:** *fFamily*

**Property type:** String

**Required**

The font family. It should be used to map the character requested by a text
layer.

##### Advance Width

**Property name:** *w*

**Property type:** Number

**Required**

The distance at which the following character should be drawn, expressed
relative to a font size of 100px.

##### Data

**Property name:** *data*

**Property type:** object

**Required**

The data object contains a set of extra properties relative to the character.

###### Shapes

**Property name:** *shapes*

**Property type:** Array[Shape]

**Required**

The list of shapes that represent the character

## Fonts

**Property name:** *fonts*

**Property type:** object

**Required**

Fonts contain a set of properties related to font information needed to render
text layers.

### Fonts List

**Property name:** *list*

**Property type:** Array[Font]

The list property contains the array of fonts needed to render all text layers.

#### Font

**Property type:** Object

Each font describes the font information needed to render a specific text layer
type.

##### Font Origin

**Property name:** *origin*

**Property type:** Enum[Number]

**Required**

The font origin indicates how to interpret the other properties in order to load
the font

* 0 for None

* 1 for Google

* 2 for Typekit

* 3 for URL

##### Font Path

**Property name:** *fPath*

**Property type:** String

**Required**

The path that should be used to load the font

##### Font Class

**Property name:** *fClass*

**Property type:** String

**Optional**

The class that should be assigned to the text element for it to get the font
assigned via a css selector

##### Font Name

**Property name:** *fName*

**Property type:** String

**Required**

The name of the font to identify which font should be used to render the text

##### Font Family

**Property name:** *fFamily*

**Property type:** String

**Required**

The value of the font family

##### Font Weight

**Property name:** *fWeight*

**Property type:** String

**Required**

The value of the font weight

##### Font Style

**Property name:** *fStyle*

**Property type:** String

**Required**

The value of the font style

##### Ascent

**Property name:** *ascent*

**Property type:** Number

**Required**

The value of the font ascent expressed relative to a font size of 100px. The
ascent references the yOffset by which to draw the character.
