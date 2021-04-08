---
layout: post
title:  "ROS Navigation REST API"
date:   2021-04-08 12:00:00 +0300
categories: nodejs rest
excerpt_separator: <!--more-->
---

While building autonomous vehicles, we tend to focus heavily on autonomy and forget that these robot are being developed for commercial purposes. Eventually, someone who does not have any robotics background will need to control these robots. And unlike us, they will not have the luxury of Rviz and ROS command line tools.

We need an interface that will let customers communicate to robots and this interface should be:
- **Fast**. Communication should be fast enough that end users can operate easily.
- **Secure**. Only authorized users should be able to give commands to robots.
- **Flexible**. Same interface should be able to be used for different customers and needs.
- **Scalable**. Interface should be (almost) independent of fleet size.

**REST API** is a good choice for this. It is matured and highly used in web industry. REST API calls can be made with almost all programming language and systems. Therefore can be integrated with any customer system. It is lightweight and fast. It can store security credentials. New capabilities can be added quickly and if needed, servers can be scaled up easily.

<br/>
In this post, i will introduce a ROS package that will provide a **basic** REST API for a fleet of autonomous vehicles. This API will have following capabilities:
- Get map file and properties 
- Get robots ip address, port and current locations
- Send initial pose to a robot
- Send navigation goal to a robot
- Send cancel navigation goal request to a robot
- Send velocity command to a robot
- Send clear costmap request to a robot


With this API, you can control mobile robot(s) with a mobile app or a web site and don't even have to know anything about ROS. For example, if you want to know where your robot is, you don't need to connect robot's ROS network and get that information manually. You will just send a GET request to a server and server will return robot's current location. All you need to do is just run corresponding launch files.
<!--more-->
<br/>
## [nav_rest_api](https://github.com/CumaOzavci/nav_rest_api)

![NAV REST API](/pictures/ros_rest_api.png){:class="img-responsive"}

### Installation
- Download package to your catkin workspace
- At your catkin workspace run `catkin build nav_rest_api`
- Install [Node.js](https://nodejs.org/en/download/)
- On server, go to `server` directory and run `npm install`
- On client, go to `client` directory and run `npm install`

### Configuration
- On server, go to `server/map` directory. Put your map here and fill `map.json` file with your map specs. It would be better, if you convert your map file to .png file
- On client, go to `client/config` directory. Update `params.json` with your parameters. If both server and client will run on same computer, you can leave ip_adresses as `localhost`.
- Go to `scripts` folder and update scripts with your catkin workspace name.

### Running
- On server, run `roslaunch nav_rest_api server.launch`
- On client, run `roslaunch nav_rest_api client.launch`

### Notes
- Currently, there is no security features implemented.
- This package does not provide any autonomy. Navigation Stack should be already up and running.
- Server and client can be run on same computer or different computers. If they run on different comupters, make sure they are on same network and ip address configurations are done.
- You can use an API client such as [Insomnia](https://insomnia.rest/) to test API. You can find my Imsonmia document at [here](https://github.com/CumaOzavci/nav_rest_api/blob/master/insomnia.txt).
- API documentation can be found [here](https://github.com/CumaOzavci/nav_rest_api/blob/master/nav_rest_api.pdf).

### Example
Imagine you want to get all robots current location. You should send an empty GET Request to `server_ip_address:8080/api/robot`. If successful, server will return a list of robot object which contains robot's name, ip address, port and location as seen below. If you want to get a spesific robots location, add `robot_name` parameter to GET Request and fill it with robot's name. 

```
[
    {
        "robot_name": "robot0",
        "ip_address": "192.168.1.1",
        "port": 9090,
        "location_x": 0.1,
        "location_y": -0.4,
        "location_th": 0.0
    },
    {
        "robot_name": "robot1",
        "ip_address": "192.168.2.1",
        "port": 9090,
        "location_x": 20.4,
        "location_y": -30.6,
        "location_th": 1.5707
    }
]
```

<br/>
## [nav_rest_api_turtlebot](https://github.com/CumaOzavci/nav_rest_api_turtlebot)
This package is an example for nav_rest_api package.

<iframe width="1150" height="700" src="https://www.youtube.com/embed/1Kw58aKMdZQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


### Installation
- Install [turtlebot3](http://wiki.ros.org/turtlebot3)
- Download [nav_rest_api](https://github.com/CumaOzavci/nav_rest_api) to your catkin workspace. Compile, install and configure
- Download this package to your catkin workspace and compile

### Running
- Run `roslaunch nav_rest_api server.launch`
- Export turtlebot model by `export TURTLEBOT3_MODEL=waffle`. On same console, run `roslaunch nav_rest_api_turtlebot test.launch`

### Testing
- Open [Insomnia](https://insomnia.rest/)
- Import test document from `your_catkin_workspace/src/nav_rest_api/insomnia.txt`
- Send goal, twist, initial pose etc.