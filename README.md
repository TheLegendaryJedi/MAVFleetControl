# MAVFleetControl
This is a wrapper around MAVSDK-Python to simplify control of multiple craft.
It uses threaded asynchronous python to ensure real time tasking of the crafts.

It is intended for use with offboard mode and allows users to create high level interactions and send them to multiple aircraft (UDP or Serial).


## Important Notes

Please install [MAVSDK-Python](https://github.com/mavlink/MAVSDK-Python) from github master. The pip3 version may not allow for multiple mavsdk-servers to be spun up dynamically. 


## Run the multiple aircraft example


Start two px4_sitl aircraft in gazebo (other simultators / mavlink compatible autopilots may work but testing was done with Gazebo and PX4)

Start string for px4 gazebo. [See PX4 Documentation for troubleshooting](https://dev.px4.io/master/en/simulation/multi_vehicle_simulation_gazebo.html)

```
test@gcs-pc:~ cd ~/path/to/px4/firmware/
test@gcs-pc:~ ./gazebo_sitl_multiple_run.sh -m iris -n 2
```

Once the aircraft are spawned, open a new terminal and simply run the example in MAVFleetControl

```
test@gcs-pc:~ cd ~/path/to/MAVFleetControl/
test@gcs-pc:~ python3 muliple_aircraft_fly_to_point.py
```
## Architecture

Within craft.py exists a craft class that allows for crafts to be instantiated.
Each craft is will have the ability to spawn awaitable (asyncio) threads and has locking feautures to allow for diffrent threads and routines to not stomp on eachother. 

Actions are tasked to each aircraft via the add_action method. This adds the action to the aircraft's queue of actions to complete. The key diffrence between this and creating a mission for example is that the "actions" can be user defined (and therefore make use of offboard mode). For example the aircraft could be tasked with the following sequence [take off, loiter, go to point, perform loop-de-loop, return home].

Becuase the actions are exposed at the highest level of the code, users can create code to script actions and prevent having to generate lots of manual lines of code and algortimcally control the aircraft. (this was all written so I could avoid using ROS)

## Queues of actions

## Emergency Actions


## Credit
Major credit to Caleb Stewart for help with the threaded asyncio implementation 