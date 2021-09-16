# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vagrant.plugins = [ "vagrant-env", "vagrant-recreate", "vagrant-libvirt", "vagrant-mutate" ]
  config.env.enable

  config.vm.define "master" do |master|
    master.vm.box          = "centos/stream8"
    master.vm.hostname     = "master"
    master.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
    master.vm.network "forwarded_port", guest: 443, host: 8443, host_ip: "127.0.0.1"
    master.vm.synced_folder ".", "/vagrant", disabled: true
    master.vm.provider "libvirt" do |vb|
      vb.memory = "4096"
      vb.cpus   = "4"
    end
  end
  
  N=1
  (1..N).each do |machine_id|
    config.vm.define "satellite-#{machine_id}" do |machine|
      machine.vm.box          = "centos/stream8"
      machine.vm.hostname     = "satellite-#{machine_id}"
      machine.vm.synced_folder ".", "/vagrant", disabled: true
      machine.vm.provider "libvirt" do |vb|
        vb.memory = "4096"
        vb.cpus   = "4"
      end
      if machine_id == N
        machine.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.galaxy_command     = "ansible-galaxy collection install -r %{role_file}"
          ansible.limit              = "all"
          ansible.galaxy_role_file   = "provisioning/requirements.yml"
          ansible.extra_vars         = "provisioning/vars.yaml"
          ansible.playbook           = "provisioning/main.yaml"
          # ansible.verbose            = "-v"
          ansible.groups = {
            "masters" => ["master"]
          }
        end
      end
    end
  end
end
