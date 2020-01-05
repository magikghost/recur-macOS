# small signal flow overview

some of this is already mentioned in the operating docs,

but since it is kind of confusing heres a separate explanation of the complex signal flows you can build in __recur__

tl;dr - the signal priory is:

video_players -- capture_preview -- feedback -- shaders

## basic use

if you are using recur just to play back samples this is probably so simple you didnt even think about it; 

signal flow:
 - video_player -> output

if you ever tried recur also with a usb camera attached, you might then notice that the capture preview takes priory over video_player , (capture_preview doesnt have an input so blocks the video_player)

signal flow:
- video_player -| capture_preview -> output

the feedback node has priory over video_player and capture_preview (also blocks , not so interesting on its own...)

signal flow:
- video_player -| capture_preview -| feedback -> output

## shaders

shaders in recur can either have 0input, 1input or 2input. a shader has priory over the three sources mentioned above. so for example if you enable a 0input shader while playing a video and previewing capture and with feedback on, you would only see the shader.

signal flow:
- video_player -| capture_preview -| feedback -| 0in_shader -> output

1input shaders lets you pass the previous source through them , for example playing a video and processing it with a 1input shader:

signal flow:
- video_player -> 1in_shader -> output

or you could process the capture preview :

signal flow:
- capture_preview -> 1in_shader -> output

2input shaders can pass the _two_ previous sources through them ; a classic example is mixing two video_samples (using video player in parallel mode)

signal flow:

- video_player_1 ->
- video_player_2 -> 2in_shader -> output

but equally you could mix a video sample with a live capture feed:

signal flow:

- video_player_1 ->
- capture_preview -> 2in_shader -> output

or feedback:

- video_player_1 ->
- feedback -> 2in_shader -> output

things get a little more complex with 3 shader layers but the idea is exactly the same.
for example you could use 3 1input shaders, to chain 3 fx together:

- capture_preview -> 1in-shader0 -> 1in-shader1 -> 1in-shader2 -> output

but if you put a 0inshader on the last layer (highest prioty) then the output would only have this shader, regardless of what came before it:

- capture_preview -> 1in-shader0 -> 1in-shader1 -| 0in-shader2 -> output

you could however mix between 2 0input shaders with a 2input shader on the last layer:

- 0in-shader0 ->
- 0in-shader1 -> 2in-shader2 -> output

## detour

todo - add how the detour part fits in
