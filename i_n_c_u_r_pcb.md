# i_n_c_u_r

this is an optional custom extension board for r_e_c_u_r. it adds 1x DIN  (serial) midi input, 4x knobs and 4x CV analog inputs.

_disclaimer: this is a fun diy project, but dont forget there are other ways to getting serial and cv control over recur parameters. USB midi is supported without any extras. other [diy] and [commercial] products exist that convert cv to midi, and serial to USB. in my opinion a general solution for cv-to-usb-midi applied to recur is more useful than a specific board that only works with recur_

this extension is currently not for sale, assembled, as a kit or as a pcb. it is possible i will offer some projects for sale in the future but for now my priory is on education and community not running a business.

## diy wiring

the [schematic] for this circuit shows exactly how the components are connected. from this you could try creating the circuit on breadboard, stripboard or even etching it yourself. if you take this route you may need to solder some wires to the GPIO pins on the bottom of the raspberry pi as some pins are covered by the screen.

add pictures here

## pcb fabrication

instead of buying a pcb from me another option is to get some fabricated yourself. you need to send some [gerber] files to a fab house and they will print the boards and send them to you. the pcb size is 100x100mm and the default options are fine. i often use [seeedstudio] because they are reliable, cheap and give you an option of colours. remember though that the cheapest Chinese fab houses are not always the most ethical or environmently friendly - if you can afford it consider supporting local companies. often pcbs can only be ordered in batches of 5 or 10. If you are getting boards printed please share any spares with the community, either locally or via the facebook group.


## BOM

REFERENCE | QUANTITY | DESCRIPTION | PACKAGE | NOTE
--- | --- | --- | --- | ---
R1, R2, R3, R4 | 4 | 1K Resistor | through-hole | -
R5 | 1 | 220 Resistor | through-hole | -
R6 | 1 | 100K Resistor | through-hole | -
R7 | 1 | 470 Resistor | through-hole | -
D1, D2, D3, D4, D5, D6, D7, D8 | 8 | BAT85 Diode | through-hole | can replace with BAT46
D9 | 1 | 1N4148 Diode | through-hole | -
U1 | 1 | MCP3008 IC | DIP-16 | you can get these from mouser ,adafruit, even ali
U2 | 1 | 6N138 IC | DIP-8 | -
RV1, RV2, RV3, RV4 | 4 | 10K_LINEAR_POT | ALPHA | tayda ones work although shaft may be too short
J1, J2, J3, J4 | 4 | 3.5MM_JACK | THONKICONN | from thonk or 50pc from modular addict
J5 | 1 | MIDI_DIN_IN | SD-50BV | mouser
J6 | 1 | 1X1_PIN_HEADER | - | -
J7 | 1 | RCA_JACK | RCJ-024 | mouser or ali
J8 | 1 | 20X2_PIN_HEADER | tall long stacking header | from [amazon] like this - the tall is needed to clear the ethernet port

### OPTIONAL :
- some long enough screws and spacers note:measure these to hold it together.
- if you want your ic's in sockets you should buy some DIP-8 /16 also now
- if you __only__ want the _analog_inputs_ and not interested in _serial_midi_, you already have usb midi etc, then you can omit _R5_, _R6_, _R7_, _D9_, _U2_ from the BOM - see circuit schematic for details

## BUILD

start with the lowest to place components : resistors, diodes, ic's

next i would place the two headers since soldering from the top can be awkward with too many components - __NOTE__ these need to be placed __upside down!__ ,:
- _J8_ needs the pins facing up from top of pcb so the screen can go ontop and raspberry pi can go underneath
- _J6_ also needs to soldered from the top so a jumper from the pi board can be run to bottom of circuit

finally place the pots and jacks.

### rca video-out

if you want RCA video out from the pi on this pcb a jumper needs to be run from _J6_ to the composite video out on the raspberry pi board. on pi0 this is a labelled pin, however on pi3 you will need to solder directly to the board. i used a header-cable, cut one side to be soldered.

need to add some images and links to this guide !

[amazon]: https://www.amazon.com/2x20-pin-Female-Stacking-Header-Raspberry/dp/B071FT161B
[these]: https://www.adafruit.com/product/2243

[diy]: http://aemodular.boards.net/thread/141/simple-cv-midi-converter-arduino?page=1&scrollTo=821
[commercial]: https://www.befaco.org/en/vcmc/ 
[schematic]: https://github.com/langolierz/r_e_c_u_r/i_n_c_u_r_pcb/incur_board_schematic.pdf
[gerber]: https://github.com/langolierz//r_e_c_u_r/i_n_c_u_r_pcb/incur_pcbV5.zip
[seeedstudio]: https://www.seeedstudio.com/fusion_pcb.html