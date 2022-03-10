## 5. .bashrcの確認と編集
任意の方法で`Raspberry Pi`を操作できるようにする
本資料ではssh接続を使用する(`<IP_ADDRESS_OF_RASPBERRY_PI>`は`Raspberry Pi`のIPアドレスに置き換える)
`ssh ubuntu@<IP_ADDRESS_OF_RASPBERRY_PI>`

以下のファイルを任意のエディターで開く
`~/.bashrc`
ここでは`nano`エディターを使用する
`nano ~/.bashrc`

ファイルの末尾へ以下の内容を追記する
ただし、exportコマンドの値(`30`, `burger`, `LDS-01`)は自身の環境に合わせて変更する
`source /opt/ros/eloquent/setup.bash`
`source ~/turtlebot3_ws/install/setup.bash`
`export ROS_DOMAIN_ID=30`
`export TURTLEBOT3_MODEL=burger`
`export LDS_MODEL=LDS-01`

追記したら別名保存を行いエディターを閉じる
※ 注意：ファイルを保存する際、末尾に空行を1行以上入れておく

以下のコマンドを実行して追記内容の反映を行う
`source ~/.bashrc`

特にこれといったエラーメッセージなどが表示されないようであれば問題ない


### 参考資料
- TurtleBot3 3.2. SBC Setup
https://emanual.robotis.com/docs/en/platform/turtlebot3/sbc_setup/#sbc-setup
