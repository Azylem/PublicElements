# Azylem's easy home (not public) dedicated personal Dero node guide, using Netrunner, Linux Mint and low cost used hardware
These are what I set up in people's houses :), so they can access Dero easily and privately in their homes. These machines can also be set up for other light duty things while in operation, like as a media center or dedicated firewall.

# Step 1
1. Requires:

	* Already configured Linux Mint boot USB stick with Etcher.
	[Further information](https://linuxmint-installation-guide.readthedocs.io/en/latest/burn.html)

	* Example low cost node hardware for around $200 on eBay (any box configured with 1x 500GB SSD (NOT AN HDD, SSD only), 8GB Ram, 4c/8t CPU: i7 4770 or i7 4790 only for this build price range)
		* HP Z230 SFF
		* HP EliteDesk SFF
![screenshot](hpboxes.png)
	* OPTIONAL: Virtual Display Adapter/Displayport Dummy Plug around $8 on eBay (for headless operation with VNC or other SSH)

	NOTE: for best performance, newer 8c/16t+ (Ryzen) CPUs are suggested and use [this official guide](https://forum.dero.io/t/dero-node-setup/1774) instead, but for the sake of low cost and easy (but still secure and effective setup for home access to the Dero network), these examples and steps (while a bit rough) have worked for me on multiple machines, for now.

	NOTE 2: I do not use the built-in miner with Netrunner on these boxes (personal choice), as they are not an ideal construction for heat dissipation (but it's your node, you can do whatever you want!)

2. Boot into preconfigured USB stick (plug the USB stick into the node box, power it on)
3. On live booted Linux Mint desktop, launch "Install Linux Mint"
![screenshot](InstallLinuxMint.png)
4. Select "install third-party software/codecs"
![screenshot](InstallThird-party-software.png)
5. In "Installation Type" panel, select "Something else"
![screenshot](SomethingElse.png)
6. Wipe the entire SSD (delete all partitions with the "-" button)
7. Create 3 new partitions from free space (1 at a time with the "+" button):

	* 1x EFI partition (1000 MB)

	* 1x ext4 partition (30000 MB), mount point "/", select "primary" (install Mint to this partition)

	* 1x xfs partition (maximum remaining space), at mount point "/home", select "logical" (this puts all data on desktop in xfs partition)

8. Carry on with remaining installation steps, as desired, following the displayed instructions, then boot into the new OS with USB stick removed

# Step 2
**Set up required golang build basics and p2p optimizations**
1. Right click on desktop, select "Open in Terminal" then enter the below commands in order
2. `wget https://go.dev/dl/go1.23.1.linux-amd64.tar.gz`
3. `sudo tar -xvf go1.20.4.linux-amd64.tar.gz`
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
11. `sudo apt install chrony`
12. `sudo echo "net.core.rmem_default=2097152" >> /etc/sysctl.conf; sudo sysctl -p`
13. `echo "ulimit -n 48000" >> ~/.bashrc ; echo "ulimit -n 48000" >> ~/.bash_profile ; ulimit -n 48000 ; source ~/.bashrc`
14. Restart the machine, log back in
15. Right click on desktop, select "Open in Terminal"
16. Verify chrony is running with `chronyc tracking`
17. Verify go environment is good with `go version`
18. Close terminal

# Step 3
**Build Netrunner from source**
1. Create new folder on desktop called "Dero", open this folder then right click inside it and select "Open in Terminal", enter below commands in order.
2. `git clone https://github.com/DEROFDN/Netrunner`
3. `cd Netrunner`
4. `go mod tidy`
5. `go build .`
6. Once built, a binary named "Netrunner" or "netrunner" will appear in the Netrunner folder. Copy this binary, then paste it to any directory you want to store your node data in (Example: 1 step back into the folder called "Dero" on desktop).

![screenshot](NetrunnerIcon.png)

7. Open new terminal in your chosen directory (right click inside it and select "Open in Terminal") use `./netrunner` or `./Netrunner` to launch Netrunner (you can also just double click the binary to launch it without the terminal displaying, if desired)
8. In CFG menu, select "fast" mode for low data use to start and fast bootstrap (~1 hour sync time). OR, use "full" mode for full node (multiple days sync time) then click RTN
9. Click RUN to start syncing!
10. After Netrunner finishes syncing, you can point any wallet, miner or dApp at  your node, over LAN by using the "Endpoint" IP:PORT

![screenshot](NetrunnerScreenshot.png)

NOTE: Use port 10100 for pointing miners at it, also set the "integrator address" to your desired wallet address in CFG menu before mining. For wallets and dApps, use port 10102 as displayed in Netrunner UI.

NOTE 2: Any time there is a Dero network upgrade/update, delete the "Netrunner" folder (leave the "mainnet" folder intact), then repeat Step 3 to build a fresh Netrunner binary again that includes all of the new updates for you, then you can replace the Netrunner binary in your Dero folder with the new one, without needing to resync from scratch.

# Step 4 (optional)
**Headless operation (no monitor/mouse/keyboard)**
1. Plug in your Displayport dummy plug into a socket, at the back of machine
2. On node machine, install [RealVNC server](https://www.realvnc.com/en/connect/download/vnc/)
3. Launch RealVNC server from LM menu (the "start" menu), configure as desired
4. On any OTHER machine (Can be Windows/Mac/Linux/Mobile), install [RealVNC viewer](https://www.realvnc.com/en/connect/download/viewer/) to access your node UI remotely!
5. Unplug your keyboard, monitor and mouse!

NOTE: RealVNC is not open source, there are other alternatives if you prefer.

# Bookmark these URLs in Firefox, for additional information
https://forum.dero.io/t/dero-node-setup/1774 (More advanced node config guide)

https://github.com/deroproject/derohe/releases/ (For all core Dero Project tools)

https://github.com/DEROFDN/netrunner (For following development of the Netrunner GUI)
