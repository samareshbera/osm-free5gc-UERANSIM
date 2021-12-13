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

3. Issue: `[ERROR] /opt/stack/devstack/functions-common:691 git call failed: [git clone https://opendev.org/openstack/keystone.git /opt/stack/keystone --branch master]`
    * Solution: add this `GIT_BASE=${GIT_BASE:-https://git.openstack.org}` to `local.conf` under `Devstack` folder: to use the HTTPS protocol instead of the Git protocol!

4. Issue: 
   `Errors were encountered while processing:
    python3-rtslib-fb
    targetcli-fb
    E: Sub-process /usr/bin/dpkg returned an error code (1)`
    
    https://askubuntu.com/questions/1334619/failed-to-start-rtslib-fb-targetctl-service
    
5. `ERROR: Cannot uninstall'httplib2'. It is a distutils installed project and thus we cannot accurately determine which files belong`: Solution: `sudo -H pip install --ignore-installed -U httplib2`

6. Issue: `/opt/stack/devstack/files/etcd-v3.3.12-linux-amd64.tar.gz: failed sha256sum: warning: 1 computed checksum did not match`: Solution: `disable_service etcd3` add this to `local.conf`

7. Issue: `pypowervm ERROR: No matching distribution found for futures>=3.0; python_version == "3.6"` =>  `pip install --upgrade pip` and `pip install --upgrade pipenv`

8. Enables password-less ssh: https://www.cyberciti.biz/faq/ubuntu-18-04-setup-ssh-public-key-authentication/ 



**OpenStack installation -- single machine Ubuntu**

1. Follow steps: https://ubuntu.com/openstack/install
2. Error on login GUI: `FileNotFoundError at /auth/login/ openstack` => Solution: `sudo systemctl restart snap.microstack.horizon-uwsgi`

