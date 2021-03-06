VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "hashicorp/precise64"

  ansible_type = "ansible"

  config.vm.network "private_network", ip: "192.168.56.102"
    config.vm.provider :virtualbox do |v|
      config.vm.hostname = "appdev"
    end

    config.vm.provider :hyperv do |v|
      ansible_type = "ansible_local"
      config.vm.network :forwarded_port, guest: 80, host: 2202
      config.ssh.forward_agent = true
    end

  #Add any alias:
  config.hostsupdater.aliases = [
    "app.dev",
    "phpmyadmin.app.dev"
  ]

  nfs_setting = RUBY_PLATFORM =~ /darwin/ || RUBY_PLATFORM =~ /linux/
  config.vm.synced_folder ".", "/vagrant", :nfs => nfs_setting

  #Fix for Ansible bug resulting in an encoding error
  ENV['PYTHONIOENCODING'] = "utf-8"

  config.vm.provision ansible_type do |ansible|
    ansible.limit = 'all'
    ansible.playbook = "ansible/development.yml"
    ansible.inventory_path = "ansible/hosts"
  end

  config.vm.post_up_message = "\n\nProvisioning is done, visit http://app.dev for your CakePHP application! \n\nVisit http://phpmyadmin.app.dev for phpMyAdmin (MySQl credentials are root:temppassword).\n\n"
end
