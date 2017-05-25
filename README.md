# Install choreonoid on docker

## Install docker and nvidia-docker

install docker and install nvidia-docker (described later in this document)

reference
- http://wiki.ros.org/docker/Tutorials/Docker
- http://wiki.ros.org/docker/Tutorials/Hardware%20Acceleration
- http://wiki.ros.org/docker/Tutorials/GUI

## Building simulation (choreonoid)
~~~
$ git clone https://github.com/YoheiKakiuchi/docker_tests.git
$ cd docker_tests/choreonoid_docker
$ ./build.sh
~~~

## Run simulation (choreonoid)
~~~
$ xhost +local:root ### warning it is not safe, you can revert by `xhost -local:root` after using choreonoid
$ ./run.sh rtmlaunch hrpsys_choreonoid_tutorials jaxon_jvrc_choreonoid.launch
~~~

## Using simulation with ROS
~~~
$ nvidia-docker exec -it choreonoid_simulation bash
docker$ source $WORKHOME/catkin_ws/devel/setup.bash
docker$ roscd hrpsys_choreonoid_tutorials
docker$ roseus euslisp/jaxon_jvrc-interface.l
docker$roseus$ jaxon_jvrc-init
;; *ri* and *jaxon_jvrc* created
~~~
http://wiki.ros.org/rtmros_common/Tutorials/WorkingWithEusLisp

# Install docker

### Install docker
https://docs.docker.com/engine/installation/linux/ubuntu/
~~~
$ sudo apt-get update

$ sudo apt-get install \
    linux-image-extra-$(uname -r) \
    linux-image-extra-virtual

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo apt-key fingerprint 0EBFCD88

$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

$ sudo apt-get update

$ sudo apt-get install docker-ce

$ sudo gpasswd -a $USER docker
~~~

### not needed but ...
~~~
$ sudo reboot
~~~

### docker test
~~~
$ docker run hello-world
~~~

# Install nvidia docker
https://github.com/NVIDIA/nvidia-docker

### Install nvidia-docker and nvidia-docker-plugin
~~~
$ sudo apt-get install nvidia-modprobe
$ wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
$ sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb
$ sudo gpasswd -a $USER nvidia-docker
$ sudo reboot
~~~

### not needed but ...
~~~
$ sudo reboot
~~~

### test nvida-docker using nvidia-smi
~~~
$ nvidia-docker run --name=nvtest nvidia/cuda nvidia-smi
$ nvidia-docker rm nvtest
~~~

### uninstall nvidia-docker
~~~
$ docker volume prune
$ docker rmi nvidia/cuda
$ docker rmi $(docker images | grep '<none>' | awk '{print$3}')
$ docker rm $(docker ps -aq)
$ sudo apt-get purge nvidia-modprobe
$ sudo apt-get purge nvidia-docker
$ sudo groupdel nvidia-docker
$ sudo rm -rf /var/lib/nvidia-docker
~~~
