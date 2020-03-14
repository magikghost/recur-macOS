# recur_2.1.0 release notes

recur_2.1.0 is the new experimental firmware. here are some details about what is new and how you can try them.

## changes:

- __some new shaders__ - now white and black lumakey mixes (x0 mix position, x1 switches key direction A -> B or B -> A) , plus a chromakey (x0 mix position, x1 key direction, x2 select hue) , plus 2 displace shaders (1 input and 2 input). offsets a pixel in a direction based on its colour (x0 mix amount, x1 offset direction, x2 offset amount)
- can eject all usb-drives from settings->system->EJECT_USB_DRIVES
- can use mouse/touchscreen to control shader params (default mapped to x->x0 and y->x1) , enable in settings->user_inputs->MOUSE_INPUT
- midi cc handling is improved - no input lag and increased resolution (now full 8bit ( 0-128)
- video samples will always output audio (only over trrs, not hdmi) and audio-level is increased
- fixed a usb sample recording bug (where it would crash the backend) - still recording samples using usb is so bad quality it is basically unusable - recording samples is only optimised for piCamera/piCapture/other csi inputs
- menus have page-up (FN + <) and page-down (FN + >) shortcuts
- default midi map is updated to align also with korgnanokontrol default arrangement and to flatten/expose some useful actions
- now supports osc control - either by connecting recur to a shared network, or setting recur as an access-point and connecting your device directly to it, osc is enabled in settings->user_inputs->OSC_INPUT , and access-point is toggles with settings->user_inputs->ACCESS_POINT ; you can use the GET_IP menu to see what address you need to connect to
- r_e_m_o_t_e server extension - option to run a web server on the pi which can be accessed (by multiple simultaneous devices) , currently set up to send actions (buttons and param values) , but also could be extended to do a bunch of other useful and fun things. can be enabled with settings->user_inputs->REMOTE_SERVER (note that osc must be enabled) , then put the ip address into a web browser

- new plugins menu, setting up a framework that allows community members to add functionality to recur without needing to dive too deep into the core codebase -> write external python scripts for your feature and add them in the pluginManager..