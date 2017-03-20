# linux-nvme

This github repository contains: <br /> 
* NVME kernel patches for linux made by Andy Lutomirski. <br /> 
* PKGBUILD for linux-nvme on Arch maintained by me. <br /> 

These patches enable NVME drives to enter lower power states.<br />
For example: my case (XPS13, linux-nvme4.9.0) it decreases idle usage by ~1.5watt (see Benchmarks file)<br />
<br />

## NEW INFO: I have built 4.11rc1 and it seems this patch has made mainline!
It seems that the patch to disable SM951 has been merged also:

```
static const struct nvme_core_quirk_entry core_quirks[] = {
	/*
	 * Seen on a Samsung "SM951 NVMe SAMSUNG 256GB": using APST causes
	 * the controller to go out to lunch.  It dies when the watchdog
	 * timer reads CSTS and gets 0xffffffff.
	 */
	{
		.vid = 0x144d,
		.fr = "BXW75D0Q",
		.quirks = NVME_QUIRK_NO_APST,
	},
```
<br />

## 5 Ways to install:
#### 1) ARCH manually compile kernels: (EASY/SLOW)

* git clone: https://github.com/damige/linux-nvme.git
* go into /src/[kernel you want]
* type "makepkg" -wait until compilation completes-
* pacman -U linux-nvme-*
* Adjust your bootloader of choice to boot linux-nvme
<br />

#### 2) non-ARCH/ARCH Patch kernel of choice: (HARD/SLOW)
4.8.x:<br />
Patch using nvmepatch1-V4.patch, nvmepatch2-V4.patch, nvmepatch3-V4.patch.
<br />
4.9.x:<br />
Patch using APST.patch, pm_qos1.patch, pm_qos2.patch, pm_qos3.patch, nvme.patch
<br />
4.10.x:<br />
Patch using APST.patch
<br />
#### 3) ARCH AUR:(EASY/SLOW)
* use AUR helper of choice to install "linux-nvme"
* Adjust your bootloader of choice to boot linux-nvme
<br />

### If you choose to trust me compiling it for you:
<br />
#### 4) ARCH REPO: (EASY/FAST)
<br />
* Add to your /etc/pacman.conf
<br />

```
[linuxnvme]
SigLevel = TrustAll
Server = http://linuxnvme.damige.net/repo
```

<br />

* Update db and install with:
<br />

```
pacman -Sy
pacman -S linuxnvme/linux-nvme linuxnvme/linux-nvme-headers linuxnvme/linux-nvme-docs
```

<br />
My GPG key (for reference) is: A6255D31F80BEC97
<br />

* Adjust your bootloader of choice to boot linux-nvme
<br />

#### 5) ARCH binary download: (EASY/FAST)
* Download: http://linuxnvme.damige.net/kernels/
* Verify sums: https://github.com/damige/linux-nvme/blob/master/compiled/sums
<br />

* install with:
<br />

```
pacman -U linux-nvme-*
```
<br />

* Adjust your bootloader of choice to boot linux-nvme
<br />
<br />

### To test if the APST is working try:
<br />

* install nvme-cli and: "nvme get-feature -f 0x0c -H /dev/nvme0" Expected output is: "Autonomous Power State Transition Enable (APSTE): Enabled"
<br />

* Check if "/sys/class/nvme/nvme0/power/pm_qos_latency_tolerance_us" exists 
<br />

* Verify measurably lower power usage when ssd is idle
<br />

### Known issues:
In the latest patches the samsung SM951 (as used in the XPS 9350) has been disabled for NVME APST.
This is due to some reported instability.<br />
I have not included the patch disabling this nvme drive, as i use it myself. Incase you want the full APST patch its listed as APST-FULL.patch
<br />

### TODO:
* None?
