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


## References

- [1] [Ubuntu Forum thread on LG Gram 17 boot problems - Italian](https://forum.ubuntu-it.org/viewtopic.php?f=30&p=5301964)
- [2] https://github.com/crazyluv/solve-lg-gram-17-linux-boot-error
- [3] https://github.com/dhedlund/kernel-patch-lg-gram-17
