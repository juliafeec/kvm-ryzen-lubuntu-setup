I've recently finished setting up my first ever KVM configuration. It's all running on a Ryzen 1700X, X370 and 16GB of ram, with Lubuntu 18.04 as the host on a GT 610 and two VMs with Windows 10, each with its own GTX 1060 and SSD. The entire PC cost under USD$900 after shipping and taxes, with all parts bought on this past Black Friday and the days leading up to it. Everything is new, except for the 3 GPUs, which were bought refurbished from Zotac.

I'm gonna go over some of the issues I ran into and the solutions, and maybe this will be useful for someone doing a similar setup. At the very least this will be a reference for myself in the future. 

The motivation for all this is that two relatives of mine were looking to build gaming desktop PCs. They live together, and planned on getting PCs with Nvidia Geforce 1060s, and hoped to spend as little as possible. This got me thinking about how wasteful it seemed to have two identical desktops in the same place and how, aside from the GPU, it seemed like everything else could be shared. I searched for solutions for sharing a PC to be used simultaneously by two independent sessions and saw Linus' youtube video of [2 gaming rigs 1 tower](https://www.youtube.com/watch?v=LuJYMCbIbPk). This was almost exactly what I needed, except that I needed to do it with a cheap CPU and Motherboard, and I hoped to do it with free software instead of having to buy Unraid. Ryzen came to mind. I also wanted to do this with identical GPUs, namely, the cheapest 1060 6GB model we could find. This makes it slightly more complicated. More on that later.

****

### Hardware

