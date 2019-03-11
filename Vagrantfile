Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.75.75"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "ftp-server"
    
    # Don't boot with headless mode
    # vb.gui = true
  
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
  config.vm.provision "shell", inline: <<-SHELL
	sudo apt-get update
	sudo apt-get -y install vsftpd
	sudo service vsftpd restart
  SHELL
end