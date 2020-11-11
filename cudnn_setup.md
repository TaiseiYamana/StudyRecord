# Ubuntuにtensorflow,CUDA,cuDNNのセットアップ作業記録
## この記事の内容
tensorflowで機械学習するために、tensorflowのバージョンに対応するCUDAとcuDNNをローカルでインストールした時の作業を記録した。

本記事はtensorflow 2.3.0を使用するために、CUDA10.1とcuDNN7.6のセットアップを行った。

環境
Ubuntu 20.04
参考サイト
https://codelabo.com/posts/20200229081221  
https://medium.com/@exesse/cuda-10-1-installation-on-ubuntu-18-04-lts-d04f89287130

# cuDNNセットアップ編
ダウンロードサイト:https://developer.nvidia.com/cudnn  
事前にnvidiaのアカウント登録が必要。ダウンロードサイトのアーカイブからバージョンが選択できる。
![Screenshot from 2020-11-10 20-11-53](https://user-images.githubusercontent.com/54575368/98667736-2cdfe480-2392-11eb-9ffc-4dc61d8c5c1a.png)

# 1.cudnn7.6のダウンロード
![Screenshot from 2020-11-10 20-19-43](https://user-images.githubusercontent.com/54575368/98668018-99f37a00-2392-11eb-8057-48c505e09335.png)
- Download cuDNN v7.6.5 (November 5th, 2019), for CUDA 10.1
- cuDNN Library for Linux

# 2.tarファイルからのインストール
[公式guide](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#installlinux-tar)を参考にし順次実行していく。
![Screenshot from 2020-11-10 20-31-31](https://user-images.githubusercontent.com/54575368/98668824-c52a9900-2393-11eb-9bc0-3bfa44c9fead.png)
## 3.解凍
```
$ tar -zxvf cudnn-10.1-linux-x64-v7.6.5.32.tgz
```
## 4.事前にインストールしたcudaにcudnnをコピーする
cudnnを`cuda/`か`cuda-<version>/`のどちらかにコピーする。公式ドキュメントには`cuda/`にしているが、`cuda-<version>/`と`cuda/`がリンクされているらしく`cuda-<version>/`にコピーしてもうまくいった。
```
# cuda-<version>/にコピーする場合 
$ sudo cp cuda/include/cudnn*.h /usr/local/cuda-<version>/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda-<version>/lib64
$ sudo chmod a+r /usr/local/cuda-<version>/include/cudnn*.h /usr/local/cuda-<version>/lib64/libcudnn*

# cuda/にコピーする場合
$ sudo cp cuda/include/cudnn*.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```
入れたCUDAのバージョンを`<version>`にいれてください。

# 5.tensorflowのバージョンの変更に伴ってcudaとcudnnのバージョンを変更する場合
### 欲しいバージョンのcudaをインストールしcudaのパスを変更or追記
追記した場合、自動的に最適なCUDAのバージョンのPATHを選んで実行してくれるらしい。参考:https://qiita.com/takeajioka/items/8737fab5cffbe0118fea　　
実際いろんなやり方が存在する。
~/.bashrcに記述したパスを変更する。

例
```
<変更前 10.1>
export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
<変更後 11.0>
export PATH=/usr/local/cuda-11.1/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
### cudnnの変更
- `cuda-<version>/`にcudnnをコピーした場合
pcにロードされているcudaのバージョンを切り替えると自動的に変更後のcudaにコピーされているcudnnのバージョンに切り替わると思う。(まだやってないが)

- `cuda/`にcudnnをコピーした場合
/usr/local/cuda/include  
/usr/local/cuda/lib64  
に変更前のcudnnが入っているので削除し、新たにダウンロードしたバージョンのcudnnを同様にコピーする。
```
sudo rm /usr/local/cuda/include/cudnn*.h
sudo rm /usr/local/cuda/lib64/libcudnn*
```
`cuda/`にコピーしてしまうとcuda変更時に削除作業が要求されるので、`cuda-<version>`にコピーしてcudaごと変更した方が便利かも知れない。
