# default midi mapping

as of v2_1_0 the default midi map has been updated to also align with the default layout of a korg nanokontrol since this is what many people use (including myself) and is a good option if you are looking to control recur with midi.

## default korg nanokontroller

![image](https://user-images.githubusercontent.com/12017938/76709106-3a0ddc00-66fc-11ea-9a65-d4f5677fc9c5.png)

this is how the default nanokontrol is mapped. you can quickly reset your controller to this by powering it on with track-< , track-> and cycle held down

## recur keys mapping

then with the default control the mapping looks like this: 

![image](https://user-images.githubusercontent.com/12017938/76710388-7430ab00-6707-11ea-9afa-70c6573f5ca4.png)

the black keys are from the numpad keymappings, the grey are continuous control over shader params from all layers, blue is shortcut for swtiching active shader in the bank, and green is shortcut for toggling x3_as_speed and feedback

## default mapping table

you definitely do not need a korg nanokontroller for recur - whatever you have around is the best tool imo - the main thing is to have a few sliders or pots to play with the shader params. if your midi controller allows you to customise the notes it sends you can use this table to set it up for recur.

midi_note | default_mapping | alternative_midi_note
--- | --- | ---
cc 0 | shader0param0 | na
cc 1 | shader0param1 | na
cc 2 | shader0param2 | na
cc 3 | shader0param3 | na
cc 4 | shader1param0 | na
cc 5 | shader1param1 | na
cc 6 | shader1param2 | na
cc 7 | shader1param3 | na
cc 8 | shader2param3 | cc 16
cc 9 | shader2param3 | cc 17
cc 10 | shader2param3 | cc 18
cc 11 | shader2param3 | cc 19
cc 12 | strobe | cc 20
cc 43 | < | cc 58
cc 44 | > | cc 59
cc 42 | SQR | -
cc 41 | SWTCH | - 
cc 61 | [ | - 
cc 62 | ] | - 
cc 45 | o | - 
cc 60 | FN | -
cc 32 | 0 | -
cc 32 | 0 | -
cc 32 | 0 | -
cc 32 | 0 | -
cc 32 | 0 | -