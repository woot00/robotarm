# robotarm

다크넷 (Darknet):

설명: 다크넷은 딥러닝을 기반으로 하는 오픈 소스 신경망 프레임워크 중 하나입니다. 주로 컴퓨터 비전 및 객체 감지 작업에 사용됩니다. 다크넷은 작은 크기의 네트워크로도 높은 정확도를 달성할 수 있어, 리소스 제약이 있는 환경에서도 효과적으로 사용될 수 있습니다.
욜로 (YOLO - You Only Look Once) 4:

설명: YOLO는 객체 감지를 위한 딥러닝 알고리즘으로, 한 번의 순전파로 이미지를 분석하여 객체를 탐지합니다. YOLO의 주요 특징은 빠른 속도와 높은 정확도입니다. YOLO 4는 YOLO 시리즈의 최신 버전 중 하나로, 더 나은 성능과 객체 감지 능력을 제공합니다.
Blob Tracking:

설명: Blob은 이미지에서 특정 색 또는 밝기로 연결된 픽셀의 묶음을 나타냅니다. Blob Tracking은 객체 추적의 한 방법으로, 연속된 이미지 프레임에서 Blob의 위치를 추적하여 객체의 움직임을 파악합니다. 주로 컴퓨터 비전 및 영상처리에서 사용되며, 움직이는 물체를 식별하고 추적하는 데 유용합니다.


$ cd ~/catkin_ws/src
$ git clone https://github.com/zeta0707/jessiarm.git
$ cd ~/catkin_ws
cd ~/Downloads/opencvDownTo34
sudo patch -p1 /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake -p1 < cv_brige.patch
cd ~/catkin_ws

#전체 빌드
cma

sudo apt install python-pip -y
pip2 install adafruit_pca9685
cd ~/catkin_ws
python src/jessiarm/jessiarm_control/src/servokit_test.py
$ roslaunch jessiarm_joy joy_teleop_axes_jetson.launch
