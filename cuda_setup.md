# Ubuntuにtensorflow,CUDA,cuDNNのセットアップ作業記録

## この記事の内容
tensorflowで機械学習をしたいので、それに対応するCUDAとcuDNNをインストールする手法の記録。localでのインストールを行った。

今回はtensorflow 2.3.0を使用したいので、CUDA10.1とcuDNN7.6のインストールを行った。

### 環境
- Ubuntu 20.04  

### 参考サイト  
https://codelabo.com/posts/20200229081221  
https://medium.com/@exesse/cuda-10-1-installation-on-ubuntu-18-04-lts-d04f89287130  

# 1.事前準備
使いたいtensorfowのバージョンに何のCUDA,cuDNNのバージョンが必要かを調べる。tensorfowの[公式サイト](https://www.tensorflow.org/install/source)で確認できる。
<img width="917" alt="スクリーンショット 2020-11-10 20 06 07" src="https://user-images.githubusercontent.com/54575368/98666304-2d777b80-2390-11eb-976b-b4859b44554c.png">

# 2.CUDAインストーラーをダウンロード
CUDAのインストールというのは実際にはcudatoolkitのインストールである。CUDAのことをcudatoolkitと言い換えている記事もある。

公式サイトの通常ダウンロードのところでは最新バージョンのcudaしかダウンロードできないため、過去のリリースしたものがダウンロードできるアーカイブからダウンロード。

[NVIDIA CUDA Archived Documentation](https://developer.nvidia.com/cuda-toolkit-archive)で自分のダウンロードしいバージョンを選ぶことができる。
各バージョンのドキュメントは[こちら](https://docs.nvidia.com/cuda/archive/)

今回は、runfile[local]で入れていく。
### Cuda 10.1
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

![Screenshot from 2020-11-08 21-42-04](https://user-images.githubusercontent.com/54575368/98465246-53bfde80-220b-11eb-914d-a99cdd7a0453.png)

# インストール 
公式ドキュメントの[Quick start guide](https://docs.nvidia.com/cuda/archive/10.1/cuda-quick-start-guide/index.html#ubuntu-x86_64-run)を見ながら順次実行していく。
![スクリーンショット 2020-11-11 10 45 54](https://user-images.githubusercontent.com/54575368/98755336-af58ba80-240b-11eb-960c-37c5d592343a.png)

## Nouveauドライバの無効化
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
## runlevelを一時的に３にして再起動するためにkernel parameterに"3","nomodest"を追加
`Reboot into runlevel 3 by temporarily adding the number "3" and the word "nomodeset" to the end of the system's kernel boot parameters.`
### runlevel
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

- 手順
起動時、Shiftを押すと、grub menuが出る。
Shiftをしても出ない場合、起動ごとにgrub menuを出す設定をするといい。  
grub menu出し方:https://qiita.com/ricrowl/items/1d038d6b4412feedb25e

* Ubuntuのところにポイントがある時に「E」をクリック。  
linuxの行のquiete splash　のあとに　"３"と"nomodeset"を記述してctr+X

runlevel 3は、Xserverを起動しないことを意味し、nomodesetはnouveauモジュールのロードをブロックこれは、ビルド後にnvidiaモジュールをロードできるようにするためです。


## gccのバージョンをグレードダウンする。
既存のgccのバージョン9.3はサポートしていなかった。
```
udo apt -y install gcc-8 g++-8
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 8
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 8
```
参考：https://askubuntu.com/questions/1236188/error-unsupported-compiler-version-9-3-0-when-installing-cuda-on-20-04
## インストーラーの実行
```
sudo sh cuda_10.1.243_418.87.00_linux.run #--silent
```
.runの実行時では、versino.418のドライバーもインストールするかの選択がある。Quick Start guideの--silentオプションをして実行すると自動でversino.418のドライバーもインストールしてしまうため、先に入れたドライバーと競合してエラーが生じてしまうかもしれないので、何もオプションなしで実行する。

```
echo -e "\n## CUDA and cuDNN paths"  >> ~/.bashrc
echo 'export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashr
source ~/.bashrc
```

cuda　確認
```
nvcc -V
```
