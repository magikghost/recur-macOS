# recur_2.1.0 release notes

recur_2.1.0 is the new experimental firmware. here are some details about what is new and how you can try them.

## changes:

- can eject all usb-drives from settings->system->EJECT_USB_DRIVES
- can use mouse/touchscreen to control shader params (default mapped to x->x0 and y->x1) , enable in settings->user_inputs->MOUSE_INPUT
- midi cc handling is improved - no input lag and increased resolution (now full 8bit ( 0-128)
- video samples will always output audio (only over trrs, not hdmi) and audio-level is increased
- fixed a few recording bugs usb quality is improved and doesnt sometimes crash, still usb recording is v resource intensive - doesnt work well for hdmi outputs. also fixed bug where piCapture recording was being encoded at half framerate
- menus have _page-up_ (FN + <) and _page-down_ (FN + >) shortcuts
- default midi map is updated to align also with korgnanokontrol default arrangement and to flatten/expose some useful actions
- now supports osc control - either by connecting recur to a shared network, or setting recur as an access-point and connecting your device directly to it, osc is enabled in settings->user_inputs->OSC_INPUT , and access-point is toggles with settings->user_inputs->ACCESS_POINT ; you can use the GET_IP menu to see what address you need to connect to
- r_e_m_o_t_e server extension - option to run a web server on the pi which can be accessed (by multiple simultaneous devices) , currently set up to send actions (buttons and param values) , but also could be extended to do a bunch of other useful and fun things. can be enabled with settings->user_inputs->REMOTE_SERVER (note that osc must be enabled) , then put the ip address into a web browser
- new plugins menu, setting up a framework that allows community members to add functionality to recur without needing to dive too deep into the core codebase -> write external python scripts for your feature and add them in the pluginManager..

### some new shaders

- _white_ and _black_ lumakey (x0 mix position, x1 switches key direction A -> B or B -> A)
- _chromakey_ (x0 mix position, x1 key direction, x2 select hue)
- plus 2 _displace_ shaders (1 input and 2 input). offsets a pixel in a direction based on its colour (x0 mix amount, x1 offset direction, x2 offset amount), 
- _spotlight_ and _rgb_pallet_ shaders, spotlight is an 1input (despite being filed in the 0input folder whoop), which passes a circle (with x0 x-pos, x1 y-pos, x2 diameter, x3 trail), and rgb_pallet is a 0input source for spotlight that creates plain colour (with x0 r, x1 g, x2 b, x3 alpha)