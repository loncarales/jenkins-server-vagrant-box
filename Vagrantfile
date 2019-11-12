# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT
yum install -y epel-release
yum -y update
yum install -y net-tools
yum install -y wget
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
yum install -y jenkins
yum install -y java-1.8.0-openjdk-devel.x86_64
yum install -y git
yum install -y python3
systemctl start jenkins.service
systemctl enable jenkins.service
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce docker-ce-cli containerd.io
systemctl start docker
systemctl enable docker
usermod -aG docker vagrant
usermod -aG docker jenkins
yum -y install ntpdate ntp
timedatectl set-timezone "Europe/Berlin"
timedatectl set-ntp true
hwclock --systohc
ntpd -gq
systemctl start ntpd
systemctl enable ntpd
SCRIPT

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.boot_timeout = 900
  config.vm.graceful_halt_timeout=100

  # vagrant-vbguest
  # set auto_update to false, if you do NOT want to check the correct
  # additions version when booting this machine
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end

  # vagrant-hostmanager
  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.manage_guest = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
  end

  config.vm.box = "bento/centos-7.4"

  # Don't check for box update
  config.vm.box_check_update = false

  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/mitchellh/vagrant/issues/5005
  config.ssh.insert_key = false

  # Control Machine
  config.vm.define "jenkins-server", primary: true do |h|
    h.vm.hostname = 'jenkins-server.site'
    h.vm.network "private_network", ip: "192.168.50.10"
    h.vm.network "forwarded_port", guest: 8080, host: 8080 # jenkins

    # Customize VirtualBox Provider
    h.vm.provider "virtualbox" do |vb|
      vb.name = "jenkins-server"
      vb.memory = 2048
    end

    # Provision
    h.vm.provision "shell", inline: $script
  end

end
