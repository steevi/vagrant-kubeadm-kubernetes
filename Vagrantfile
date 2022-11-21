NUM_WORKER_NODES=2
IP_NW="192.168.56."
IP_START=10

Vagrant.configure("2") do |config|
  config.vm.provision "shell", env: {"IP_NW" => IP_NW, "IP_START" => IP_START}, inline: <<-SHELL
      apt-get update -y
      echo "$IP_NW$((IP_START)) cks-master" >> /etc/hosts
      echo "$IP_NW$((IP_START+1)) cks-worker01" >> /etc/hosts
  SHELL

  config.vm.box = "generic/ubuntu2004"
  config.vm.box_check_update = true
  config.vm.synced_folder ".", "/vagrant", disabled: false

  config.vm.define "cks-master" do |cksmaster|
    # master.vm.box = "bento/ubuntu-18.04"
    cksmaster.vm.hostname = "cks-master"
    cksmaster.vm.network "private_network", ip: IP_NW + "#{IP_START}"
    cksmaster.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
    end
    cksmaster.vm.provision "shell", path: "scripts/install_master.sh"
  end

  (1..NUM_WORKER_NODES).each do |i|

  config.vm.define "cks-worker0#{i}" do |cksworker|
    cksworker.vm.hostname = "cks-worker0#{i}"
    cksworker.vm.network "private_network", ip: IP_NW + "#{IP_START + i}"
    cksworker.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
    end
    cksworker.vm.provision "shell", path: "scripts/install_worker.sh"
  end

  end
end 
