# Unreal-ROS-Plugin

Unreal Engine 4 plug-in that allows to send and receive standard messages between ROS nodes and an Unreal Engine Blueprint


## Supported Platforms

This plug-in was last built against **Unreal Engine 4.15** and works on Windows 10. It has not been tested on other Unreal Engine Version or platform.


## Dependencies

This plug-in requires Visual Studio and either a C++ code project or a the full
Unreal Engine 4 source code from GitHub. If you are new to programming in UE4,
please see the official [Programming Guide](https://docs.unrealengine.com/latest/INT/Programming/index.html)!


## Usage

You can use this plug-in as a project plug-in, or an Engine plug-in.

If you use it as a project plug-in, clone this repository into your project's
*/Plugins* directory and compile your game in Visual Studio. A C++ code project
is required for this to work.
To compile the plugin in Visual Studio, please perform the following steps :
 1. Right-click on */yourProject.uproject* file
 2. Click on "generate visual studio project files"
 3. Open the generated *.sln* file in Visual Studio 
 4. Build

If you use it as an Engine plug-in, clone this repository into the
*/Engine/Plugins/Media* directory and compile your game. Full Unreal Engine 4
source code from GitHub (4.9 or higher) is required for this.

After compiling the plug-in, you have to **enable it** in Unreal Editor's plug-in
browser.

Perform the following steps to test the components:
- Modify the ROSSerialClientComponent.cpp file and its header to create the advertisers and the publishers that you need
- Compile
- Drag a Sphere from the Basic Shapes tab into your level
- Select the actor and click the Add Component button in the Details panel
- Add the ROSSerialClientcomponent
- Initialize Rosserial with the IP adress of your RosMaster in your actor blueprint thanks to the function **InitROS**
- You can use the example functions **PublishTransformMsg**, **PublishCartNavStateMsg**, **OnNewYawCommand**, **OnNewVelocityCommand** or create your own functions in your actor's blueprint.

You can also find a quick procedure to create your own ROS messages in the file [AddCustomMsg.txt](https://github.com/Camille78/Unreal-ros-plugin/blob/master/AddCustomMsg.txt). 

## Example

If you need more help, you can take a look at the repositories [SimpleROSProjectForUnreal](https://github.com/Camille78/SimpleROSProjectForUnreal) and [SimpleUnrealProjectForROS](https://github.com/Camille78/SimpleUnrealProjectForROS) which are an Unreal Project and a ROS Project using this plugin to exchange messages. This will help you to understand how the plugin can be used.
- Download the two repositories
- Open the Unreal Project in Unreal Engine 
- Build and launch the ROS node : 

		catkin build
		roslaunch unrealROS UnrealSimu.launch
	
- Set the IP adress of your ROSMaster in the RosCube Blueprint
- Play the scene in Unreal
- The cube should move thanks to speed and yaw commands sent from the ROS node "thrusterControllerNode" (Note don't panic if the cube movement is awkward, the PID controller in the trhusterControllerNode is not correctly set)

- Everything you should pay attention for is located in the ROScube blueprint, in the files ROSSerialClientComponent.cpp and its header. You can also find a quick procedure to create your own ROS messages in the file [AddCustomMsg.txt](https://github.com/Camille78/Unreal-ros-plugin/blob/master/AddCustomMsg.txt). 



## Known Errors :
- **Error	C2059	syntax error: 'constant'	plugins\unreal-ros-plugin\thirdparty\ros_lib\rosserial_msgs\Log.h	22**

Some macros have been declared in Rosserial and in Unreal with the same name.

One way to correct this :

In plugins\unreal-ros-plugin\thirdparty\ros_lib\rosserial_msgs\Log.h    **line 22**

Replace

	enum { ERROR = 3 };
	
by

	enum { ROS_ERROR = 3 };


Then in \plugins\unreal-ros-plugin\thirdparty\ros_lib\ros\node_handle.h	**line 478**

Replace
	
	void logerror(const char*msg){
        	      log(rosserial_msgs::Log::ERROR, msg);
	}

by

	void logerror(const char*msg){
        	      log(rosserial_msgs::Log::ROS_ERROR, msg);
      	}
