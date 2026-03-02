1. Update your system
	- `sudo dnf update`
	- If new updates were installed (in particular regarding the kernel) → **Restart your virtual machine** 
2. Install additional packages
	- `sudo dnf install epel-release`
3. Update your system again
	- `sudo dnf update`
4. Install additional tools
	- `sudo dnf install gcc kernel-devel kernel-headers make bzip2 perl`
5. Install the Guest Additions
	- In VirtualBox menu bar: `Devices > Insert Guest Additions CD Image…`
	- Execute `VBoxLinuxAdditions.run`
6. Restart your virtual machine
	- Shared folder permission
		- ``sudo usermod --append --groups vboxsf $USER``
		- for me USER is `lkim`