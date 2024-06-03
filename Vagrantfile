# -*- mode: ruby -*-
# vim: set ft=ruby :

ENV['VAGRANT_SERVER_URL']="https://vagrant.elab.pro"
OS="ubuntu/jammy64"

MACHINES = {
  :frontEnd => {
        :box_name => OS,
        :vm_name => "frontEnd",
        #:public => {:ip => "10.10.10.1", :adapter => 1},
        :web => "frontEnd",
        :net => [   
                    #ip, adpter, netmask, virtualbox__intnet
                    ["192.168.255.1", 2, "255.255.255.0",  "router-net"], 
                    ["192.168.50.10", 8, "255.255.255.0"],
                ]
  },

  :backEnd01 => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "backEnd01",
        :net => [
                   ["192.168.255.10",  2, "255.255.255.0",  "router-net"],
                   ["192.168.50.11",  8, "255.255.255.0"],
                ]
  },

  :backEnd02 => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "backEnd02",
        :net => [
                   ["192.168.255.20",  2, "255.255.255.0",  "router-net"],
                   ["192.168.50.12",  8, "255.255.255.0"],
                ]
  },

#   :masterBD => {
#         :box_name => "ubuntu/jammy64",
#         :vm_name => "masterBD",
#         :net => [
#                    ["192.168.255.100",  2, "255.255.255.0",  "router-net"],
#                    ["192.168.50.20",   8,  "255.255.255.0"],
#                 ]
#   },

#   :slaveBD => {
#         :box_name => "ubuntu/jammy64",
#         :vm_name => "slaveBD",
#         :net => [
#                    ["192.168.255.200",  2, "255.255.255.0",  "router-net"],
#                    ["192.168.50.21",  8,  "255.255.255.0"],
#                 ]
#   },

#   :backup => {
#    :box_name => "ubuntu/jammy64",
#    :vm_name => "backup",
#    :net => [
#               ["192.168.255.250",  2, "255.255.255.0",  "router-net"],
#               ["192.168.50.100",  8,  "255.255.255.0"],
#            ]
# },

#   :office2Router => {
#        :box_name => "ubuntu/jammy64",
#        :vm_name => "office2Router",
#        :net => [
#                    ["192.168.255.6",  2,  "255.255.255.252",  "office2-central"],
#                    ["192.168.1.1",    3,  "255.255.255.128",  "dev2-net"],
#                    ["192.168.1.129",  4,  "255.255.255.192",  "test2-net"],
#                    ["192.168.1.193",  5,  "255.255.255.192",  "office2-net"],
#                    ["192.168.50.30",  8,  "255.255.255.0"],
#                ]
#   },

#   :office2Server => {
#        :box_name => "ubuntu/jammy64",
#        :vm_name => "office2Server",
#        :net => [
#                   ["192.168.1.2",    2,  "255.255.255.128",  "dev2-net"],
#                   ["192.168.50.31",  8,  "255.255.255.0"],
#                ]
#   }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]
      
      box.vm.provider "virtualbox" do |v|
        v.memory = 512
        v.cpus = 1
       end

      boxconfig[:net].each do |ipconf|
        if boxconfig.key?(:web)
          box.vm.network "forwarded_port", guest: 80, host: 8080
        end
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
      end

      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end

      box.vm.provision "shell", inline: <<-SHELL
        mkdir -p ~root/.ssh
        cp ~vagrant/.ssh/auth* ~root/.ssh
      SHELL
    end
  end
end
