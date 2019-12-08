# updating firmware

starting from r_e_c_u_r V2.0 images, it is now possible to update to the latest firmware version without needing to download and reflash the sd card.

instead all you need is to connect your pi to the internet and using the `UPDATE_CODE` action in the _system_ settings. you can connect to the internet easily by connecting a ethernet cable between your pi and you modem.

_Note: if you have made any modifications to the code on your image - including changing any input mapping - this meathod probably will not work ; you will need to manually resolve merge conflicts or reset your changes `git reset --hard` before pulling_