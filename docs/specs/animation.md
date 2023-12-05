# Animation

## General Considerations

### Floating-point coordinates

All values (both time and spatial) are defined as floating point numbers unless
otherwise specified. Values do not have units to accommodate for different
programming language capabilities, format preferences, and varying pixel density
devices.

### Required properties

It is recommended that any required property that is not defined in the format
should throw an exception and the animation should refuse to play. Although in
some cases a default value could fill the missing property, or an element could
be ejected from the animation, to preserve consistency and fidelity to the
original animation it is better to reject the animation altogether.

## Properties  

Definition | Name | Type | Required
-- | :--: | :--: | :--:
[Width](#width-and-height) | w | [Number](../properties/prop-types/#number) | ✅ 
[Height](#width-and-height) | h | [Number](../properties/prop-types/#number) | ✅ 
[In Point](#in-point) | ip | [Number](../properties/prop-types/#number) | ✅ 
[Out Point](#out-point) | op | [Number](../properties/prop-types/#number) | ✅ 
[Frame Rate](#frame-rate) | fr | [Number](../properties/prop-types/#number) | ✅ 

## Width and Height

??? Details
    **Property names:** *w / h*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

An animation has a defined width and height. These two values will define the
visible drawing area. The (0,0) coordinate is positioned at the top, left corner
of the resulting rectangle.

All coordinates both positive and negative are valid. The width and height will
work as a visible frame of all the drawn scene.

## Duration

An animation must have a duration set in the number of frames. Its value is
expressed as a pair of properties: In point and Out point.

By definition the Out point should be larger or equal to the In point.

If this condition is not met, the player should refuse to play and throw an
exception.

Since these properties indicate a segment of an implicit timeline that goes from
*-Infinity* to *Infinity*, any keyframe that is out of bounds won’t be rendered
and only the parts that are within the segment will. Segments can nonetheless
have keyframes defined outside the time segment; but only the overlapping part
of the segment with the animation time boundaries will be rendered.

Exposing an API to adjust the segment at runtime could be helpful, for example
to animate various states responding to an interaction, or simply to allow for
multiple animations to be in a single file.

### In point

??? Details
    **Property name:** *ip*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

The In point can be any number, positive or negative, and indicates the initial
frame of the animation that should be rendered.

### Out point

??? Details
    **Property name:** *op*

    **Property type:** [Number](../properties/prop-types/#number)

The Out point can be any number, positive or negative, and indicates the final
frame (not included) of the animation that should be rendered.

## Frame Rate

??? Details
    **Property name:** *fr*

    **Property type:** [Number](../properties/prop-types/#number)

    **Required**

An animation has a defined framerate. It represents the number of frames per
second that should be used to calculate time interpolations. Its value can range
from 0 (not included) to any positive number.

The larger the frame rate, the more time-based resolution the animation will
have. This allows for more fine grained interpolations, specially useful for
heavy based discrete effects.

On the other hand, a large frame rate will need an equally large refresh rate,
which will be covered in the next section.

## Refresh Rate

The refresh rate of an animation is not explicitly defined in the animation
definition. It can depend on platform capabilities, engine preferences, and
could change during the course of an animation. 

It describes the number of times per second the engine should compute new values
and redraw the canvas. The larger its value, the smoother the animation will
look, but it will also require more computational power to calculate values and
redraw the result.

In general, devices have a refresh rate of 60Hz or 120Hz which means that
animations have a 16ms or 8ms budget, respectively, to redraw. A 60Hz default
refresh rate should be enough to render a smooth looking animation.

It is encouraged to develop an API to modify this value. This API can be used
for different scenarios:

* When performance issues can be detected and signaled, modifying the refresh
  rate should reduce per frame calculations and hence free computing cycles for
  other requirements.

* When the animation is not the most relevant element in the view, a lower
  refresh rate should be enough and it can be modified when the animation is
  front and center.

* When a user has reduced motion settings enabled.

* When a specific refresh rate reflects a particular design decision to convey a
  message, effect or experience.

Note: If the refresh rate differs from the defined original frame rate, it can
have unexpected values when interpolating between keyframes. Particularly when
two keyframes are positioned on consecutive frames, with different values, and
its easing expects an interpolation between them.

Relying on how the animation looks in the authoring tool is not enough, since it
can have a fixed refresh rate that doesn’t match the default one used inthe
runtime environment.

This issue can be solved in two ways. The first is to make sure that when
designing the animation, frames that should not interpolate are correctly set to
behave that way by using the hold type interpolation.

The second is to match refresh rate and frame rate. This should guarantee parity
between authoring tools and runtimes, but it can be a limitation to take
advantage of the full display rate of the device. The animation might look
degraded compared to other transitions. Nevertheless, this feature can be
especially useful for testing purposes where one set of stills has to match
another set.

## Playback speed

Playback speed is a value that describes the ratio between the total duration of
the original animation and the duration at which the animation is playing. 

This value is not part of the animation format but it should be an implicit or
explicit value of the runtime engine. Default playback speed is 1 which means
that the animation duration and playback duration are equal.

Negative values are supported, and they describe an animation that is played in
a reverse direction. (See direction as another option to reverse animation
playback.)

It is encouraged to expose an API to modify this value, but there is no
specification on how that API should be implemented.

This capability, combined with the previous refresh rate, allows for effects
like slow motion or high rate animations.

## Direction

Playback direction describes the direction at which the animation is
progressing. This value is not part of the animation format, but it is part of
the runtime engine. Its value can be either 1 or -1. Implicitly it has a default
value of 1, which means that the animation is running in its original direction.

It is encouraged to expose an API to modify this value, but there is no
specification on how that API should be implemented.

## Loop

Loop describes if the animation should restart once it reaches the end. This
value is not part of the animation format, but it is part of the runtime engine.
Its value can be either a boolean or a natural number including the 0 value. If
the value is 1, the animation should loop one time after it reaches the end,
totalling a number of 2 full cycles.

The true value should mean infinite loops, and false should mean 0 loops.