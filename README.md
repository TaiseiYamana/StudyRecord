# Visual Studio CodeとCode Running使用の調査記録
## 調査背景
Visual Studio for mac　ではC#とF#のプロジェクトしかできなかった。WindowsではワークロードからダウンロードできるがVS for macではできない...なんて不便なんだ。  
でもmacでGUIでC++を動かす環境はないだろうかと探したところVS Codeの拡張機能を使用するとVS Code上でgccを通して実行できるとのこと。よって、その使用法を調べたのが始まりである。

## Vs codeとは
VS codeはプログラムのテキストエディタである。さまざまな拡張機能用いることによって、IDEのようなものとして使用できる。

#　インストール手順
[公式のダウンロードサイト](https://code.visualstudio.com/Download)からmac版をダウンロードする。

<img width="1261" alt="スクリーンショット 2020-10-12 19 54 21" src="https://user-images.githubusercontent.com/54575368/95738898-f0eb2e00-0cc4-11eb-8f35-e8a84076fffd.png">

するとすでにアプリケーションがインストールされた状態でダウンロードフォルダーにあるからそのままアプリケーションのフォルダーにドラックしていれるといい。

そしたらアプリを開く。すると拡張機能追加の項目の検索欄で「C++」と検索すると、C++関連の拡張パッケージがでるので「C/C++」と「C/C++ Clang Command Adapter」をインストールする。  
また「Japanese Language Pack for Visual Studio Code」をインストールするとVS Codeが日本語に対応すると便利である。入れた場合再起動を要求される。

<img width="1507" alt="スクリーンショット 2020-10-12 20 04 22" src="https://user-images.githubusercontent.com/54575368/95739667-23e1f180-0cc6-11eb-80ab-96e007e31881.png">

```
#include 

```
