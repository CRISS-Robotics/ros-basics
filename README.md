# ros-basics
Learn how to create a publisher and subscriber in ROS.

1. Create a package in the workspace
```
cd catkin_ws/src
```
```
catkin_create_pkg criss std_msgs rospy roscpp
```
2. Create a new Python file in the package
```
cd criss/src
```
```
touch name.py
```
3. Paste the following code in the Python file
```
#!/usr/bin/env python3
import rospy
from std_msgs.msg import String

def talker():
    pub = rospy.Publisher('name', String, queue_size=10)
    rospy.init_node('talker', anonymous=True)
    rate = rospy.Rate(10) # 10hz
    while not rospy.is_shutdown():
        name = "<your name here>"
        rospy.loginfo(name)
        pub.publish(name)
        rate.sleep()

if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```
4. Open a new terminal and run roscore
```
roscore
```
5. In the previous terminal, run the publisher
```
chmod +x name.py
```
```
rosrun criss name.py
```
6. Create the subscriber file
Open a new terminal window and paste the following
```
cd catkin_ws/src/criss/src
```
```
touch fullname.py
```
7. Paste the following code in the fullname.py file
```
#!/usr/bin/env python3
import rospy
from std_msgs.msg import String

def callback(data):
    print(data.data, '<your surname here>')

def listener():

    rospy.init_node('listener', anonymous=True)

    rospy.Subscriber('name', String, callback)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()

if __name__ == '__main__':
    listener()
```
8. Run the subscriber file
```
chmod +x fullname.py
```
```
rosrun criss fullname.py
```
