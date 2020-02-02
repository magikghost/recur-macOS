# Plugins (experimental)

doctea has implemented a basic experimental plugin system at https://github.com/doctea/r_e_c_u_r/tree/feature_plugins and https://github.com/doctea/r_e_c_u_r/tree/feature_plugins_shader_gadgets.

## Basic idea

Make it easy to add new custom or experimental functionality to recur!  eg add interfaces for your other video/music gear, run scripts, make hotswitch shader macros, automatically loop sequences of parameter changes...

## How it works so far

Plugin base classes are defined in _/data_centre/plugin_collection.py_.  recur loads python classes that inherit from 
 //Plugin// that it finds in the _~/r_e_c_u_r/plugins/_ directory.

## Plugin classes

### Plugin

Base class that all plugins must inherit from.

### MidiFeedbackPlugin

Base class to implement MIDI feedback, ie sending data back out to the controller to light pad LEDs etc.

Working quite nicely in the example _MidiFeedbackAPCKey25_ plugin, with the Akai APC Key 25.  I suspect it'll work with the Akai APC MkII with minimal changes too but I haven't tested it.  There's also a less-working _MidiFeedbackLaunchpad_ plugin that demonstrates how to write more hardware support.

If you have any hardware you want to see supported or want to implement it it yourself let me know!

### ActionPlugin

Base class for plugins that register new action handlers that can be used from mappings (OSC, MIDI, keys etc)

### SequencePlugin

Base class that implements sequences of events, subclass it to implement your own cool hands-free features.

Can alter speed/go backwards in realtime, pause, loop on/off.  Adds action bindings to make it controllable in realtime from mappings.

# Current Plugins

## ManipulatePlugin

Adds bindings to do advanced mappings:-
* `&&` = run parts to left and right as actions
* `invert|` = take value input and invert it, ie 1.0-value, pass it on to action to the right
* `f:sin(x*pi):|` = run part between :: as python code with 'x' containing value, pass it on to the right
* `set_variable_UPPERCASEORNUMERICVARIABLENAME` = set variable to passed value
* `UPPERCASEORNUMERICVARIABLENAME>` = recall a previously set variable and pass it as value into action on the right
* `>&` = pass the same value to the action on the right

eg bind a control to something like:

`           "DEFAULT": ["invert|f:sin(x*pi):|set_the_shader_param_0_layer_offset_0_continuous>&set_variable_B>&print_arguments&&set_variable_A&&print_arguments","set_strobe_amount_continuous"],`

This example has three branches separated by the `&&`.  The first branch _invert_s the value sent from the control, then calls _sin(value*pi)_ on the result, then passes the result of that on to set the shader param, be stored in variable name B and also printed to debug console.  The second branch simply sets variable B to the initial value from the control.  The third branch prints the initial value.

`            "DEFAULT": ["set_the_shader_param_1_layer_offset_0_continuous&&B>print_arguments","set_shader_speed_layer_0_amount"],
`
This second example sets the shader param to the control vlaue and also recalls the value of 'B', passing to to the console to print (but you could also pass it to any other action or do further manipulation on it, etc)

## Shader Gadgets

There's experimental goodies in https://github.com/doctea/r_e_c_u_r/tree/feature_plugins_shader_gadgets..

### ShaderQuickPresetPlugin

Adds mappable actions to allow saving and hot-switching between 8 different 'shader preset slots'.  Check the docs and code in the branch or refer to my APC Key 25 config for how to set up the mappings.

### ShaderLoopRecordPlugin

Record/overdub 8 different looping or triggered automation clips (parameter movements, shader speed changes, feedback setting changes, etc) and layer them as you wish via bindings.  Saved to disk for later performance.