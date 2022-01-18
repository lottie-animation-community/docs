# Mask Mode Types

Name | Description | Value
-- | -- | --
None | Mask has no effect on the layer. This mode is solely used for attaching shapes to a layer that can be used by other properties, effects or expressions. | n
Add | This mode will always take the original layer as input and add the opacity values to the final output. | a
Subtract | This mode will take the output of masks higher in the stack as input. If there are no masks, it will take the layer as input. As output it will subtract the opacity values from the input. | s
Intersect | This mode outputs the areas of opacity that overlap with masks higher in the stack. | i
Difference | This mode outputs the subtracted areas of opacity that overlap with masks higher in the stack. | f
