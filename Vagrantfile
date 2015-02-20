VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "withinboredom/Trusty64"

  config.vm.provider :virtualbox do |v|
    config.vm.network "private_network", ip: "192.168.13.37"
    config.vm.hostname = "appdev"
  end

  config.vm.provider :hyperv do |v|
    config.vm.network :private_network, ip: "192.168.56.102"
      config.vm.network :forwarded_port, guest: 80, host: 2201
      config.ssh.forward_agent = true
  end

  #Add any alias:
  config.hostsupdater.aliases = [
    "app.dev",
    "phpmyadmin.app.dev"
  ]

  config.vm.synced_folder ".", "/vagrant", :nfs => true

  #Fix for Ansible bug resulting in an encoding error
  ENV['PYTHONIOENCODING'] = "utf-8"

  config.vm.provision "ansible" do |ansible|
    ansible.limit = 'all'
    ansible.playbook = "ansible/development.yml"
    ansible.inventory_path = "ansible/hosts"
  end

  config.vm.post_up_message = "\n\nProvisioning is done, visit http://app.dev for your CakePHP application! \n\nVisit http://phpmyadmin.app.dev for phpMyAdmin (MySQl credentials are root:temppassword).\n\n"
end
