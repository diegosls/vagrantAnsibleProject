# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Box base
  config.vm.box = "debian/bookworm64"
  config.ssh.insert_key = false
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end


  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.linked_clone = true
  end

  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.diego.igor.devops"
    arq.vm.network :private_network, ip: "192.168.56.134"
    (1..3).each do |i|
      arq.vm.disk :disk, size: "10GB", name: "disk#{i}"
      arq.vm.provider :virtualbox do |vb|
        vb.gui = true
      end
    end
  end

  config.vm.define "db" do |db|
    db.vm.hostname = "db.diego.igor.devops"
    db.vm.network :private_network, type: "dhcp"
  end

  config.vm.define "app" do |app|
    app.vm.hostname = "app.diego.igor.devops"
    app.vm.network :private_network, type: "dhcp"
  end

  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.diego.igor.devops"
    cli.vm.network :private_network, type: "dhcp"
    cli.vm.provider :virtualbox do |v|
      v.memory = 1024
    end
  end
end

