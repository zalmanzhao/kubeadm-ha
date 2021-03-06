# -*- mode: ruby -*-
# vi: set ft=ruby :

$centos_script = <<-SCRIPT
mv /etc/yum.repos.d /etc/yum.repos.d.orig.$(date -Iseconds)
mkdir -p /etc/yum.repos.d/
wget -qO /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
SCRIPT

$rhel7_script = <<-SCRIPT
mv /etc/yum.repos.d /etc/yum.repos.d.orig.$(date -Iseconds)
mkdir -p /etc/yum.repos.d/
wget -qO /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
sed -i 's/$releasever/7/g' /etc/yum.repos.d/CentOS-Base.repo
sed -i 's/PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd
SCRIPT

$debian_script = <<-SCRIPT
sed -i 's security.debian.org mirrors.aliyun.com g' /etc/apt/sources.list
sed -i 's httpredir.debian.org mirrors.aliyun.com g' /etc/apt/sources.list
SCRIPT

$ubuntu_script = <<-SCRIPT
sed -i 's security.ubuntu.com mirrors.aliyun.com g' /etc/apt/sources.list
sed -i 's archive.ubuntu.com mirrors.aliyun.com g' /etc/apt/sources.list
SCRIPT

Vagrant.configure(2) do |config|
    config.vm.define "node1" do |s|
        s.vm.box = "bento/centos-7.4"
        s.vm.box_url = "http://files.saas.hand-china.com/vagrant/bento_centos-7.4.box"
        s.vm.hostname = "node1"
        s.vm.network "private_network", ip: "192.168.56.11"
        # s.vm.network "forwarded_port", guest: 6443, host: 6443
        # s.vm.network "forwarded_port", guest: 8443, host: 8443
        s.vm.provision "shell", inline: $centos_script
        s.vm.provider "virtualbox" do |v|
            v.memory = 4096
            v.cpus = 2
        end
    end
    config.vm.define "node2" do |s|
        s.vm.box = "generic/rhel7"
        s.vm.box_url = "http://files.saas.hand-china.com/vagrant/generic_rhel7.box"
        s.vm.hostname = "node2"
        s.vm.network "private_network", ip: "192.168.56.12"
        s.vm.provision "shell", inline: $rhel7_script
        s.vm.provider "virtualbox" do |v|
            v.memory = 4096
            v.cpus = 2
        end
    end
    config.vm.define "node3" do |s|
        s.vm.box = "bento/debian-9.6"
        s.vm.box_url = "http://files.saas.hand-china.com/vagrant/bento_debian-9.6.box"
        s.vm.hostname = "node3"
        s.vm.network "private_network", ip: "192.168.56.13"
        s.vm.provision "shell", inline: $debian_script
        s.vm.provider "virtualbox" do |v|
            v.memory = 4096
            v.cpus = 2
        end
    end
    config.vm.define "node4" do |s|
        s.vm.box = "bento/ubuntu-16.04"
        s.vm.box_url = "http://files.saas.hand-china.com/vagrant/bento_ubuntu-16.04.box"
        s.vm.hostname = "node4"
        s.vm.network "private_network", ip: "192.168.56.14"
        s.vm.provision "shell", inline: $ubuntu_script
        s.vm.provider "virtualbox" do |v|
            v.memory = 4096
            v.cpus = 2
        end
    end
end