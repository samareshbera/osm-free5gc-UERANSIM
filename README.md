# osm-5gc-UERANSIM

This repository explains the installation of OSM, free5Gc, and UERANSIM in a virtual machine using VirtualBox. All the steps have been explained in the respective official websites. I thought it may be useful to put all things together, so this repository explains the installation. Further, I have mentioned a few ways to handle errors during installation (at least they worked for me).

***Setting up the Environment***
1. Set up the system and configure it, see [requirements.md](https://github.com/samareshbera/osm-5gc/blob/main/requirements.md)


***OSM Installation***

1. Make `install.sh` as executable: `chmod +x install_osm.sh`
2. Install using: `./install_osm.sh --vimemu` (vimemu if want to install vim emulator, for all options check [here](https://osm.etsi.org/docs/user-guide/03-installing-osm.html#other-installer-options))
3. For Charmed OSM Installation: https://discourse.charmhub.io/t/first-steps-with-charmed-osm/1533


***After Successful Installation of OSM***

1. Add user to docker: `sudo usermod -aG docker $USER`
2. Make it effect using: `newgrp docker`
3. Check docker without sudo: `docker run hello-world`
4. Open OSM GUI at `http://<IP_of_the_host_machine>`
5. For walkthrough example, check [here](https://osm.etsi.org/docs/vnf-onboarding-guidelines/00-introduction.html) and [here](https://osm.etsi.org/gitlab/vnf-onboarding/osm-packages) (written in SOL006 as well)
6. Execute the bash of a container: `kubectl exec -it <container_name> -n <namespace> -- bash`
   * Ping test: `ping command not found`: `apt-get install iputils-ping`
7. `kubectl` commands: https://cloud.google.com/migrate/anthos/docs/troubleshooting/executing-shell-commands

***free5Gc Installation***
1. Install latest stable build of free5Gc (version `v3.0.5)`:
  * `cd ~`
  * `git clone --recursive -b v3.0.5 -j ``nproc`` https://github.com/free5gc/free5gc.git` (use single \tilde for nproc) 
  * `cd free5gc`

2. Compile the network function services in `free5gc`:
  * `cd ~/free5gc`
  * `make` to build all functions (you may do it one by one, e.g., `make amf` for AMF only)

3. Install UPF:
  * `cd ~`
  * `git clone -b v0.2.1 https://github.com/free5gc/gtp5g.git`
  * `cd gtp5g`
  * `make`
  * `sudo make install`
  * Do `make upf` if not done for all network functions with `make` in Step 2

4. Install WebConsole of `free5gc`:
  * `cd ~/free5gc/webconsole`
  * `git checkout v1.0.1`
  * `cd ~/free5gc`
  * `make webconsole` (you can build it manually in Setp 5)

5. Start the WebConsole:
  * `cd ~/free5gc/webconsole/frontend`
  * `yarn install`
  * `yarn build`
  * `rm -rf ../public`
  * `cp -R build ../public`
  * `cd ~/free5gc/webconsole/`
  * `go run server.go` (web server is started and listening at port 5000)
  * Open browser and access `http://ip-address:5000/`
  * Username: `admin`
  * Password: `free5gc`


***UERANSIM installation***

1. `cd ~`
2. Clone the UERANSIM: `git clone https://github.com/aligungr/UERANSIM`
3. Install dependencies:
   * `sudo apt install make`
   * `sudo apt install gcc`
   * `sudo apt install g++`
   * `sudo apt install libsctp-dev lksctp-tools`
   * `sudo apt install iproute2`
   * `sudo snap install cmake --classic`

4. Go to repository: `cd ~/UERANSIM`
5. `make`


***Troubleshooting***

1. **OSM**: Check for possible solutions, see [debug_osm.md](https://github.com/samareshbera/osm-5gc/blob/main/debug_osm.md)
