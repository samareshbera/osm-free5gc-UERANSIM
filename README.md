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




***Troubleshooting***

1. **OSM**: Check for possible solutions, see [debug_osm.md](https://github.com/samareshbera/osm-5gc/blob/main/debug_osm.md)
