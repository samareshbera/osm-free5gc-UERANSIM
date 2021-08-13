# osm-5gc

This repository explains the installation of OSM and free5Gc in a virtual machine using VirtualBox and their configuration with simple example scenarios.

***Setting up the Environment***
1. Set up the system and configure it, see [requirements.md](samareshbera/osm-5gc/requirements.md)

***OSM Installation***

1. Make `install.sh` as executable: `chmod +x install_osm.sh`
2. Install using: `./install_osm.sh --vimemu` (vimemu if want to install vim emulator, for all options check [here](https://osm.etsi.org/docs/user-guide/03-installing-osm.html#other-installer-options))
