Assumptions
-----------
It is assumed that vagrant has been installed.  A good set of instructions can be found here:

    https://pario.no/2019/03/20/installing-vagrant-on-centos-7

It is assumed that ansible has been installed on the local box

These instructions were used to build the crunchy-ha-pg-demo-0 on a CentOS 7.6 laptop

Introduction
------------
Clone the repo at 

    https://github.com/CrunchyData/priv-all-ansible-roles

Clone the current repo

From the the priv-all-ansible-roles, copy the roles directory to the local directoy holding the current repo

We will use virsh as the driver

The directory structure is shown as follows:

crunchy-ha-pg-demo-0-vagrant-standalone/

    crunchy-ha-pg-demo-0.playbook.yml
    hosts/
    roles/
    Vagrantfile

Additional packages may be required:
yum install virt-manager qemu qemu-kvm qemu-img libvirt libvirt-python libvirt-devel python-virtinst libvirt-client ruby-devel gcc

The libvirtd service should be started
    systemctl enable  libvirtd 
    systemctl start libvirtd

To build the demo, execute the following commands:
    vagrant up

Once the vagrant boxes are up, ssh into each and create public/private keys for each vagrant machine:
# Login to the vagrant box
    vagrant ssh demo0-pg1

# Switch to the root account
    sudo su - 

# Change the root password
    passwd

# Modify the sshd_config file to allow root login
    vi /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes

# Restart the sshd service
    systemctl restart sshd

# Change to the root home directory and create the .ssh directory
    mkdir .ssh
    cd .ssh

# Generate the keys
    ssh-keygen <enter> <enter>

# Copy the keys to the host machine
    ssh-copy-id root@192.168.56.108

# ssh to the host machine
    ssh root@192.168.56.108

# Copy the keys to the vagrant machine
    ssh-copy-id root@192.168.122.16

# ssh to the vagrant machine
    ssh root@192.168.122.16

# Reset the sshd_config file
    vi /etc/ssh/sshd_config
    PasswordAuthentication yes

# Restart the sshd service
    systemctl restart sshd
    exit # this exits to the host
    exit # this exits to the vagrant machine
    exit # this exits to the host

Repeat for all servers

Run Ansible
-----------
Run the ansible playbook as follows:

    ansible-playbook -i hosts/crunchy-ha-pg-demo-0.inventory.yml crunchy-ha-pg-demo-0.playbook.yml

After the ansible playbook completes, one can login using psql as follows:

    psql -U testuser -p 5432 -h 192.168.122.14 -d testdb

