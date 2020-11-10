# Cudaのインストール 作業調査

# バージョン指定したcudaをダウンロード
tensorflowのバージョンに応じたバージョンのCuda toolkitが必要。公式サイトの通常ダウンロードのところでは最新バージョンのcudaしかダウンロードできない。

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
公式ドキュメントのQuick start guideを見ながら順次実行していく。
![Screenshot from 2020-11-09 21-01-15](https://user-images.githubusercontent.com/54575368/98539243-57b53480-22cf-11eb-93c6-1cf41b1c2822.png)

## Nouveauドライバの無効化
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
## runlevelを一時的に３にして再起動 & kernel parameterに”nomodest”を追加
### runlevel
- 3 マルチユーザーモード（テキストログイン）
- 5 マルチユーザーモード (グラフィカルログイン)

現在のrunlevelの確認
```
$ runtime
N 5
```
lebel3にして再起動
```
$ systemctl set-default multi-user.target
$ sudo reboot
```
* Ubuntuのところにポイントがある時に「E」をクリック。  
linuxの行のquiete splash　のあとに　"３"と"nomodeset"を記述してctr+X

lebel5にして再起動
```
$ systemctl set-default graphical.target
```
Reboot into runlevel 3 by temporarily adding the number "3" and the word "nomodeset" to the end of the system's kernel boot parameters.
runlevel 3は、Xserverを起動しないことを意味し、nomodesetはnouveauモジュールのロードをブロックします。これは、ビルド後にnvidiaモジュールをロードできるようにするためです。
https://docs.nvidia.com/cuda/archive/10.1/cuda-quick-start-guide/index.html#ubuntu-x86_64-run

## gccのバージョンをグレードダウンする。
既存のgccのバージョン9.3はサポートしていなかった。
```
udo apt -y install gcc-8 g++-8
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 8
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-8 8
```
参考：https://askubuntu.com/questions/1236188/error-unsupported-compiler-version-9-3-0-when-installing-cuda-on-20-04
```
sudo sh cuda_10.1.243_418.87.00_linux.run --silent --override-driver-check
```
