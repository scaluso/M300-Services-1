# M300-Services

## LB2

### [Dokumentation](https://github.com/scharfschiesser/M300-Services/wiki)

### vsftpd Server installieren, konfigurieren und mit einem FTP-Client erfolgreich auf die VM zugreifen

#### Verzeichnis für Vagrant Box erstellen.

```
mkdir ftp-server
cd ftp-server
```

#### Vagrant Box herunterladen.

`vagrant box add http://10.1.66.11/vagrant/ubuntu/xenial64.box --name ubuntu/xenial64`

#### Vagrantfile erstellen.

`vagrant init ubuntu/xenial64`

#### Vagrantfile anpassen damit Vagrant das Netzwerk richtig konfiguriert und vsftpd installiert.

```
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
```
Link zum Vagrantfile: [hier](https://github.com/scharfschiesser/M300-Services/blob/master/Vagrantfile)

#### Mittels SSH auf die VM zugreifen um die vsftpd.conf Datei anzupassen.

`vagrant ssh`

#### vsftpd.conf anpassen.

`sudo nano /etc/vsftpd.conf`

`write_enable=YES`

#### Mit einem FTP-Client auf die VM zugreifen.

In diesem Beispiel wurde FileZilla benutzt.

`Host: 192.168.75.75`

`Username: vagrant`

`Password: vagrant`

![FileZilla](https://i.imgur.com/iawaZG9.png)
