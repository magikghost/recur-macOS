# how to develop r_e_c_u_r

i have tried to write this application so it can easily be read and modified for different use cases. i recommend forking the repo to experiment with the codebase. open a pull request into origin <your_branch> if you want to contribute your changes back into the project.

this [diagram] might help understand the design : 

![design_overview][design_overview]

here are some examples of changes you might want to make:

## rearranging the _keypad_ controls

to simplify the key-mapping process, i have pre-mapped the numpad keys to the _labels_ `a` to `s` like this:

![premapped_keys][premapped_keys]

(see [dotfiles] for description of this process)

for each _label_ the application will read the [keypad_action_mapping.json] file and map it to an [action]. the format also allows unique actions per _control_mode_ and per the `FUNCTION` toggle :

```
...
"x": {
	"NAV_BROWSER": ["trigger_this_action_in_browser_mode"],
	"DEFAULT": ["trigger_this_action_in_any_other_mode_with_FN_off","trigger_this_action_in_any_other_mode_with_FN_on"],
}
```
## creating a new action

# updates in archetecture for v2 :
need to write this :
-explain a bit about using openframeworks as backend , with this repo and this etc, how they communicate - osc etc ...

## beyond

i hope the foundations iv provided encourage you to make larger changes for more ambitious features. if so you could try getting in touch (langolierz@gmail.com) first and maybe i could help align your approach with the rest of the project

[diagram]: https://docs.google.com/drawings/d/1ltWCv82rKVzOiFe6GaDDPlneG2oki0yRujArPU5V2ss/edit?usp=sharing
[design_overview]: images/design_overview.jpg
[premapped_keys]: https://github.com/langolierz/r_e_c_u_r/blob/master/enclosure/vectorfront_blank_keys.png?raw=true
[dotfiles]: https://github.com/langolierz/r_e_c_u_r/tree/master/dotfiles
[keypad_action_mapping.json]: https://github.com/langolierz/r_e_c_u_r/tree/master/json_objects/keypad_action_mapping.json
[action]: https://github.com/langolierz/r_e_c_u_r/tree/master/actions.py 

