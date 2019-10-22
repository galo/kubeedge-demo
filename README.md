# Kubeedge

This guide goes over teh steps to setup [Kubeedge](https://github.com/kubeedge/kubeedge) on a local enviroment, and a Raspeberry PI device.

Setup the enviroment, I recomed to use [Vagrant](https://www.vagrantup.com/) as most kuebeedge expects to run with root privileges, this way you will keep your enviroment clean.

```
vagrant up
vagrant ssh
```

Sometimes provisioning times out and docker ins't there, use snap to setup.

```bash
sudo snap install docker
sudo docker run hello-world
```
