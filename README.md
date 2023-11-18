# robotarm

다크넷 (Darknet):

설명: 다크넷은 딥러닝을 기반으로 하는 오픈 소스 신경망 프레임워크 중 하나입니다. 주로 컴퓨터 비전 및 객체 감지 작업에 사용됩니다. 다크넷은 작은 크기의 네트워크로도 높은 정확도를 달성할 수 있어, 리소스 제약이 있는 환경에서도 효과적으로 사용될 수 있습니다.
욜로 (YOLO - You Only Look Once) 4:

설명: YOLO는 객체 감지를 위한 딥러닝 알고리즘으로, 한 번의 순전파로 이미지를 분석하여 객체를 탐지합니다. YOLO의 주요 특징은 빠른 속도와 높은 정확도입니다. YOLO 4는 YOLO 시리즈의 최신 버전 중 하나로, 더 나은 성능과 객체 감지 능력을 제공합니다.
Blob Tracking:

설명: Blob은 이미지에서 특정 색 또는 밝기로 연결된 픽셀의 묶음을 나타냅니다. Blob Tracking은 객체 추적의 한 방법으로, 연속된 이미지 프레임에서 Blob의 위치를 추적하여 객체의 움직임을 파악합니다. 주로 컴퓨터 비전 및 영상처리에서 사용되며, 움직이는 물체를 식별하고 추적하는 데 유용합니다.


4장, Install jessiarm code
1. jessiarm install on Jetson
$ cd ~/catkin_ws/src
$ git clone https://github.com/zeta0707/jessiarm.git
$ cd ~/catkin_ws
cd ~/Downloads/opencvDownTo34
sudo patch -p1 /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake -p1 < cv_brige.patch
cd ~/catkin_ws

#전체 빌드
cma

2. jessiarm install on PC
 $ sudo apt update
$ sudo apt install ros-melodic-desktop-full

$ cd ~/catkin_ws/src
$ git clone https://github.com/zeta0707/jessiarm.git
$ cd ~/catkin_wss
$ cma  

5장, Test servo motor
sudo apt install python-pip -y
pip2 install adafruit_pca9685
cd ~/catkin_ws
python src/jessiarm/jessiarm_control/src/servokit_test.py

7장, Teleop by keyboard
1. teleop_twist_keyboard 설치
jetson@jp4612GCv346Py37:~/catkin_ws$ git clone https://github.com/ros-teleop/teleop_twist_keyboard.git
jetson@jp4612GCv346Py37:~/catkin_ws$ cd ..
jetson@jp4612GCv346Py37:~/catkin_ws$ cma
 
2.  teleop_keyboard 키 사용 방법
```bash
---------------------------
Moving around:
   u    i    o
   j    k    l
   m    ,    .

For Holonomic mode (strafing), hold down the shift key:
---------------------------
   U    I    O
   J    K    L
   M    <    >

t : up (+z)
b : down (-z)

anything else : stop

q/z : increase/decrease max speeds by 10%
w/x : increase/decrease only linear speed by 10%
e/c : increase/decrease only angular speed by 10%

CTRL-C to quit 
```

위의 teleop_twist_keyboard 기본 기능에서 Jessiarm은 아래의 기능으로 변경했습니다.

```bash
j l: 로봇암 좌우
i , : motor1 기울어짐
o . : motor2 기울어짐
u m : motor3 기울어짐
t b: gripper 닫힘, 열림 
U, M: motor4 회전, 기능 삭제
```
3. launch파일로 실행
$ roslaunch jessiarm_control teleop_keyboard.launch

4. rosrun으로 각각 파이썬 코드 실행
#terminal #1
jetson@jp4612GCv346Py37:~$  roscore

#terminal #2
jetson@jp4612GCv346Py37:~$  rosrun teleop_twist_keyboard teleop_twist_keyboard.py

#terminal #3
jetson@jp4612GCv346Py37:~$  rosrun jessiarm_control keyboard_control.py

8장, Automatic Move

automove.txt 복사
6,7장에서 rosrun으로 ~/catkin_ws에서 joystick, keyboard로 운전한 경우
jetson@jp4612GCv346Py37:~/catkin_ws$ cp automove.txt ~/catkin_ws
jetson@jp4612GCv346Py37:~/catkin_ws$ cp automove.txt ~/catkin_wssrc/jessiarm/jessiarm_control/src/

6,7장에서 roslaunch로 운전한 경우
jetson@jp4612GCv346Py37:~/catkin_ws$ cp ~/.ros/automove.txt ~/catkin_ws/src/jessiarm/jessiarm_control/src
jetson@jp4612GCv346Py37:~/catkin_ws$ cp ~/.ros/automove.txt ~/catkin_ws

automove 실행하는 방법
$ cd catkin_ws
$ python src/jessiarm/jessiarm_control/src/auto_move.py

10장, blob PicknPlace
$ roslaunch jessiarm_control blob_control.launch

2. rosrun으로 실행

$ roscore

# 터미널 #2, camera image publisher
$ rosrun jessiarm_csicamera webcam_pub.py

# 터미널 #3, CV Magic, Bind Ball with Pixel Value
# requires load parameter at first
jetson@jp4612GCv346Py37:~/catkin_ws$ rosparam load src/jessiarm/jessiarm_control/config/find_ball.yaml
jetson@jp4612GCv346Py37:~/catkin_ws$ rosrun jessiarm_cv find_ball.py 

# 터미널 #4, Blob Point to Twist
$ rosrun jessiarm_control chase_the_ball.py

# 터미널 #5, PWM Control node
$ rosrun jessiarm_control blob_chase.py

11장, Yolo4 PicknPlace
1.1 darknet_ros 설치
sudo apt-get install -y ros-melodic-image-pipeline

cd ~/catkin_ws/src
git clone --recursive https://github.com/Tossy0423/yolov4-for-darknet_ros.git
1.2 darknet_ros 수정
yolov4 → yolov4-tiny로 수정, 그리고 custom dataset에 해당하는 config, weights 적용

cd ~/catkin_ws/src/yolov4-for-darknet_ros/darknet_ros
git clone https://github.com/zeta0707/darknet_ros_custom.git
cp -rf darknet_ros_custom/* darknet_ros/
package가 준비 되었으므로 빌드합니다.

zeta@zeta-nano:~/catkin_ws/src$ cd ..
zeta@zeta-nano:~/catkin_ws$ cma
2. darknet_ros 실행
2.1 coco 객체 PicknPlace
#terminal #1, object detect using Yolo_v4
jetson@jp4612GCv346Py37:~$ roslaunch darknet_ros yolo_v4.launch

#terminal #2, camera publish, object x/y -> robot move
jetson@jp4612GCv346Py37:~$ roslaunch jessiarm_control yolo_chase.launch      



