# -*- mode: ruby -*-
# # vi: set ft=ruby :

# Specify minimum Vagrant version and Vagrant API version
VAGRANTFILE_API_VERSION = "2"

# Since these playbooks and Vagrantfiles are for SAMPLE/TEST/DEMO
# uses, we will use the same SSH keypair for both 'vagrant ssh'
# and pgBackRest.
# For production use cases, put an SSH keypair ito be used for just
# pgBackRest into the 'files/' directory!
system("
    if [[ #{ARGV[0]} = 'up' ]]
    then
      # if either part of the keypair is missing, generate our own keypair
      if [[ ! -f ./files/id_rsa || ! -f ./files/id_rsa.pub ]]
      then
        echo 'Generating SSH keys ...'
        mkdir -p ./files
        # the 'echo y' is here to force overwrite if we have half a pair
        # otherweise ssh-keygen throws a prompt asking the user to confirm
        echo y | ssh-keygen -b 2048 -t rsa -C \"vagrant generated key. do not use for prod\" -f ./files/id_rsa -q -N \"\"
        chmod 400 ./files/id_rsa*
      fi
    fi
")

servers = [
    {
        :name => "demo0-pg1",
        :ipaddr => "192.168.122.16",
        :mem => "1024"
    },
    {
        :name => "demo0-pg2",
        :ipaddr => "192.168.122.26",
        :mem => "1024"
    },
    {
        :name => "demo0-common",
        :ipaddr => "192.168.122.14",
        :mem => "2048"
    }
]

# Create servers
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    servers.each do |server|
        config.vm.define server[:name] do |srv|
            # don't have Vagrant generate and then insert a keypair since we want to use ours
            config.ssh.insert_key = false
            #config.ssh.insert_key = true
            # but if, for some reason , ours didn't generate, then fallback to the insecure
            # Vaagrant key which is clearly marked so we'll know that generating a pair failed
            config.ssh.private_key_path = ["files/id_rsa", "~/.vagrant.d/insecure_private_key"]
            config.vm.provision "file", source: "files/id_rsa.pub", destination: "~/.ssh/authorized_keys"
            # do not mount the /vagrant share
            config.vm.synced_folder '.', '/vagrant', disabled: true

            srv.vm.box = "centos/7"
            srv.vm.box_version = "1905.1"
            srv.vm.network :private_network, ip: server[:ipaddr]
            srv.vm.hostname = server[:name]

            srv.vm.provider :virtualbox do |vb|
                vb.name = server[:name]
                vb.memory = server[:mem]
                vb.gui = false
            end
        end
    end
end
