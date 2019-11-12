# Jenkins the easy way

Jenkins server running on CentOS 7.4 in a Vagrant Box

## Requirements

* [Git](https://git-scm.com/)
* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)

## Installed software after box provisioning

* Jenkins 2.X
* Git 1.8
* Java 8
* Python 3.6
* Docker server 19.X

## Prerequisits

These Vagrant plugins are needed

```bash
$ vagrant plugin install vagrant-vbguest
$ vagrant plugin install vagrant-hostmanager

```

## Create and configure machine according to Vagrantfile

Hit vagrant up and sit back. You'll be asked for Administrator password, vagrant-hostmanager will update your hosts file accordingly.

```bash
$ vagrant up
```

## SSH into a running Vagrant machine

You can ssh via vagrant or directly to your machine via hostname.

```bash
$ vagrant ssh
```

```bash
# Find your private Vagratn box ssh key
$ vagrant ssh-config
# Use this key to log into the box
$ ssh vagrant@jenkins-server.site -i /Users/XXXXXX/.vagrant.d/insecure_private_key
```

## Get the content of Jenkins Administrator password

```bash
# After you've logged in
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

## Access Jenkins

Open [http://jenkins-server.site:8080/](http://jenkins-server.site:8080/) in your browser and proceed with the installation.
