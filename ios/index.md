# Could not find Developer Disk Image

You’ll see this error in xcode when trying to run an application on a device which is not compatible with the xcode version. Mostly because your device has a newer iOS version than your xcode. But you can trick your xcode to think it has the right version by copying the directories.

  open /Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport

Then copy the f.e. "9.2" folder to a new "9.3" if your device runs a 9.3 iOS

**from http://stackoverflow.com/questions/30736932/xcode-error-could-not-find-developer-disk-image**
