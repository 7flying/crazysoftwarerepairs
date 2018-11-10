# Crazy software repairs

When hw and sw don't understand each other.

## Crazy touchpad

Taken from here [askubuntu.com](https://askubuntu.com/questions/763763/touchpad-under-16-04-not-working)

1. Edit GRUB
   ``` sudo -H gedit /etc/default/grub```

    In the open window edit line

    ```GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"```

    It should look this way

    ```GRUB_CMDLINE_LINUX_DEFAULT="quiet splash i8042.nopnp"```

    Save file and run

    ```sudo update-grub```

2. Prevent i2c_hid from loading

    ```
    echo "blacklist i2c_hid" | sudo tee /etc/modprobe.d/i2c-hid.conf
    sudo depmod -a
    sudo update-initramfs -u
    echo "synaptics_i2c" | sudo tee -a /etc/modules
    ```
3. Reboot.

What it does is, it removes the synaptics hid drivers from the blacklist and
allows them to be loaded at the initialization of the RAM file system, allowing
your touch pad to work at boot

## Crazy wifi module

Unable to fix this.

### Workaround

1. ```git clone https://github.com/abperiasamy/rtl8812AU_8821AU_linux.git```
2. Select branch with the kernel version that we want.
3. ```make; sudo make install``` (for older kernels ensure that ```gcc```'s
version matches the one that the module wants, e.g. don't try to compile with
gcc 8 a 3.X and so...)
