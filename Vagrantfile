# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "opentable/win-2012r2-standard-amd64-nocm"

  config.vm.hostname = "windows-2012"
  config.vm.guest = :windows

  config.vm.communicator = "winrm"
  config.winrm.username = "Administrator"
  config.winrm.password = "vagrant"

  config.winrm.transport = :plaintext
  config.winrm.basic_auth_only = true
  config.vm.network "forwarded_port", guest: 3389, host: 3389, id: "rdp", auto_correct: true
  config.vm.network "forwarded_port", guest: 5985, host: 5985, id: "winrm", auto_correct: true
  # Set VM network type
  config.vm.network "private_network", type: "dhcp"

  config.windows.set_work_network = true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true

    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
  end

  # Execute external script
  config.vm.provision "shell", path: "script.ps1"

  # Teamcity Build Agent provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playwinagent.yml"
    ansible.extra_vars = {
      ansible_user: "Administrator",
      ansible_password: "vagrant",
      ansible_port: 5986,
      ansible_connection: "winrm",  teamcity_server_url: "http://ec2-teamcity-ip-address.compute-1.amazonaws.com" }
    ansible.host_key_checking = false
    ansible.groups = {
      "windows-agent" => ["default"]
    }
  end


end
