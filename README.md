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

> wget -qO - https://raw.githubusercontent.com/linux-surface/linux-surface/master/pkg/keys/surface.asc \\<br>
    | gpg --dearmor | sudo dd of=/etc/apt/trusted.gpg.d/linux-surface.gpg

> echo "deb [arch=amd64] https://pkg.surfacelinux.com/debian release main" \\<br>
	| sudo tee /etc/apt/sources.list.d/linux-surface.list

> sudo apt update

> sudo apt install linux-image-surface linux-headers-surface iptsd libwacom-surface

> sudo systemctl enable iptsd

> sudo apt install linux-surface-secureboot-mok

Read details at https://github.com/linux-surface/linux-surface/wiki/Installation-and-Setup#debian--ubuntu 

15. Reboot

16. Select 'Enroll Mok' > OK / Yes
17. Enter 'surface' as password
18. Reboot and run:

> sudo update-grub

19. Reboot and run, to check if kernel information has '-surface' suffix:

> uname -a

Or install, run 'neofetch':

> sudo apt install -y neofetch

> neofetch

20. Download Linux Zoom App from https://zoom.us/download?os=linux , install Zoom App and dependencies:

> sudo apt install -y libglib2.0-0 libgstreamer-plugins-base1.0-0 libxcb-shape0 libxcb-shm0 libxcb-xfixes0 libxcb-randr0 libxcb-image0 libfontconfig1 libgl1-mesa-glx libxi6 libsm6 libxrender1 libpulse0 libxcomposite1 libxslt1.1 libsqlite3-0 libxcb-keysyms1 libxcb-xtest0 ibus

> sudo apt install ./zoom_amd64.deb

21. Set up camera driver

> sudo apt install -y git build-essential meson ninja-build pkg-config libgnutls28-dev openssl     python3-pip python3-yaml python3-ply python3-jinja2 qtbase5-dev libqt5core5a libqt5gui5 libqt5widgets5 qttools5-dev-tools libtiff-dev libevent-dev libyaml-dev gstreamer1.0-tools libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev

> git clone https://git.libcamera.org/libcamera/libcamera.git

> cd libcamera/

> meson build -Dpipelines=uvcvideo,vimc,ipu3 -Dipas=vimc,ipu3 -Dprefix=/usr -Dgstreamer=enabled

> ninja -C build

> sudo ninja -C build install

22. Set Up Python

> sudo apt install -y make build-essential python3 python-setuptools python3-pip python3-dev python3-venv libssl-dev libffi-dev libnss3 zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

23. Set Up Qt

> sudo apt-get --no-install-recommends install libqt\*5-dev qt\*5-dev qml-module-qtquick-* qt*5-doc-html libqt5xml5 libqt5x11extras5 libqt5x11extras5-dev

> cd $HOME

> mkdir playqt6

> cd playqt6

> python3 -m venv venv

> source venv/bin/activate

> pip3 install PySide6

> echo 'alias qt6examples="source $HOME/playqt6/venv/bin/activate && cd $HOME/playqt6/venv/lib/python3.10/site-packages/PySide6/examples"' >> $HOME/.bashrc

24. Install Microsoft Fonts

> sudo apt install -y ttf-mscorefonts-installer

25. Set Up UniqueBible

> cd

> git clone https://github.com/eliranwong/UniqueBible

> cd UniqueBible

> python3 uba.py

Open menu 'UniqueBible > Config Flag Window', search for and check 'enableSystemTrayOnLinux', restart UBA

26. Connect Android phone

Android phone:

Install and run 'KDE Connect'; select a folder and grant permission for file sharing

On Ubuntu, run either:

> sudo apt install kdeconnect nautilus-kdeconnect gnome-shell-extension-gsconnect gnome-shell-extension-manager

Launch Gnome Shell Extension Manager to enable 'GSConnect'

Pair Android phone with Ubuntu via 'KDE Connect' on Android phone.

# Switch to Xorg Session

Ubuntu 22.04 uses wayland by default, switch to Xorg Session to improve application compatablility.

> sudo nano /etc/gdm3/custom.conf

Uncomment the line "#WaylandEnable=false" by removing the "#" sign.

Note: moving window programmatically in Qt applications do not work on wayland, read https://forum.qt.io/topic/142043/pyside6-qmainwindow-move-not-working-on-ubuntu-22-04  Switch to Xorg session to work around the issue.

Instaall X11 gestures:

> sudo add-apt-repository ppa:touchegg/stable

enter sudo password

press "Enter"

> sudo apt update

> sudo apt install touchegg

To verify,

> systemctl status touchegg.service

Launch gnome shell extension manager > Browser > "X11 gestures"

Click "Install"

Read more about hand gestures on X11 at https://ubuntuhandbook.org/index.php/2022/06/touchpad-gestures-ubuntu-22-04-xorg/

# Useful Gnome Shell Extension

* GSConnect
* X11 gestures
* Clipboard Indicator
* Vitals
* Caffeine
* Application Menu
* CPU Power Manager
* Places Status Indicator
* Removable Drive Menu
* Improved Workspace Indicator
* Impatience
* Just perfection
* Sound input & output device chooser
* System monitor
* Hide Activities Button

# References

https://github.com/linux-surface/linux-surface/wiki/Installation-and-Setup

https://www.zdnet.com/article/how-i-put-linux-on-a-microsoft-surface-go-in-just-an-hour/

https://www.medo64.com/2020/03/surface-go-wifi-driver-package/

https://github.com/linux-surface/linux-surface/wiki/Camera-Support#build-libcamera-from-the-latest-git-source

https://9to5linux.com/you-can-now-install-linux-kernel-6-1-on-ubuntu-heres-how

https://updf.com/edit-pdf/ubuntu-pdf-editor/

https://vitux.com/how-to-edit-pdf-files-in-ubuntu/

https://superuser.com/questions/1705684/does-the-apple-magic-trackpad-2-work-on-linux

https://medium.com/@kaikoenig/apple-hardware-and-linux-964a4310bd4
