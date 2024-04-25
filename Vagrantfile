$hostsfile = <<-SCRIPT
echo "192.168.99.9    ansible" >> /etc/hosts
echo "192.168.99.10   node1" >> /etc/hosts
echo "192.168.99.11   node2" >> /etc/hosts
SCRIPT

Vagrant.configure("2") do |config|

  # Define virtualbox as the provider
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
  end

 # Create the first VM called ansible
  config.vm.define :ansible do |ansible|
    ansible.vm.box = "generic/rocky9"
    config.vm.box_version = "4.3.12"
    ansible.vm.hostname = "ansible"
    ansible.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "2"
    end
    ansible.vm.provision "hostsfile setup", type: "shell", inline: $hostsfile
    ansible.vm.network :private_network, ip: "192.168.99.9"
    end


  # Create the second VM called node1
  config.vm.define :node1 do |node1|
    node1.vm.box = "generic/rocky9"
    config.vm.box_version = "4.3.12"
    node1.vm.hostname = "node1"
    node1.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "2"
    end
    node1.vm.network :private_network, ip: "192.168.99.10"
    node1.vm.provision "hostsfile setup", type: "shell", inline: $hostsfile
  end

  # Create the third VM called node2
  config.vm.define :node2 do |node2|
    node2.vm.box = "generic/rocky9"
    config.vm.box_version = "4.3.12"
    node2.vm.hostname = "node2"
    node2.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "2"
    end
    node2.vm.provision "hostsfile setup", type: "shell", inline: $hostsfile
    node2.vm.network :private_network, ip: "192.168.99.11"
  end
end

