# osm-5gc

This repository explains the installation of OSM and free5Gc in a virtual machine using VirtualBox and their configuration with simple example scenarios.

***Setting up the Environment***
1. Set up the system and configure it, see [requirements.md](https://github.com/samareshbera/osm-5gc/blob/main/requirements.md)


***OSM Installation***

1. Make `install.sh` as executable: `chmod +x install_osm.sh`
2. Install using: `./install_osm.sh --vimemu` (vimemu if want to install vim emulator, for all options check [here](https://osm.etsi.org/docs/user-guide/03-installing-osm.html#other-installer-options))


***After Successful Installation of OSM***

1. Add user to docker: `sudo usermod -aG docker $USER`
2. Make it effect using: `newgrp docker`
3. Check docker without sudo: `docker run hello-world`
4. Open OSM GUI at `http://<IP_of_the_host_machine>`
5. For walkthrough example, check [here](https://osm.etsi.org/docs/vnf-onboarding-guidelines/00-introduction.html) and [here](https://osm.etsi.org/gitlab/vnf-onboarding/osm-packages) (written in SOL006 as well)


***free5Gc Installation***
1. Install latest stable build of free5Gc (version `v3.0.5)`:
  * `cd ~`
  * `git clone --recursive -b v3.0.5 -j ``nproc`` https://github.com/free5gc/free5gc.git` (use single ` ` for nproc) 
  * `cd free5gc`

2. Install UPF:
  * `git clone -b v0.2.1 https://github.com/free5gc/gtp5g.git`
  * `cd gtp5g`
  * `make`
  * `sudo make install`

3. Compile the network function services in `free5gc`:
  * `cd ~/free5gc`
  * `make` to build all functions (you may do it one by one, e.g., `make amf` for AMF only)

4. Install WebConsole of `free5gc`:
  * `cd ~/free5gc/webconsole`
  * `git checkout v1.0.1`
  * `cd ~/free5gc`
  * `make webconsole`


***Troubleshooting***

1. **OSM**: Check for possible solutions, see [debug_osm.md](https://github.com/samareshbera/osm-5gc/blob/main/debug_osm.md)
