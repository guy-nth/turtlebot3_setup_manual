## 1. microSDカードにubuntuを書き込む
1. ubuntuのダウンロード
2. Raspberry Pi Imagerのダウンロードとインストール
3. ubuntuの書き込み


-----
### 1.1. ubuntuのダウンロード
本資料では`Ubuntu 18.04`のサーバ版(`Raspberry Pi 3B+`向け)を使用する

以下のwebサイトにアクセスする
- Index of /releases/18.04.4
http://old-releases.ubuntu.com/releases/18.04.4/

webサイト内にある、以下のファイルをダウンロードする  
`ubuntu-18.04.4-preinstalled-server-arm64+raspi3.img.xz`

ダウンロードしたファイルを解凍する  
`.xz`が解凍できれば解凍ソフトは何でも良い(7-Zip, Archive Extractor Onlineなど)  
解凍するとファイル名は以下のようになる  
`ubuntu-18.04.4-preinstalled-server-arm64+raspi3.img`


-----
### 1.2. Raspberry Pi Imagerのダウンロードとインストール
※ `Raspberry Pi Imager`が既にインストール済みなのであれば、この作業は飛ばして良い

以下のwebサイトにアクセスする
- Raspberry Pi OS - Raspberry Pi
https://www.raspberrypi.com/software/

webサイト内にある`Raspberry Pi Imager`をダウンロードする  
ubuntuの書き込み作業を行うPCのOSに合ったものをダウンロードする(本資料ではwindows版の`v1.7.1`を使用する)

ダウンロードしたファイルを実行し、`Raspberry Pi Imager`をインストールする  
インストールする際の設定(ショートカットの作成など)は好みに合わせて変更する


### 1.3. ubuntuの書き込み
`Raspberry Pi Imager`を起動する  
以下の手順に沿って`1.1.`で用意したイメージファイル(`ubuntu-18.04.4-preinstalled-server-arm64+raspi3.img`)をmicroSDカードに書き込む  
※ 注意：microSDカードに保存されているデータは全て上書きされるので必要に応じてバックアップを取る  

1. microSDカードをPCに接続する
2. `CHOOSE OS`→`Use custom`の順に選択
3. 事前にダウンロードした`ubuntu-18.04.4-preinstalled-server-arm64+raspi3.img`を選択
4. `SDHC CARD`を選択
5. 書き込むmicroSDカードを選択する(関係ないメディアを選択しないように注意)
6. `WRITE`を選択, 書き込み処理を実行して良いか確認されるので`YES`を選択
7. 書き込み成功の通知が表示されたら完了

ubuntuの書き込みが完了したら、microSDカードのマウントが解除されたことを確認してPCから外す


### 参考資料
- TurtleBot3 3.2. SBC Setup
https://emanual.robotis.com/docs/en/platform/turtlebot3/sbc_setup/#sbc-setup
- 圧縮・解凍ソフト 7-Zip
https://sevenzip.osdn.jp/
- Archive Extractor Online(アーカイブ エクストラクタ オンライン)
https://extract.me/ja/
