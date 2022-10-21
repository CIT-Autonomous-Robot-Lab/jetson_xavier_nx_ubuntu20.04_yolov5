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
![image](https://user-images.githubusercontent.com/95160686/196377663-230894f2-0d87-4f03-bd37-1da613aad8b9.png)

## SDカードへの書き込み  
ダウンロードしたファイルを balenaEtcher や Raspberry Pi Imager を用いてSDカードに書き込む  
書き込み終了後JetsonにSDカードを差し込み起動するか確認する　　
<br><br><br>
うまく起動した場合は運がいい  
なぜだか10回中9回はうまくいかない  
原因の可能性　　https://forums.developer.nvidia.com/t/boot-failed-with-find-partition-via-pt-failed/172663/9  
<br>
起動後はこちらのサイトを参考にSSDにシステムボリュームを変更する  
[NVIDIA Jetson Xavier NXのシステムボリュームをNVMe SSDに切り替える](https://dev.classmethod.jp/articles/nvidia-jetson-xavier-nx-system-volume-migrate-to-nvme/)
## うまく起動しなかった場合  
別のインストール方法を行う（こちらのほうが時間がとてもかかる）  
NVIDIA SDK Managerをインストールする  
NVIDIAのアカウント登録が必要  
https://developer.nvidia.com/nvidia-sdk-manager  
<br><br>
![image](https://user-images.githubusercontent.com/95160686/196400434-3b7a9392-29fb-41eb-a6b4-7a6a2ea6401b.png)
支持に従い全部インストールする  

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
以下は上記サイトから引っ張ってきただけ  
まず`sudo apt update`を行う  
Pytorchに必要なパッケージをインストールする  
<br>
`sudo apt-get -y install autoconf bc build-essential g++-8 gcc-8 clang-8 lld-8 gettext-base gfortran-8 iputils-ping libbz2-dev libc++-dev libcgal-dev libffi-dev libfreetype6-dev libhdf5-dev libjpeg-dev liblzma-dev libncurses5-dev libncursesw5-dev libpng-dev libreadline-dev libssl-dev libsqlite3-dev libxml2-dev libxslt-dev locales moreutils openssl python-openssl rsync scons python3-pip libopenblas-dev;`  
<br>
エクスポート から　インストールまで  
<br>
`export TORCH_INSTALL=https://developer.download.nvidia.cn/compute/redist/jp/v502/pytorch/1.13.0a0+d0d6b1f2.nv22.09`  
これはJetpack5.0.2用のコマンドで自分のバージョンに応じて`/jp/v502/`の部分を適宜変更する  
<br>
`export TORCH_INSTALL=path/to/1.13.0a0+d0d6b1f2.nv22.09`  
<br>
`python3 -m pip install --upgrade pip; python3 -m pip install aiohttp numpy=='1.19.4' scipy=='1.5.3' export "LD_LIBRARY_PATH=/usr/lib/llvm-8/lib:$LD_LIBRARY_PATH"; python3 -m pip install --upgrade protobuf; python3 -m pip install --no-cache $TORCH_INSTALL`
# YOLOv5
