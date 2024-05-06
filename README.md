# ROS_turtlesim_Letter-Paths (J,K,W,P)
        #!/usr/bin/env python
        import rospy
        import math
        from geometry_msgs.msg import Twist, Vector3  # ROS related
        from turtlesim.msg import Pose  # ROS and Turtlesim related

        def callback(data):
            global pub

            theta = data.theta
            y_position = data.y
            if (theta < math.pi and y_position < 9.2):
                pub.publish(Twist(Vector3(1, 0, 0), Vector3(0, 0, .75)))


        def draw_letter(letter):
            global pub
            rospy.sleep(1)  # Give some time to settle before drawing
            if letter == 'J':
                pub.publish(Twist(Vector3(2, 0, 0), Vector3(0, 0, 0))) # Move right 2 step 
                rospy.sleep(2)
                pub.publish(Twist(Vector3(-1, 0, 0), Vector3(0, 0, 0)))  # Move back 1 step
                rospy.sleep(2)
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, -1.5)))  # Turn facing down
                rospy.sleep(2)
                pub.publish(Twist(Vector3(2, 0, 0), Vector3(0, 0, 0)))  # Move down 2 step
                rospy.sleep(2)
                for i in range(18):  # Create a semi-circle by small increments
                    pub.publish(Twist(Vector3(1, 0, 0), Vector3(0, 0, -0.75)))  
                    rospy.sleep(0.1)

            elif letter == 'K':
                # Draw letter K with correct angle adjustments
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, -1.5)))
                rospy.sleep(2) # Turn facing down
                pub.publish(Twist(Vector3(2, 0, 0), Vector3(0, 0, 0)))  
                rospy.sleep(2)# Move down 2
                pub.publish(Twist(Vector3(-.75, 0, 0), Vector3(0, 0, 0))) 
                rospy.sleep(2) # Move up.75
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, .75)))  
                rospy.sleep(2)#Turn .75
                pub.publish(Twist(Vector3(1, 0, 0), Vector3(0, 0, 0)))  
                rospy.sleep(2)#move up
                pub.publish(Twist(Vector3(-1, 0, 0), Vector3(0, 0, 0)))  
                rospy.sleep(2)#move back
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, 1.5)))  
                rospy.sleep(2)#turn down .75
                pub.publish(Twist(Vector3(1, 0, 0), Vector3(0, 0, 0)))  
                rospy.sleep(2)#move down

            elif letter == 'W':
                # Draw letter W
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, -1.5)))
                rospy.sleep(1)
                pub.publish(Twist(Vector3(2, 0, 0), Vector3(0, 0, 0)))
                rospy.sleep(1)
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, 2.5)))
                rospy.sleep(1)
                pub.publish(Twist(Vector3(1, 0, 0), Vector3(0, 0, 0)))
                rospy.sleep(1)
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, -2.5)))
                rospy.sleep(1)
                pub.publish(Twist(Vector3(1, 0, 0), Vector3(0, 0, 0)))
                rospy.sleep(1)
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, 2.5)))
                rospy.sleep(1)
                pub.publish(Twist(Vector3(2, 0, 0), Vector3(0, 0, 0)))
                rospy.sleep(1)

            elif letter == 'P':
                # Draw letter P with a loop at the top
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, 1.5)))
                rospy.sleep(2)
                pub.publish(Twist(Vector3(3, 0, 0), Vector3(0, 0, 0)))
                rospy.sleep(2)
                pub.publish(Twist(Vector3(0, 0, 0), Vector3(0, 0, -1.57)))  
                for i in range(18): 
                    pub.publish(Twist(Vector3(1, 0, 0), Vector3(0, 0, -0.85))) 
                    rospy.sleep(0.3)

            else:
                rospy.logerr("Invalid letter provided")

        def listener():
            rospy.Subscriber('/turtle1/pose', Pose, callback)

        if __name__ == '__main__':
            try:
                rospy.init_node('draw_letters')
                pub = rospy.Publisher('/turtle1/cmd_vel', Twist, queue_size=1)
                while not rospy.is_shutdown():
                    letter = input("Enter the letter to draw (J, K, W, P): ")
                    draw_letter(letter.upper())

            except rospy.ROSInterruptException as e:
                print(e)
