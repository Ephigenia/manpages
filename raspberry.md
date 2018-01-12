# Disable Screensaver

    # /home/pi/.config/lxsession/LXDE-pi/autostart
    # disable those lines:
    #@lxpanel --profile LXDE
    #@pcmanfm --desktop --profile LXDE
    #@xscreensaver -no-splash

    # disable sleep mode
    @xset s off
    @xset -dpms
    @xset s noblank



# Chrome Startup

    # /home/pi/.config/lxsession/LXDE-pi/autostart
    @chromium-browser --noerrdialogs --disable-session-crashed-bubble --disable-infobars --kiosk <url>



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
