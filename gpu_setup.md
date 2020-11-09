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
runlevel 3は、Xserverを起動しないことを意味し、nomodesetはnouveauモジュールのロードをブロックします。これは、ビルド後にnvidiaモジュールをロードできるようにするためです。
