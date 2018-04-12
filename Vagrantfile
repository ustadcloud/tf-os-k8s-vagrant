VAGRANTFILE_API_VERSION = "2"

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
#  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
#  config.vm.provision "shell", path: "scripts/resolve-conf.sh"
#  config.vm.provision "shell", path: "scripts/ntp.sh"
  config.vm.provision "shell", path: "scripts/enable_root_login.sh"
  config.vm.provision "shell", path: "scripts/set-centos-gw.sh"
#  config.vm.provision "shell", path: "scripts/kernel-update.sh"
  config.vm.provision "file", source: "instances.yaml", destination: "/home/vagrant/instances.yaml"
  config.vm.provision "shell", path: "scripts/aio-ansible-os-k8s.sh"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 32768
    vb.cpus = 6
    vb.customize ["setextradata", :id, "VBoxInternal/CPUM/SSE4.1", "1"]
    vb.customize ["setextradata", :id, "VBoxInternal/CPUM/SSE4.2", "1"]
    vb.customize ["modifyvm", :id, "--paravirtprovider", "kvm"]
  end

  config.vm.define "aio-node", primary: true do |aio|
    aio.vm.network "public_network", bridge: "br0", ip: "10.13.82.34"
    aio.vm.network "public_network", bridge: "br1", ip: "192.168.1.34"
#    aio.vm.provision "shell",inline: "route add default gw 10.13.82.1"
#    aio.vm.provision "shell",inline: "eval `route -n | awk '{ if ($8 ==\"enp0s3\" && $2 != \"0.0.0.0\") print \"route del default gw \" $2; }'`"  
    aio.vm.hostname = "aio-node"
  end
end
