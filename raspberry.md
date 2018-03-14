## Update

Before doing anything update everything

    sudo apt-get update 
    sudo apt-get upgrade -y

## Configuration

use `sudo raspi-config`:

- set ssh user password
- set timezone in "Localisation Options"
- enable VNC & SSH in "Interfacing Options"
- enable overscan & set resolution
- run the update

# Disable Screensaver

    # /home/pi/.config/lxsession/LXDE-pi/autostart

    # deactivated default lines
    #@lxpanel --profile LXDE-pi
    #@pcmanfm --desktop --profile LXDE-pi
    #@xscreensaver -no-splash
    #@point-rpi

    # now the new lines:
    # disable sleep mode
    @xset s off
    @xset -dpms
    @xset s noblank


# Disable mouse pointer

install unclutter

    sudo apt-get install unclutter

then activate unclutter:

    # /home/pi/.config/lxsession/LXDE-pi/autostart
    # hide mouse cursor
    # requires "unclutter" to be installed
    #   sudo apt-get install unclutter
    @unclutter -idle 0


# Chrome Autostart

change autostart settings:

    # read about supported comamnd line arguemnts:
    # https://peter.sh/experiments/chromium-command-line-switches/
    @chromium-browser --noerrdialogs --incognito --disable-infobars --kiosk <url>



# SD CARD

## Backup

Insert SD Card into reader, then check which drive is associated with that reader (i.e. `disk2`, `disk3` etc):

    diskutil list

    sudo dd if=/dev/disk2 of=~/Desktop/raspberrypi.dmg

# Restore

Unmount, format and restore from dmg.

    diskutil unmountDisk /dev/disk2
    sudo newfs_msdos -F 16 /dev/disk2

 Note that not `disk2` is used but `rdisk2` which makes the command much faster. Also the `bs=1m` parameter tells dd to write in 1MB blocks.

    sudo dd if=~/Desktop/raspberrypi.dmg of=/dev/rdisk2

You can press CTRL+t to check how fast it’s working. `dd` will report the current write speed and overall process. MacBookPro 2017 took 4 Minutes to write 3GBs. So 16 GB will take ~20 Minutes.



# Sources

- [sd card copy on osx](https://computers.tutsplus.com/articles/how-to-clone-raspberry-pi-sd-cards-using-the-command-line-in-os-x--mac-59911)
