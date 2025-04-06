## Download
- Download the Ubuntu Desktop image
	- image contains the whole operating system - including an installer
	- create virtual machine, and let virtual box install ubuntu for us
- use the latest LTS version of ubuntu (usually updated every 6 months)
## Configuration
>- Need to install some drivers to work smoothly with
>	- enables features like drag and dropping files, copy paste clipboard, shared folder, etc
>- To do this:
>	- we need to update our system
>	- install tools required to compile the drivers (make them executable)
>	- install the drivers

### Make sure system is up-to-date
```bash
sudo apt update
sudo apt full-upgrade 
```
### For dragging and moving data between VM and laptop
```bash
#if something's updated, restart VM
sudo apt install build-essential linux-headers-generic dkms
```
- difference between [[update vs upgrade]]
- Next step
	- Devices > Inset Guest Additions Guest Image
	- A CD will appear, then execute the `VBoxLinuxAdditions.run` file through `sudo` (write sudo in the terminal, then drag and drop the file) 
- Restart Ubuntu VM
- Now Devices > Drag and Drop/ Shared clipboard/ Shared Folder, etc should work

- Shared folder permission
	- `sudo adduser $USER vboxsf`