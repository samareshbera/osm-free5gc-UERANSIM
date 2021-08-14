***Configuration before OSM installation***

1. Environment requirements
    * Ubuntu 18.04 LTS (64 bit) -- you can download the image from [here](https://releases.ubuntu.com/18.04.5/)
    * RAM: 16 GB (minimum)
    * CPU: 2 (at least)
    * HDD: 200 GB (recommended)

2. VirtualBox configuration and Ubuntu installation
    * Configure VirtualBox with a machine name -- VirtualBox software can be downloaded from [here](https://www.virtualbox.org/wiki/Downloads)
    * Change the number of CPUs in settings (need at least 2 CPUs for OSM)
    * Add another network adapter: Host-Only, so you have 2 adapters: NAT + Host-Only
    * Start the Ubuntu machine (installation process will start)
    * Follow the installation instructions

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


8. Test LXD
    * `lxc launch ubuntu:16.04 test`
    * `lxc exec test bash`
    * `sudo apt-get update` (check all things work)
    * `exit` to logout from `test`
    * `lxc stop test` to stop `test`
    * ```lxc delete test``` to delete `test`

