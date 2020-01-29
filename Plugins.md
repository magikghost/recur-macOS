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

# Shader Gadgets

There's experimental goodies in https://github.com/doctea/r_e_c_u_r/tree/feature_plugins_shader_gadgets..

## ShaderQuickPresetPlugin

Adds mappable actions to allow saving and hot-switching between 8 different 'shader preset slots'.  Check the docs and code in the branch or refer to my APC Key 25 config for how to set up the mappings.

## ShaderLoopRecordPlugin

Record/overdub 8 different looping or triggered automation clips (parameter movements, shader speed changes, feedback setting changes, etc) and layer them as you wish via bindings.  Saved to disk for later performance.