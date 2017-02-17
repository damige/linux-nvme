# linux-nvme

Arch linux kernel plus NVME patches made by Andy Lutomirski 

These patches enable NVME drives to enter lower power states.
In my case (XPS13, linux-nvme4.9.0) it decreases idle usage by ~1.5watt (see Benchmarks file)

## Options:
#### Manually compile the Archlinux kernels, follow steps:

* git clone: https://github.com/damige/linux-nvme.git
* go into /src/[kernel you want]
* type "makepkg" -wait until compilation completes-
* pacman -U linux-nvme-*
* Adjust your bootloader to boot linux-nvme
<br />

#### AUR: linux-nvme
<br />
#### REPO: Add this to your /etc/pacman.conf <br />
`[linuxnvme]
SigLevel = Never
Server = http://linuxnvme.damige.net/repo`
<br />
#### Patch your own (non ARCH) kernel:
<br />
4.8:<br />
Patch using nvmepatch1-V4.patch, nvmepatch2-V4.patch, nvmepatch3-V4.patch.
<br />
4.9:<br />
Patch using APST.patch, pm_qos1.patch, pm_qos2.patch, pm_qos3.patch, nvme.patch
<br />
4.10:<br />
Patch using APST.patch
<br />
#### Download binary:
http://linuxnvme.damige.net/
<br />
### To test if the APST is working try:
<br />
install nvme-cli and: "nvme get-feature -f 0x0c -H /dev/nvme0"
Expected output is: "Autonomous Power State Transition Enable (APSTE): Enabled"
<br />
Check if "/sys/class/nvme/nvme0/power/pm_qos_latency_tolerance_us" exists 
<br />
Verify measurably lower power usage when ssd is idle
