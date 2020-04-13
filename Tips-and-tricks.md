# Shaders

## Blending camera/video/shaders together

_Winston Giles Edwards 2020-04-12_
Something fun I just learned. You can mix your video input with feedback, and then mix that with a gen shader for a total of three layers together. Getting some whacky results this way!

Try it by first playing a video file or activating a camera input. Load luma_key on lay0, then a gen shader on lay1, and then luma_key again on lay2. Activate frame buffer feedback and then dial in the luma keys. Lay0 composites the video source with frame buffer, and then lay2 composites the preceding result with the gen shader.

This also works to mix a video file, camera input and gen shader. Do the same as above, but play a video file and activate the camera input, but don’t engage feedback.

BONUS: you can also use an FX shader, instead of a gen shader, between two luma keys and it lets you layer feedback or sources in some different ways. Right now I'm using the rotate shader and it's giving some incredible results

This actually lets you apply FX to the feedback layer separately. I have my video totally centered and the feedback is spiraling. Normally the video frame rotates while the feedback stays static. This still creates spiraling effects but now you can do it without moving your video

# Videos

...

# Plugins

...

# General

...

# Control mapping/customisation

...

