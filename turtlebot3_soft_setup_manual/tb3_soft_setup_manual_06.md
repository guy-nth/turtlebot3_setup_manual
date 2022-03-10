## 6.  OpenCRのセットアップ(ファームウェアのアップデート, 動作確認)
1. OpenCRに必要なパッケージのインストール
2. ファームウェアアップデートの準備, 実施
3. OpenCRの動作テスト


### 6.1. OpenCRに必要なパッケージのインストール
任意の方法で`Raspberry Pi`を操作できるようにする
本資料ではssh接続を使用する(`<IP_ADDRESS_OF_RASPBERRY_PI>`は`Raspberry Pi`のIPアドレスに置き換える)
`ssh ubuntu@<IP_ADDRESS_OF_RASPBERRY_PI>`

以下のコマンドを実行して`OpenCR`のUSBポートの設定を行う
```
sudo cp `ros2 pkg prefix turtlebot3_bringup`/share/turtlebot3_bringup/script/99-turtlebot3-cdc.rules /etc/udev/rules.d/
```
`sudo udevadm control --reload-rules`
`sudo udevadm trigger`

以下のコマンドを実行して`OpenCR`に必要なパッケージのインストールおよび環境変数の設定を行う
ただし、exportコマンドの値(`burger`)は自身の環境に合わせて変更する
`sudo dpkg --add-architecture armhf`
`sudo apt-get update`
`sudo apt-get install libc6:armhf`
`export OPENCR_PORT=/dev/ttyACM0`
`export OPENCR_MODEL=burger`
`rm -rf ./opencr_update.tar.bz2`


### 6.2. ファームウェアアップデートの準備, 実施
任意の方法で`Raspberry Pi`を操作できるようにする
本資料ではssh接続を使用する(`<IP_ADDRESS_OF_RASPBERRY_PI>`は`Raspberry Pi`のIPアドレスに置き換える)
`ssh ubuntu@<IP_ADDRESS_OF_RASPBERRY_PI>`

以下のコマンドを実行してファームウェアとローダーのダウンロードおよび解凍を行う
`wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2`
`tar -xjf ./opencr_update.tar.bz2`

以下のコマンドを実行してファームウェアのアップロードを行う
`cd ~/opencr_update`
`./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr`

以下のような出力を確認できれば良い
https://emanual.robotis.com/assets/images/platform/turtlebot3/opencr/shell01.png


### 6.3. OpenCRの動作テスト
十分に平らかつ障害物の無い場所に`TurtleBot3`を置き、電源を入れる
起動を確認したら(ドレミファの音が鳴る)、以下の手順に沿って`OpenCR`および左右の`DYNAMIXEL`の動作テストを行う
https://emanual.robotis.com/assets/images/platform/turtlebot3/opencr/opencr_models.png

- `OpenCR`の`PUSH SW 1`を数秒間押し続け、`TurtleBot3`が約30cm前方へ移動することを確認する
- `OpenCR`の`PUSH SW 2`を数秒間押し続け、`TurtleBot3`がその場で180度回転することを確認する

前進と回転が確認できれば良い


### 参考資料
- TurtleBot3 3.2. SBC Setup
https://emanual.robotis.com/docs/en/platform/turtlebot3/sbc_setup/#sbc-setup
