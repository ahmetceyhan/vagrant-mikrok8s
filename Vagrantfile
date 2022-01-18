$microk8sscript = <<SCRIPT
	## Configure vagrant clients dir
	printf "alias kubectl='microk8s.kubectl'\n" >> /home/vagrant/.bashrc
	printf "source /etc/bash_completion\n" >> /home/vagrant/.bashrc
	printf "source <(kubectl completion bash)\n" >> /home/vagrant/.bashrc

	#Helm allows you to define, install and upgrade even the most complex deployments.
	HELM_VERSION=3.7.2

	# Faster than VirtualBox's DNS Server
	sed -i 's/127.0.0.53/1.1.1.1/' /etc/resolv.conf

	wget https://get.helm.sh/helm-v$HELM_VERSION-linux-amd64.tar.gz
	tar -xzf helm-v$HELM_VERSION-linux-amd64.tar.gz
	mv linux-amd64/helm /usr/local/bin/helm
	rm -rf helm-v$HELM_VERSION-linux-amd64.tar.gz linux-amd64

	#Kubelet will not start if the system has swap enabled 
	#disabling swap
	swapoff -a
    #remove swap space entry from fstab
	sed -i '/swap/d' /etc/fstab

	snap install microk8s --classic

	# Waits until K8s cluster is up
	sleep 20

	microk8s.enable dns
	microk8s.enable metallb:192.168.66.100-192.168.66.200

	mkdir -p /home/vagrant/.kube
	microk8s config > /home/vagrant/.kube/config
	usermod -a -G microk8s vagrant
	chown -f -R vagrant /home/vagrant/.kube

	#enable docker
	curl https://get.docker.com | sh -
	usermod -aG docker vagrant
	
	#optional for tree command
	apt install tree
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
    v.name = 'vagrant-microk8s-6630'
  end
  config.vm.network "private_network", ip: "192.168.66.30"
  config.vm.box = "generic/ubuntu2004"
  config.vm.provision "shell", inline: $microk8sscript, privileged: true
  config.ssh.username = "vagrant"
  config.ssh.password = "vagrant"
end