**Openstack as Virtual Infrastructure Manager (VIM)**

1. Openstack installation:
    * `sudo apt update`
    * `sudo apt -y upgrade`
    * `sudo apt -y dist-upgrade`
    * `sudo reboot`
    * `sudo useradd -s /bin/bash -d /opt/stack -m stack`
    * `echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack`
    * `sudo su - stack`
    * `sudo su -`
    * `su - stack`
    * `sudo apt -y install git`
    * `git clone https://git.openstack.org/openstack-dev/devstack`
    * `cd devstack`
    
    * `nano local.conf`: add the following
    
            [[local|localrc]]
            # Password for KeyStone, Database, RabbitMQ and Service
            ADMIN_PASSWORD=your_password
            DATABASE_PASSWORD=$ADMIN_PASSWORD
            RABBIT_PASSWORD=$ADMIN_PASSWORD
            SERVICE_PASSWORD=$ADMIN_PASSWORD
            # Host IP - get your Server/VM IP address from ip addr command
            HOST_IP=your_ip
            disable_service etcd3   # error with this if enabled
    
    * `echo "FORCE=yes" > localrc`
    * `./stack.sh`



2. Issue: permission denied `PermissionError: [Errno 13] Permission denied: '/opt/stack/.cache/pip/wheels/25/af/b8/******`
    * Solution: associate the stack user with the stack directory: `sudo chown -R stack:stack /opt/stack`
