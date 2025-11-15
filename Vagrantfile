Vagrant.configure("2") do |config|

#  config.vm.provision "shell", path: "script.sh"
#  config.vm.provision "ansible_local" do |ansible|
#  	ansible.playbook = "playbook.yml"
#	ansible.install_mode = "pip"
#  end

 config.vm.define "controle2" do |controle2|
 	controle2.vm.box = "shekeriev/debian-12"
        controle2.vm.hostname = "controle2"
#        controle.vm.network "public_network", ip: "192.168.15.70"
        controle2.vm.network "private_network", ip: "172.17.177.150"
        controle2.vm.provider "virtualbox" do |vb|
                vb.name = "controle2"
                vb.memory = "8096"
                vb.cpus = 3
  	end
	controle2.vm.provision "ansible_local" do |al|
		al.playbook = "installjenkins.yml"
		al.install_mode = "apt"
	end
  end

  config.vm.define "agent" do |agent|
        agent.vm.box = "shekeriev/debian-12"
        agent.vm.hostname = "agent"
        agent.vm.network "private_network", ip: "172.17.177.151"
        agent.vm.provider "virtualbox" do |vb|
                vb.name = "agent"
                vb.memory = "512"
                vb.cpus = 2
        end
  end
#
#  config.vm.define "db" do |db|
#        db.vm.box = "shekeriev/debian-11"
#        db.vm.hostname = "db"
##        db.vm.network "public_network", ip: "192.168.15.72"
#        db.vm.network "private_network", ip: "172.17.177.102"
#        db.vm.provider "virtualbox" do |vb|
#                vb.name = "db"
#                vb.memory = "512"
#                vb.cpus = 2
#        end
#  end
#
#  config.vm.define "puppet" do |puppet|
#        puppet.vm.box = "shekeriev/debian-12"
#        puppet.vm.hostname = "puppet"
#        puppet.vm.network "private_network", ip: "172.17.177.200"
#        puppet.vm.provider "virtualbox" do |vb|
#                vb.name = "puppet"
#                vb.memory = "4096"
#                vb.cpus = 2
#        end
#        puppet.vm.provision "shell", inline: "echo 'nameserver 8.8.8.8' > /etc/resolv.conf"
#	puppet.vm.provision "puppet" do |puppet|
#   		puppet.manifests_path = "manifests"
#    		puppet.manifest_file = "site.pp"
#	end

	
#  end

end
