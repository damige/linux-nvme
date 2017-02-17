# linux-nvme

Arch linux kernel plus NVME patches made by Andy Lutomirski 

These patches enable NVME drives to enter lower power states.
In my case (XPS13, linux-nvme4.9.0) it decreases idle usage by ~1.5watt (see Benchmarks file)

## Options to use:
#### Manually compile the Archlinux kernels, follow steps:

* git clone: https://github.com/damige/linux-nvme.git
* go into /src/[kernel you want]
* type "makepkg" -wait until compilation completes-
* pacman -U linux-nvme-*
* Adjust your bootloader to boot linux-nvme
<br />

#### Patch your own (optionally non ARCH) kernel:
<br />
4.8.x:<br />
Patch using nvmepatch1-V4.patch, nvmepatch2-V4.patch, nvmepatch3-V4.patch.
<br />
4.9.x:<br />
Patch using APST.patch, pm_qos1.patch, pm_qos2.patch, pm_qos3.patch, nvme.patch
<br />
4.10.x:<br />
Patch using APST.patch
<br />
#### AUR: linux-nvme
<br />
#### REPO: Add this to your /etc/pacman.conf <br />
```
[linuxnvme]
SigLevel = Never
Server = http://linuxnvme.damige.net/repo
```
<br />
#### Download Arch binary:
http://linuxnvme.damige.net/
<br />
<br />
<br />
### To test if the APST is working try:
<br />
install nvme-cli and: "nvme get-feature -f 0x0c -H /dev/nvme0"
Expected output is: "Autonomous Power State Transition Enable (APSTE): Enabled"
<br />
Check if "/sys/class/nvme/nvme0/power/pm_qos_latency_tolerance_us" exists 
<br />
Verify measurably lower power usage when ssd is idle
<br />
### Known issues:
In the latest patches the samsung SM951 (as used in the XPS 9350) has been disabled for NVME APST.
This is due to some reported instability.<br />
I have not included the patch disabling this nvme drive, as i use it myself. Incase you want the full APST patch its listed as APST-FULL.patch<br />
I have had a single lock i can attribute to the ssd in the last 60 days, for me the benifit of these patches outways the occasional issue with specifically the SM951.
