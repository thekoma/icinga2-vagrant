# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vagrant.plugins = [ "vagrant-recreate", "vagrant-libvirt", "vagrant-mutate" ]
  # config.vm.box          = "generic/rocky8" # Still prefer stream but you know...
  config.vm.box          = "centos/stream8"
  config.vm.hostname     = "icinga"
  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 443, host: 8443, host_ip: "127.0.0.1"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provision "ansible" do |ansible|
    # ansible.verbose            = "-vvv"
    ansible.compatibility_mode = "2.0"
    ansible.galaxy_role_file   = "src/requirements.yml"
    ansible.playbook           = "src/playbook.yaml"
    ansible.galaxy_command     = "ansible-galaxy collection install -r %{role_file}"
    ansible.extra_vars        = "src/extra_vars.yaml"
  end

  config.vm.provider "libvirt" do |vb|
    vb.memory = "4096"
    vb.cpus   = "4"
  end

end
