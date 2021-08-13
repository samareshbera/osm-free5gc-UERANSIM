# osm-5gc

This repository explains the installation of OSM and free5Gc in a virtual machine using VirtualBox and their configuration with simple example scenarios.

* Environment requirements
-- Ubuntu 18.04 LTS (64 bit)
-- RAM: 16 GB (minimum)
-- CPU: 2 (at least)
-- HDD: 200 GB (recommended)

* VirtualBox configuration and Ubuntu installation
-- Configure VirtualBox with a machine name
-- Change the number of CPUs in settings (need at least 2 CPUs for OSM)
-- Add another network adapter: Host-Only, so you have 2 adapters: NAT + Host-Only
-- Start the Ubuntu machine
