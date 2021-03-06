# Sound (and more) reactivity in r_e_c_u_r [experimental work-in-progress]

Sound reactivity is now available in the 2.1 release.

## How to use

### Plug'n'play...maybe?

* havent tested so dont know if this works in 2.1 image by default -- however fingers crossed should be able to plug in a USB sound card, enable the SoundReactPlugin from the recur Plugins screen, and then fiddle with the settings while on the SoundReactPlugin to set levels and modulation slot(s) to send to.  (Then use Shader Modulation bank to connect the modulation output to a shader parameter).


### Requirements

* (for running external sound into the Pi) a USB sound card (I've only tested with https://www.amazon.co.uk/TechRise-External-Adapter-Splitter-Converter/dp/B01J3QGU50/ and it seems to work) _(**todo**: find out if any problems with this device in use)_
* a MIDI controller to set modulation levels etc _(**todo**: menu system on the recur screen to allow config of modulation and mappings without mapping controller)_
* able to login to your recur RPI to make mapping config changes

### Using SoundReactPlugin

SoundReactPlugin currently sends two values to assignable modulation slots, 'energy' (ie volume) and 'peakfreq' (loudest frequency, but it doesn't seem to work like expected).

use eg `"NAV_SND": ["sound_set_modulation_energy_slot_0_level"]` in mapping to set the amount of 'energy' to output to modulation slot 0/A.

use `"NAV_SND": ["sound_set_config_energy_threshold"]` to set the threshold of the input -- set to max to completely mute the input, minimum to have no minimum threshold at all.

use `"NAV_SND": ["sound_set_config_energy_gain"]` to set the gain on the input signal -- set to minimum to have no input at all, max to boost input to maximum

For best results you want to set it so that the levels are peaking/varying around 0.5 - halfway up the little bar graph indicator that is next to the listing for 'energy' on the SOUNDMOD page.  If you get it right the modulation output will fluctuate around the 0 point.

***


### [deprecated but useful if you want to send external sources into recur modulation] Using helpers/soundreact.py to read live volume from sound input

To read live sound you'll need to plug a sound source into your USB input device and verify that its working.

In my branches that support this you'll find a script _helpers/soundreact.py_ which is presently an extremely primitive python script that opens the first sound input device and reads the incoming stream.  It then continually broadcasts the volume of the stream via OSC over UDP, as a float with range 0.0-1.0, addressed to the channel named __/volume__.  The _dotfiles/launcher.sh_ script has been modified to load this script when recur launches.

_TODO: check this is true_ You could also use any OSC source transmitting on the same network on port 5433 to communicate with recur, to send any values.  You could use for example any custom input type you can imagine, another Pi connected to the sounddesk at the venue, but also not limited to sound.  Have the recur respond to a light sensor or camera in the room or buttons or humidity or...

https://github.com/ETCLabs/Sound2Light and https://github.com/ETCLabs/OSCWidgets are great Windows and Mac apps that have full GUIs and handy functions for either interpreting sound (Sound2Light) or building UI dashboards (OSCWidgets) to send out OSC messages.

### Mapping OSC messages to recur parameters

The basic way to do this is to login to your recur and edit __~/r_e_c_u_r/json_objects/osc_action_mapping.json__.  At present the config looks like this:-

>        "/volume": {
>                "DEFAULT": ["modulate_param_0_to_amount_continuous"]
>        },
>        "/kick": {
>                "DEFAULT": ["modulate_param_1_to_amount_continuous"]
>        },
>        "/himid": {
>                "DEFAULT": ["modulate_param_2_to_amount_continuous"]
>        },
>        "/high": {
>                "DEFAULT": ["modulate_param_3_to_amount_continuous"]
>        }

This maps the ///volume/ OSC channel to internal recur action //modulate_param_0_to_amount_continuous//, and so on for kick+himid+high (note this are not currently used by __soundreact.py__ but you can set them up in Sound2Light or something else, hopefully we'll have more sophisticated __soundreact.py__ in future.


### TODO: is it possible to use the sound from a USB capture device/piCapture?

Maybe this would need to be done on the conjur side of things instead of using the soundreact script?  Maybe that is even a more sensible place to put the soundreact script functionality as it already communicates in two directions with recur and so recur could easily tell conjur when to turn the sound device on/off or choose input etc?

### [deprecated?] Setting up the PI to use the USB sound device

_**todo**: instructions on doing this, implementing in default recur image?_

I didn't keep notes on how I did this, but roughly you need to disable the Pi's onboard sound and enable the USB sound device instead.  I think I followed instructions from https://raspberrypi.stackexchange.com/questions/80072/how-can-i-use-an-external-usb-sound-card-and-set-it-as-default to do this but it took a bit of fiddling.


See page [using the modulation parameters] to see how modulate parameters work!


- doctea 2020-03-16