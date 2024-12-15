# Simple Linux Driver
Simple Linux driver

The tutorial is located here:
https://www.apriorit.com/dev-blog/195-simple-driver-for-linux-os

On a fresh system, to get the module to build, we have to make sure that the
kernel headers are installed. Under Fedora, the command is ``dnf install -y
kernel-devel dwarves``. On Debian based systems the command is ```apt install -y
linux-headers-`uname -r` dwarves```. The dwarves package is required to mitigate
a "BTF" error, which is required to allow the module to load and be recognized.

Also, the built kernel needs to be present in the module build directory. This
can be done with a symbolic link using the command ```ln -s /sys/kernel/btf/vmlinux
/usr/lib/modules/`uname -r`/build/```

The module is installed using the command ```sudo make load``` and uninstalled
using the command ```sudo make unload```.

Monitor the kernel messages with the command ```sudo dmesg -w```. Type
```CTRL-C``` to exit.

Creating the dev file in the dev directory:

* First find the major device number that was assigned by the kernel using the
command ```grep imple-dri /proc/devices```. I get a line such as
```239 Simple-driver```.
* Then make the node using the command ```sudo mknod /dev/simple-driver c 239 0```.
* Prove it works with the command ```sudo cat /dev/simple-driver```.

Then one may unload the driver and delete the node as root.
