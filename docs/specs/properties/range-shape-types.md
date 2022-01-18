# Range Shape Types

Name | Description | Value
-- | -- | --
Square based | All blocks within the range will have their transformations applied equally. Factor is always 1. | 1
Ramp Up | Transformations will be applied linearly increasing within the range. Factor grows from 0 to 1. | 2
Ramp Down | Transformations will be applied linearly increasing within the range. Factor grows from 0 to 1. | 3
Triangle | Transformations will be applied linearly up to the middle of the range. First half increasingly, second half decreasing. Factor grows linearly from 0 to 1 and then from 1 to 0. | 4
Round | Similar to Triangle but instead of progressing linearly, it progresses describing half a circle. | 5
Smooth | Similar to Triangle but it describes two segments with easing values to progress from 0 to 1 and 1 to 0. | 6