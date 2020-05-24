I also had no experience with Raspberry Pi nor any type of coding. 

Tim C. was kind and patient enough to spend a day helping me get mine up and running. Here's a step by step for all those that are just as lost as I was.

Before starting, download the image (I used recur_2_0_2.img), the app balenaEtcher (referred to as Etcher) and have all your cards formatted in FAT32.

1. Flash image onto microSD using Etcher

2. Reconnect SD card into computer

3. Do step 3 & 4 here: https://desertbot.io/blog/headless-raspberry-pi-3-bplus-ssh-wifi-setup

touch /Volumes/boot/ssh
create wpa_supplicant.conf file and paste info into it. (open using textedit)

Make sure you get all your Wi-Fi info correctly.

4. Since the ssh file and the wpa_supplicant.conf files disappear, copy these two files on your computer, just in case you need to paste them in again. Also make a copy of your config.txt so you have a reference on how to undo any changes that you may make down the line. 

5. Eject SD card properly, put it into your Pi. Turn on Pi.

6. use this command to Wi-Fi access (SSH) your Pi:

ssh pi@raspberrypi.local

default password is: raspberry

If your Pi doesn't connect, your Wi-Fi info is wrong. Make it right in your computer's copy of wpa_supplicant.conf , put the microSD back into the computer and copy that plus the SSH file back in.

Assuming you're connected by now...

7. (THIS STEP IS FOR THOSE WHO BOUGHT THE 3.2" TOUCHSCREEN) 
http://www.lcdwiki.com/3.2inch_RPi_Display
Follow instructions "How to Use with Raspbian"

sudo rm -rf LCD-show
git clone https://github.com/goodtft/LCD-show.git
chmod -R 755 LCD-show
cd LCD-show/
sudo ./LCD32-show

system restarts automatically

8. ssh pi@raspberrypi.local
password: raspberry

9. nano ~/r_e_c_u_r/display_centre/display.py

Line 30: change 13 to 9

@staticmethod
    def _create_display_text(tk):
        return Text(tk, bg="black", fg="white", font=('Liberation Mono', 13), undo=False)

to

@staticmethod
    def _create_display_text(tk):
        return Text(tk, bg="black", fg="white", font=('Liberation Mono', 7), undo=False)

replace line 30 from github into the file. CTRL o, ENTER, CTRL x to get out.

10. sudo cp ~/r_e_c_u_r/dotfiles/config.txt /boot/config.txt

11. sudo nano /boot/config.txt

12. Find this line near the bottom and copy this one in:

## switch for enabling lcd screen (the next line is being used even if its commented out)
dtoverlay=tft9341:rotate=270

13. sudo nano /boot/cmdline.txt

replace:

dwc_otg.lpm_enable=0 console=tty1 console=ttyAMA0,115200 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait fbcon=map:10 fbcon=font:ProFont6x11

with:

dwc_otg.lpm_enable=0 console=tty1 console=ttyAMA0,115200 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait fbcon=map:10 fbcon=font:ProFont6x11 quiet splash plymouth.ignore-serial-consoles

14. sudo reboot , reboot your r_e_c_u_r

15. ssh pi@raspberrypi.local
password: raspberry

16. time to reconfigure some keys if you need! My top 3 keys were messed up, so...

nano ~/r_e_c_u_r/json_objects/keypad_action_mapping.json

changed the order to CAB instead of ABC.

17. Lastly I was experiencing a RED CRT screen. I went into the settings tab by pressing display a couple times, switching the settings around back and forth until I had a usable image.

Hope this helps! Thanks Tim!

