Vagrant Docker Swarm
====================

Although [Kubernetes](https://kubernetes.io/) has become the _de facto standard_ for container orchestration we still find environments that use [Docker Swarm mode](https://docs.docker.com/engine/swarm/).  This repo provides the configuration to spin up a Docker Swarm mode cluster using [Vagrant](https://www.vagrantup.com/).

### Testing environment

The configuration in this repository is designed and tested to run on a Windows 10 environment (tested with an Intel i7 laptop with 8GB RAM) using [Vagrant](https://www.vagrantup.com/), [Docker Toolbox](https://github.com/docker/toolbox) and [VirtualBox](https://www.virtualbox.org/).  
_Note_: [Docker Desktop](https://www.docker.com/products/docker-desktop) is now the preferred method to install Docker support on WIndows 10 so it should work with that too but for my requirements in this particular example I needed to work with the older Docker Toolbox.

Tested versions of products (may work with different versions):

* Windows 10 20H2
* Docker Toolbox [19.03.1](https://github.com/docker/toolbox/releases/download/v19.03.1/DockerToolbox-19.03.1.exe)
* VirtualBox [6.1.16](https://download.virtualbox.org/virtualbox/6.1.16/VirtualBox-6.1.16-140961-Win.exe) (updated after installing Docker Toolbox)
* Vagrant - Windows 64-bit [2.2.13](https://releases.hashicorp.com/vagrant/2.2.13/vagrant_2.2.13_x86_64.msi)
* Git for Windows 64-bit [2.29.2](https://github.com/git-for-windows/git/releases/download/v2.29.2.windows.2/Git-2.29.2.2-64-bit.exe)

### Architecture layout
![Alt text](images/architecture.png "Architecture layout")

In the above diagram we can see that after deployment we will have a Docker Swarm mode 3-node cluster with a single manager and 2 x worker nodes.  The design is as simple as possible to demonstrate some higher level concepts of working with Swarm and purposely avoids production level aspects such as TLS certificates and high availability.  All that we want for our purposes is a simple 3-node cluster.

### Spinning up the environment

The following steps will bring up the Docker swarm mode cluster once you have all the products installed detailed in the [Testing environment](#testing-environment) section.  Depending on the speed of your system and Internet connection the environment should hopefully be up and running in around an hour or so.

````powershell

mkdir \vagrant
cd \vagrant
git clone https://github.com/tonyskidmore/docker-swarm.git
cd docker-swarm
vagrant up

````

### Learning material

[Swarm mode overview](https://docs.docker.com/engine/swarm/)  
[Getting Started with Docker Swarm Mode](https://www.pluralsight.com/courses/docker-swarm-mode-getting-started) by Wes Higbee

