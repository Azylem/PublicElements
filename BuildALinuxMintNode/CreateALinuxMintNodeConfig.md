*Step 1*
**Linux Mint fresh install config with 500GB SSD**
1. Requires:
	A. Already configured Linux Mint boot USB stick with Etcher. 	Screenshot: `https://linuxmint-installation-guide.readthedocs.io/en/latest/burn.html`
	B. Node machine with 1x 500GB SSD installed
2. Boot into preconfigured USB stick
3. On live booted Linux Mint desktop, launch "Install Linux Mint" 	Screenshot: `https://linuxmint-installation-guide.readthedocs.io/en/latest/_images/cinnamon.png`
4. Select "install third-party software" 						Screenshot: `https://linuxmint-installation-guide.readthedocs.io/en/latest/_images/installer-codecs.png`
5. In "Installation Type" panel, select "Something else" 			Screenshot: `https://linuxmint-installation-guide.readthedocs.io/en/latest/_images/installer-install.png`
6. Wipe the entire SSD (delete all partitions with the "-" button)
7. Create 3 new partitions
	A. 1x EFI partition (1000 MB)
	B. 1x ext4 partition (30000 MB), mount point "/"
	C. 1x xfs partition (maximum remaining space), at mount point "/home"
8. Carry on with remaining installation steps, as desired, following the displayed instructions, then boot into the new OS with USB stick removed

*Step 2*
**Setup required machine config basics**
1. Right click on desktop, select "Open in Terminal" then enter the below commands in order
2. `wget https://golang.google.cn/dl/go1.19.4.linux-amd64.tar.gz`
3. `sudo tar -xvf go1.19.4.linux-amd64.tar.gz`
4. `sudo mv go /usr/local`
5. `sudo nano /etc/profile`
6. Scroll to the bottom of Nano and add these lines to the bottom:
	`export GOROOT=/usr/local/go`
	`export GOPATH=$HOME/`
	`export PATH=$GOPATH/bin:$GOROOT/bin:$PATH`
7. Press CTRL+O to save, then press Enter/Return key to confirm, then press CTRL+X to exit Nano
8. `sudo apt install git`
9. `sudo apt-get update && sudo apt-get upgrade`
10. `sudo apt-get install gcc libgl1-mesa-dev xorg-dev build-essential libc6`
11. `sudo apt install xfsprogs ntpdate`
12. `sudo apt install chrony`
13. `chronyc tracking`
14. `sudo echo "net.core.rmem_default=2097152" >> /etc/sysctl.conf; sudo sysctl -p`
15. `echo "ulimit -n 48000" >> ~/.bashrc ; echo "ulimit -n 48000" >> ~/.bash_profile ; ulimit -n 48000 ; source ~/.bashrc`
16. Restart the machine

*Step 3*
**Build Netrunner from source**
1. Create new folder on desktop called "Dero", open this folder then right click inside it and select "Open in Terminal", enter below commands in order.
2. `git clone https://github.com/DEROFDN/Netrunner`
3. `cd Netrunner`
4. `go mod tidy`
5. `go build .`
6. `./netrunner` or `./Netrunner` (depending on how the file is named) to launch Netrunner!

**Bookmark these URLs in Firefox, for additional information**
https://forum.dero.io/t/dero-node-setup/1774
https://github.com/deroproject/derohe/releases/
https://github.com/DEROFDN/netrunner
