# Kubeedge

This guide goes over teh steps to setup [Kubeedge](https://github.com/kubeedge/kubeedge) on a local enviroment, and a Raspeberry PI device.

Setup the enviroment, I recomed to use [Vagrant](https://www.vagrantup.com/) as most kuebeedge expects to run with root privileges, this way you will keep your enviroment clean. Before starting vagrant setup a few plugins used in teh confioguration 

```bash
vagrant plugin install vagrant-omnibus
vagrant plugin install vagrant-proxyconf
vagrant plugin install vagrant-cachier
vagrant plugin install vagrant-vbguest
```

Then you are ready to start the enviroments

```bash
vagrant up
vagrant ssh
```

Sometimes provisioning times out and docker ins't there, use snap to setup.

```bash
sudo snap install docker
sudo docker run hello-world
```

## Download Kubeedge installer

```bash
curl -L https://github.com/kubeedge/kubeedge/releases/download/v1.1.0/keadm-v1.1.0-linux-amd64.tar.gz -o keadm-v1.1.0-linux-amd64.tar.gz

tar xvf keadm-v1.1.0-linux-amd64.tar.gz
```

## Run the installer

The installer will instoll Docker and Kubernetes.

```bash
sudo ./keadm init
```