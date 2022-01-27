# -*- mode: ruby -*-
# vi: set ft=ruby :

box = "generic/rhel8"
# box = "generic/centos8"
nodes = 2 #number of nodes in addition to control node
cpu = 2
mem = 2048

Vagrant.configure("2") do |config|
  (0..nodes).each do |node_id|
    config.vm.define "node#{node_id}" do |node|
      node.vm.box = box
      node.vm.provider "virtualbox" do |vbox|
        vbox.name = "ex294_ansible_node#{node_id}"
        vbox.cpus = cpu
        vbox.memory = mem
      end
      node.vm.network "private_network", ip: "192.168.4.20#{node_id}", virtualbox__intnet:true

      if node_id == 0
        node.vm.network "private_network", ip: "192.168.56.10"
        node.vm.synced_folder "./synced", "/synced", create: true, type: "nfs", nfs_export: true, nfs_udp: false, nfs_version: 4
      end

      if node_id == nodes
        node.vm.provision :ansible do |ansible|
          ansible.config_file = "ansible.cfg"
          ansible.playbook = "provision.yaml"
          ansible.limit = "all"
          ansible.groups = {
            "control" => ["node0"],
            "managed" => ["node1","node2"]
          }
          ansible.host_vars = {
            "node0" => {"hostname" => "control"},
            "node1" => {"hostname" => "ansible1"},
            "node2" => {"hostname" => "ansible2"}
          }
        end
      end
    end
  end
end
