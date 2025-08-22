> [!NOTE] 
> I made this a long time ago, when I knew much less.<br/>I also no longer use Windows or keep up with VMware.<br/>
> Therefore, I am archiving this.



# vmw17-arch-guest
Tips and Tweaks for creating a high performance Arch linux VM using VMware Workstation 17 on Windows 11 
 - config.ini file located in "C:\ProgramData\VMware\VMware Workstation" by default
 - VMX file is in VM folder. You can right click the VM in the sidebar to "Open VM Directory"

GLMark2 score: 4342 (Host is using an RTX 3080) 
 
Geekbench5 score: 1875/10016 (8 p-cores of a modestly clocked 12700K, within 5% of host score) 
 
In-guest disk speed: 3500MBPS read 2500MBPS write (on crucial P5+)
 
Desktop visual performance equivalent to virgl w/ libvirt 
 

Initial Conditions:
- Affinity set to only P-cores in taskmanager
- Hyper-V and associated features turned off
- PVSCSI (nvme) pre-allocated disk for arch root
- PVSCSI (nvme) independent-persistent disk for /var/cache/pacman/pkg
		- Common pkg cache between all the future snapshot forks of VM
		- I set it to uses nvme1:0 rather than nvme0:1 so it shows as seperate nvme dev instead of another namespace
- Archiso CD-ROM changed from IDE to SCSI
    - IDE was extremely slow (>10 minutes to login shell)
- Options>Advanced: Disable page trimming, select UEFI
- 3d acceleration enabled
- All CPU options selected > nested virtualization enabled (untested)
- "Other 5.X 64 Linux" selected as OS
- VMW-Workstation config.ini edited to use memory directly
  
- Some highlited VMX file parameters:
  	- vcpu.hotadd = "FALSE"
	- mem.hotadd = "FALSE" 
		- arch-wiki mentions problems with mem.hotadd
	- cpuid.numSMT = "2"
		- Enable hyperthreading awareness - set # vcpus equal to # threads
	- priority.grabbed = "high"
		- personal preference
	- mks.forceDiscreteGPU = "TRUE"
	- Possible keyboard lag fix
		- mks.keyboardFilter = "require"
	  	- keyboard.softAutorepeat = "FALSE"
	  	- keyboard.typematicMinDelay = "5"
		- 
 	- vmxnet.alwaysEnhanced = TRUE" 
	- ethernet0.virtualDev = "vmxnet3"
		- Defaults to e1000 when choosing "Other Linux"
	- DELETED after done with iso:
	    - scsi0.virtualDev = "pvscsi"
	    - scsi0.present = "TRUE"
      
Other VMX paremeters that may do nothing:
  - sched.cpu.affinity = "0-15"
  - mks.forceDiscreteGPU = "TRUE"
  - mks.enableVulkanPresentation = "TRUE"
  - mks.enableX11Presentation = "TRUE"
  - mks.maxRefreshRate = "120"
  - mks.vsync = "FALSE"
  - mks.useD3D = "TRUE"
  - svga.maxFullscreenRefreshTick = "120"
  - svga.maxChangeTick = "120"
  - svga.maxLocalChangeTick = "120"
  - svga.maxNoChangeTick = "120"
  - svga.showUpdateRate = "TRUE"
  - svga.debug.showFPS = "TRUE"

![image](https://user-images.githubusercontent.com/8879385/222939206-ef699cc5-0045-4d04-9757-0b6f3adc1bcc.png)
![image](https://user-images.githubusercontent.com/8879385/222939247-bbf5f23e-eb05-4466-b027-ab408c0e4f52.png)
![image](https://user-images.githubusercontent.com/8879385/222939228-e4e75d49-26c9-47c7-9981-414cc464c502.png)
![image](https://user-images.githubusercontent.com/8879385/222939237-67053530-f72e-47d8-9dbd-658cc2384207.png)



