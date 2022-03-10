## 4. ROS2と関連パッケージのセットアップ
1. ROS2と関連パッケージのインストール
2. ROS2の動作テスト


### 4.1. ROS2と関連パッケージのインストール
本資料では`ROS2 Eloquent Elusor`(通称：`eloquent`)を使用する

任意の方法で`Raspberry Pi`を操作できるようにする
本資料ではssh接続を使用する(`<IP_ADDRESS_OF_RASPBERRY_PI>`は`Raspberry Pi`のIPアドレスに置き換える)
`ssh ubuntu@<IP_ADDRESS_OF_RASPBERRY_PI>`

以下のコマンドを実行してロケールの設定を行う
`sudo apt update && sudo apt install locales`
`sudo locale-gen en_US en_US.UTF-8`
`sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8`
`export LANG=en_US.UTF-8`
`locale`

以下のコマンドを実行してソースの設定を行う
`sudo apt update && sudo apt install curl gnupg2 lsb-release`
`curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -`
`sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'`

以下のコマンドを実行してパッケージのインストールを行う
`sudo apt update`
`sudo apt install ros-eloquent-ros-base`

以下のコマンドを実行してROS関連パッケージのインストールを行う
`sudo apt install python3-argcomplete python3-colcon-common-extensions libboost-system-dev build-essential`
`sudo apt install ros-eloquent-hls-lfcd-lds-driver`
`sudo apt install ros-eloquent-turtlebot3-msgs`
`sudo apt install ros-eloquent-dynamixel-sdk`
`sudo apt install libudev-dev`

`mkdir -p ~/turtlebot3_ws/src && cd ~/turtlebot3_ws/src`
`git clone -b eloquent-devel https://github.com/ROBOTIS-GIT/turtlebot3.git`
`git clone -b ros2-devel https://github.com/ROBOTIS-GIT/ld08_driver.git`
`cd ~/turtlebot3_ws/src/turtlebot3`
`rm -r turtlebot3_cartographer turtlebot3_navigation2`
`cd ~/turtlebot3_ws/`
`source /opt/ros/eloquent/setup.bash`
`colcon build --symlink-install --parallel-workers 1`

特にこれといったエラーメッセージなどが表示されないようであれば問題ない


### 4.2. ROS2の動作テスト
任意の方法で`Raspberry Pi`を操作できるようにする
本資料ではssh接続を使用する(`<IP_ADDRESS_OF_RASPBERRY_PI>`は`Raspberry Pi`のIPアドレスに置き換える)
`ssh ubuntu@<IP_ADDRESS_OF_RASPBERRY_PI>`

以下のコマンドを実行してtestトピックに対してメッセージを送信する(publisher側)
`ros2 topic pub /test std_msgs/msg/String "{data: 'Hello, world'}"`

別のターミナル(`Raspberry Pi`に接続した状態)を立ち上げる
以下のコマンドを実行してtestトピックからメッセージを受信する(subscriber側)
`ros2 topic echo /test`
以下のような出力を確認できれば良い

publisher側
```
publisher: beginning loop
publishing #1: std_msgs.msg.String(data='Hello, world')
publishing #2: std_msgs.msg.String(data='Hello, world')
publishing #3: std_msgs.msg.String(data='Hello, world')
...
```
subscriber側
```
data: Hello, world
---
data: Hello, world
---
data: Hello, world
---
...
```


### 参考資料
- TurtleBot3 3.2. SBC Setup
https://emanual.robotis.com/docs/en/platform/turtlebot3/sbc_setup/#sbc-setup
- ROS 2 Eloquent Elusor (codename ‘eloquent’; November 22nd, 2019)
https://docs.ros.org/en/eloquent/Releases/Release-Eloquent-Elusor.html
- Installing ROS 2 via Debian Packages
https://docs.ros.org/en/eloquent/Installation/Linux-Install-Debians.html

