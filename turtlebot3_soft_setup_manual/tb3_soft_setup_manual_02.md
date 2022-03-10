## 2. ubuntuのセットアップ(初期設定, WiFi, sshの設定)
1. ubuntuの初期設定
2. WiFiの設定
3. sshのインストールと有効化
4. スワップ領域の作成


-----
### 2.1. ubuntuの初期設定
以下の手順に沿ってubuntuを初回起動させる  
※ 注意：バッテリーの接続を外しておく(電源を直接供給するため)

1. `TurtleBot3`の`Raspberry Pi`にディスプレイ(HDMI接続)とキーボード(USB)を接続する
2. `TurtleBot3`の`Raspberry Pi`にmicroSDカード(`1.3.`でubuntuを書き込んだもの)を挿入する
3. `TurtleBot3`の`OpenCR`に直接電源を接続する

電源の接続からしばらく経つとディスプレイにubuntuへログインを促す旨のメッセージが表示される  
デフォルトはユーザ名, パスワード共に`ubuntu`なのでこれを入力する

ログインに成功すると、パスワード変更を促す旨のメッセージが表示される  
現在のパスワードを入力の後、任意の新しいパスワードを入力しパスワードを変更する  
※ 新しいパスワードとしてデフォルトの`ubuntu`を入力すると弾かれる

パスワード変更後、`Welcom to ubuntu 18.04.04 LTS...`とメッセージが表示され、`ubuntu@ubuntu:~$`の位置でカーソルが点滅していれば良い

以下のファイルをスーパーユーザ権限かつ任意のエディターで開く  
`/etc/apt/apt.conf.d/20auto-upgrades`  
ここでは`nano`エディターを使用する  
`sudo nano /etc/apt/apt.conf.d/20auto-upgrades`

ファイル内から以下の記述を探す  
```
APT::Periodic::Update-Package-Lists
APT::Periodic::Unattended-Upgrade
```
これらを以下のように書き換える  
```
APT::Periodic::Update-Package-Lists "0";
APT::Periodic::Unattended-Upgrade "0";
```
書き換えたら上書き保存を行いエディターを閉じる


-----
### 2.2. WiFiの設定
リモートPC(ROSをインストールしている/しようとしているPC)に接続しているWiFiのSSIDとパスワードをメモする

本資料では`netplan`を用いてネットワークの設定を行う

以下のファイルをスーパーユーザ権限かつ任意のエディターで開く  
`~/etc/netplan/50-cloud-init.yaml`
ここでは`nano`エディターを使用する  
`sudo nano ~/etc/netplan/50-cloud-init.yaml`
このyamlファイルには、デフォルトで以下のような記述がされている

```yaml
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
```

ファイルへ以下のように内容を追記する  
`SSID`と`PASSOWORD`は事前にメモしたSSIDとパスワードをそれぞれ記述する

```yaml
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
    wifis:
        wlan0:
            dhcp4: true
            access-points:
                "SSID":
                    password: "PASSWORD"
```

追記したら別名保存を行いエディターを閉じる  
別名保存をしたので、ディレクトリには2つのファイルが存在することになる
```sh
$ ls ~/etc/netplan/
50-cloud-init.yaml  99-cloud-init.yaml
```

以下のコマンドを実行してネットワーク設定の反映および確認を行う  
`sudo netplan apply`
`ip addr`
`ip addr`で出力されたメッセージの`wlan0`の項目に`inet ...`や`valid_lft ...`などがあれば問題ない

以下のコマンドを実行して`Raspberry Pi`を再起動させる  
`sudo reboot`


-----
### 2.3. sshのインストールと有効化
sshのインストールを行う前に、以下のコマンドを実行してシステムの起動遅延防止およびシステムのサスペンドとハイバネーションの無効化を行う  
`systemctl mask systemd-networkd-wait-online.service`
`sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target`

以下のコマンドを実行してsshに必要なパッケージのインストールおよびsshの有効化を行う  
`sudo apt install ssh`
`sudo systemctl enable --now ssh`

以下のコマンドを実行してsshが正しくインストールされていることを確認する  
`ssh -V`
以下のような出力を確認できれば良い(必ずしも同じメッセージが出力されるとは限らない)  
`OpenSSH_7.6p1 Ubuntu-4ubuntu0.6, OpenSSL 1.0.2n  7 Dec 2017`

以下のコマンドを実行して`Raspberry Pi`を再起動させる  
`sudo reboot`


-----
### 2.4. スワップ領域の作成
以下のコマンドを実行してスワップ領域の作成を行う  
※ 以下のようなエラーメッセージは無視して良い  
`swapoff: /swapfile: swapoff failed: No such file or directory`

`sudo swapoff /swapfile`
`sudo fallocate -l 2G /swapfile`
`sudo chmod 600 /swapfile`
`sudo mkswap /swapfile`
`sudo swapon /swapfile`

以下のファイルをスーパーユーザ権限かつ任意のエディターで開く  
`~/etc/fstab`
ここでは`nano`エディターを使用する  
`sudo nano ~/etc/fstab`

ファイルの末尾へ以下の内容を追記する  
`/swapfile swap swap defaults 0 0`

追記したら上書き保存を行いエディターを閉じる  
以下のコマンドを実行し、2GBのスワップ領域が作成されていることを確認する  
`sudo free -h`
以下のような出力を確認できれば良い(Memなど他の項目があっても良い)  
```sh
        total    used    free
Swap:    2.0G      0B    2.0G
```


### 参考資料
- TurtleBot3 3.2. SBC Setup
https://emanual.robotis.com/docs/en/platform/turtlebot3/sbc_setup/#sbc-setup
- Netplan | Backend-agnostic network configuration in YAML
https://netplan.io/
- 【Ubuntu】 /etc/netplan/50-cloud-init.yamlを編集するの止めろ
https://qiita.com/yas-nyan/items/9033fb1d1037dcf9dba5
- 【Linux】systemdとsystemctlコマンド：サービスの自動起動、停止、再起動
https://office54.net/iot/linux/linux-systemd-systemctl
