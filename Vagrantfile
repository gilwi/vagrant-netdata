# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # config.vm.box_check_update = false
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network"

  # config.vm.synced_folder "../data", "/vagrant_data"

  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end

  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  config.vm.provider :libvirt do |libvirt|
    # otherwise it defaults to qemu://localhost/system?...
    libvirt.uri = "qemu:///system"
  end

  # config.vm.define "prometheus", primary: true do |prom|
  #   prom.vm.box = "centos/8"
  #   prom.vm.network "public_network", dev: "virbr0", mode: "bridge", type: "bridge"
  #   prom.vm.network "private_network", ip: "10.21.30.40", libvirt__network_name: "vagrant-net2"
  # end

  # config.vm.define "graphite", primary: true do |graph|
  #   graph.vm.box = "centos/8"
  #   graph.vm.network "public_network", dev: "virbr0", mode: "bridge", type: "bridge"
  #   graph.vm.network "private_network", ip: "10.21.30.40", libvirt__network_name: "vagrant-net2"
  # end

  config.vm.define "graphitebuntu", primary: true do |graph|
    graph.vm.box = "generic/ubuntu1804"
    graph.vm.network "public_network", dev: "virbr0", mode: "bridge", type: "bridge"
    graph.vm.network "private_network", ip: "10.21.30.50", libvirt__network_name: "vagrant-net2"
  end

  N = 1
  
  (1..N).each do |machine_id|

    config.vm.define "netdata-#{machine_id}" do |netdata|
      netdata.vm.box = "centos/8"
      netdata.vm.network "public_network", dev: "virbr0", mode: "bridge", type: "bridge"
      netdata.vm.network "private_network", ip: "10.21.30.4#{machine_id}", libvirt__network_name: "vagrant-net2"
      if machine_id == N

        netdata.vm.provision :ansible do |ansible|
          ansible.limit = "all"
          ansible.playbook = "playbook.yml"
          ansible.groups = {
            "netdata" => ["netdata-[1:#{machine_id}]"]
          }
        end
      
      end

    end

  end

end
