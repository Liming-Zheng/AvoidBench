Now we have docker image for AvoidBench!
Follow this [guide](https://docs.docker.com/engine/install/ubuntu/) to install docker. The following steps are used to make your NVIDIA graphics device can work with docker container:

```bash
# in terminal of the host computer
xhost +
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

Then get our docker image and run a container:

```bash

# in terminal of the host computer
sudo docker pull catflying/slapdrone:main
sudo docker run -it --device=/dev/dri --group-add video --volume=/tmp/.X11-unix:/tmp/.X11-unix  --env="DISPLAY=$DISPLAY" --env="QT_X11_NO_MITSHM=1" --gpus all --name=noetic_ab -e NVIDIA_DRIVER_CAPABILITIES=compute,utility,graphics -e NVIDIA_VISIBLE_DEVICES=all -v /home/dawn/Drone/SlapDroneOutdoors/AvoidBench:/AvoidBench catflying/slapdrone:main /bin/bash

# in terminal of the docker container
cd AvoidBench
catkin build
```