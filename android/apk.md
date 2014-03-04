
# Check SHA1 Checksum of android file

Compare the app store version against a stored version by comparing the sha1 checksums of the files:

	/usr/bin/openssl sha1 <filename>

# Decompile APK

Requirements: Java, Android APK installed properly

Decompiling an apk requires a tool called "[apktoool](https://code.google.com/p/android-apktool)" which is available with different runners for osx, linux and windows on [google code project page of apktool](https://code.google.com/p/android-apktool/downloads/list).

You’ll need to download the jar file and the runners and put them into the same directory. After that you can easily decompile apk files:

	./apktool decode <apk-filename>

If all this is to complicated or you’re not able to install Java or the SDK try the online decompiler [DecompileAndroid](http://www.decompileandroid.com/).