### I take no credit for the files or projects linked to in here!
### This is me documenting how i got it running on my machine running Arch Linux.
----
This was a little more involved than what i expected, im guessing its much easier to just go with maven or gradle for managing java packages and modules. But the learning was good!

[Installing java on Arch](https://wiki.archlinux.org/title/Java)</br>
Make sure you have java installed </br>
```bash
java --version
# Should output java version and the vendor you chose
```


If not, find the version of java you want and install it:
`sudo pacman -S jdk-openjdk` </br>
Java is by default installed to /usr/lib/jvm/

Set a java home path of some kind, i followed the javafx tutorial so its called $PATH_TO_FX:</br>
`export $PATH_TO_FX=/usr/lib/jvm/{java version}/lib/`

Getting gstreamer up and running with java through the terminal is a task:
First download gstreamer through pacman, together with packages:
```bash
sudo pacman -S gstreamer gst-libav gst-plugins-bad gst-plugins-good gst-plugins-ugly gst-plugins-base
# This will get you all the plugins you would ever need
```
Install a version of javafx
```bash
yay -S java-openjfx
```

gstreamer does not natively work with java, so we need to install some bindings
- [java bindings for gstreamer](https://github.com/gstreamer-java/gst1-java-core)
- [jna lib](https://github.com/java-native-access/jna)
- [javafx-gstreamer bindings](https://github.com/gstreamer-java/gst1-java-fx)

(Again, all credit goes to the respective authors)

These all need to be moved to a place you know. Its easiest to download to the default install location of the jdk you have, AKA move them to `$PATH_TO_FX/` in the case of this project.

## Making the java build and run string
Java needs to be compiled before it can be run.
Just running javac on the [[FXCamera.java]] test project will cause a bunch of errors because it doesnt know where all the jars are. 
We need to point it to the modules using the -cp (CLASS PATH) flag. Confusingly, javafx uses a different (newer?) way to point to modules, using the --module-path flag.

```bash
javac -cp .:$PATH_TO_FX/gst1-java-core-{version}.jar:$PATH_TO_FX/gst1-java-fx-{version}.jar:$PATH_TO_FX/jna-{version}.jar \
--module-path $PATH_TO_FX --add-modules javafx-controls FXCamera.java
# Use a colon : to separate the paths to the modules we are using
```

Then you should be able to run it by doing
```bash
java -cp .:$PATH_TO_FX/gst1-java-core-{version}.jar:$PATH_TO_FX/gst1-java-fx-{version}.jar:$PATH_TO_FX/jna-{version}.jar \
--module-path $PATH_TO_FX --add-modules javafx-controls FXCamera
# Only difference here is literally just changing javac to java, and running the class instead
