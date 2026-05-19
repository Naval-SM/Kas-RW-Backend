# -- mode: ruby --
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # 1. KONFIGURASI VM DATABASE
  config.vm.define "vm-database" do |db|
    db.vm.box = "bento/ubuntu-22.04"
    db.vm.hostname = "vm-database"
    db.vm.network "private_network", ip: "192.168.56.11"
    db.vm.network "forwarded_port", guest: 80, host: 8081

    db.vm.provider "virtualbox" do |vb|
      vb.name = "vm-database"
      vb.memory = "1024"
      vb.cpus = 1
    end
  end

 # 2. KONFIGURASI VM FRONTEND (Sekarang berada di dalam blok Vagrant.configure)
  config.vm.define "vm-frontend" do |frontend|
    frontend.vm.box = "bento/ubuntu-22.04"
    frontend.vm.hostname = "vm-frontend"
    frontend.vm.network "private_network", ip: "192.168.56.12"
    frontend.vm.network "forwarded_port", guest: 80, host: 8082

    frontend.vm.provider "virtualbox" do |vb|
      vb.name = "vm-frontend"
      vb.memory = "1024"
      vb.cpus = 1
    end
  end

# 3. KONFIGURASI VM BACKEND
  config.vm.define "vm-backend" do |be|
    be.vm.box = "bento/ubuntu-22.04"
    be.vm.hostname = "vm-backend"
    be.vm.network "private_network", ip: "192.168.56.10"
    be.vm.network "forwarded_port", guest: 80, host: 8080

    be.vm.provider "virtualbox" do |vb|
      vb.name = "vm-backend"
      vb.memory = "1024"
      vb.cpus = 1
 end
  be.vm.provision "shell", inline: <<-SHELL
    cp /vagrant/ansible/insecure_private_key /home/vagrant/insecure_private_key
    chmod 600 /home/vagrant/insecure_private_key
    chown vagrant:vagrant /home/vagrant/insecure_private_key
  SHELL

  be.vm.provision "ansible_local" do |ansible|
    ansible.playbook       = "ansible/playbook.yml"
    ansible.inventory_path = "ansible/inventory"
    ansible.limit          = "all"
  end
end

end # Akhir dari Vagrant.configure	