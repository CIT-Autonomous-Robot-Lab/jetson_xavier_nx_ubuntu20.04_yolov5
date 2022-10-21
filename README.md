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
別のインストール方法を行う  
NVIDIA SDK Managerをインストールする  
NVIDIAのアカウント登録が必要  
https://developer.nvidia.com/nvidia-sdk-manager  
<br><br>
![image](https://user-images.githubusercontent.com/95160686/196400434-3b7a9392-29fb-41eb-a6b4-7a6a2ea6401b.png)
支持に従い全部インストールする  

# インストール後CUDAサンプルを用いてGPUの確認を行う
jetsonのインストールを行うと自動でCUDAのサンプルが入っている  
usr/local/cuda-11.4内のsamplesをホームに持ってくる
![Screenshot from 2022-10-21 15-58-15](https://user-images.githubusercontent.com/95160686/197136458-23e3dae2-5d5e-4464-9a8a-e09c58f17c10.png)
samples/5_Simulations 内のサンプルのどれかに入りmakeを動かそうとすると  
libGLU.soとglu.hがないと言われることがある
![Screenshot from 2022-10-21 15-57-29](https://user-images.githubusercontent.com/95160686/197137999-01fd46a8-bb87-4a3a-b059-61047f14645d.png)
その場合は  
`sudo apt install freeglut3-dev libglu1-mesa-dev`  
これを入力してインストールを行うと無事makeが走るようになる
参考にしたサイト　https://ksakiyama134.hatenablog.com/entry/2016/09/25/005130
![Screenshot from 2022-10-21 16-02-00](https://user-images.githubusercontent.com/95160686/197137540-2cad5aed-ccf0-4ed8-9205-790c644cd043.png)
