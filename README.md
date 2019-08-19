Assumptions
-----------
It is assumed that vagrant has been installed.  A good set of instructions can be found here:
https://pario.no/2019/03/20/installing-vagrant-on-centos-7

It is assumed that ansible has been installed on the local box

These instructions were used to build the crunchy-ha-pg-demo-0 on a CentOS 7.6 laptop

Introduction
------------
Clone the repo at https://github.com/CrunchyData/priv-all-ansible-roles

Clone the current repo

From the the priv-all-ansible-roles, copy the roles directory to the local directoy holding the current repo

We will use virsh as the driver

The directory structure is shown as follows:
tree
.
├── defaults
│   └── main.yml
├── handlers
│   └── main.yml
├── hosts
│   └── crunchy-ha-pg-demo-0.inventory.yml
├── inventory
│   └── hosts
│       └── crunchy-ha-pg-demo-0.inventory.yml
├── meta
│   └── main.yml
├── molecule
│   └── default
│       ├── INSTALL.rst
│       ├── molecule.yml
│       ├── playbook.yml
│       ├── prepare.yml
│       └── tests
│           ├── test_default.py
│           └── test_default.pyc
├── README.md
├── roles
│   └── crunchydata
│       ├── backrest
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   │   ├── id_rsa
│       │   │   └── id_rsa.pub
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   │   ├── pgbackrest-client.conf.j2
│       │   │   ├── pgbackrest-global.conf.j2
│       │   │   ├── pgbackrest-local.conf.j2
│       │   │   └── pgbackrest-server.conf.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── copyhostsfile
│       │   └── tasks
│       │       └── main.yml
│       ├── crunchyrepo
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   │   ├── CRUNCHY-GPG-KEY.public
│       │   │   ├── crunchypg10.repo
│       │   │   ├── crunchypg11.repo
│       │   │   └── RPM-GPG-KEY-crunchydata
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── etcd
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   │   └── etcd.conf.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── firewall
│       │   ├── defaults
│       │   │   └── main.yml
│       │   └── tasks
│       │       └── main.yml
│       ├── grafana
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   │   └── grafana.ini.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── haproxy
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   │   ├── haproxy-cluster.yml.j2
│       │   │   ├── haproxy_sysconfig.j2
│       │   │   └── haproxy.yml.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── localrepo
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── patroni
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   ├── configure.yml
│       │   │   ├── install.yml
│       │   │   ├── main.yml
│       │   │   └── service.yml
│       │   ├── templates
│       │   │   └── patroni.yml.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── pgbackup
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── pgbench
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   │   └── pgpass.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── pgbouncer
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   └── main.yml
│       │   ├── templates
│       │   │   ├── pgbouncer-cluster.ini.j2
│       │   │   ├── pgbouncer_function.sql.j2
│       │   │   ├── pgbouncer.ini.j2
│       │   │   └── userlist.txt.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── pgmonitor
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   ├── db-global.yml
│       │   │   ├── db-user.yml
│       │   │   ├── main.yml
│       │   │   └── node.yml
│       │   ├── templates
│       │   │   ├── pgmonitor.conf.j2
│       │   │   ├── pgmonitor_global_db.j2
│       │   │   ├── pgmonitor_user_db.j2
│       │   │   └── pgpass.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── postgresql
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   ├── cluster.yml
│       │   │   ├── configure.yml
│       │   │   ├── install.yml
│       │   │   ├── main.yml
│       │   │   ├── recovery_check.yml
│       │   │   ├── replica.yml
│       │   │   ├── service_setup.yml
│       │   │   ├── service.yml
│       │   │   └── setup.yml
│       │   ├── templates
│       │   │   ├── pg_hba.conf.j2
│       │   │   ├── pgpass.j2
│       │   │   ├── postgresql.crunchydata.conf.j2
│       │   │   ├── recovery.conf.j2
│       │   │   └── template.service.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       ├── prometheus
│       │   ├── defaults
│       │   │   └── main.yml
│       │   ├── files
│       │   ├── handlers
│       │   │   └── main.yml
│       │   ├── meta
│       │   │   └── main.yml
│       │   ├── README.md
│       │   ├── tasks
│       │   │   ├── etcd.yml
│       │   │   ├── main.yml
│       │   │   ├── node.yml
│       │   │   └── pg.yml
│       │   ├── templates
│       │   │   ├── prometheus_etcd.yml.j2
│       │   │   ├── prometheus_node.yml.j2
│       │   │   └── prometheus_pg.yml.j2
│       │   ├── tests
│       │   │   ├── inventory
│       │   │   └── test.yml
│       │   └── vars
│       │       └── main.yml
│       └── restartfwd
│           ├── default
│           │   └── main.yml
│           ├── files
│           │   ├── environment
│           │   └── locale.conf
│           └── tasks
│               └── main.yml
├── tasks
│   └── main.yml
└── vars
    └── main.yml

139 directories, 175 files

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

