# jetson_xavier_nx_ubuntu20.04_yolov5  
Jetson Xavier NX Dev kit でYOLOv5をGPUで動かすまでの手順  
準備するもの　　
- Jetson Xavier NX Developer Kit  
- SDカード　32GB以上のもの  
- Micro USB Type-B（2.0）とPCをつなぐデータ転送ができるタイプのケーブル　
<br>　　  
# 1:JetsonへのOSのインストール  
## OSイメージのダウンロード  
次のサイトから  
Jetson Xavier NX Developer Kit SD Card Image  をダウンロードする  
https://developer.nvidia.com/embedded/downloads  
ここでは[JetPack 5.0.2.](https://developer.nvidia.com/jetson-nx-developer-kit-sd-card-image)をダウンロード   
  
この一つ前のバージョンのJetPack 4.6.1. はUbuntu 18.04でJetPack 5.0.2.では Ubuntu 20.04 が起動する  
<br>
![image](https://user-images.githubusercontent.com/95160686/196377663-230894f2-0d87-4f03-bd37-1da613aad8b9.png)

## SDカードへの書き込み  
ダウンロードしたファイルを balenaEtcher や Raspberry Pi Imager を用いてSDカードに書き込む  
書き込み終了後JetsonにSDカードを差し込み起動するか確認する　　
<br><br><br>
うまく起動した場合は運がいい  
なぜだかうまくいかないことが多発している  
原因の可能性　　https://forums.developer.nvidia.com/t/boot-failed-with-find-partition-via-pt-failed/172663/9  
<br>
起動後はこちらのサイトを参考にSSDにシステムボリュームを変更する  
[NVIDIA Jetson Xavier NXのシステムボリュームをNVMe SSDに切り替える](https://dev.classmethod.jp/articles/nvidia-jetson-xavier-nx-system-volume-migrate-to-nvme/)
## うまく起動しなかった場合  
別のインストール方法を行う（こちらのほうが時間がとてもかかる）  
NVIDIA SDK Managerをインストールする  
NVIDIAのアカウント登録が必要  
https://developer.nvidia.com/nvidia-sdk-manager  
<br>
<br>
![image](https://user-images.githubusercontent.com/95160686/196400434-3b7a9392-29fb-41eb-a6b4-7a6a2ea6401b.png)
指示に従い全部インストールする  

# インストール後CUDAサンプルを用いてGPUの確認を行う
jetsonのインストールを行うと自動でCUDAのサンプルが入っている  
usr/local/cuda-11.4内のsamplesをホームに持ってくる
<img src="https://user-images.githubusercontent.com/95160686/197136458-23e3dae2-5d5e-4464-9a8a-e09c58f17c10.png" width="800">
samples/5_Simulations 内のサンプルのどれかに入りmakeを動かそうとすると  
libGLU.soとglu.hがないと言われることがある
![Screenshot from 2022-10-21 15-57-29](https://user-images.githubusercontent.com/95160686/197137999-01fd46a8-bb87-4a3a-b059-61047f14645d.png)
その場合は  
`sudo apt install freeglut3-dev libglu1-mesa-dev`  
これを入力してインストールを行うと無事makeが走るようになる
![Screenshot from 2022-10-21 16-02-00](https://user-images.githubusercontent.com/95160686/197137540-2cad5aed-ccf0-4ed8-9205-790c644cd043.png)
参考にしたサイト　https://ksakiyama134.hatenablog.com/entry/2016/09/25/005130
## 実行例  
マウスカーソルを動かして反応するか試す  
<img src="https://user-images.githubusercontent.com/95160686/197140534-812b792d-09f7-4f79-b1e8-740b9b7fcf51.png" width="400">  
<img src="https://user-images.githubusercontent.com/95160686/197140541-ba2049b6-f6ae-4aab-a2be-e25ac5200aa6.png" width="400">
# Pytorch
Jetsonでは通常のpipでインストールしたPytorchでは動かずうまくGPUを使うことができない  
さらにエラーも出ないので最初はわけがわからなかった  
このようになにか調子が悪いと思ったらその状況をJetson xavier nx特有の問題か調べることが必要となる  
NVIDIA公式サイトからインストールを行う  
https://docs.nvidia.com/deeplearning/frameworks/install-pytorch-jetson-platform/index.html  
<br>
以下は上記サイトを参考にした  
まず`sudo apt update`を行う  
Pytorchに必要なパッケージをインストールする  
<br>
`sudo apt-get -y install autoconf bc build-essential g++-8 gcc-8 clang-8 lld-8 gettext-base gfortran-8 iputils-ping libbz2-dev libc++-dev libcgal-dev libffi-dev libfreetype6-dev libhdf5-dev libjpeg-dev liblzma-dev libncurses5-dev libncursesw5-dev libpng-dev libreadline-dev libssl-dev libsqlite3-dev libxml2-dev libxslt-dev locales moreutils openssl python-openssl rsync scons python3-pip libopenblas-dev;`  
<br>
エクスポート から　インストールまで  
<br>
`export TORCH_INSTALL=https://developer.download.nvidia.com/compute/redist/jp/v50/pytorch/torch-1.12.0a0+2c916ef.nv22.3-cp38-cp38-linux_aarch64.whl
`  
これはJetpack5.0.2用のコマンドで自分のバージョンに応じて`/jp/v502/`の部分を適宜変更する  
<br>
`export TORCH_INSTALL=path/to/1.13.0a0+d0d6b1f2.nv22.09`  
<br>
`python3 -m pip install --upgrade pip; python3 -m pip install aiohttp numpy=='1.19.4' scipy=='1.5.3' export "LD_LIBRARY_PATH=/usr/lib/llvm-8/lib:$LD_LIBRARY_PATH"; python3 -m pip install --upgrade protobuf; python3 -m pip install --no-cache $TORCH_INSTALL`  
よくわからないエラーが出ているがインストールはできる  
<br>
# Torchvision
また当然pytorchのバージョンに対応したtorchvisionをインストール必要がある  
対応しているバージョンは各々の環境で確認が必要  
上のコマンドで入れた人は0.13になるので以下のコマンドを入れる  
`git clone --branch v0.13.0 https://github.com/pytorch/vision torchvision`  
`cd torchvision`  
<br>
権限を求められるので/usr/lib/python3.8に与える  
`sudo chmod 777 -R /usr/lib/python3.8`  
`python3 setup.py install`  
このあとかなり時間がかかってインストールされるので待機  
Pytorchとtorchvisionのインストールが終わったら  
`pip list`　　
で確認を行う。うまくインストールされると下のような表示になる  
![Screenshot from 2022-09-01 00-29-25](https://user-images.githubusercontent.com/95160686/199663841-2043d341-9a57-421b-bed4-b28527600698.png)  

# YOLOv5  
## githubからYOLOv5のファイルをインストールする  
`git clone https://github.com/ultralytics/yolov5`  
`cd yolov5`  
## パッケージのインストール  
次に入力するコマンドでrequirements.txtというファイル内に書かれているパッケージがインストールされるのだが、このままだと先程とは違うバージョンのtorchとTorchivsionをインストールしようとするため、requirements.txtのPytorchとTorchivsionの部分を以下のようにコメントアウトしてからコマンドを入力  
![Screenshot from 2022-11-03 16-09-16](https://user-images.githubusercontent.com/95160686/199664330-38718742-8441-4567-82ff-a22c53cd250a.png)  

`sudo pip3 install -r requirements.txt`  
sudoをしっかりつけることが重要  
`pip list`  で多くのパッケージがインストールされたのを確認する  
<br>
## YOLOv5を実行  
YOLOv5内にはもとからサンプル画像が入っているのでそちらを使って実行してみる  
`python3 detect.py --source ./data/images/bus.jpg `  
ここでIPythonやtqdmやseabornがないと言われた場合は  
`sudo pip3 install ipython`  などそれぞれpipでインストールを行う  
![Screenshot from 2022-11-03 16-20-36](https://user-images.githubusercontent.com/95160686/199666227-7784f9a9-415a-4d95-8b7a-415ee4c8b84b.png)

最初の実行時には学習データのインストールがされるため少し時間がかかる  
ここで注目してほしいのが実行画面の最初の一文の最後で  
`CUDA:0 (GPUの名前, ~~MiB)`と出る  
ここでCPUと出ているとうまくインストールができていないか依存関係が間違っているのでやり直す  
![Screenshot from 2022-11-03 16-30-00](https://user-images.githubusercontent.com/95160686/199667610-5b97d9c9-3ac8-4077-99fd-929f39f57bd5.png)

<br>
うまく動くと`runs`というファイルの中に結果のファイルがdetect~という名前が作成される  

### YOLOv5のオプションについて  
`python3 detect.py`のあとに様々なオプションを設定することができる  
ここでは最低限のもののみ説明する  
<br>
### source  
`--source`コマンドは画像認識するターゲット（ソース）の設定を行う先程のように画像のパスを渡したり、`ファイル名/*`を入力するとファイル内の画像すべてを選択したりできる  
webカメラを接続してリアルタイムで行いたい場合は`--source 0`でカメラからの動画をソースに設定できる  
もしつながらない場合は`lsusb`で番号を調べることができる  
<br>
### weight  
`--weight`コマンドは使用する学習データを変更できる  
なにも入れない状態では`--weights yolov5s.pt`というサンプルの学習データがデフォルトに設定されている  
<br>
### conf  
`--conf`コマンドは尤度の設定で0.~の１未満で設定してそれ以下の場合の認識を表示しなくなる  
簡単にいえば似ている度合いの基準を設定できて、ある程度似ているなら表示や完璧にそっくりなもののみ表示といったことができる  
デフォルトでは0.25に設定されている  
<br>
以上でインストールは完了  
