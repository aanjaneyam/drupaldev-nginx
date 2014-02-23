Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.hostname = "drupal.dev"

  config.vm.network :private_network, ip: "33.33.33.10"
    config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--memory", 2048]
  end

  config.vm.synced_folder "./sites", "/var/www", :nfs => true
  config.vm.provision :shell, :inline => "sudo apt-get update"
  config.vm.provision :shell, :path => "upgrade_puppet.sh"

  config.vm.provision :puppet do |puppet|
    puppet.facter = {
      "ssh_username" => "vagrant"
    }

    puppet.manifests_path = "manifests"
    puppet.module_path = "modules"
    puppet.options = ["--verbose", "--hiera_config /vagrant/hiera.yaml"]
  end

  config.ssh.username = "vagrant"
end
