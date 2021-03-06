***Handle Errors during and after OSM installation***

Errors related to OSM can be troubleshooted from [here](https://osm.etsi.org/wikipub/index.php/Common_issues_and_troubleshooting). The following issues were faced during installation and instantiation:

1. **Error:** run as docker swarm, etc.: the error is network "netosm" is declared as external, but could not be found. You need to create a swarm-scoped network before the stack is deployed
    * **Solution** is [here](https://osm.etsi.org/wikipub/index.php/Common_issues_and_troubleshooting), OR it is as follows:
      - `docker swarm init --advertise-addr <IP_addr_OSM>`
      - Create OSM Docker Network ...
      - ` [ -z "$OSM_STACK_NAME" ] && OSM_STACK_NAME=osm`
      - `OSM_NETWORK_NAME=net${OSM_STACK_NAME}`
      - `DEFAULT_INTERFACE=$(route -n | awk '$1~/^0.0.0.0/ {print $8}')`
      - `DEFAULT_MTU=$(ip addr show $DEFAULT_INTERFACE | perl -ne 'if (/mtu\s(\d+)/) {print $1;}')`
      - Check OSM_STACK_NAME: `echo \# OSM_STACK_NAME = $OSM_STACK_NAME`
      - Check OSM_NETWORK_NAME: `echo \# OSM_NETWORK_NAME = $OSM_NETWORK_NAME`
      - Check Default_Interface: `echo \# DEFAULT_INTERFACE = $DEFAULT_INTERFACE`
      - Check MTU value: `echo \# DEFAULT_MTU = $DEFAULT_MTU`
      - Run: `sg docker -c "docker network create --driver=overlay --attachable --opt com.docker.network.driver.mtu=${DEFAULT_MTU} ${OSM_NETWORK_NAME}"`
      - Run to create a container name vim-emu: `docker run --name vim-emu -t -d --pid='host' --network=netosm -v /var/run/docker.sock:/var/run/docker.sock docker.io/docker5gmedia/vim-emu-img:latest python3 examples/osm_default_daemon_topology_2_pop.py` (make sure you correctly specify the path to vim-emu-img)



2. **Error:** cannot ping docker container from the host (solution is [here](https://training.play-with-docker.com/docker-networking-hol/)), Details:
      * `docker network ls`
      * `sudo apt update`
      * `sudo apt-get install bridge-utils`
      * `brctl show` (you won’t see an interface attached to `docker0`)
      * Connect the container to the host
      * Create docker container
      * `docker run <image_name>` (see the docker container whether it is created)
      * `docker ps | grep <container_name>`
      * See the interface of `docker0`: `brctl show` (you will see an interface now)
      * Now it would be able to ping to the container from the host using ip of the container, e.g., `ping -c 4 172.17.0.2`


3. **Error:** error execution phase preflight: preflight] Some fatal errors occurred: `ERROR DirAvailable--var-lib-etcd]: /var/lib/etcd is not empty`
      * `rm -rf /var/lib/etcd` this will remove, this issue happens when you have deleted the node and trying to re-create it. If you have setup your local kubernetes cluster on your local development machine then it is quite often you will face this issue.
      * check this page for all errors related to `kubernetes`: https://jhooq.com/kubernetes-error-execution-phase-preflight-preflight/


4. If you want to uninstall:
   * `./install.sh --uninstall`
   * Remove all Kubernetes related configuration:
      `
      kubeadm reset
      sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*   
      sudo apt-get autoremove  
      sudo rm -rf ~/.kube`
      
   * `sudo rm -rf /var/lib/etcd` to dela with `Some fatal errors occurred: `ERROR DirAvailable--var-lib-etcd]: /var/lib/etcd is not empty`

5. Error: `k8scloud` already exists
   * Sometime `k8scloud` is not removed just by uninstalling OSM. Need to remove the juju cloud using `juju remove-cloud k8scloud`

6. Add ip route: `sudo ip route add 172.16.0.0/24 via 192.168.122.1 dev ens3`: `172.16.0.0` is the target, `192.***` is the gateway of the source machine (router address)

