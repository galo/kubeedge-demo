# -*- mode: ruby -*-
# vi: set ft=ruby :


use_proxy = false
use_proxy = ENV['use_proxy'].to_s == 'false' ? false : true  if ENV['use_proxy']
url_proxy = ENV['http_proxy'] || 'http://web-proxy.corp.hp.com:8080'
not_proxy = ENV['no_proxy'] || 'localhost,127.0.0.1,.hpicorp.net,15.0.0.0/8,10.0.0.0/8,192.168.0.0/16'


# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"


  # Proxy configuration:
  #
  if use_proxy
    if Vagrant.has_plugin?('vagrant-proxyconf')
      config.proxy.http     = url_proxy
      config.proxy.https    = url_proxy
      config.proxy.no_proxy = not_proxy
      config.proxy.enabled  = { npm: false }
    else
      puts '.'
      puts 'ERROR: Could not find vagrant-proxyconf plugin.'
      puts 'INFO: This plugin is required to use this box inside HPinc network.'
      puts 'INFO: $ vagrant plugin install vagrant-proxyconf'
      puts 'ERROR: Bailing out.'
      puts '.'
      exit 1
    end
  end

  # VB Guest Additions configuration:
  #
  if Vagrant.has_plugin?('vagrant-vbguest')
    config.vbguest.auto_reboot = true
    # Do **NOT** force update if image already has pre-installed vbguest
    # - https://forums.virtualbox.org/viewtopic.php?f=7&t=86819
    # - https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1783267
    # - https://stackoverflow.com/questions/37556968/vagrant-disable-guest-additions
    # 
    config.vbguest.auto_update = false
  else
    puts '.'
    puts 'WARN: Could not find vagrant-vbguest plugin.'
    puts 'INFO: This plugin is highly recommended as it ensures that your VB guest additions are up-to-date.'
    puts 'INFO: $ vagrant plugin install vagrant-vbguest'
    puts '.'
  end


  # Cache configuration:
  #
  if Vagrant.has_plugin?('vagrant-cachier')
    config.cache.scope = :machine
    config.cache.auto_detect = false
    config.cache.enable :apt_lists
    config.cache.enable :apt
  else
    puts '.'
    puts 'WARN: Could not find vagrant-cachier plugin.'
    puts 'INFO: This plugin is highly recommended as it reduces the time required to provision the devenv.'
    puts 'INFO: $ vagrant plugin install vagrant-cachier'
    puts '.'
  end

  # Port Forwarding:
  # Use ~/.vagrant.d/Vagrantfile for adding your service custom ports.
  #
  config.vm.network :forwarded_port, guest: 8000,  host: 8000,  auto_correct: true # debug
  config.vm.network :forwarded_port, guest: 8001,  host: 8001,  auto_correct: true # kubectl proxy
  config.vm.network :forwarded_port, guest: 8080,  host: 8080,  auto_correct: true # http
  config.vm.network :forwarded_port, guest: 8443,  host: 8443,  auto_correct: true # https
  config.vm.network :forwarded_port, guest: 30000, host: 30000, auto_correct: true # localkube dashboard

  
  config.vm.network :public_network, bridge: 'eth0'



  config.vm.provider :virtualbox do |vb|
    vb.name = 'devenv'
    vb.customize ['modifyvm', :id, '--memory', '2048']
    vb.customize ['modifyvm', :id, '--cpus', '2']
    vb.customize ['modifyvm', :id, '--ioapic', 'on']
    vb.customize ['modifyvm', :id, '--cableconnected1', 'on']
  end

  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  config.vm.provision "docker" do |d|
    d.run "hello-world"
  end
  
end
