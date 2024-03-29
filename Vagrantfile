# -*- mode: ruby -*-
# # vi: set ft=ruby :

require "yaml"
VAGRANTFILE_API_VERSION = "2"

ANSIBLE_PATH = __dir__
default_sites_config = File.join(ANSIBLE_PATH, 'playbooks/group_vars', 'all', 'sites.yml')
config_file = File.join(ANSIBLE_PATH, 'playbooks/group_vars', 'all', 'main.yml')
sites_config_path = YAML.load_file(config_file)['sites_config']

if defined?(sites_config_path)
  sites_config = File.expand_path(sites_config_path)
else
  sites_config = File.expand_path(default_sites_config)
end

def local_site_path(site)
  File.expand_path(site['local_path'], ANSIBLE_PATH)
end

def remote_site_path(site_name)
  "/srv/www/#{site_name}/web"
end

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 1
  end

  # Box
  config.vm.box = "ubuntu/trusty64"

  # Networking
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "private_network", ip: "192.168.50.4"

  # Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
      'localhost' => ['default']
    }

    ansible.playbook = "playbooks/localhost.yml"
  end

  # Hosts
  if File.exists?(config_file)
    hosted_sites = YAML.load_file(sites_config)['hosted_sites']
    fail_with_message "No sites found in #{sites_config}." if hosted_sites.to_h.empty?
  else
    fail_with_message "#{sites_config} was not found. Please set `ANSIBLE_PATH` in your Vagrantfile."
  end

  hostname, *aliases = hosted_sites.flat_map { |(_name, site)| site['site_hosts'] }
  config.vm.hostname = hostname
  www_aliases = ["www.#{hostname}"] + aliases.map { |host| "www.#{host}" }

  if Vagrant.has_plugin? 'vagrant-hostsupdater'
    config.hostsupdater.aliases = aliases + www_aliases
  else
    fail_with_message "vagrant-hostsupdater missing, please install the plugin with this command:\nvagrant plugin install vagrant-hostsupdater"
  end

  # Synced Folders
  hosted_sites.each_pair do |name, site|
    config.vm.synced_folder local_site_path(site), remote_site_path(name), type: 'nfs', map_uid: 0, map_gid: 0
  end

end
