Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "apt update && apt install vim -y"

  config.vm.define "controle2" do |controle2|
    controle2.vm.box = "shekeriev/debian-12"
    controle2.vm.network "private_network", ip: "172.17.177.150"
    controle2.vm.hostname = "controle2"
    controle2.vm.provision "shell", inline: "apt install git -y"
    controle2.vm.provision "ansible_local" do |al|
      al.playbook = "installdocker.yml"
      al.install_mode = "apt"
    end
    controle2.vm.provision "ansible_local" do |al|
      al.playbook = "installjenkins.yml"
      al.install_mode = "apt"
    end

    controle2.vm.provider "virtualbox" do |vb|
      vb.name = "controle2"
      vb.memory = "6000"
      vb.cpus = 2
    end
  end


end
