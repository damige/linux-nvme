# linux-nvme

Arch linux kernel plus NVME patches made by Andy Lutomirski 

These patches enable NVME drives to enter lower power states.
In my case (XPS13, linux-nvme4.9.0) it decreases idle usage by ~1.5watt (see Benchmarks file)

To manually compile the Archlinux kernels from here, follow steps:

(1) git clone: https://github.com/damige/linux-nvme.git

(2) go into /src/[kernel you want]

(3) type "makepkg"
-wait until compilation completes-

(4) pacman -U linux-nvme-*

(5) Adjust your bootloader to boot linux-nvme
<br />
<br />
<br />
Also available on AUR: linux-nvme
<br />
<br />
To manually patch your own kernel:

4.8:
Patch using nvmepatch1-V4.patch, nvmepatch2-V4.patch, nvmepatch3-V4.patch.

4.9:
Patch using APST.patch, pm_qos1.patch, pm_qos2.patch, pm_qos3.patch, nvme.patch

4.10(testing):
Patch using APST.patch
<br />
<br />
<br />
To Download binary:
http://linuxnvme.damige.net/

To test if the APST is working try:

install nvme-cli and: "nvme get-feature -f 0x0c -H /dev/nvme0"
Expected output is: "Autonomous Power State Transition Enable (APSTE): Enabled"

Check if "/sys/class/nvme/nvme0/power/pm_qos_latency_tolerance_us" exists 

Verify measurably lower power usage when ssd is idle
