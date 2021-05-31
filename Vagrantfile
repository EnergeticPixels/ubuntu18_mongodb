# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_version = "20210526.0.0"

  config.vm.network "forwarded_port", guest: 27017, host: 27017

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.synced_folder ".", "/vagrant"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = 2048
    vb.cpus = 4
    vb.name = "single-mongo"
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    sudo add-apt-repository 'deb [arch=amd64] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse'
    sudo apt-get update
    sudo apt-get install -y mongodb-org

    echo "    CHANGING MONGODB CONFIG    "
    sudo rm -f /etc/mongod.conf
    sleep 2
    sudo cp ../../vagrant/vagBuild/pri-mongodb.conf /etc/mongod.conf
    sudo chmod 644 /etc/mongod.conf

    echo "    STARTING MONGODB     "
    sudo systemctl start mongod
    sudo systemctl status mongod


    sleep 2

    echo "    MODIFY mongod.conf for security turned on     "
    sudo sh -c 'echo "\nsecurity: \n  authorization: enabled" >> /etc/mongod.conf'

    echo "    CREATE MongoDB Root User and User Admin    "
    mongo --eval "db=db.getSiblingDB('admin');db.createUser(
      {
        user: 'pers_admin', 
        pwd: 'password', 
        roles: [
          {
            role: 'userAdmin', 
            db: 'admin'
          }
        ]
      }
    );"

     echo "      RESTARTING MONGOD        "
    sudo systemctl restart mongod
    sudo systemctl status mongod

    sudo systemctl enable mongod

  SHELL
end
