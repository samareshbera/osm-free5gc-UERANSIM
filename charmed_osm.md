1. Microk8s installation: `sudo snap install microk8s --classic --channel=1.21`, for details see [here](https://microk8s.io/docs)

2. Microk8s configuration: `cd $HOME` `mkdir .kube` `cd .kube` `microk8s config > config`, for details see [here](https://microk8s.io/docs/working-with-kubectl)

3. Install juju: `sudo snap install --classic juju`, for details see [here](https://juju.is/docs/olm/other-clusters) OR `sudo snap install juju --classic --channel=2.9/stable`

4. Create juju controller: `juju bootstrap microk8s osm-vca`, where `microk8s` is the cloud_name and `osm-vca` is the controller_name.

5. 
