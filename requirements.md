***Configuration before OSM installation***

1. Environment requirements
    * Ubuntu 18.04 LTS (64 bit) -- you can download the image from [here](https://releases.ubuntu.com/18.04.5/)
    * RAM: 16 GB (minimum)
    * CPU: 2 (at least)
    * HDD: 200 GB (recommended)
    * GCC: `gcc 7.3.0` for free5Gc
    * `Go 1.14.4 linux/amd64` for free5Gc
    * `kernel version 5.0.0-23-generic` for free5Gc (required for the UPF element)

2. VirtualBox configuration and Ubuntu installation
    * Configure VirtualBox with a machine name -- VirtualBox software can be downloaded from [here](https://www.virtualbox.org/wiki/Downloads)
    * Change the number of CPUs in settings (need at least 2 CPUs for OSM)
    * Add another network adapter: Host-Only, so you have 2 adapters: NAT + Host-Only
    * Start the Ubuntu machine (installation process will start)
    * Follow the installation instructions
    * Go to `Ubuntu Software and Update` and change the server to `Main Server`

3. Add certificates
    * Docker
      - Install curl: `sudo apt-get install curl`
      - `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
      - `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"`

    * Kubernetes
      - `curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -`
      - `cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list`
      - `deb https://apt.kubernetes.io/ kubernetes-xenial main`
      - `EOF`

4. Update the Ubuntu: `sudo apt-get update`

5. Install Open vSwitch: `sudo apt-get install openvswitch-switch`

6. Download OSM Version 9: `wget https://osm-download.etsi.org/ftp/osm-9.0-nine/install_osm.sh`

7. [Install LXC and create bridge](https://osm.etsi.org/wikipub/index.php/LXD_configuration_for_OSM_Release_FIVE)
    * Get a list of LXC/LXD packages that are installed via apt: `dpkg -l|grep lx[cd]`
    * If already installed, remove using: `sudo apt-get remove --purge lxd lxd-client lxcfs liblxc1 liblxc-common`
    * Install LXD: `sudo snap install lxd`
    * Add user to LXD: 
        `sudo adduser <user_name> lxd`
        
        `newgrp lxd`
        
        `id`
        
        ```lxc list```
    * Initialize LXD: ```lxd init```
    * Follow the instructions (for name and storage type refer to [here](https://osm.etsi.org/wikipub/index.php/LXD_configuration_for_OSM_Release_THREE)). Make sure to check the following:
      - `Do you want to setup an IPv6 subnet? No`
      - `name: lxdbr0`
      - `driver: zfs`
    * Do `lxc list --dump` to make it effect (not sure whether to do it, but first time installation failed and no lxc was present using `lxc list`)


8. Test LXD
    * `lxc launch ubuntu:16.04 test`
    * `lxc exec test bash`
    * `sudo apt-get update` (check all things work)
    * `exit` to logout from `test`
    * `lxc stop test` to stop `test`
    * ```lxc delete test``` to delete `test`


***Configuration before free5Gc installation***

1. Check the hardware/software requirements
   * Check Linux kernel version: `uname -r` (it must be `5.0.0-23-generic` or above)
   * Check GO verson: `go version` (it must be Go 1.14.4 for free5Gc)
   * Install Go using:
      - `wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz`
      - `sudo tar -C /usr/local -zxvf go1.14.4.linux-amd64.tar.gz`
      - `mkdir -p ~/go/{bin,pkg,src}`
      
      The following assume that your shell is bash
      - `echo 'export GOPATH=$HOME/go' >> ~/.bashrc`
      - `echo 'export GOROOT=/usr/local/go' >> ~/.bashrc`
      - `echo 'export PATH=$PATH:$GOPATH/bin:$GOROOT/bin' >> ~/.bashrc`
      - `echo 'export GO111MODULE=auto' >> ~/.bashrc`
      - `source ~/.bashrc`

2. Install control-plane supporting packages
   * `sudo apt-get update`
   * `sudo apt-get install mongodb wget git`
   * `sudo systemctl start mongodb`

3. Install user-plane supporting packages
   * `sudo apt-get update`
   * `sudo apt-get install git gcc g++ cmake autoconf libtool pkg-config libmnl-dev libyaml-dev`
   * `go get -u github.com/sirupsen/logrus`

4. Linux host network settings
   * `sudo sysctl -w net.ipv4.ip_forward=1`
   * `sudo iptables -t nat -A POSTROUTING -o <dn_interface> -j MASQUERADE` (here `<dn_interface>` refers to ethernet interface when connecting to Internet, e.g., `enp0s3`)
   * `sudo iptables -A FORWARD -p tcp -m tcp --tcp-flags SYN,RST SYN -j TCPMSS --set-mss 1400`
   * `sudo systemctl stop ufw` to stop `uncomplicated firewall`




