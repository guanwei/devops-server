# -*- mode: ruby -*-

# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  required_plugins = %w( vagrant-vbguest vagrant-disksize )
  required_plugins.each do |plugin|
    system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
  end

  config.vm.box = "ubuntu/xenial64"
  config.vm.box_check_update = false
  config.vbguest.auto_update = false
  
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
    host.vm.network :forwarded_port, guest: 8081, host: 8002
    host.disksize.size = "100GB"

    host.vm.provider "virtualbox" do |v|
      v.name = "devops-server"
      v.memory = 2048
      v.cpus = 2
    end

    host.vm.synced_folder "data/jenkins", "/data/jenkins", mount_options: ["uid=1000"]
    host.vm.synced_folder "data/artifactory", "/data/artifactory", mount_options: ["uid=1030"]

    host.vm.provision "docker" do |d|
      d.build_image "/vagrant/docker/jenkins", args: "-t jenkins"
      d.run "jenkins", args: "-p 8080:8080 -p 5000:5000 -v /data/jenkins:/var/jenkins_home -e TRY_UPGRADE_IF_NO_MARKER=true"
      d.build_image "/vagrant/docker/artifactory", args: "-t artifactory"
      d.run "artifactory", args: "-p 8081:8081 -v /data/artifactory:/var/opt/jfrog/artifactory"
    end
  end
end