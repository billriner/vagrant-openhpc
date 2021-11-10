# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # Head node --------------------------------------------------------------------

  config.vm.define "redfin" do |head_node|

    # Use CentOS 7 box
    head_node.vm.box = "bento/centos-7.7"

    # Use the public (bridged) network
    #head_node.vm.network "public_network"  # This will prompt for bridge interface
    head_node.vm.network "public_network", :bridge => "em1"

    # Set the hostname
    head_node.vm.hostname = "redfin"

    # Provision using an inline shell script
    head_node.vm.provision "shell", inline: <<-SHELL
      echo "Hello from the head node!"
    SHELL

    # Provision as NIS client and server using shell script
    #head_node.vm.provision "shell", path: "nis_client.sh"
    #head_node.vm.provision "shell", path: "nis_slave.sh"

  end

  # Provider-specific configuration
  config.vm.provider "virtualbox" do |vb|

    # Don't display the VirtualBox GUI
    vb.gui = false
 
    # Set the amount of memory
    vb.memory = "1024"

    # MAC address for bridge network
    vb.customize ["modifyvm", :id, "--macaddress2", "0242ac116543"]

  end

  # Compute nodes ----------------------------------------------------------------

  compute_node = {
    "rf1" => "0242ac116544",
    "rf2" => "0242ac116545",
    "rf3" => "0242ac116546",
    "rf4" => "0242ac116547"
  }

  compute_node.each do |name, mac| 

    config.vm.define name do |compute_node|

      # Use centos 7 box
      compute_node.vm.box = "bento/centos-7.7"

      # Use the public (bridged) network
      # compute_node.vm.network "public_network"  # This will prompt for bridge interface
      compute_node.vm.network "public_network", :bridge => "em1"

      # Set the hostname
      compute_node.vm.hostname = name

      # Provision using an inline shell script
      compute_node.vm.provision "shell", inline: <<-SHELL
        echo "Hello from a compute node!"
        #yum -y update
        #yum -y groupinstall "Compute Node"
      SHELL

      # Provision as NIS client using shell script
      #compute_node.vm.provision "shell", path: "nis_client.sh"

      # Provider-specific configuration
      compute_node.vm.provider "virtualbox" do |vb|

        # Don't display the VirtualBox GUI
        vb.gui = false
 
        # Set the amount of memory
        vb.memory = "1024"

        # MAC address for bridge network
        vb.customize ["modifyvm", :id, "--macaddress2", mac]

      end
    end
  end
end
