# variables
docker_worker_count = ENV['DOCKER_WORKER_COUNT']&.to_i || 3


Vagrant.configure "2" do |config|
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
  else
    config.vm.synced_folder ".", "/vagrant"
  end

  config.vm.box = "generic/centos7"

  config.vm.define "docker-swarm-01" do |manager|
    manager.vm.network "private_network", ip: "192.168.217.133"
    manager.vm.hostname = "docker-swarm-01"

    manager.vm.provision "docker" do |docker|
      docker.post_install_provision "shell", inline: <<-SCRIPT
        docker swarm init --advertise-addr 192.168.217.133
        docker swarm join-token --quiet worker > /vagrant/docker-swarm-token

      SCRIPT
    end

    manager.vm.provision "file", source: "daemon.json", destination: "/tmp/daemon.json"
    manager.vm.provision "file", source: "override.conf", destination: "/tmp/override.conf"
    manager.vm.provision "shell", inline: "mkdir -p /etc/systemd/system/docker.service.d", privileged: true
    manager.vm.provision "shell", inline: "cat /tmp/override.conf > /etc/systemd/system/docker.service.d/override.conf", privileged: true
    manager.vm.provision "shell", inline: "cat /tmp/daemon.json > /etc/docker/daemon.json", privileged: true
    manager.vm.provision "shell", inline: "firewall-cmd --zone=public --add-port=2375/tcp", privileged: true
    manager.vm.provision "shell", inline: "firewall-cmd --zone=public --add-port=2377/tcp", privileged: true
    manager.vm.provision "shell", inline: "systemctl daemon-reload", privileged: true
    manager.vm.provision "shell", inline: "systemctl restart docker", privileged: true
  end

  (2..docker_worker_count).each do |i|
    worker_name = "docker-swarm-0#{i}"
    worker_ip = "192.168.217.#{132 + i}"
    config.vm.define worker_name do |worker|
      worker.vm.network "private_network", ip: worker_ip
      worker.vm.hostname = worker_name

      worker.vm.provision "docker" do |docker|
        docker.post_install_provision "shell",
          inline: "docker swarm join --advertise-addr #{worker_ip} --token $(cat /vagrant/docker-swarm-token) 192.168.217.133"
      end
      
      worker.vm.provision "file", source: "daemon.json", destination: "/tmp/daemon.json"
      worker.vm.provision "file", source: "override.conf", destination: "/tmp/override.conf"
      worker.vm.provision "shell", inline: "mkdir -p /etc/systemd/system/docker.service.d", privileged: true
      worker.vm.provision "shell", inline: "cat /tmp/override.conf > /etc/systemd/system/docker.service.d/override.conf", privileged: true
      worker.vm.provision "shell", inline: "cat /tmp/daemon.json >> /etc/docker/daemon.json", privileged: true
      worker.vm.provision "shell", inline: "firewall-cmd --zone=public --add-port=2375/tcp", privileged: true
      worker.vm.provision "shell", inline: "systemctl daemon-reload", privileged: true
      worker.vm.provision "shell", inline: "systemctl restart docker", privileged: true
    end
  end

  config.vm.provider "virtualbox" do |v|
    v.cpus = 2
    v.memory = 2048
  end
end
