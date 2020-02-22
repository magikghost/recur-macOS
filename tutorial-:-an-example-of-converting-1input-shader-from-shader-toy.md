# tutorial for converting a simple 1input shader from shadertoy to recur/structure

![image](https://user-images.githubusercontent.com/12017938/75094881-6e73f980-558f-11ea-8cd0-19cafbd8b51a.png)

this is the [effect](https://www.shadertoy.com/view/XtfXzX) from shadertoy we want to get working with recur(/structure): 

```
float xOffset = 0.011;

void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    vec2 uv = fragCoord.xy / iResolution.xy;
    
    vec4 bg = texture(iChannel0, uv);
 
    bg.r = texture(iChannel0, vec2(uv.x+xOffset, uv.y)).r;

    fragColor = bg;
}
```

## the sandbox

go to http://glsl.erogenous-tones.com , _click to create new shader_ , it should look something like this : 

![image](https://user-images.githubusercontent.com/12017938/75093619-bc830000-5583-11ea-8e85-ea1b27c60562.png)

## copy / paste

now we want to copy the content of `void mainImage` in shadertoy code and paste it in the top of the `void main` in glsl-sandbox (can leave the `gl_FragColor` for now):

![image](https://user-images.githubusercontent.com/12017938/75093720-901bb380-5584-11ea-842a-7daff7828fa3.png)

## replace variable names

you will see we have errors with these new lines. this is because shadertoy and structure/recur use different names for the same variables.

you can see the names that structure/recur expect at the top of the glsl file : 

```
varying vec2 tcoord;    // location
uniform sampler2D tex;  // texture one
uniform sampler2D tex2; // texture two
uniform vec2 tres;      // size of texture (screen)
uniform vec4 fparams;   // 4 floats coming in
uniform ivec4 iparams;  // 4 ints coming in
uniform float ftime;    // 0.0 to 1.0
uniform int itime;      // increases when ftime hits 1.0
```

and you can see the names that shadertoy is using by clicking a little dropdown called _shader inputs_ at top of the web page : 

![image](https://user-images.githubusercontent.com/12017938/75093796-38317c80-5585-11ea-8582-f8ee77e805c4.png)

the trick to fixing these errors is to replace any of the shadertoy names that are being used with the corresponding structure/recur names. this can take some experience / guessing , but should be quite obvious in most cases ...

```
ichannel0 -> tex
iResolution -> tres
fragCoord -> tcoord
```

this should resolve some of the errors. also you might have noticed the shadertoy line `float xOffset = 0.011;` above the _void mainImage_, lets copy that across to the sandbox now too : 

![image](https://user-images.githubusercontent.com/12017938/75094070-e2120880-5587-11ea-8c78-8968d3285017.png)

## replace function names

we still have some errors now. this is because of the different types of shader language being used. raspberry pi's, phones and other hardware systems use a type of glsl called _embedded systems_ or __gles__. this is similar to shaders that run in the browser (like the sandbox we are using), which is called __webgl__ .

however the shader we can see on shadertoy is a version made to run on desktop computers, and this has a few syntactic differences for built in functions which we will replace now:

```
texture -> texture2D
fragColor -> gl_FragColor
```

this should fix the remaining errors.

## understanding position input

one final difference you need ti know - consider the shadertoy line:

```
vec2 uv = fragCoord.xy / iResolution.xy;
```

this is saying _take the position on the screen and divide it by the total size of the screen_, 
- if the x position was in the far left (pixel 0) , and the screen was 400 pixels, the result is 0/400 = 0 ;
- if x in the middle (pixel 200), the result 200 / 400 = 0.5 ;
- if x in far right (pixel 400), the result 400 / 400 = 1 ;

regardless of the screen size this line will convert the actual pixel position to a percentage (value between 0 and 1) - this is called _normalising_.

however, in gles , the _tcoord_ is __already__ normalised, so we dont need to do the division again here. you can remove the ` / tres.xy` from line 24 in the sandbox

## see it working

finally we can remove the last line `gl_FragColor = vec4(f0, f1, f2, 1.0);` , which was the placeholder that came with the template.

![image](https://user-images.githubusercontent.com/12017938/75094381-c0fee700-558a-11ea-954c-8c5f8aa6cfae.png)

now the shader is running on the sandbox and will run also on the hardware. yay!

note you can change the input example (_use clip_) with the dropdown in the top middle of the sandbox

## adding custom params

however there is one more thing we can do : both recur and structure have parameters which can be controlled (by knobs or otherwise) to make the shader responsive in real-time performance environments. the shadertoy code does not come with any parameters, but we can try to add some.

to do this well takes some understanding and intuition of how the shader works. but still we can try hacking around and see what we find - even just one well placed parameter can make all the difference !

in the sandbox you can see the parameter _f0_ defined on line 14. this is connected to the slider _f0_ at top of the sandbox. the _mix_ function says that this param will go between 0.05 and 0.95. personally i prefer having the full range of 0 - 1 so just change these lines. but this is up to you. by placing this parameter in the code you can control some values from your hardware.

i try adding _f0_ to the offset on line 28. this moves the red channel in horizontal direction.

next i multiply _f1_ by the position input on the red channel. this will stretch and shink it in the vertical position. but i would rather have _f1 = 0_ be no effect, and have the squash increase as _f1 increases_, so instead of multiplying by _f1_ i will use `(1.0 + f1)` - this is multiply by 1 when f1 is 0 : no effect , and multiply by 2 when f1 is 1.

__NOTE: you must use decimal points when defining numbers in glsl - just 1 wont work__

## final shader

![image](https://user-images.githubusercontent.com/12017938/75094846-1dfc9c00-558f-11ea-973c-c5c73a44d0c8.png)

here is the final code - you can put this straigt in a text file and play in recur. or can pop it in the sandbox and play around with it there:

```
precision mediump float;
// template : glsl.ergonenous-tones.com
varying vec2 tcoord;    // location
uniform sampler2D tex;  // texture one
uniform sampler2D tex2; // texture two
uniform vec2 tres;      // size of texture (screen)
uniform vec4 fparams;   // 4 floats coming in
uniform ivec4 iparams;  // 4 ints coming in
uniform float ftime;    // 0.0 to 1.0
uniform int itime;      // increases when ftime hits 1.0
//f0::
//f1::
//f2::
float f0 = mix(0.0, 1.0, fparams[0]);
float f1 = mix(0.0, 1.0, fparams[1]);
float f2 = mix(0.0, 1.0, fparams[2]);

float time = float(itime) + ftime;
vec2 resolution = tres;

float xOffset = 0.011; // from line 1 shadertoy

void main( void ) {
    vec2 uv = tcoord.xy ;
    
    vec4 bg = texture2D(tex, uv);
 
    bg.r = texture2D(tex, vec2(uv.x+xOffset+f0, uv.y * (1.0 + f1))).r;

    gl_FragColor = bg;

}
```

hope this walkthrough helped ! 

