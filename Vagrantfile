# -*- mode: ruby -*-

# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  #config.vm.provision "docker"
  config.vm.provision "docker" do |d|
    d.post_install_provision "shell", path: "set-docker-mirror.sh"
  end

  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http = "http://180.166.223.108:10015/"
    config.proxy.https = "http://180.166.223.108:10015/"
    config.proxy.no_proxy = "127.0.0.1,localhost,.philips.com,.philips.com.cn"
  end

  config.vm.define "devops-server" do | host |
    host.vm.hostname = "devops-server"
    host.vm.network :private_network, ip: "10.10.10.10"
    host.vm.network :forwarded_port, guest: 8080, host: 8001

    host.vm.provider "virtualbox" do |v|
      v.name = "devops-server"
      v.memory = 1024
      v.cpus = 2
    end

    host.vm.provision "docker" do |d|
      d.build_image "/vagrant/jenkins", args: "-t jenkins"
      d.run "jenkins", args: "-p 8080:8080 -p 5000:5000 -v /vagrant/var/jenkins_home:/var/jenkins_home -e TRY_UPGRADE_IF_NO_MARKER=true"
    end
  end
end