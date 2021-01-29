---
layout: post
title:  "Cartographer GRPC"
date:   2021-01-25 16:08:11 +0300
categories: cartogpraher grpc
excerpt_separator: <!--more-->
---
Cartographer has built in cloud capabilities but it is not well documented. In this post, i will give a basic server and client example.

To use Cartographer, Cloud you need `cartographer_grpc_node`. This node is not available with default compile settings. You can check by double tabbing after `rosrun cartographer_ros`.


![Cartographer GRPC](/pictures/cartographer_grpc_1.png){:class="img-responsive"}

<br />

Before recompile, you need to install GRPC dependencies. Assuming you already installed basic dependencies, you can use following scripts provided by cartographer:

```
~/catkin_ws/src/cartographer/scripts/install_grpc.sh
~/catkin_ws/src/cartographer/scripts/install_async_grpc.sh
```
<!--more-->
<br />

Now go to your workspace and build `cartographer_ros` with `BUILD_GRPC` flag like this:

```
catkin build cartographer_ros -DBUILD_GRPC=ON
```

<br />

If successful, you should see `cartographer_grpc_node`:

![Cartographer GRPC](/pictures/cartographer_grpc_2.png){:class="img-responsive"}

<br />

Below you can find launch and lua files for both Cartographer Server and Client. You can also find them on [my Github repository](https://github.com/CumaOzavci/cartographer_grpc).


#### **`server.launch`**
```xml
<launch>
  <node name="cartographer_grpc_server" pkg="cartographer_ros"
      type="cartographer_grpc_server.sh" args="
          -configuration_directory $(find cartographer_grpc)/config
          -configuration_basename server.lua
          ">
  </node>
</launch>
```

<br />

#### **`server.lua`**
```lua
include "map_builder.lua"

MAP_BUILDER_SERVER = {
  map_builder = MAP_BUILDER,
  num_event_threads = 8,
  num_grpc_threads = 8,
  server_address = "0.0.0.0:50051",
  uplink_server_address = "",
  upload_batch_size = 100,
  enable_ssl_encryption = false,
  enable_google_auth = false,
}

MAP_BUILDER_SERVER.map_builder.use_trajectory_builder_2d = true

return MAP_BUILDER_SERVER
```

<br />

#### **`client.launch`**
```xml
<launch>
  <node name="cartographer_grpc_node" pkg="cartographer_ros"
      type="cartographer_grpc_node" args="
          -configuration_directory $(find cartographer_grpc)/config
          -configuration_basename client.lua
          -server_address localhost:50051
          -client_id CLIENT_ID
          "
      output="screen">
  </node>
</launch>
```

<br />

#### **`client.lua`**
```lua
include "map_builder.lua"
include "trajectory_builder.lua"

options = {
  map_builder = MAP_BUILDER,
  trajectory_builder = TRAJECTORY_BUILDER,
  map_frame = "map",
  tracking_frame = "base_link",
  published_frame = "base_link",
  odom_frame = "odom",
  provide_odom_frame = true,
  publish_frame_projected_to_2d = false,
  use_pose_extrapolator = true,
  use_odometry = false,
  use_nav_sat = false,
  use_landmarks = false,
  num_laser_scans = 0,
  num_multi_echo_laser_scans = 1,
  num_subdivisions_per_laser_scan = 10,
  num_point_clouds = 0,
  lookup_transform_timeout_sec = 0.2,
  submap_publish_period_sec = 0.3,
  pose_publish_period_sec = 5e-3,
  trajectory_publish_period_sec = 30e-3,
  rangefinder_sampling_ratio = 1.,
  odometry_sampling_ratio = 1.,
  fixed_frame_pose_sampling_ratio = 1.,
  imu_sampling_ratio = 1.,
  landmarks_sampling_ratio = 1.,
}

MAP_BUILDER.use_trajectory_builder_2d = true
TRAJECTORY_BUILDER_2D.num_accumulated_range_data = 10

return options
```

<br />

Full list of cartographer_grpc_node parameters and their descriptions can be found at :

```
~/catkin_ws/src/cartographer_ros/cartographer_ros/cartographer_ros/cartographer_grpc/node_grpc_main.cc
```

<br />

If everything went correctly, you can use Cartographer Cloud now. To start server and client, run respective `launch` files. After client registered to server, server output should look like this:

![Cartographer GRPC](/pictures/cartographer_grpc_3.png){:class="img-responsive"}

<br />

#### **Notes**
1. Server address is the IP address of the computer which `cartographer_grpc_server.sh` is working on. If server and client is working on the same computer, you can leave it as `localhost`
2. Unless `upload_load_state_file` param is set to true, `pbstream` file pointed with `load_frozen_state` is relative to the servers file system. This means pbstream file should be located at server side.
3. Last `pbstream` file uploaded to server overrides current submaps. For example, if you are mapping with a client and later you start another client with localization mode, all submaps in the server will be replaced with the ones in new `pbstream` file.
4. `cartographer_grpc_server.sh` does not publish submaps therefore if clients are on different computers you should use something else (ROS Network, etc.) to see the map. 
5. `client_id` is a string and should be unique for every client.
