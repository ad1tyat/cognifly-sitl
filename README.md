# cognifly-sitl

Trying to develop a SITL for the CogniFly Drone based on pre-existing SITLs

## Setup
1. Run 
	```
	sudo docker pull radarku/ardupilot-sitl
	```
	or build it using the dockerfile given in its [repo](https://github.com/radarku/ardupilot-sitl-docker)
2. Run 
   ```
   sudo docker images
   ```
   and find out the IMAGE ID of the ardupilot-sitl image
4. Run 
```
sudo docker run -it --rm -p 5760:5760 \
   --env=DISPLAY --env=QT_X11_NO_MITSHM=1 \
   -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
   --env VEHICLE=ArduCopter \
   --env MODEL=gazebo-iris \
   <IMAGE_ID>
```
5. Now follow the instructions given in the betaflight SITL [repo](https://github.com/betaflight/betaflight/tree/master/src/main/target/SITL). 
   If you use a docker image, run it with the following arguments
   
   ```
   sudo docker run -it --rm --net=host --privileged \
   --gpus all --env=NVIDIA_VISIBLE_DEVICES=all \
   --env=NVIDIA_DRIVER_CAPABILITIES=all --env=DISPLAY \
   --env=QT_X11_NO_MITSHM=1 \
   -v /tmp/.X11-unix:/tmp/.X11-unix:rw IMAGE_ID
   ```
   And in the host machine, run this in a terminal to allow external (the docker container) connections
	```
	xhost +
	``` 

## Running the SITL
**Currently controlling the gazebo-iris drone using betaflight configurator**
1. Start ardupliot docker
2. Start betaflight `./obj/main/betaflight_SITL.elf`
3. Start gazebo `gazebo --verbose ./iris_arducopter_demo.world`
4. Start betaflight configurator and enter the following port manually `tcp://127.0.0.1:5761`


## Notes
* Currently the ardupilot-sitl not yet tested with gpus.
* Important to connect the docker image of ardupilot to port 5760:5760, if connected with 5761:5760, betaflight returns error, saying no free ports.
* Trying to integrate [YAMSPy](https://github.com/thecognifly/YAMSPy) using TCP Protocol