Below is my part list (I've edited the Video Cards' markdown manually as pcpartpicker doesn't support multiple GPUs of different models). The only thing without a price below is the case, as it was bought elsewhere in another currency. Anything that fits the components would work though, and you should be able to find that for under $50. One option we considered was [this Raijintek Arcadia](https://www.newegg.com/Product/Product.aspx?Item=2AM-002C-00003), which was going for about $30 on Black Friday.
For virtualization purposes, I guess the critical parts are just the CPU, Motherboard and GPUs.

[PCPartPicker part list](https://pcpartpicker.com/list/HhLczY) / [Price breakdown by merchant](https://pcpartpicker.com/list/HhLczY/by_merchant/)

Type|Item|Price
:----|:----|:----
**CPU** | [AMD - Ryzen 7 1700X 3.4 GHz 8-Core Processor](https://pcpartpicker.com/product/9Q98TW/amd-ryzen-7-1700x-34ghz-8-core-processor-yd170xbcaewof) | Purchased For $152.95 
**CPU Cooler** | [Deepcool - GAMMAXX 400 74.34 CFM CPU Cooler](https://pcpartpicker.com/product/hJFPxr/deepcool-cpu-cooler-gammaxx400) | Purchased For $16.10 
**Motherboard** | [MSI - X370 GAMING PLUS ATX AM4 Motherboard](https://pcpartpicker.com/product/dCx9TW/msi-x370-gaming-plus-atx-am4-motherboard-x370-gaming-plus) | Purchased For $99.47 
**Memory** | [Crucial - Ballistix Sport AT 16 GB (2 x 8 GB) DDR4-3000 Memory](https://pcpartpicker.com/product/VNDJ7P/crucial-ballistix-sport-at-16gb-2-x-8gb-ddr4-3000-memory-bls2k8g4d30cestk) | Purchased For $97.64 
**Storage** | [PNY - CS900 240 GB 2.5" Solid State Drive](https://pcpartpicker.com/product/dF448d/pny-cs900-240gb-25-solid-state-drive-ssd7cs900-240-pb) | Purchased For $37.39 
**Storage** | [PNY - CS900 240 GB 2.5" Solid State Drive](https://pcpartpicker.com/product/dF448d/pny-cs900-240gb-25-solid-state-drive-ssd7cs900-240-pb) | Purchased For $37.39 
**Video Card** | [Zotac - GeForce GTX 1060 6GB 6 GB Mini Video Card](https://pcpartpicker.com/product/Ft7CmG/zotac-geforce-gtx-1060-6gb-mini-video-card-zt-p10600a-10l) | Purchased For $141.33 
**Video Card** | [Zotac - GeForce GTX 1060 6GB 6 GB Mini Video Card](https://pcpartpicker.com/product/Ft7CmG/zotac-geforce-gtx-1060-6gb-mini-video-card-zt-p10600a-10l) | Purchased For $141.33
**Video Card** | [Zotac - GeForce GT 610 2 GB Video Card](https://pcpartpicker.com/product/VvrG3C/zotac-video-card-zt6060110l) | Purchased For $15.17
**Case** | [SHARKOON - M25-V ATX Mid Tower Case](https://pcpartpicker.com/product/FrFXsY/sharkoon-m25-v-atx-mid-tower-case-m25-v) |-
**Power Supply** | [EVGA - SuperNOVA G1+ 750 W 80+ Gold Certified Fully-Modular ATX Power Supply](https://pcpartpicker.com/product/xTMwrH/evga-supernova-g1-750w-80-gold-certified-fully-modular-atx-power-supply-120-gp-0750-x1) | Purchased For $63.90 
**Case Fan** | [Cooler Master - R4-S2S-124K-GP 44.73 CFM 120mm Fans](https://pcpartpicker.com/product/L8DwrH/cooler-master-case-fan-r4s2s124kgp) | Purchased For $9.76 
 **Other** | [Samsung 32GB FIT Plus USB 3.1 Flash Drive, Speed Up to 200MB/s](https://pcpartpicker.com/product/KqXnTW/samsung-32gb-fit-plus-usb-31-flash-drive-speed-up-to-200mbs) | Purchased For $8.67 
 | *Prices include shipping, taxes, rebates, and discounts* |
 | **Total** | **$821.10**
 | Generated by [PCPartPicker](https://pcpartpicker.com) 2019-01-07 09:18 EST-0500 |
 
****

### Basics

Now for the actual setup:
I've read that Ubuntu is not the easiest to setup for KVM, but I found some great guides and I'm most comfortable with Debian-based systems so that's why I chose it. I'm using Lubuntu 18.04.1 because it's LXDE-based so it's very lightweight. I still wanted to have some graphical interface instead of running fully headless because it's simpler to troubleshoot and change configurations.

For the most part, I followed these two great guides:

[AMD Ryzen based passthrough setup between (X)Ubuntu 16.04 and Windows 10](http://mathiashueber.com/amd-ryzen-based-passthrough-setup-between-xubuntu-16-04-and-windows-10)

[Ubuntu 17.04 – VFIO PCIe Passthrough & Kernel Update (4.14-rc1)](https://forum.level1techs.com/t/ubuntu-17-04-vfio-pcie-passthrough-kernel-update-4-14-rc1/119639)

I should note that I installed Lubuntu on the Samsung 32GB USB drive using another bootable USB drive containing the Lubuntu iso (which is only about 1GB).
As of 18.04.1, the kernel is already 4.15 so it's not necessary to update it.
I uninstalled AppArmor using the info in the second link. It's certainly not required, but to keep it, it needs to be configured properly. I planned on reinstalling it after I got everything working, to reduce the number of things to deal with while configuring KVM for the first time, but in the end I never did.

The xml configuration files for both VMs ([gpu1.xml](https://github.com/juliafeec/kvm-ryzen-lubuntu-setup/blob/master/gpu1.xml) and [gpu2.xml](https://github.com/juliafeec/kvm-ryzen-lubuntu-setup/blob/master/gpu2.xml)) are made available in this repo.

****

### IOMMU Groups

 For my motherboard and processor, I also didn't need to apply the ACS kernel patch. I updated the motherboard BIOS to the latest version and the primary and secondary GPUs were already in their own IOMMU groups. Additionally, one of the USB Controllers (the one for the 3.0 ports on the back) was in its own IOMMU group as well. I passed it through to one of the VMs. Maybe the ACS kernel patch would separate an additional USB Controller in another IOMMU group, which would allow it to be passed through for the other VM, but I didn't test it. For the second VM, the USB devices are passed through individually in the configuration, which means no hot-plugging of devices. In the future, another option is trying this script for hot-plugging based on udev rules: <https://github.com/olavmrk/usb-libvirt-hotplug>
  
****

### Dealing with identical GPUs

Because I had identical GPUs, instead of following the instructions in the above guides for separating them, I instead followed [this post](https://forums.linuxmint.com/viewtopic.php?f=231&t=212692&start=60#p1195606) which deals with this scenario. I expected this part to be more complicated, but following those instructions it was actually very straightforward. 

In my motherboard, it wasn't possible to boot from the host GPU. I had to boot from one of the 1060s (more on that in the next section). Because of this, before I applied these settings, I used nvidia-settings to change my X11 configuration to use the GPU in the third slot (the GT610). I'm not sure if this is needed, since the vfio drivers take precedence over the nvidia drivers, Xorg might default to the third GPU by itself because it is the only one it finds, but I didn't test without this.

Since I'm passing through two GPUs, and each GPU also has its own audio device, I have 4 device ids, that I found using `lspci -nnk`. This is what my `/sbin/vfio-pci-override-vga.sh` looked like:

```
#!/bin/sh

DEVS="0000:26:00.0 0000:26:00.1 0000:27:00.0 0000:27:00.1"

for DEV in $DEVS; do
    echo "vfio-pci" > /sys/bus/pci/devices/$DEV/driver_override
done

modprobe -i vfio-pci

echo 0 > /sys/class/vtconsole/vtcon0/bind
echo 0 > /sys/class/vtconsole/vtcon1/bind
echo efi-framebuffer.0 > /sys/bus/platform/drivers/efi-framebuffer/unbind
```

And my `/etc/modprobe.d/local.conf`:
```
install vfio-pci /sbin/vfio-pci-override-vga.sh
```

See GRUB config in the next section.
****

### Issues with pass-through of primary GPU

I should note is that sadly the X370 Gaming Plus doesn't let you choose which GPU slot to boot from, it always boots from the first slot (If you have integrated graphics, you can choose to boot from it instead, but it's not an option with the 1700X). And the slots are x8/x8/x4 in that order, meaning the two 1060s had to be in the first two slots in order to not be bottlenecked. As a result, this means that the primary (boot) GPU will need to be passed-through, which is a similar scenario as when you're running the host headless.

The first issue I ran into was not being able to pass-through the primary GPU because part of its memory was occupied. In `dmesg`, I was getting the "Failed to mmap" error. I saw in `sudo cat /proc/iomem` that `vesa` was showing up under my primary GPU. I solved this using `vesa:off` and `vesafb:off`. After adding those, this is what my line looks like in /`etc/default/grub`: (it's likely that not all of those settings are required)
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nomodeset amd_iommu=on nofb nouveau.modeset=0 rd.driver.blacklist=nouveau video=vesafb:off,vesa:off,efifb:off vga=normal"
```

At this point I was able to successfully launch a Ubuntu VM on GPUs 1 and 2. But a Windows VM would only launch on GPU 2, not on GPU 1. See the next two sections.

****

### Other things not covered in depth

I won't write about all the configurations because I think most things are already very clear in the guides I linked at the beginning. I will just add some reminders:

- Error 43 in NVIDIA GPUs
Error 43 can be caused by many things. But first of all you need to make sure you handle the usual Error 43, which is due to the fact that NVIDIA drivers for consumer GPUs (such as GeForce) don't support running in a virtual machine. You need to edit the VM's xml files to hide that it's running in KVM. [Here is a guide.](http://mathiashueber.com/fighting-error-43-how-to-use-nvidia-gpu-in-a-virtual-machine/)

- Raw disks
Instead of using images, using raw disks can give better performance. I ran a benchmark using SATA and VirtIO, and VirtIO was in fact faster. [This is explained in the Level1Tech guide](https://forum.level1techs.com/t/ubuntu-17-04-vfio-pcie-passthrough-kernel-update-4-14-rc1/119639). In my case, one VM had `/dev/sda` and the other had `/dev/sdb` (which correspond to the two SSDs), like so:

```
<disk type='block' device='disk'>
      <driver name='qemu' type='raw' cache='none' io='native'/>
      <source dev='/dev/sda'/>
      <target dev='vda' bus='virtio'/>
      <boot order='1'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x0'/>
    </disk>
```

- CPU pinning
[Here is a very detailed guide](http://mathiashueber.com/cpu-pinning-on-amd-ryzen/). I wondered what would happen if I didn't pin the CPUs. 
As a test, I ran a single VM using 8 cores without pinning, and looked at `htop` on the Lubuntu host. I could see the usage spikes all over the 16 cores of the 1700X. Due to the Ryzen architecture, I'm pinning cores 0-7 to one VM and 8-15 to the other. Like so:

```
  <cputune>
    <vcpupin vcpu='0' cpuset='0'/>
    <vcpupin vcpu='1' cpuset='1'/>
    <vcpupin vcpu='2' cpuset='2'/>
    <vcpupin vcpu='3' cpuset='3'/>
    <vcpupin vcpu='4' cpuset='4'/>
    <vcpupin vcpu='5' cpuset='5'/>
    <vcpupin vcpu='6' cpuset='6'/>
    <vcpupin vcpu='7' cpuset='7'/>
    <emulatorpin cpuset='0-1'/>
    <iothreadpin iothread='1' cpuset='0-1'/>
  </cputune>
```
  
and in the other VM:
  
```
   <cputune>
    <vcpupin vcpu='0' cpuset='8'/>
    <vcpupin vcpu='1' cpuset='9'/>
    <vcpupin vcpu='2' cpuset='10'/>
    <vcpupin vcpu='3' cpuset='11'/>
    <vcpupin vcpu='4' cpuset='12'/>
    <vcpupin vcpu='5' cpuset='13'/>
    <vcpupin vcpu='6' cpuset='14'/>
    <vcpupin vcpu='7' cpuset='15'/>
    <emulatorpin cpuset='0-1'/>
    <iothreadpin iothread='1' cpuset='0-1'/>
  </cputune>
```

I didn't set up Hugepages because it wasn't clear if it actually made a difference. But here's a link to a discussion about this:
<https://www.reddit.com/r/VFIO/comments/6p7p6n/whats_the_advantage_of_static_hugepages/>

****

### Pass-through of primary GPU - Windows Error 43

Once you've dealt with the usual Error 43 as mentioned in the previous section, you might encounter another Error 43 that is caused by a different factor.
At this point, I was able to boot into a Ubuntu VM on either GPU, but Windows would only boot in GPU 2. By the way, the only way I could even see Error 43 at all was by temporarily using `Display Spice` in this VM. As in, I would see the Windows VM in this virtual display shown in the host (Lubuntu) display connected to the third GPU. Because the primary GPU would not display anything at all on the monitor this case. Then by looking at the device manager in Windows, I could see the 1060 GPU with a warning sign and Error 43.

There is an issue that happens with Pascal NVIDIA cards. I'm not sure if it happens with others as well. As it turns out, for reasons I don't completely understand, booting a GPU somehow changes the primary GPU's BIOS. And then when it is passed-through to a Windows VM, it gives the error (even though when it is passed-through to a Ubuntu VM it boots as usual). The solution to this is actually kind of simple, but figuring out what the issue was took me quite some time.

Anyway, the way to fix this is to pass an unaltered copy of the BIOS to the VM, allowing the GPU to be used properly. One way to get this unaltered copy is to dump the BIOS yourself while the GPU is not being used as the primary boot GPU (you could move it to a secondary slot, for example, and then dump it). [There is a way that doesn't require you to move your GPU though, thanks to this great script that patches the altered ROM and retrieves the original version for the first generation of Pascal cards, such as the 1060.](https://www.reddit.com/r/VFIO/comments/6qyg64/i_made_a_python_script_to_patch_nvidia_pascal/)
I dumped the primary GPU BIOS myself using CPU-Z in Windows, and then patched it with the above script, and it worked like a charm. I also found that the BIOS I dumped myself (*before* patching) was identical to the one I found on [TechPowerUp](https://www.techpowerup.com/vgabios/) for my model. So I guess you can try to download it from there, patch it and use it on your VM, and if it works you don't even have to dump it yourself. I should mention that the size of my patched ROM (`/usr/share/kvm/GP106_patched.rom`) was 127K, while the dumped version before patching was almost twice that. If you find a version that is around that size, it might already be the original unaltered version.

Here's the relevant portion of my configuration:

```
    <hostdev mode='subsystem' type='pci' managed='yes'>
      <driver name='vfio'/>
      <source>
        <address domain='0x0000' bus='0x26' slot='0x00' function='0x0'/>
      </source>
      <rom file='/usr/share/kvm/GP106_patched.rom'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x05' function='0x0'/>
    </hostdev>
```

****

### Pass-through of identical USB devices

In my case, I had two identical mouses and keyboards. USB devices are often assigned based on Vendor / Product, which in this case is a problem because it isn't unique. A solution to this is to use Address Bus and Device instead. The downside is that if the devices are moved to another port, that information will change. So in that case you need to make sure they stay on the same place. More on this:
<https://forums.unraid.net/topic/39966-vm-usb-passthrough-multiple-devices-with-the-same-vendorproduct>

I found that you don't need to edit the XML manually for this work though, it can also work using the Virtual Manager GUI. However, in that case, the duplicate devices need to be present and appear to the host when they are added to the VM. That is, if when you select a USB device on the GUI, it is the only such device appearing in the list, the GUI will add it as Vendor/Product. But if two or more are showing up, the GUI will add if correctly as Address Bus/Device.

****

### Windows shutdown

In the Virtual Manager GUI, there's the option to shutdown your VM. I found that this was never working for Windows unless I did a forced shutdown. It turns out, it's because Windows asks for a confirmation, that the Virtual Manager doesn't do. [A solution is to remove this confirmation from Windows when you shutdown or restart.](https://www.techworm.net/2018/03/how-to-shutdown-restart-windows-10-without-prompts.html)

****

### Setting up a network bridge

Many guides provide instructions to set-up a virtual network (its default name is usually virbr0). This works fine if you just want the VMs to have internet access, but they become inaccessible to other pcs in the network (though they can reach each other). I wanted the Windows VMs to show up in the router as regular devices, with the host being transparent. This would be useful for things such as using Steam Link (see next section). 

So this meant setting up a network bridge. To do this, I followed this guide:

<https://www.techotopia.com/index.php/Creating_a_CentOS_6_KVM_Networked_Bridge_Interface#CentOS_6_Virtual_Networks_and_Network_Bridges>

Though it is for CentOS, the GUI configuration is exactly the same.

I only had to follow the instructions in sections **Creating a CentOS 6 Network Bridge using virt-manager** and **Configuring a Virtual Machine to use the Network Bridge**.

****

### Steam Link

Once the network bridge was setup, using Steam Link was very simple. Using Steam Link also means that the VMs can be used in different rooms!
One room would have the PC tower and the monitor connected to it as usual using VM 1, and the other room would have a Steam Link, connected to a monitor, keyboard and mouse, using VM 2. And as a bonus, this also means that the issue of duplicate mouses / keyboards does not have to be dealt with at all, since one mouse and one keyboard would be connected directly to Steam Link, leaving a unique mouse and keyboard physically connected to the PC in the first room.
We didn't test this extensively, only briefly, with spotty wi-fi. It worked, but the wi-fi connection to the Steam Link kept dropping, so it wasn't a great experience. But I believe this should work great over ethernet, and perhaps it could work well over a stable wi-fi connection as well.
Unfortunately, Steam Link doesn't natively work with usb drives, but there seems to be a plugin that makes it work with FAT usb drives. In this multi-room setup, I think a possible solution to hot-plug usb drives in the second room would be to use something like a raspberry pi or a router that would expose the usb drive to the network.

****

### Telegram bot for remotely starting / shutting down VMs

I also wanted a simple solution for shutting down / starting the VMs, checking their status and shutting down the host (which should only be done when both VMs are off).
Since everything is in DHCP, meaning there are no fixed IPs, and I wanted a very user friendly setup, I thought a Telegram Bot would be a good solution.

Here's the documentation for Telegram's python wrapper:
<https://github.com/python-telegram-bot/python-telegram-bot>

It isn't the safest solution in the world because I'm not doing any checks and in theory anyone could find the bot and send it a command to shutdown a VM, but the odds of random person finding the bot and sending it a valid command are small enough that I'm not overly concerned. They wouldn't be able to do anything more than shutting it down, even in the worst case scenario.

The relevant part of my code is below, and it's based on the very simple [echobot.py example in github](https://github.com/python-telegram-bot/python-telegram-bot/blob/master/examples/echobot.py).

```python
def do_action(text, date):
    now = datetime.datetime.now()
    secs = (now - date).seconds
    if secs > 5:
        return "timeout"
    text = text.lower().strip()
    if text == "status":
        output = subprocess.check_output("/usr/bin/virsh list --all", shell=True)
        return output
    elif text == "shutdown gpu1":
        output = subprocess.check_output("/usr/bin/virsh shutdown gpu1", shell=True)
        return output
```

And I have something like the following in `sudo crontab -e` to make it start-up on boot.

```
SHELL=/bin/bash
PATH=/home/host/miniconda/envs/py2/bin:/usr/bin/env:/bin:$PATH

@reboot sleep 60 && python /home/host/telegram_bot.py &>> /home/host/bot.log
```
 The sleep command is because it takes a while for the network bridge to start working. I also made some changes in the python script that waits until a successful connection is established, because in the original example, if the first connection attempt fails, the script exits with an error.

****

### Benchmark

Before setting up KVM, I directly booted into Windows to have a baseline benchmark, so I could later compare it to the virtualized Windows and see if there was too much of a  performance drop.

I ran 3DMark's Time Spy benchmark, without any overclock, so the 1700X and the Zotac 1060 6GB were running at stock speeds. In this non-virtualized case, the available memory was the total 16GB, and obviously all cores of the 1700X were available. This was the result:


After everything was set-up, I ran Time Spy again on both Windows VMs **at the same time**. This ensured that both VMs were under load, so I could test the worst case scenario. In this case, each VM had just under 8GB of RAM (some of it is left for the host), and 8 (half) of the cores of the 1700X. Everything was still at stock speeds. This was the result:

| **Before KVM** | **After KVM** |
|:---:|:---:|
| ![TimeSpyBefore](https://github.com/juliafeec/kvm-ryzen-lubuntu-setup/blob/master/images/timespy_before.png?raw=true)  | ![TimeSpyAfter](https://github.com/juliafeec/kvm-ryzen-lubuntu-setup/blob/master/images/timespy_after.png?raw=true)|


So we can see that the GPU score is practically unchanged, reaching bare metal performance. And the CPU score is roughly half of before, which makes sense since half of the cores are available. In practice, the performance when gaming seems identical, since most of the work is done by the GPU, and games are typically single-core bound (and single-core performance is almost the same in the VM, only the multi-core performance inevitably takes a hit due to having fewer cores).

And this can definitely be improved even further with overclocking.