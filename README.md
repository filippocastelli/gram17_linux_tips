# LG Gram 17 Linux Tips

Config tips for installing and using Linux on LG Gram 17 (my model is **17Z90P**, most of these will probably apply to other models as well)



## Accessing BIOS Advanced Settings:

Hold ```F10``` while starting to access the boot selector, press ```TAB``` to access the BIOS.

Enable Advanced Settings with ```CTRL```+```SHIFT```+```ALT```+```F7```


## Boot Problems: Can load GRUB but can't load a Linux distro

See references [1] [2] [3].

This happened to me while installing ```Kubuntu 20.0.4 LTS```: GRUB loads fine but when attempting to launch an actual distro it sistematically fails.
This seems to be related to Thunderbolt issues. Proposed solutions include adding the ```pci=nommconf``` boot option in GRUB, but the only thing that actually worked for me was disabling temporarily Thunderbolt functionality.

Repo [2] tells you how to do it but probably references an old BIOS version. On ```T2ZF03060``` you need to access BIOS, enable advanced settings with ```CTRL```+```SHIFT```+```ALT```+```F7```, select ```Advanced``` tab, then ```Intel Advanced Menu / Platform Settings / TCSS Platform Setting / Thunderbolt(TM) Configuration```. You can disable Thunderbolt entirely setting ```Integrated Thunderbolt(TM) Support``` to ```Disabled```.
The live distro should boot normally now.

Newer kernel versions (namely 5.13/5.15) don't seem to be affected by this problem: after installing the OS and updating the kernel you can re-enable Thunderbolt support.

## Enabling Battery Care Limit
See ref [5] [6]
The ```lg-laptop``` kernel module exposes additional functionality for the LG Gram series, but most of them seem to work only under kernel version ```5.15```. 
You need to first update the kernel to update the kernel to ```5.15``` (see below).

Source [5] seems to be inaccurate: the battery care limit functionality in my case was not exposed under `/sys/class/power_supply/CMB0/charge_control_end_threshold` but under `/sys/devices/platform/lg-platform/battery_care_limit`

You can just write `80` there to stop charging when 80% is reached.
```
echo '80' | sudo tee /sys/devices/platform/lg-laptop/battery_care_limit
```
This value is reset at every reboot: it's advisable to write a custom service to handle setting at every restart. My own is included in this repo and you can copy it under `/etc/systemd/system/setbatterycarelimit.service`

then enable and start it
```
systemctl start setbatterycarelimit
systemctl enable setbatterycarelimit
```

## Updating Kernel to ```5.15``` in Ubuntu
At the time of writing ```5.15``` is in testing and not available to install from Ubuntu's main PPAs.
In the meantime you can install it from the Ubuntu Proposed PPA:

```
sudo add-apt-repository ppa:canonical-kernel-team/proposed
```
then update your repository list with ```sudo apt-get update```.

You can install `low-latency` or `generic`

```
sudo apt install linux-headers-5.15.*-*-generic linux-image-5.15.*-*-generic
OR
sudo apt install linux-headers-5.15.*-*-generic* linux-image-5.15.*-*-lowlatency
```

lastly
```sudo apt update && sudo apt upgrade```

## Installing a second SSD:

The 17Z90P has an unpopulated M.2 NVMe slot, you can easily access it by removing the 5 rubber pads and 4 plastic pads, removing the screws and just lifting the back plastic shell.

## References

- [1] [Ubuntu Forum thread on LG Gram 17 boot problems - Italian](https://forum.ubuntu-it.org/viewtopic.php?f=30&p=5301964)
- [2] https://github.com/crazyluv/solve-lg-gram-17-linux-boot-error
- [3] https://github.com/dhedlund/kernel-patch-lg-gram-17
- [4] [Upgrade Kernel to 5.15](https://www.linuxcapable.com/how-to-install-linux-kernel-5-15-on-ubuntu-20-04/)
- [5] [LG Gram Laptop Extra Kernel Features](https://www.kernel.org/doc/html/latest/admin-guide/laptops/lg-laptop.html)
- [6] [Limiting Battery Charge on LG Gram in Linux](https://www.lorenzobettini.it/2022/03/limiting-battery-charge-on-lg-gram-in-linux/)

## Contribute to this Repo
Just open an issue if you have any additional info you'd like to see here.
