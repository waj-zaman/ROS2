# This is the file in which all the ROS learnings are going to be entered. 

- This entire notes are creating on basis of the youtube playlist --> [https://www.youtube.com/playlist?list=PLLSegLrePWgJudpPUof4-nVFHGkB62Izy]

## ROS2 HUMBLE HAWKSBILL

#### We Are going to use ROS2 in ubuntu. The installation must be done by following all the steps in the official documentation on the official website.

- If we want to run ROS2 we need to source the setup.bash file first (Initial in every terminal.)
- But that is not efficien, so we will do it in better way. We will add the sourcing code line in the ./bachrc file.
    1. Terminal1 : `gedit ~/.bashrc`
    2. A file opens, in the end of the file add the source code line:
    `source /opt/ros/humble/setup.bash`
Now the source code will automatically run at the terminal startup.

## The layout of runnning ros2 will be 4 terminals.

- To test the ROS2 we will follow the steps:
    1. Terminal1: `ros2 run demo_nodes_cpp talker`
    2. Terminal2: `ros2 run demo_nodes_cpp listner`
    - The terminal 1 will start sending some texts continously, and the terminal 2 will start recieving the texts which are being sent from the terminal 1. This shows ros2 is running fine.
    3. Terminal3: `rqt_graph`
    - This will show the working order or procedure of the current cycle in a flowchart manner.
    - Whenever we are required to understand what is happening and in what order we can use this command to understand what is going on.

- Second example is following steps:
    1. Terminal1: `ros2 run turtlesim turtlesim_node`
    2. Terminal2: `ros2 run turtlesim turtlesim_teleop_key`
    - This will show the terminal 1 opens up a window in which a turtle is spawned and this is the listening terminal and the terminal 2 will take input from our arrow keys and send the commands to terminal1 which will cause the turtle to move according to our directions.
    3. Terminal2 : `rqt_graph`

- Upon using rqt_graph we understand that each process runs in terms of ros nodes.

## Node : A node is any program that has access of ros2 and it is used to communicate with other node programs. In above example the program running in terminal 1 is the listening node which is also called as SUBSCRIBER node and the program running in terminal 2 is the sending node which is also called as PUBLISHER node.

### Creating an ROS2 Workspace: We need to install a built tool:

1. Terminal1 : `sudo apt update`
2. Terminal1 : `sudo apt install python3-colcon-common-extensions`
- This installed built tool also require to be sourced everytime we we need to use it so we will add this source code in the bashrc file as well. Steps:
1. Terminal1 : `gedit ~./bashrc`
- This will open a file, go to the end of the file and after the setup.bash code add the code `source /usr/share.colcon_arg_complete/hook/colcon_argcomplete.bash`

- Whenever we make changes to the bashrc file we are required to execute it once. So we will do
- Terminal1 : `source ~/.bashrc`

### ROS2 workspace : An ROS2 Workspace is a repository in your system in which all the nodes and packages of the project at stored. Its an organised and systematic way of storing the project items.

- Now we will create an ROS2 WORKSPACE:
1. Terminal1 : `ced ..`
2. Terminal1 : `source ~./bashrc`
3. Terminal1 : `mkdir ros2_ws`
4. Terminal1 : `cd ros2_ws`
5. Terminal1 : `mkdir src`
6. Terminal1 : `cd .`
7. Terminal1[ros2_ws] : colcon build
- Colcon build is the command used to process the contents of the src folder. This is done to know what all packages and nodes are there in the project and these nodes are then converted into usable entities in the ubuntu file system. Also the colcon build command creates the folder structure inside the ros2_ws that we will use to create the project files. [Like a boiler plate]
- Then the ROS2 will go inside the src folder code and bring the nodes into install/ to use them.
- In ros2_ws/install/ there is another setup.bash file which needs to be installed in the terminal everytime we want to run packages, SO WE WILL ADD THIS INTO THE .BASHRC FILE TO RUN THIS ON THE STARTUP. 
- Now we must run `source ~/.bashrc` in each terminal or restart all the terminals.


### ROS2 Packages : A package in ROS2 ia the smallest,self-contained unit of software in ROS2 that contains everything needed to perform a specific function. It typically includes code, executables, nodes, libraries, configuration files, interfaces, and other resources required for a ROS2 application. A package is the basic building block used to organize, build and reuse ROS2 functionality.

- Basically, a package is a folder that contains all the code and files needed to create ROS2 Nodes and biuld them using colcon.

- Now we will create a package in the ros2_ws workspace:
1. Terminal1 : `cd ros2_ws/src/`
2. Terminal1[ros2_ws/src/] : `ros2 pkg create my_package_name --build-type ament_python --dependencies rclpy`
- The src folder will have a package folder created with internal folder structure also created. 
- RCLPY : ROS client library for Python. A client library to use PYTHON in ROS development.
3. Terminal1[ros2_ws/src/] : `code .` [assuming vscode is installed.]
- In vscode, we can see that already few folders are created, this is the actual structure of the project that we will follow.
- Now to run/execute the package:
4. Terminal1 : `colcon build`
- The package will be created and will be usable now.

### ROS2 Node : An ROS2 Node is a single, independent program the performs one specific task in an ROS2 system. Nodes communicate with each other using topics, services and actions to build a complete robot application. Instead of creating one giant program we break it into smaller programs which are called as NODES.



- Now we will create a node inside our package. 
1. In `ros2_ws/src/my_first_package/my_first_package` there will be one file called as `_init_.py`. In this folder create a python file called as `my_first_node.py`. This can be done using terminal or vscode, it doent matter much.
- Now this newly created file must be converted into an executable. An **Executable** file is the file that can be run directly by the computer as a program. A python file cannot run directly unless we explicitely call it. So we need to convert the python file as an executable.
2. Terminal1[ros2_ws/src/my_first_package/my_first_package] : `chmod +x my_first_node.py`  
3. Terminal1[ros2_ws/src/my_first_package/my_first_package] : `ls`
- Now we see that the python file "my_first_node.py" is now green which means its converted into an executable file. But this alone wont be enough for us to run the python file as a program directly. We will do following. 

- We will start writing code in the my_first_node.py :
**In every node file we must always start the code by ROS2 shebang line for python which is**
`#!/usr/bin/env python3` 
- Then enter the testing code for that particular node in the file and save. The next step is to add this into the setup.py so that it can be called as a program.
- Go to VSCode --> my_first_package --> setup.py. In this file under entry_points --> console_scripts, add the line 
"test_node = my_first_package/my_first_node:main".
4. Terminal1 : `colcon build`
5. Terminal1 : `source ~/.bashrc`
6. Terminal1 : `ros2 run my_robot_controller test_node`
- This will perform the functionality that we described using the python code. We realize the calling method as it is direct.

**We must try not to use the same names for node_name and the file_name and the executable call name so that confusion can be eliminated.**
- Now the testing is complete. Now we must change the testing code with actual code that we want the program to do. So change the code.
- But when we change the code, even if save the code in VSCODE the ubuntu doesnt realize the change automatically. For that we need to build the project again. But building the project everytime is not optimum, so we must add the auto realization.
7. Terminal1[ros2_ws] : `colcon build --symlink-install`
- The symlink works as an auto realizer that updates the build everytime we make changes in the file code.
8. Terminal1 : `source ~/.bashrc`

- ENTIRE SAMPLE CODE FOR my_first_node.py:
```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node

#OOP
class MyNode(Node):

    def _init_(self):
        super().__init__("first_node")
        self.counter_ = 0
        self.create_timer(1.0, self.timer_callback)

    def timer_callback(self):
        self.get_logger().info("Hello" + str(self.counter_))
        self.counter_ += 1

def main(args=None):
    rclpy.init(args=args)
    node = MyNode()
    rclpy.spin(node)
    rclpy.shutdown()

if __name__ = "__main__":
    main()
```

- The nodes communicate with each other using topics or services. 

### ROS2 Topics : A Topic is named bus over which nodes exchange messages in a publish-subscribe pattern. Its an asynchronous contact [publisher doesnt wait for the reciever to respond] in which there is many to many relationship of publishers and listners over a single Topic. This is used for streaming data like sensor readings, camera Images etc.

-Lets look at some topic commands.
1. Terminal1 : `ros2 topic list` --> Gives me a list of topics that are available.
2. Terminal1 : `ros2 topic info /topic_name` --> This command give me the description of the topic_name. The topic_name must be the actual name of the topic. We will know the type of message at this step.
3. Terminal1 : `ros2 interface show Type` --. This command shows us the structure of the message of the topic_name. The message type of the node must be same as this message type of the service in order to communicate. The TYPE must be taken from the description of COMMAND 2. 

### ROS2 Publisher : A publisher is a component inside a node that sends [ publishes ] messages to a specific topic so that any subscribing nodes can recieve those messages. It continously or periodically outputs data such as a sensor readings, robot commands etc

- Now we will create a publisher with python code:
- We will be creating a sample draw_circle functionality that the turtle will follow.
1. Terminal1 : `cd ros2_ws/src/my_first_package/my_first_package`
2. Terminal1 : `touch draw_circle.py`
3. Terminal1 : `chmod +x draw_circle.py`
4. Terminal1 : `cd ../..` [until src]
5. Terminal1 : code .
- In draw_circle.py we will develop the functionality. First in order to send a message to a topic we must know and understand the type of message that is to be sent. We determine that by following steps :
1. Terminal1 : `ros2 topics list`
- This will give us all the topics available. We will determine the topic name we need.
2. Terminal1 : `ros2 topic info /topic_name`
- This will give us a description of the topic and there we will find the message type. 
- We must note down the topic_name as well as the its message type.

- Now that we know the topic name and topic message type, we will create the publisher node in following steps :
1. In the draw_circle.py we must enter the code. The imports and all the functionality in order :
    - Shebang line, the imports, the def of main function.
    - Creation of the class and the supporting functions inside the class.
    - Modification of the main function. 
    - Add the imported things in the .xml file as a dependable`
2. Add the file in the setup.py as an executable name.
3. Colcon build in terminal using symlink install.
    - Terminal1 : `colcon build --symlink-install`
4. Spawn the turtle by using the turtlesim_node.
    - Terminal1 : `ros2 run turtlesim turtlesim_node`
5. Source the created file before calling it.
    - Terminal1 : `source /.bashrc`
6. Send the created function by calling it as a direct ros function.
    - Terminal1 : `ros2 run my_first_package draw_circle`
- Now our publisher must be sending the messge to the turtle sim listner. 

- draw_circle.py Entire code :
```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist

class DrawCircleNode(Node):

    def __init__ (self):
        super().__init__("draw_circle")
        self.cmd_vel_publisher_ = seld.create_publisher(Twist, "/turtle1/cmd_vel", 10)
    # The Twist is the type of message and the name in the quotation marks is the name of the topic. This is the point of connection of the publisher to the topic.
        self.timer_ = self.create_timer(0.5, self.send_vel_command)
        self.get_logger().info("Draw Circle node has been started.")

    def send_vel_commmand(self):
        msg = Twist()
        msg.linear.x = 2.0
        msg.angular.z = 1.0
        self.cmd_vel_publisher_.publish(msg)

def main (args = None ):
    rclpy.init(args = args)
    node = DrawCircleNode()
    rclpy.spin(node)
    rclpy.shutdown()

```

### ROS2 Listener : A listner which is also called as a SUBSCRIBER is a part of the node that receives messages from a specific topic. It waits and listens for any data that is being published on that topic, processes the input and acts whenever new information arrives.

- Now we will create a listener / subscriber :
- For this purpose as well we will need the topic name that we will subscribe to and the type of message of that topic For that we will do the following :
1. Terminal1 : `ros2 run turtlesim turtlesim_node`
2. Terminal2 : `ros2 topic list`
- This will give us the list of topics that are available on current scenerio. Now we wil identify what topic we will listen to using `rqt_graph`. In our case the topic is `/turtlesim/pose`
3. Terminal2 : `ros2 topic info /turtlesim/pose`
- This will give us the type of message that we will recieve from the topic. And we got the name of the topic as well. 
4. Terminal2 : `ros2 interface show /turtlesim/msg/pose`
- This will show us the type of data that can be entered in the message.
- We will create the python file and we will convert it into a executable file and then we will start writing the code in following steps :
    1. Shebang line and the imports
        - Add the imports in the .xml file as depends.
    2. Define the main function.
    3. Create the Class PoseSubscriberNode(Node)
    4. Define the supporting functions inside the class. 
    5. After Completion of code, add the file in the setup.py as a callable executable.
    5. Build the program using colcon build with simlink install
    6. Source the built program using bashrc
    7. Run The turtlesim node.
    8. Run our built program just like a direct program.
- Now the listener must be receiving the data continously that is being sent from the turtlesim/pose which will send position information to the listener.

- pose_subsriber.py Entire Code :
```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node
from turtlesim.msg import Pose

class PoseSubscriberNode(Node):
    
    def __init__(self):
        super().__init__("pose_subscriber")
        self.pose_subscriber_ = self.create_subscription(Pose, "/turtlesim/pose", self.pose_callback, 10)

    def pose_callback(self, msg : Pose):
        self.get_logger().info("(" + str(msg.x) + ", " + str(msg.y) + ")")

def main (args=None):
    rclpy.init(args=args)
    rclpy.shutdown()
```

- Then in Terminal we will use the command : `ros2 run my_first_package pose_subscriber`
- The node must start receiving the messages from the topic using the /turtlesim/pose/


### Now lets create a closed loop system with a Publisher and a Subscriber :
- Using the System the turtle will continously move on the turtlesim window but as soon as it reaches the edge the trtle turns.
1. Terminal1[ros2_ws/src/my_first_package/my-first_package] : `touch turtle_controller.py`
2. Terminal1[ros2_ws/src/my_first_package/my-first_package] : `chmod +x turtle_controller.py`
3. Terminal[ros2_ws/src] : `code .`
- Open Turtle_controller and start editing the code
    1. Shebang Line and the imports.
    2. Define the main function.
    3. Create a Class for the Node.
    4. Add the executable name in the setup.py.
    5. Colcon build with symlink
    6. Source .bashrc
    7. Run our program.
- While writing the code we must break the complex problem into smaller steps and work in proceeding steps by checking everthing is working fine on intervals.


 - turtle_controller.py Entire code :
 ```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node
from turtlesim.msg import Pose
from geometry_msgs.msg import Twist

class TurtleControllerNode(Node):

    def __init__(self):
        super().__init__("turtle_controller")
        self.cmd_vel_publisher_ = self.create_publisher(Twist, "/turtle1/cmd_vel", 10)
        self.pose_subscriber_ = self.create_subscription(Pose, "/turtle1/pose", self.pose_callback, 10)
        self.get_logger().info('Turtle Controller has started.')

    def pose_callback(self, pose: Pose):
        cmd = Twist()
        if pose.x > 9.0 or pose.x < 2.0 or pose.y > 9.0 or pose.y < 2.0:
            cmd.linear.c = 1.0
            cmd.angular.z = 0.9
        else:
            cmd.linear.x = 0.5
            cmd.angular.z = 0.0
        self.cmd_vel_publisher_.publish(cmd)

def main(args=None):
    rclpy.init(args=args)
    node = TurlteControllerNode()
    rclpy.spin(node)
    rclpy.shutdown()
 ```

### ROS2 Services : A Service is a synchronous request response communication between nodes. One node acts as a service server and another node acts as a service client. The client waits for the server to respond. This is a sunchronous one to one relationship between the server and the client. Used for descrete actions like RESET SIMULATION or ADD TWO NUMBERS.

- We will look at few terminal commands for ROS services : 
1. Terminal1 : `ros2 run demo_nodes_cpp add_two_ints_server` --> Mind that the outputs of following commands are respect to this.
2. Terminal2 : `ros2 service list` --> This will return the list of services that are available. From this list we will select what we want. In our case it will be /add-two-ints
3. Terminal2 : `ros2 node list` --> Gives the list of nodes.
4. Terminal2 : `ros2 service type /add-two-ints` --> This gives us the service details along with its package name and also the functionality in it.
5. Terminal2 : `ros2 interface show 'output from the above command'` --> This will give us further details of the service functionality and what is inside the service.
6. Terminal3 : `ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{'a': 2, 'b': '5'}"` --> We call a service in this manner.

- For a service there can be multiple clients but only one server. Mostly one-to-many relationship.
- A service we usually have mostly two kinds of requests :
    1. Computational requests
    2. Change of settings requests

- The change of settings requests can be checked as:
1. Terminal1 : `ros2 run turtlesim turtlesim_node`
2. Terminal2 : `ros2 service list` --> This will give us all the services with respect to command in terminal1. In our case we will use /turtle1/set_pen
3. Terminal2 : `ros2 service type /turtle1/set_pen` --> turtlesim/srv/SetPen
4. Terminal2 : `ros2 interface show turtlesim/srv/SetPen` --> This will give us the type of inputs we can send.
5. Terminal2 : `ros2 service call /turtle1/set_pen turtlesim/srv/SetPen "{'r': 255 , 'g' : 0, 'b' : 0, 'width' : 3, 'off' : 0}"` --> This is the calling of the service for our current spwned turtle.

### When to use TOPICS and when to use SERVICES ??
- Topics are used to send some data only and no interaction is needed and Services are used when we want some interaction between nodes like computation or changing settings.

### Lets Write a Service Client with Python --> 
- We will add the service in our turtle_controller that we created previously. 

- turtle_controller.py Entire code after calling the service:
```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node
from turtlesim.msg import Pose
from geometry_msgs.msg import Twist
from turtlesim.srv import SetPen
from functools import partial

class TurtleControllerNode(Node):

    def __init__(self):
        super().__init__("turtle_controller")
        self.previous_x_ = 0
        self.cmd_vel_publisher_ = self.create_publisher(Twist, "/turtle1/cmd_vel", 10)
        self.pose_subscriber_ = self.create_subscription(Pose, "/turtle1/pose", self.pose_callback, 10)
        self.get_logger().info('Turtle Controller has started.')

    def pose_callback(self, pose: Pose):
        cmd = Twist()
        if pose.x > 9.0 or pose.x < 2.0 or pose.y > 9.0 or pose.y < 2.0:
            cmd.linear.c = 1.0
            cmd.angular.z = 0.9
        else:
            cmd.linear.x = 0.5
            cmd.angular.z = 0.0
        self.cmd_vel_publisher_.publish(cmd)

        # We will call the service here
        if pose.x > 5.5 and self.previous_x_ <= 5.5:
            self.previous_x_ = pose.x
            self.get_logger().info("Set Color to red!")
            self.call_set_pen_service(255, 0, 0, 3, 0)
        elif pose.x <= 5.5 and self.previous_x_ > 5.5:
            self.previous_x_ = pose.x
            self.get_logger().info("Set Color to green!")
            self.call_set_pen_service(0, 255, 0, 3, 0)


    # We will define a service below
    def call_set_pen_service(self, r, g, b, width, off):
        client = self.create_client(SetPen, "/turtle1/set_pen") 
        # The service type and the service name are obtained using the above described terminals.
        while not client.wait_for_service(1.0):
            self.get_logger().warn("Waiting for service...")
        
        request = SetPen.Request()
        request.r = r
        request.g = g
        request.b = b
        request.width = width
        request.off = off

        future = client.call_async(request)
        future.add_done_callback(partial(self.callback_set_pen))
    
    def callback_set_pen(self, future): 
        try:
            response = future.result()
        except Exception as e:
            self.get_logger().error("Service call failed: %r" % (e,))

def main(args=None):
    rclpy.init(args=args)
    node = TurlteControllerNode()
    rclpy.spin(node)
    rclpy.shutdown()
```

- A service can be called at any desired rate but we must always call service at the most optimum rate.

