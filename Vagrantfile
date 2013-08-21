# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant::Config.run do |config|
  config.vm.customize ["modifyvm", :id, "--memory", 512]
  config.vm.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]

  config.vm.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  config.vm.customize ["modifyvm", :id, "--natdnsproxy1", "on"]

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  
  config.vm.host_name = "logging-server-backend.local"
  config.vm.network :hostonly, "10.10.10.98"

  #Fixes ownership bug
  config.vm.share_folder "vagrant-root", "/vagrant", ".", :owner => "vagrant"

  # Provision docker and new kernel if deployment was not done
  if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
    # Add lxc-docker package
    pkg_cmd = "apt-get update -qq; apt-get install -q -y build-essential; " \
      "apt-get install -q -y libssl-dev; apt-get install -q -y wget; " \
      "apt-get install -q -y git; "\
      "wget -O /tmp/otp_src_R16B01.tar.gz http://www.erlang.org/download/otp_src_R16B01.tar.gz; " \
      "cd /tmp; tar xzvf otp_src_R16B01.tar.gz; cd otp_src_R16B01; ./configure; " \
      "make; make install;"
    config.vm.provision :shell, :inline => pkg_cmd
  end

end

