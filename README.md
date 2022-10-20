# jetson_xavier_nx_ubuntu20.04_yolov5  
Jetson Xavier NX Dev kit でYOLOv5をGPUで動かすまでの手順  
準備するもの　　
- Jetson Xavier NX Developer Kit  
- SDカード　32GB以上のもの  
- Micro USB Type-B（2.0）とPCをつなぐデータ転送ができるタイプのケーブル  
- SSD 必須
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
SSDに直接書き込むためSDカードは不要  
NVIDIA SDK Managerをインストールする　（NVIDIAのアカウント登録が必要）  
https://developer.nvidia.com/nvidia-sdk-manager  
<br><br>
#### インストール手順
まず電源を入れる前にSDカードを抜きSSDを本体の裏に装着する  
SDKManagerを起動し、本体とケーブルで接続する  
案内に沿って最新版のイメージをインストールする　DeepStreamは任意で入れる
![image](https://user-images.githubusercontent.com/95160686/196888194-d82529d6-d6ee-4584-8547-202fe4a380eb.png)  
<br>
すべてダウンロードする　JetsonSDKcomponentsにはCUDAのサンプルが入っている  
![image](https://user-images.githubusercontent.com/95160686/196889931-89e2fd3b-3f0e-45cc-960b-68a2067e47bd.png)
![image](https://user-images.githubusercontent.com/95160686/196890151-836eb6ec-75c1-4d64-9742-1345af89079f.png)
![image](https://user-images.githubusercontent.com/95160686/196890718-b51e723e-2ec6-430c-ba26-bab748f7a3fe.png)
![image](https://user-images.githubusercontent.com/95160686/196896923-a2e48a22-80db-4ecf-b42e-e444ab93c5eb.png)

