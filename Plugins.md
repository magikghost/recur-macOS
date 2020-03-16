# Plugins (experimental)

## Basic idea

Make it easy to add new custom or experimental functionality to recur!  eg add interfaces for your other video/music gear, run scripts, make hotswitch shader macros, automatically loop sequences of parameter changes...

# Current Plugins

## SoundReactPlugin

Listens on the input to a USB soundcard and sends modulation values to the rest of r_e_c_u_r.  See the docs on Sound Reactivity for further details.

## WJSendPlugin

Sends commands to Panasonic mixers using a USB->serial dongle - tested with MX30 and MX50 but probably works with AVE55 and any other video mixer that uses the same serial protocol.

UI allows assignment of r_e_c_u_r modulation to any parameter: with the WJMX page selected, use <> to select current parameter and [] to select the argument of the parameter.  Then use bound controls to adjust modulation level.

There are only a handful of commands with proper support at the moment, but you can also hardcode commands using MIDI, keypad or osc mappings.

Automation is also recorded and played back via ShaderLoopRecordPlugin.

Note that this is a bit buggy, it works great when only sending one type of parameter automation but when you try and modulate multiple commands at the same time the mixer seems to lag up and misses a lot of the commands lead to some pretty horrendous lag, especially noticeable when automating the wipe.  Am not currently sure if this is something that can be fixed, or if its a limitation of the mixer/serial speed, but have tried many things to fix so if anyone has any ideas to try lmk!

## LFOModulation

Has 4 LFOs sending modulation on slots 0-3 (A-D).  Can adjust overall speed and the amount of each.  This modulation can be assigned to shader parameters, or to WJSendPlugin parameters etc.

Modulation output goes from -1 to +1 so the amount determines how much it moves around the 0 point set by the shader parameter.  (well I think thats how it should work anyway, up for discussion..)

Configurable formulas to add different shapes are TODO.  Each channel is currently sending a different shaped wave.

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

This example has three branches separated by the `&&`.  The first branch _invert_s the value sent from the control, then calls _sin(value*pi)_ on the result, then passes the result of that on to set the shader param, be stored in variable name B and also printed to debug console.  The second branch simply sets variable A to the initial value from the control.  The third branch prints the initial value.

`            "DEFAULT": ["set_the_shader_param_1_layer_offset_0_continuous&&B>print_arguments","set_shader_speed_layer_0_amount"],
`
This second example sets the shader param to the control vlaue and also recalls the value of 'B', passing to to the console to print (but you could also pass it to any other action or do further manipulation on it, etc)

## Shader Gadgets

### ShaderQuickPresetPlugin

Adds mappable actions to allow saving and hot-switching between 8 different 'shader preset slots'.  Check the docs and code in the branch or refer to my APC Key 25 config for how to set up the mappings.

### ShaderLoopRecordPlugin

Record/overdub 8 different looping or triggered automation clips (parameter movements, shader speed changes, feedback setting changes, etc) and layer them as you wish via bindings.  Saved to disk for later performance.


# Developing new plugins

Plugin base classes are defined in _/data_centre/plugin_collection.py_.  recur loads python classes that inherit from 
 //Plugin// that it finds in the _~/r_e_c_u_r/plugins/_ directory.

## Plugin classes

### Plugin

Base class that all plugins must inherit from.

### MidiFeedbackPlugin

Base class to implement MIDI feedback, ie sending data back out to the controller to light pad LEDs etc.

Working quite nicely in the example _MidiFeedbackAPCKey25_ plugin, with the Akai APC Key 25.  I suspect it'll work with the Akai APC MkII with minimal changes too but I haven't tested it.  There's also a less-working _MidiFeedbackLaunchpad_ plugin that demonstrates how to write more hardware support.

If you have any hardware you want to see supported or want to implement it it yourself let me know!

### ActionsPlugin

Base class for plugins that register new action handlers that can be used from mappings (OSC, MIDI, keys etc).

### SequencePlugin

Base class that implements sequences of events, subclass it to implement your own cool hands-free features.

Can alter speed/go backwards in realtime, pause, loop on/off.  Adds action bindings to make it controllable in realtime from mappings.

Used in LFOModulation and ShaderLoopRecordPlugin.

### DisplayPlugin

Plugins of this type implement a new Display page on the r_e_c_u_r screen, for showing controls/parameters related to the plugin.

### ModulationReceiverPlugin

Plugins that inherit from this will be sent updates when the modulation values change -- used for implementing modulation response (eg WJSendPlugin parameters can be modulated).

### AutomationSourcePlugin

Plugins of this type record and playback parameter changes -- eg for allowing WJSendPlugin to record/playback automation through ShaderLoopRecordPlugin.


***

All plugins so far by Tristan Rowley https://github.com/doctea/, let me know any questions, bugfixes, ideas or contributions!