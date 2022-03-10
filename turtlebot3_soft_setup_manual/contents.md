# TrutleBot3 セットアップ手順書
基本的にはTurtleBot3 Quick Start GuideのROS2 Dashing版を参考に進めていく

大まかな流れ
- Raspberry PiまたはリモートPCからubuntuを起動できるようにする
- ubuntuにROS2をインストールする
- TurtleBot3が正しく組み立てられているかを確認する
- リモートPCからTurtleBot3が操作できるかを確認する

目次
1. microSDカードにubuntuを書き込む
    1. ubuntuのダウンロード
    2. Raspberry Pi Imagerのダウンロードとインストール
    3. ubuntuの書き込み
2. ubuntuのセットアップ(初期設定, WiFi, sshの設定)
    1. ubuntuの初期設定
    2. WiFiの設定
    3. sshのインストールと有効化
    4. スワップ領域の作成
3. リモートPCのセットアップ(vscode, sshの設定)
    1. vscodeのダウンロードとインストール
    2. vscodeの設定
    3. sshの設定
    4. Raspberry Piとの接続テスト
4. ROS2と関連パッケージのセットアップ
    1. ROS2と関連パッケージのインストール
    2. ROS2の動作テスト
5. .bashrcの確認と編集
6.  OpenCRのセットアップ(ファームウェアのアップデート, 動作確認)
    1. OpenCRに必要なパッケージのインストール
    2. ファームウェアアップデートの準備, 実施
    3. OpenCRの動作テスト
7.  TurtleBot3の動作テスト(bringup, teleop)
    1. TurtleBot3の起動
    2. テレオペレーションの実行
8. 補足説明
    1. ubuntuを書き込んだmicroSDカードを再利用したい場合
    2. テレオペレーションがうまく動作しない場合
    3. ssh接続が繋がらない場合
    4. VirtualBoxの設定
        1. 設定しておくと便利な機能(クリップボード共有, 画面リサイズ)
        2. TurtleBot3のテレオペレーション実行時に必要な設定


## 参考資料
- ROS2のディストリビューション
https://docs.ros.org/en/rolling/Releases.html
- TurtleBot3 Quick Start Guide(Dashing, Eloquentと同じくubuntu18.04が前提なので, Foxyはubuntu20.04)
https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/


## メモ(後で消す)
- TurtleBot絡みの情報(bringupの仕方など)のチートシートを作る
- 8-3. ssh接続が繋がらない場合→baffaloを例にPCからブラウザ経由でアクセスし接続機器一覧を確認する方法と普通にディスプレイに繋いで確認する方法の2種類を書く
- どこかわかりやすい箇所に更新履歴を作る

