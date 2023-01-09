# Setup Ubuntu on Surface Go

Notes about setting up Ubuntu on Surface Go

Device: Surface Go (gen 1; LTE version)

Install Ubuntu on Surface Go:

1. Download Ubuntu image file from https://ubuntu.com/#download
2. Download 'balenaEtcher' from https://www.balena.io/etcher/
3. Insert USB stick
4. Launch 'balenaEtcher', select the image iso file downloaded in step 1, select the USB stick inserted in step 3, and run
5. Go to the Windows Settings App and search "Recovery Options"
6. Select Advanced start-up > USB device > Linpus Lite
7. After re-boot, install Ubuntu, select 'install third-party softwares' as well (orient the surface go in portrait mode so that the 'continue' button is clickable)
8. Unplug the USB stick after Ubuntu is installed
9. Restart and launch Ubuntu
10. To set-up wifi, download https://www.medo64.com/download/surface-go-wifi_0.0.2_amd64.deb [with a computer which can go online], copy to Surface Go and run:

> sudo apt install ./surface-go-wifi_0.0.3_amd64.deb'

11. Restart, go to settings, select wifi network
12. Launch Terminal and run:

> sudo apt update && sudo apt dist-upgrade

type 'Y' and press 'Enter'

13. Restart computer
14. Install Surface Kernel:

> wget -qO - https://raw.githubusercontent.com/linux-surface/linux-surface/master/pkg/keys/surface.asc \<br>
    | gpg --dearmor | sudo dd of=/etc/apt/trusted.gpg.d/linux-surface.gpg

> echo "deb [arch=amd64] https://pkg.surfacelinux.com/debian release main" \<br>
	| sudo tee /etc/apt/sources.list.d/linux-surface.list

> sudo apt update

> sudo apt install linux-image-surface linux-headers-surface iptsd libwacom-surface

> sudo systemctl enable iptsd

> sudo apt install linux-surface-secureboot-mok

Read details at https://github.com/linux-surface/linux-surface/wiki/Installation-and-Setup#debian--ubuntu 

15. Reboot

16. Select 'Enroll Mok' > OK / Yes
17. Enter 'surface' as password
18. Reboot
19. Install, run 'neofetch' and check if kernel information shows 6.1.3-surface:

> sudo apt install -y neofetch

> neofetch



# References

https://github.com/linux-surface/linux-surface/wiki/Installation-and-Setup

https://www.zdnet.com/article/how-i-put-linux-on-a-microsoft-surface-go-in-just-an-hour/

https://www.medo64.com/2020/03/surface-go-wifi-driver-package/

