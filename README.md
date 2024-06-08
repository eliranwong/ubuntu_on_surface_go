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

24. Install Common Fonts

> sudo apt install fonts-dejavu* fonts-liberation* ttf-mscorefonts-installer fonts-crosextra-* fonts-takao-gothic fonts-opensymbol

Install Google Fonts

> wget https://github.com/google/fonts/archive/master.tar.gz -O gf.tar.gz

> sudo tar -xf gf.tar.gz --directory /usr/share

> sudo chown -R :users /usr/share/fonts-main

> sudo mkdir -p /usr/share/fonts/truetype/google-fonts

> sudo find /usr/share/fonts-main/ -name "*.ttf" -exec install -m644 {} /usr/share/fonts/truetype/google-fonts/ \\; || return 1

> rm -f gf.tar.gz

> sudo fc-cache -f && sudo rm -rf /var/cache/*

[Optional] Chinese Fonts

> sudo apt install xfonts-wqy ttf-wqy-zenhei ttf-wqy-microhei fonts-arphic-bkai00mp fonts-arphic-bsmi00lp fonts-arphic-gbsn00lp fonts-arphic-gkai00mp xfonts-intl-chinese xfonts-intl-chinese-big

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

> sudo apt install gnome-shell-extension-gsconnect gnome-shell-extension-manager

Related packages: kdeconnect nautilus-kdeconnect

Launch Gnome Shell Extension Manager to enable 'GSConnect'

Pair Android phone with Ubuntu via 'KDE Connect' on Android phone.

27. Support MP4 playback

> sudo apt install -y ubuntu-restricted-addons ubuntu-restricted-extras gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly

28. Set up ibus

Settings > Language and Region > Manage Installed Languages > Install / Remove Languages > "Chinese (Simplified)" and "Chinese (Traditional)"

> sudo locale-gen

> sudo apt install ibus-pinyin

> ibus restart

> ibus-setup

Settings > Keyboard > Input Sources > Add > Chinese (Intelligent Pinyin) > Preferences > General > Chinese > Traditional

> nano ~/.bashrc

export LC_CTYPE=zh_CN.UTF-8<br>
export XIM=ibus<br>
export XIM_PROGRAM=/usr/bin/ibus<br>
export QT_IM_MODULE=ibus<br>
export GTK_IM_MODULE=ibus<br>
export XMODIFIERS=@im=ibus<br>
export DefaultIMModule=ibus

# Switch to Xorg Session

Ubuntu 22.04 uses wayland by default, switch to Xorg Session to improve application compatablility.

> sudo nano /etc/gdm3/custom.conf

Uncomment the line "#WaylandEnable=false" by removing the "#" sign.

Note: moving window programmatically in Qt applications do not work on wayland, read https://forum.qt.io/topic/142043/pyside6-qmainwindow-move-not-working-on-ubuntu-22-04  Switch to Xorg session to work around the issue.

Do not use the touchegg package provided by Ubuntu.  It does not work with X11 gestures.

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
* Clipboard Indicator
* Vitals
* Caffeine
* Apps Menu
* Places Status Indicator
* Removable Drive Menu
* Replace Activities Text
* Improved Workspace Indicator
* Impatience
* Just perfection
* CPU Power Manager
* Sound input & output device chooser
* X11 gestures

# Set up office 365 Email

> sudo apt update && sudo apt dist-upgrade

> sudo apt install -y evolution evolution-ews

> evolution

1) New > Mail Account, enter "Full Name" and "Email Address"

2) uncheck "Look up mail server details based on the entered email address" and click "Next"

3) Server Type > Exchange Web Services > Host URL, enter:

> https://outlook.office365.com/EWS/Exchange.asmx

4) Authentication > Click "Check for Supported Types" and Keep "OAuth2 (Office365)"

5) Click "Fetch URL"

6) Enter username and password for authentication

7) Next > Next > Next > Apply

Alternately, read https://sites.utexas.edu/glenmark/2021/02/01/how-to-setup-your-office-365-email-using-evolution-ews-linux/#:~:text=Configuring%20Evolution%2DEWS%20to%20connect%20to%20Exchange%20Online&text=Enter%20your%20name%20and%20your,%2FEWS%2FExchange.asmx%20.

# Free Up Space

> sudo apt -s clean

> sudo apt --purge autoremove

> journalctl --disk-usage<br>
> sudo journalctl --vacuum-time=3d

> sudo apt install stacer<br>
> stacer

# Copy Shortcuts on Desktop

Copy favourite shortcuts files

from:

> /usr/share/applications/

> /var/lib/snapd/desktop/applications/

to:

> ~/Desktop/

Right-click each shortcut file, select "Allow Launching"

# Change Desktop to a Solid Color

Run in terminal:

> gsettings set org.gnome.desktop.background color-shading-type 'solid'
   
> gsettings set org.gnome.desktop.background picture-uri none
             
> gsettings set org.gnome.desktop.background picture-uri-dark none
           
> gsettings set org.gnome.desktop.background primary-color '#0D1117'

Alternately, use run gui app 'dconf-editor'

> sudo apt install dconf-editor

![background_highlighted](https://github.com/eliranwong/ubuntu_on_surface_go/assets/25262722/aae5a98d-ff64-4bba-9b29-a69bffeb4088)

![primary_color](https://github.com/eliranwong/ubuntu_on_surface_go/assets/25262722/15da96f1-1578-4115-a9ac-19f185ecf7d5)

# Install Waydroid

> sudo apt install curl ca-certificates linux-headers-generic

> curl https://repo.waydro.id | sudo bash

> sudo apt install waydroid dkms

> https://docs.waydro.id/usage/install-and-run-android-applications

# Fix Pulseaudio Playback Delay

> sudo nano /etc/pulse/default.pa

comment out 'load-module module-suspend-on-idle'

#load-module module-suspend-on-idle

> systemctl --user restart pulseaudio

Read more about trouble-shooting pulseaudio at https://wiki.archlinux.org/title/PulseAudio/Troubleshooting

# Replace PulseAudio with PipeWire on Ubuntu 22.04

The fix for PulseAudio, mentioned above, is not a satisfactory one.  Therefore, we find it better to replace PulseAudio with Pipewire on Ubuntu 22.04.

Check current status:

> systemctl --user status pipewire pipewire-session-manager

Set up:

> sudo add-apt-repository ppa:pipewire-debian/pipewire-upstream

> sudo add-apt-repository ppa:pipewire-debian/wireplumber-upstream

> sudo apt update && sudo apt dist-upgrade

> systemctl --user daemon-reload

> sudo apt install pipewire-audio-client-libraries libspa-0.2-bluetooth libspa-0.2-jack

> sudo apt install wireplumber pipewire-media-session-

NOTE: there’s a ‘-‘ in the end of the command indicates to remove the package. The command will also install the required pipewire-pulse automatically.

> systemctl --user --now enable wireplumber.service

Configure ALSA:

> sudo cp /usr/share/doc/pipewire/examples/alsa.conf.d/99-pipewire-default.conf /etc/alsa/conf.d/

Configure Jack:

> sudo cp /usr/share/doc/pipewire/examples/ld.so.conf.d/pipewire-jack-*.conf /etc/ld.so.conf.d/

> sudo ldconfig

Configure Bluetooth:

> sudo apt remove pulseaudio-module-bluetooth

Reboot to make all changes effective

> reboot

To verify, run:

> pactl info

Source: 

https://ubuntuhandbook.org/index.php/2022/04/pipewire-replace-pulseaudio-ubuntu-2204/

https://www.how2shout.com/linux/enable-pipewire-for-audio-and-bluetooth-in-ubuntu-22-04-or-20-04/

# Set up Remote Desktop Server

https://github.com/eliranwong/ubuntu_on_surface_go/blob/main/setup_remote_destkop_on_ubuntu.md

# HP Printer Drivers

First, install libcanberra-gtk-module

> sudo apt install libcanberra-gtk-module

Then, download hplip software and install printer driver:

https://developers.hp.com/hp-linux-imaging-and-printing/

Download: https://developers.hp.com/hp-linux-imaging-and-printing/gethplip

Installation instructions: https://developers.hp.com/hp-linux-imaging-and-printing/install/install/index

Run 'hp-setup' after restart:

> hp-setup

Remarks: Enter IP address manually if printer is not found. 

# Install Windows VM

https://ubuntu.com/tutorials/how-to-install-a-windows-11-vm-using-lxd#1-overview

https://fostips.com/install-incus-container-ubuntu-debian/

https://discussion.scottibyte.com/t/windows-11-incus-virtual-machine/362

https://blog.simos.info/how-to-run-a-windows-virtual-machine-on-incus-on-linux/

https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/

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
