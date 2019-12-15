# feature request ideas

at the moment lots of feature requests get posted on the facebook group, or written to me directly. this is ok and i love discussing these things but i think also we need a central place to organize these ideas, and maybe some way people can express which they are most interested in.

i think the github issues section works quite well for this, but also it can get a bit messy and confusing with all the bugs etc.

i will try to at least list the ideas that are in my head at the moment here...


## support pi4
## osc input + pi as wifi transmitter/hotspot
## gpio matrix input
## touch screen as shader params
## camera settings - exposure, shutter, resolution, ratio etc for openframeworks backend
## copying files to and from external device
## support videos audio track - bonus record audio in??
## saving shader param settings per slot
including per-slot parameter mapping...?
## support NDI ; waiting for offical pi support here
## player types to detour such as pingpong
## option to record output rather than input
## support images / gifs in the sampler mode
## a bank mode similar to sampler / shdr_bnk but for triggering generic system actions - mapping python files?
## playing a video from the browser - similar to how you can play shaders from shader browser
## page up / page down nav shortcuts
## improve some of the mix shaders
## design laser-cut enclosure
## usb eject setting
## cue mode - different output on display to screen - not sure if this is possible ?
## optimize analog read / midi performance to update at least once per frame with minimal stepping
## ??? some kindof projection mapping option ???
Maybe using ofxPiMapper? https://github.com/kr15h/ofxPiMapper
## audio reactivity / faux audio reactivity (tap tempo, sync'd modulation, etc)
Audio reactivity can be implemented via Osc -- will open it up to be controlled from many different external input sources.
To provide 'in the box' support, add a small helper app running on the recur that listens to usb soundcard audio input and sends values via Osc?
Add some shader(s) that can use audio data to draw level meters/spectrograms etc.
## enhanced MIDI control / configuration / MIDI feedback
Use a sort of plugin architecture and automatic detection of MIDI device name to load appropriate controller mappings.
Provide callbacks to allow sophisticated MIDI feedback (lighting pads to indicate slots are full, playing status etc)
doctea's already started implementing this in his branch at https://github.com/doctea/r_e_c_u_r/tree/feature_midi_apckey25_feedback - it currently supports the pads on the Akai APC Key 25 (https://www.akaipro.com/apc-key-25) and shows shader bank/slot/playing status.
Can be expanded to support other controllers and functionality (config contributions welcome!)
## record/loop parameter automation
eg hold down a button, wiggle a CC knob for a few seconds, release it and the recur will loop the automation
should be able to layer multiple automation recordings, save them to slots for later reuse, and start/stop/retrigger them manually or in time with beat?
## meta presets / macros
an extra 'browser' mode that stores macros in triggerable banks/slots
macros should be able to do things like set multiple shaders active, set parameters, set videos playing, set custom midi/parameter mappings, trigger external scripts, play back sequences of parameter changes/automation, switch between configurations
perhaps this could be the functionality that parameter automations are built upon
looping / one shot macros
it would be great if I could use this to eg trigger a macro that sends serial commands to external devices
perhaps implement macros as a base class of macro functionality, with some base types for different macro functionality, then chain them together and extend with custom code where necessary
eg when a macro is selected, particular set of shaders are loaded and the CC knobs are temporarily mapped to controlling the 'best' parameters of those shaders via formulas.  
a button to save the current state as a macro (loaded shader statuses+params, video settings, etc) for later callback or customisation
## formula mappings
map Osc and MIDI input values to parameter values via user-editable formulas, discussed in https://github.com/langolierz/r_e_c_u_r/issues/101
have these mappings per-slot or per-macro
## multiple cameras / step through multiple cameras