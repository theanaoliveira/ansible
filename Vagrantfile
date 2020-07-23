Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 512
    vb.cpus = 1
  end

  config.vm.define "ansible" do |ansible|
  
    ansible.vm.provider "virtualbox" do |machine|
      machine.name = "alura_ansible"
    end
  
    ansible.vm.network "private_network", ip: "192.168.40.1"
    ansible.vm.provision "shell", inline: "apt update && \
                                           apt install -y software-properties-common && \
                                           apt-add-repository --yes --update ppa:ansible/ansible && \
                                           apt install -y ansible && \
                                           cp /vagrant/id_ansible /home/vagrant && \
                                           chmod 600 /home/vagrant/id_ansible && \
                                           chown vagrant:vagrant /home/vagrant/id_ansible"
  end

  config.vm.define "wordpress" do |wordpress|
    wordpress.vm.network "private_network", ip: "192.168.40.2"
    wordpress.vm.provision "shell", inline: "cat /vagrant/id_ansible.pub >> .ssh/authorized_keys && \
                                             apt update && \
                                             add-apt-repository --yes --update ppa:ondrej/php && \
                                             apt update"
  end

  config.vm.define "mysql" do |mysql|

    mysql.vm.provider "virtualbox" do |machine|
      machine.name = "alura_mysql"
    end

    mysql.vm.network "private_network", ip: "192.168.40.3"
    mysql.vm.provision "shell", inline: "cat /vagrant/id_ansible.pub >> .ssh/authorized_keys"
  end
end
