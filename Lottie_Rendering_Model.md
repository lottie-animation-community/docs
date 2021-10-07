# Lottie Rendering Model

## Introduction

Any implementation of the Lottie format must follow the rendering model
described in this document. Some degree of variability can be accepted both in
terms of refresh rate or visual fidelity if the device in use has any type of
time or graphic limitation.

## Rendering tree

The object model described by the lottie format does not describe a one to one
relationship with the rendering tree. There are objects that are not directly
rendered on the tree but they are referenced from within other layers, for
example those on the assets model. These assets can also be used multiple times
on the render tree, with different spatial matrices applied to them and rendered
at different points in time.

There are also other assets that do not have a representation on the render tree
but that are auxiliary elements useful to complement it. For example font data
and marker data.

Finally, some other elements don’t have any visual representation on the render
tree but act as containers of properties that affect rendered elements. Null
layers for example aren’t drawn on the canvas but can be used as parents for
multiple other types of elements that will inherit its transform matrix. 

Other elements act as modifiers of the rendered shapes like fills, strokes, and
trim paths. And there are other types of modifiers, like repeaters, that can
have a multiplying effect on the render tree, increasing the number of nodes
that were originally defined on the object model.

## Definitions

### Rendering tree

The rendering tree is the final output of properly combining all layers and
modifiers defined in the lottie format. It might vary throughout the lifetime of
the animation since layers have In Points and Out Points limiting their presence
on the canvas to a certain amount of time.

### Renderable Elements

Any element that needs to be displayed on the screen and is part of the render
tree. It can be a Solid which is the output of the Solid Layer Object, a path
which can be part of the Shape Layer Object or Text Layer Object, or any other
drawable element.

### Non Renderable Elements

Many elements on the Document Object Model don’t have a direct representation on
the render tree. For example layers like Null Layer Object, or modifiers like
Gradient Fill, Trim Paths or Masks

### Reusable Assets

Some elements on the DOM are containers of Renderable Elements or Non Renderable
Elements that can be reused multiple times on the render tree. PreComps can be
rendered at different places and at different times in a single animation. Other
examples are character data related to a specific font that can be used to draw
multiple instances of a single letter.

## Rendered vs Non Rendered Elements

During the lifetime of a lottie animation, multiple factors can affect what
elements will be rendered on the canvas.

This is the list of conditions that need to be met for an element to be included
on the render tree:

* **In Point and Out Point**. Any element whose time bounds don’t contain the
  current position of the time cursor need to be excluded from the render tree.
  This condition also applies if the element is a child of a PreComp. A
  necessary condition for the element to be rendered is that the containing
  PreComp is also rendered within the time boundaries. Since PreComps can have a
  separate timeline and that timeline can be affected by time effects like time
  remapping or time stretching, the In Point and Out Point of the element might
  end up shifted, excluding the element from the render tree. 

* **Spatial boundaries. **If an element’s bounding box doesn’t overlap with the
  boundaries of the animation canvas, it will be excluded from the tree. When an
  element is rendered within a PreComp, only the overlapping area of the
  PreComp, the rendered element and the PreComp’s container should be rendered.

* **Paint modifiers. **Some layers have a fixed explicit paint modifier like
  Solid Layers. Those layers are always defined with a specified color. On the
  other hand, some layers can have nested definitions and even multiple painting
  rules applied to them. On the contrary, some other layers or shapes defined
  within the layer can lack all types of painting operations. If that’s the
  case, those shapes should be excluded from the render tree. Some examples can
  be text layers with no fill color or stroke color defined, or shape elements
  defined within a group that have no fills or stroke.

* **Clipping Masks. **When a layer has a clipping mask or any kind of track
  matte mask linked to them, if their boundaries don’t overlap with the rendered
  element, they should be excluded from the render tree.

* **Repeaters. **Shape layers can include in their stack of modifiers, a
  repeater that will affect any shape in that stack above them. If the amount of
  copies defined by the repeater is 0, no elements should be added to the render
  tree. On the contrary, if more than one copy is set, those same elements will
  be added multiple times to the stack.

* **Opacity**. Elements might have their opacity set to 0 because of a direct
  effect of their opacity property, or as a consequence of the nested containers
  that can affect it. Although this means that visually, the element will have
  no effect on the final output, it is recommended to keep the element in the
  render tree if that tree is used for collision detection or interaction with
  user input.

## Painting model

Lottie frames are rendered in a sequence of paint operations. Each element is
rendered on top of the current canvas and becomes part of the background for the
next paint operation.

### Precomps

Layers that belong to a precomp should be treated as a separate painting stack.
They should be drawn on a temporary canvas and once the stack is traversed, the
final canvas should be painted on the containing canvas.

This allows to apply blend modes, clipping or any other type of effect to the
resulting temporary canvas before painting on the containing canvas.

Since precomps can be nested within precomps, this should be a recursive
operation until the main canvas is reached.

Precomps can have different dimensions than the root element. In consequence,
this stacking context should respect the width and height of the layer and clip
out anything that exceeds it; even if the containing canvas is larger and could
display the exceeding texture.

### Track matte layers

