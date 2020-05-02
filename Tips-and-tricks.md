# Shaders

## Blending camera/video/shaders together

_Winston Giles Edwards 2020-04-12_
Something fun I just learned. You can mix your video input with feedback, and then mix that with a gen shader for a total of three layers together. Getting some whacky results this way!

Try it by first playing a video file or activating a camera input. Load luma_key on lay0, then a gen shader on lay1, and then luma_key again on lay2. Activate frame buffer feedback and then dial in the luma keys. Lay0 composites the video source with frame buffer, and then lay2 composites the preceding result with the gen shader.

This also works to mix a video file, camera input and gen shader. Do the same as above, but play a video file and activate the camera input, but don’t engage feedback.

BONUS: you can also use an FX shader, instead of a gen shader, between two luma keys and it lets you layer feedback or sources in some different ways. Right now I'm using the rotate shader and it's giving some incredible results

This actually lets you apply FX to the feedback layer separately. I have my video totally centered and the feedback is spiraling. Normally the video frame rotates while the feedback stays static. This still creates spiraling effects but now you can do it without moving your video

# Videos

## Generating interference patterns

_Winston Giles Edwards 2020-04-27_
https://www.facebook.com/groups/114465402691215/permalink/685094172294999/

I just discovered how to turn r_e_c_u_r into an interference pattern generator, aka Moire pattern.

I took a simple black and white wavy line pattern and converted it into a 60-second video on Kapwing, then downscaled it to high quality SD resolution in Handbrake.

The r_e_c_u_r sampler needs to be in parallel mode. Load the video into a bank by itself and then play it in both the Now and Next players.

Go to the shaders and load rotate.frag into lay0 and set the controls to 25-50-50-50. Now load the white or black luma key shader into lay1. Make sure the second control is at 0, use the first control to dial in your key.

You should now have one video file layered and keyed against a copy itself. When you use the controls in the rotate shader, it will cause interference patterns to appear.

You can try it with this video https://www.dropbox.com/s/ywf1tllsdobgzvx/wavymoirebait-1.mp4?dl=0&fbclid=IwAR2N7tyCDNrcHEGmxZHcUQ5MLrIAcJjHBACl9VIAiUgWbjxES1ZjIWzgav4

This is just one pattern layered... two different patterns will be bonkers!!

This also works with other transforming shaders like s-wobble in the 1-input shader folder

Sad to report that, for whatever reason, this doesn't work as expected with two different videos. Will keep trying...

...

# Plugins

...

# General

...

# Control mapping/customisation

...

