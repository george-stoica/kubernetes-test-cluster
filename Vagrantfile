# variables

VM_IMAGE = "debian/stretch64"
NODES = 2

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vagrant.plugins = ["vagrant-vbguest"]
  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end

  config.vm.define "master" do |master|
    master.vm.box = VM_IMAGE
    master.vm.network "private_network", ip: "192.168.10.10"
    master.vm.hostname = "master"

    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/provision_master_playbook.yaml"
      ansible.extra_vars = {
        node_ip: "192.168.10.10"
      }
    end

  end

  (1..NODES).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = VM_IMAGE
      node.vm.network "private_network", ip: "192.168.10.#{i + 20}"
      node.vm.hostname = "node#{i}"

      node.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/provision_worker_playbook.yaml"
        ansible.extra_vars = {
          node_ip: "192.168.10.#{i + 20}"
        }
      end
      
    end
  end
end
