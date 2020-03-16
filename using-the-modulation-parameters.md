# Using modulation parameters

The newer branches contain _modulation_ functionality, intended mostly to be used creatively with sources of external control like OSC or MIDI, and also internal sources like the LFOModulationPlugin or SoundReactPlugin.

## How it works

### Internally

There are 4 'modulation parameters', ABCD (aka 0-3).  Each of these stores a float from -1.0 to +1.0, this is the 'modulation value'.

Each modulation slot has its value set using the recur actions _*modulate_param_0_to_amount_continuous*_ through _*modulate_param_3_to_amount_continuous*_ in a _json_objects/osc_action_mapping.json_ or _json_objects/midi_action_mapping.json_ file.  This is also how internal plugins should send their own modulation to slots via _actions.call_method_name_.

### User controls - setting shader modulations

By default, modulation parameter 0 is selected.  Switch to different modulation parameters using _**select_shader_modulation_slot_X**_ where X is the parameter 0-3.  Reset all modulations for the currently selected modulation parameter using _**reset_selected_modulation**_.

To set the currently selected modulation parameter's level for a shader parameter, use _**set_param_X_layer_offset_Y_modulation_level_continuous**_ where X is the shader parameter number 0-3 and Y is the shader layer.

In recur, there are 3 shader layers, and each can run one shader at a time.  Each shader has 4 parameters, numbered 0-4, making for a total of 12 controllable parameters.  Each shader layer parameter keeps a 'modulation level' for each modulation parameter (so 48 assignations). The _average_ of all four _modulation parameter values_ is applied to each _shader parameter_, according to the _modulation parameter level_ respective to the parameter.

Check out the _json_objects/osc_actions_mapping_APC Key 25.json_ file for an example of how I've mapped this to my controller -- I have it so that holding FN makes the knobs on my controller adjust the //modulation level// for the respective shader parameter, with some other buttons to select a different modulation parameter and to reset the existing one.

### Plugin support

LFOModulationPlugin and SoundReactPlugin send modulation signals, and WJSendPlugin can receive modulation signals and map them to internal parameters.

ManipulatePlugin stores a copy of recent mod values as variable, so you can reuse them in other bindings if necessary (check the ManipulatePlugin page to see these updating)

- doctea 2020-01-29 (revised 2020-03-16)
