# -*- mode: ruby -*-
# vi: set ft=ruby :

require "yaml"
# Go to the Vagrantfile folder, so relative path are resolved.
Dir.chdir(File.dirname(__FILE__))

# Load base parameters from YAML file.
params_file = "parameters.yml"
params = File.exists?(params_file) ? YAML::load_file(params_file) : {}

# If vagrant-trigger isn't installed then exit
if !Vagrant.has_plugin?("vagrant-triggers")
  puts "The 'vagrant-triggers' plugin is required."
  puts "It can be installed by running: vagrant plugin install vagrant-triggers"
  puts
  exit
end

Vagrant.configure(2) do |config|

  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
  else
    puts "The 'vagrant-hostmanager' plugin is recommended."
    puts "It can be installed by running: vagrant plugin install vagrant-hostmanager"
    puts
  end

  config.vm.box = "ubuntu/trusty64"

  config.vm.hostname = params.fetch('hostname', "drupal.dev")

  config.vm.network "private_network", ip: params.fetch('network_ip', "192.168.9.10")

  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true

  config.vm.synced_folder "./www", "/var/www", id: "webroot", type: params.fetch('synced_folder_type', "nfs")

  config.vm.usable_port_range = (2200..2250)
  config.vm.provider :virtualbox do |virtualbox|
    virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    virtualbox.customize ["modifyvm", :id, "--memory", params.fetch('memory', "512")]
    virtualbox.customize ["setextradata", :id, "--VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
  end

  # Install Ansible roles
  config.vm.provision "trigger", :stderr => true, :stdout => true do |trigger|
    trigger.fire do
      run "ansible-galaxy install --role-file=provisioning/requirements.yml --roles-path=provisioning/roles --ignore-errors"
    end
  end

  # Run Ansible playbook
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.extra_vars = {
      drush_version: params.fetch('drush_version', "6.*"),
      nginx: {
        server_name: params.fetch('hostname', "drupal.dev")
      }
    }
  end

  config.ssh.username = "vagrant"

  config.ssh.shell = "bash -l"

  config.ssh.keep_alive = true
  config.ssh.forward_agent = false
  config.ssh.forward_x11 = false
  config.vagrant.host = :detect
end
