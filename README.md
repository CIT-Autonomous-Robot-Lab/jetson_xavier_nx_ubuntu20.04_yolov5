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
## うまく起動しなかった場合  
別のインストール方法を行う  
NVIDIA SDK Managerをインストールする  
https://developer.nvidia.com/nvidia-sdk-manager  
<br><br>
