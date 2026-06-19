# ubuntu-gaming-settings

Optimization of Ubuntu for gaming
This repository provides a set of information, files, and scripts in order to optimize the performance of Ubuntu for gaming. With these settings, users can expect a performance increase that is equal to the performance of Arch Linux and CachyOS, both known their speed.

Settings are done under /etc/sysctl.d and /etc/udev/rules.d
/etc/sysctl.d/70-performance.conf sets performance settings for gaming.
/etc/udev/rules.d/60-ioscheduler.rules sets relevant IO scheduler depending on the block device. Inspired by CachyOS settings (with thanks).

Other optimizations:

1- use btrfs filesystem
Not a must but btrfs is a modern filesystem that enables snapshots and also flexible tunings.

2- etc/fstab:
UUID=<device-uuid> /              btrfs   subvol=/@,noatime,compress=zstd:5,ssd,autodefrag,discard=async,space_cache=v2,commit=60 0 0
UUID=<device-uuid> /home          btrfs   subvol=/@home,noatime,compress=zstd:5,ssd,autodefrag,discard=async,space_cache=v2,commit=60 0 0

Examples above for / and /home.
Settings for btrfs. If your filesystem is ext4 and such, you cannot use subvol, compress, ssd, discard tunings. 
You may try "lazytime" as well instead of noatime; it offers the same performance in most settings. Try to avoid relatime unless you do not use your PC for other purposes as well (such as server and such).

3- etc/default/grub:
GRUB_CMDLINE_LINUX_DEFAULT='quiet splash processor.max_cstate=1 pci=pcie_bus_perf numa=off idle=nomwait mitigations=auto,nosmt transparent_hugepage=always amdgpu.ppfeaturemask=0xffffffff'

These settings are for AMD CPU and GPU. GPU setting is for overclocking. You may then install LACT via Flatpak or ppa respository for OC settings. For Intel CPUs, adjust processor.max_cstate=1 for its Intel counterpart.

4- install gamemode 
sudo apt install gamemode 

gamemode is a good app for gaming performance. On Steam, remember to enter gamemoderun %command% in the command field so that gamemode can run with Steam games.

5- Feel free to try sched-ext userspace schedulers.
Frankly, the best performance are scx_bpfland and scx_lavd for gaming. Though,they are not faster than EEVDF in my own experience; they are not slower, either. To download and install sched-ext rust schedulers, you may visit my other power-sched-ext repository, or you can install scx-manager and sched-ext schedulers from Github. My repository combines power management and schedulers.

6- try to install irqbalance as service
sudo apt install irqbalance
suso systemctl enable --now irqbalance.service

Please note, in some systems, it may cause overhead. Test it and uninstall in case your performance does not get any better.

---
Note: That XFCE4 or any other lightweight DE offers faster gaming is not necessarily true. With power house machines, KDE and GNOME provide quite strong performance also in Wayland - in my own experience, Wayland has been offering better gaming performance lately. 

I am not a person who plays games too often. And my benchmarks are generally done in Cyberpunk 2077.
With these settings, Ubuntu is even a bit faster than CachyOS and Arch Linux; but the difference is within the margin errors, and it is not representative since one game does not offer a certain verdict. Cyberpunk heavily uses the CPU as well, and those game that use GPU mainly can offer different performance benchmarks.

All in all, these settings are based on some research but they are one person's experiments. Try to optimize based on your own system in case you feel enthusiastic.
The settings above are done and run in stock Ubuntu kernel(s). It is not Xanmod, Liquorix etc. which in my case did not offer any better result.
Also, such settings would optimize LLM performance in case you run local large language models in LM Studio or Ollama. 
My system has Gen4 NVMe SSD, AMD RX 9070XT, and 96GB DDR5 RAM.

Best wishes.
