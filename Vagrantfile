# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
$MEMSIZE=4096

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # The ordering of these 2 lines expresses a preference for a hypervisor
  config.vm.provider "virtualbox"
  config.vm.provider "vmware_fusion"

  config.ssh.forward_agent = false
  config.ssh.insert_key = false

  # Timeouts
  config.vm.boot_timeout = 900
  config.vm.graceful_halt_timeout=100

  # Use the Ansible playbook provision.yml to setup the virtual machines.
  config.vm.provision "ansible" do |ansible|
    ansible.inventory_path = "inventory/vagrant.ini"
    ansible.playbook = "site.yml"
    ansible.skip_tags = "letsencrypt"
    ansible.verbose = "vv"
    ansible.host_key_checking = "false"
  end

  # enable guest additions
  config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: false

  config.vm.define :hub, autostart: true do |hub_config|
    hub_config.vm.box = "ubuntu14"
    hub_config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
    hub_config.vm.network "private_network", ip: "192.168.10.20", :netmask => "255.255.255.0",  auto_config: true
    hub_config.vm.hostname = "hub"
    hub_config.vm.network "forwarded_port", id: 'ssh', guest: 22, host: 2224, auto_correct: true
    hub_config.vm.network :forwarded_port, guest:443, host:7443
    hub_config.vm.provider "vmware_fusion" do |vmware|
      vmware.vmx["memsize"] = "#$MEMSIZE"
      vmware.vmx["numvcpus"] = "2"
    end
    hub_config.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "#$MEMSIZE", "--natnet1", "172.16.1/24"]
      vb.gui = false
      vb.name = "hub"
    end
    hub_config.vm.provider "vmware_fusion" do |vmware|
      vmware.gui = false
      vmware.vmx["memsize"] = "#$MEMSIZE"
      vmware.vmx["numvcpus"] = 2
    end
  end
end
