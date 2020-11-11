# Ubuntuにtensorflow,CUDA,cuDNNのセットアップ作業記録
## この記事の内容
tensorflowで機械学習するために、tensorflowのバージョンに対応するCUDAとcuDNNをローカルでインストールした時の作業を記録した。

本記事はtensorflow 2.3.0を使用するために、CUDA10.1とcuDNN7.6のセットアップを行った。

環境
Ubuntu 20.04
参考サイト
https://codelabo.com/posts/20200229081221  
https://medium.com/@exesse/cuda-10-1-installation-on-ubuntu-18-04-lts-d04f89287130


# CUDAセットアップ編
# 1.事前準備
使いたいtensorfowのバージョンに何のCUDA,cuDNNのバージョンが必要かを調べる。tensorfowの[公式サイト](https://www.tensorflow.org/install/source)で確認できる。
<img width="917" alt="スクリーンショット 2020-11-10 20 06 07" src="https://user-images.githubusercontent.com/54575368/98666304-2d777b80-2390-11eb-976b-b4859b44554c.png">

# 2.CUDAインストーラーをダウンロード
CUDAのインストールというのは実際にはCUDA toolkitのインストールである。CUDAとCUDA toolkitが混同しないように。

公式サイトの通常ダウンロードのところでは最新バージョンのcudaしかダウンロードできないため、アーカイブからダウンロード。

[NVIDIA CUDA Archived Documentation](https://developer.nvidia.com/cuda-toolkit-archive)で自分のダウンロードしいバージョンを選ぶことができる。
各バージョンのドキュメントは[こちら](https://docs.nvidia.com/cuda/archive/)

![Screenshot from 2020-11-08 21-42-04](https://user-images.githubusercontent.com/54575368/98465246-53bfde80-220b-11eb-914d-a99cdd7a0453.png)

Archived Releases から目的のCUDAを選択

### CUDA 10.1
- Select Target Platform  
Linux

- Architecutre  
x86_64

- Distribution  
Ubuntu

- Version  
18.04

- Installer Type  
runfile [local]

上記の選択項目にチェックをしてでてきたwgetコマンドをターミナルで実行。
```
wget http://developer.download.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda_10.1.243_418.87.00_linux.run
```

# 3.インストーラーの実行
公式ドキュメントの[Quick start guide](https://docs.nvidia.com/cuda/archive/10.1/cuda-quick-start-guide/index.html#ubuntu-x86_64-run)にはインストーラーの実行前に必要な作業が載っているのでそれを順次実行する。
![スクリーンショット 2020-11-11 10 45 54](https://user-images.githubusercontent.com/54575368/98755336-af58ba80-240b-11eb-960c-37c5d592343a.png)

## 3.1.Nouveauドライバの無効化
- 確認コマンド
```
#nouveauがロードされているかを確認 (何も出なかったら無効化の作業をしなくていいかも)
$ lsmod | grep nouveau
```

- 無効化への変更作業
etc/modprobe.d/blacklist-nouveau.confに    
blacklist nouveau  
options nouveau modeset=0  
を記述して適応する。
```
$ cd /etc/modprobe.d
$ sudo touch blacklist-nouveau.conf
$ sudo chmod 777 blacklist-nouveau.conf
$ echo blacklist nouveau > blacklist-nouveau.conf
$ echo options nouveau modeset=0 >> blacklist-nouveau.conf

$ cat blacklist-nouveau.conf #中身確認
```
カーネルinitramfsを再生成します。
```
sudo update-initramfs -u
```
Reboot into runlevel 3 by temporarily adding the number "3" and the word "nomodeset" to the end of the system's kernel boot parameters.
## 3.2.runlevelを一時的に３にして再起動するためにkernel parameterに"3","nomodest"を追加
※この作業はしなくても大丈夫だった。
### runlevelについて
- 3 マルチユーザーモード（テキストログイン）
- 5 マルチユーザーモード (グラフィカルログイン)

現在のrunlevelの確認
```
$ runtime
N 5
```
- level3への変更
```
$ systemctl set-default multi-user.target
```
- level5への変更
```
$ systemctl set-default graphical.target
```

### 手順
起動時、すぐShiftを押すと、grub menuが出る。
Shiftをしても出ない場合、起動ごとにgrub menuを出す設定をするといい。  
grub menu出し方:https://qiita.com/ricrowl/items/1d038d6b4412feedb25e

* Ubuntuのところにポイントがある時に「E」をクリック。  
![IMG_6926](https://user-images.githubusercontent.com/54575368/98759686-f8f9d300-2414-11eb-873d-4ea9bde1b55d.JPG)
次に表示されるlinuxの行のquiete splash　のあとに　"３"と"nomodeset"を記述してctr+X
![IMG_6934](https://user-images.githubusercontent.com/54575368/98760011-a79e1380-2415-11eb-9884-24f67679e9d4.JPG)

runlevel 3は、Xserverを起動しないことを意味し、nomodesetはnouveauモジュールのロードをブロックこれは、ビルド後にnvidiaモジュールをロードできるようにするためです。

## 3.3 表示にNVIDIAGPUを使用するxorg.confファイルを作成
```
$ sudo nvidia-xconfig
```
これをしたあとに再起動するとマザボからのHDMIの信号がなくなった。GPUの他の映像ポートに繋いで対処。
このコマンドの意味は調べていないので、後で調べる必要がある。


## 3.4.gccのバージョンをグレードダウンする。
既存のgccのバージョン9.3はサポートしていため、最新のgccのバージョンでは、.runの実行でエラーが生じる
```
udo apt -y install gcc-8 g++-8
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 8
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 8
```
参考：https://askubuntu.com/questions/1236188/error-unsupported-compiler-version-9-3-0-when-installing-cuda-on-20-04
tensorflowのドキュメントでは、gcc7.3.1を推奨しているが、gcc8でも実行できた。

## 3.5.runファイルの実行
```
sudo sh cuda_10.1.243_418.87.00_linux.run #--silent
```
.runの実行時では、versino.418のドライバーもインストールするかの選択がある。Quick Start guideの--silentオプションをして実行すると自動でversino.418のドライバーもインストールしてしまうため、先に入れたドライバーと競合してエラーが生じてしまう。何もオプション付けないで実行する。

## 3.6.CUDAのPATHを通す
```
echo -e "\n## CUDA and cuDNN paths"  >> ~/.bashrc
echo 'export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashr
source ~/.bashrc
```

## 3.7.CUDAの確認
```
nvcc -V
```