Track matte layers are pairs of layers where one acts as a mask for the other.
Those layers are indicated by a *tt* property on the masked layer and a *td* on
the masking one. They are always stacked one next to the other on the stacking
order.

Since the masked layer is always below the masking layer and painting order
starts from the end, when a masked layer is found, before painting it, the
canvas should be prepared with the preceding layer to clip or composite with it.
As a side effect of this, the masking layer should not be directly rendered on
the canvas.

## Painting order

Order of painting of elements should always start with the last layer on the
layers stack and traverse the stack upwards.

Unless specified explicitly by the layer, elements are painted on a 2
dimensional space. But painting order implicitly works as a z axis, where the
next layer should be painted above the previous one, obscuring what is already
painted on the surface.

When encountering a precomp layer, the painting of the current stack should be
paused and a new context created. Within that context, the painting order should
respect the same rules described above.

## Stacking context

Stacking contexts can contain further stacking contexts. A stacking context is
atomic from the point of view of its parent stacking context; elements in
ancestor stacking contexts may not come between any of its elements.

As mentioned before, certain layers or operations applied to layers might need
creating a separate stacking context. Depending on the capabilities of the
underlying device, these conditions may vary, but here are some common cases
where a new context might be needed:

* The root element

* A layer of type precomp is encountered where a new context will contain the
  inner layers.

* Track matte layers

* Effects applied to the layer like blur, displacement map

* Clipping masks

* Shape layers with inner groups or modifiers

## Rendering elements

In general, a layer could be considered as a single element. But there are
certain layers that contain multiple individual shapes, such as shape layers or
text layers.

For this section, an element means any operation that will paint to the current
context. This operation can be the result of previous painted individual
elements that once processed are part of a larger single paint operation.

### Time constraints

Elements should only be rendered if their ancestors can be rendered on a
specific frame. Any ancestor whose time boundaries don’t overlap with the
current frame will prevent its children from being rendered. For example, there
could be situations where a child element has an explicit *inPoint* and
*outPoint* that would be renderable at the current frame, but if the ancestor’s
*inPoint* and *outPoint*  are not, the child element should be excluded as well.

Elements that don’t possess an *inPoint *and *outPoint* property should
implicitly share those values with its closest ancestor.

Precomps possess a special property called time remapping. Same rendering
constraints apply to this case. Its children will be rendered only if the
precomp is within the time boundaries. More information about this property can
be found on the format specifications.

### Spatial constraints

Any child layer that is outside the boundaries of the containing context should
be excluded from the render. If the child is partially outside the boundaries,
only the visible part should be rendered.

Before rendering an element, a set of transformations should be applied to set
the final coordinates to perform the painting operation. Layers need to follow
the parenting hierarchy and shape groups must apply the transform chain defined
by the containing groups.

Shape repeaters also define a set of transformations that must be calculated and
multiplied before applying each one of the copies.

## Rendering Groups

Groups create a new drawing context. Certain operations should be applied to
them once the individual operations have been rendered.

For example, when a group has a certain opacity applied to it, its value cannot
be propagated downwards to each internal element as a multiplier, because it
might have unexpected results. If there are two overlapping rectangles and
opacity is applied individually to each one of them, the overlapping area will
reveal the layer that’s below. Instead, what should be done is paint both
rectangles on its own context and apply the opacity to the result, thus making
sure that the shape below won’t be visible in the final result.

### Precomps

Precomps are a type of group. These are the steps that must be followed to paint
them.

* A new empty canvas should be created with its corresponding width and height.

* Layers should be iterated and painted following the rendering order and the
  time remapping if present.

* Effects should be applied in the order they are defined where the output of
  one acts as the input of the following one

* Clipping and/or masking.

* Transform hierarchy.

* Blend mode.

* Paint.

### Shape Groups

Shape groups are also types of groups. For more information about them, see the
shape specification. These are the steps that must be followed to paint them.

* A new empty canvas should be created. Width and height are inherited from the
  closest parent precomp or root element.

* Layers should be iterated and painted.

* Transform applied.

* Blend mode.

* Paint.

## Types of graphics elements

There are several types of elements that contain graphic information to be
painted within the corresponding context.

### Shapes

Shapes describe a surface that can be painted. The type of fill can be a stroke
operation or a fill operation. And those operations in turn can be described
with a single color or a type of gradient.

Since a shape is potentially accompanied by multiple color operations, it should
be drawn as many times as the colors in the stack are available at that time.

The surface described by the shape can be parametrized as a rectangle, polystar,
path. More information about them is available on the format specification.

### Texts

Text layers contain a sequence of code points that should be drawn as a group.
Depending on the device capabilities and rendering information, text will be
drawn as a set of shapes available in the format itself, retrieved from a font
file, or from the system font if available.

### Images

Image layers use an original texture as input and should be modified according
to any layer specification before painted on the containing canvas.

### Videos

Video layers should be treated as image layers in regards to its painting rules,
but they should also follow the time constraints to keep them in sync with the
main timeline of the animation.

## Clipping and Masking

There are several ways of applying masks to a layer. Both track matte masks and
maskProperties can be combined and stacked to affect a single layer before it
gets painted. More information about them is available on the lottie format
specification.

