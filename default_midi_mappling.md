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

keep in mind that this is just a copy of the actual midi map which can be found at [json_objects/midi_action_map.json](https://github.com/langolierz/r_e_c_u_r/blob/dev/json_objects/midi_action_mapping.json) the alternative mappings are there for backward compatibility. if your controller doesnt allow you to change the notes it returns you only need to edit this json file to update your own mapping

- note1: when using the FN key with midi , it does not behave 'gated', ie you need to press it again to toggle off.
- note2: if you are using a controller with enough continuous controls to map all 12 shader_layer params then you should switch on `FIX_PARAM_OFFSET_LAYER` in settings->shaders , this ensures these mappings are fixed (by default the mappings are relative to which shader layer is selected in the ui - for inputs with only 4 continuous controls)

midi_note | default_mapping | alternative_midi_note
--- | --- | ---
cc 0 | shader0param0 | -
cc 1 | shader0param1 | -
cc 2 | shader0param2 | -
cc 3 | shader0param3 | -
cc 4 | shader1param0 | -
cc 5 | shader1param1 | -
cc 6 | shader1param2 | -
cc 7 | shader1param3 | -
cc 8 | shader2param3 | cc 16
cc 9 | shader2param3 | cc 17
cc 10 | shader2param3 | cc 18
cc 11 | shader2param3 | cc 19
cc 12 | strobe | cc 20
cc 43 | < | cc 58, note_on 72, note_on_36
cc 44 | > | cc 59, note_on 73, note_on_37
cc 42 | SQR | note_on 74, note_on_38
cc 41 | SWTCH | note_on 75, note_on_39 
cc 61 | [ | note_on 76, note_on_40
cc 62 | ] | note_on 77, note_on_41
cc 45 | o | note_on 78, note_on_42
cc 43 | DSPLY | note_on 79, note_on_43
cc 60 | FN | note_on 80, note_on_44
cc 32 | 0 | note_on 81, note_on_45
cc 33 | 1 | note_on 82, note_on_46
cc 34 | 2 | note_on 83, note_on_47
cc 35 | 3 | note_on 84, note_on_48
cc 36 | 4 | note_on 85, note_on_49
cc 37 | 5 | note_on 86, note_on_50
cc 38 | 6 | note_on 87, note_on_51
cc 39 | 7 | note_on 88, note_on_52
cc 48 | 8 | note_on 89, note_on_53
cc 49 | 9 | note_on 90, note_on_54
cc 51 | x3_as_shader | -
cc 52 | feedback | -
cc 64 | shader_slot_0 | -
cc 65 | shader_slot_1 | -
cc 66 | shader_slot_2 | -
cc 67 | shader_slot_3 | -
cc 68 | shader_slot_4 | -
cc 69 | shader_slot_5 | -
cc 70 | shader_slot_6 | -
cc 71 | shader_slot_7 | -