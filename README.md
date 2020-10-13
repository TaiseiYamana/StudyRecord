# Visual Studio CodeとCode Running使用の調査記録
## 調査背景
Visual Studio for mac　ではC#とF#のプロジェクトしかできなかった。WindowsではワークロードからダウンロードできるがVS for macではできない...なんて不便なんだ。 

でもmacでGUIでC++を動かす環境はないだろうかと探したところVS Codeの拡張機能を使用するとVS Code上でgccを通して実行できるとのこと。よって、その使用法を調べたのが始まりである。

## Vs codeとは
VS codeはプログラムのテキストエディタである。さまざまな拡張機能用いることによって、IDEのようなものとして使用できる。

##　使用環境
ディバイス:MAC book pro  
OS:Catelina 10.15.6  

# インストール手順
## Visual Studio Codeのインストール
[公式のダウンロードサイト](https://code.visualstudio.com/Download)からmac版をダウンロードする。

<img width="1261" alt="スクリーンショット 2020-10-12 19 54 21" src="https://user-images.githubusercontent.com/54575368/95738898-f0eb2e00-0cc4-11eb-8f35-e8a84076fffd.png">

するとすでにアプリケーションがインストールされた状態でダウンロードフォルダーにあるからそのままアプリケーションのフォルダーにドラックしていれるといい。
##　C＋＋に関する拡張機能のインストール 　
そしたらアプリを開く。すると拡張機能追加の項目の検索欄で「C++」と検索すると、C++関連の拡張パッケージがでるので「C/C++」と「C/C++ Clang Command Adapter」をインストールする。  
また「Japanese Language Pack for Visual Studio Code」をインストールするとVS Codeが日本語に対応すると便利である。入れた場合再起動を要求される。

<img width="1507" alt="スクリーンショット 2020-10-12 20 04 22" src="https://user-images.githubusercontent.com/54575368/95739667-23e1f180-0cc6-11eb-80ab-96e007e31881.png">

## パソコン側の事前準備

Mac側に事前にgccのコンパイラをインストールしておく。(結構時間かかります) macに最初からあるgccはなんか良くなく、brewでインストールするgccが本物のgccらしいと聞いた。

```
$ brew install gcc
```

Homebrew自体のセットアップは省略する。以下のurlを参照してください。
[Mac Homebrewインストール手順 | Awesome Blog](https://awesome-linus.com/2019/08/17/mac-homebrew-install/)

## ワークスペースの作成
C++用のワークスペースを作成する。メニューバーのファイル→add→

## c_cpp_properties.json を設定する。
code runnnerで実行する場合Mac側のコンパイラーとインクルードを使用するためそれぞれのデータパスをc_cpp_properties.jsonに登録する必要がある。

手順はC++用のワークスペースを開いた状態で、以下の手順を行う。

1.Ctr+Shift+Pでコマンドパレットを開く  
2.C/Cpp:Edit configurations...を選択    
3.c_pp_properties.jsonがワークスペース内に作成される。  
3.c_pp_c_pp_properties.jsonを編集する。  

c_pp_c_pp_properties.jsonの"includePath"のところに自分のディバイスのgccのインクルードパスを確認し、このパスのうち~/includeまでをコピペする。デフォルトであるパスはワークスペース内のヘッダファイルのインクルードに必要で、任意で別のヘッダファイルをワークスペース内に入れとくとそのインクルードも行ってくれるという意味である。  
gccのインクルードパス確認方法
```gccのインクルードパス確認方法
$ gcc -v
この行のパスを確認してコピペ
--with-gxx-include-dir=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/c++/4.2.1
```
g++のコンパイルパスの確認
```g++のコンパイルパスの確認
$ g++ with
```

設定方法は以下のサイト参照。
[C++で始める競プロ / VSCodeの環境構築（Mac） - Qiita](https://qiita.com/YasufumiNakama/items/6efa6975f551a1986e89#c_cpp_propertiesjson-%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)


## codeコマンドのインストール
ターミナルからワークショップを開くことができる。

事前にVScodeのコマンドパレットを開き「シェルコマンド:PATH内に'code'コマンドをインストールします。(Shell Command:Install 'code' command in PATH)を選択する。

次にターミナルに以下のコマンドを実行することによって、ワークショップを簡単に開ける。macのコマンドパレットを開くショートカットは「sift+command＋P」

```
$ code ワークショップのフォルダ名
```

## 標準入力に対応させる

プログラム中に値をユーザーが入力し、処理を行うことを標準入力という。
code runnnerではデフォルトではこの標準入力ができないが、settings.jsonにこの二行を追加すると標準入力が可能になる。

```settings.json
"code-runner.clearPreviousOutput": true,
"code-runner.runInTerminal": true,
```
参照:[VS Codeのsettings.jsonの開き方 - Qiita](https://qiita.com/y-w/items/614843b259c04bb91495)

## 実証
試しにhelloworld.cppを作成し、動かしてみよう。

helloworld.cppを作成する手順。

1.  
2.  
3.  

エディター内のcppのコードをすべて選択した状態で右上にある再生ボタンを押すと実行される。


