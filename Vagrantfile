# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

   # General Vagrant VM configuration.
   config.vm.box = "debian/stretch64"
   config.ssh.insert_key = false
   config.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key"]
   config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
   config.vm.synced_folder ".", "/vagrant", disabled: true
   
   config.vm.provider :virtualbox do |v|
       v.memory = 512
       v.linked_clone = true
   end

   # Application server 1.
   config.vm.define "webserver" do |app|
       app.vm.network :private_network, ip: "192.168.1.10"
   end

   # Database server.
   config.vm.define "database" do |db|
       db.vm.network :private_network, ip: "192.168.1.20"
   end

end
