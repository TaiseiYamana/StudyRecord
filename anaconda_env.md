# anacondaの仮想環境でjupyter notebookを動かす。
参考：https://qiita.com/yuj/items/b9e82aeb0e4b2ffd34b9

## 1.base環境でjupyter_environment_kernelsのインストール
`$ pip install environment_kernels`

## 2.Jupyterの設定ファイル生成
`$ jupyter notebook --generate-config`

## 3./Users/userName/.jupyter/jupyter_notebook_config.pyに記述  
```
c.NotebookApp.kernel_spec_manager_class='environment_kernels.EnvironmentKernelSpecManager'
c.EnvironmentKernelSpecManager.env_dirs=['仮想環境が生成されているパス']
```
仮想環境のパスの確認
```
conda info -e
```
## 4.仮想環境側でjupyterをインストール
```
conda install jupyter
```
https://www.it-swarm-ja.tech/ja/keras/importerror%EF%BC%9Apydot%E3%81%AE%E3%82%A4%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%88%E3%81%AB%E5%A4%B1%E6%95%97%E3%81%97%E3%81%BE%E3%81%97%E3%81%9F%E3%80%82-%E3%80%8Cpydotprint%E3%80%8D%E3%82%92%E6%A9%9F%E8%83%BD%E3%81%95%E3%81%9B%E3%82%8B%E3%81%AB%E3%81%AF%E3%80%81pydot%E3%81%A8graphviz%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B%E5%BF%85%E8%A6%81%E3%81%8C%E3%81%82%E3%82%8A%E3%81%BE%E3%81%99/834870070/

# SVGでモデルを表示するための事前準備
https://www.it-swarm-ja.tech/ja/keras/importerror%EF%BC%9Apydot%E3%81%AE%E3%82%A4%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%88%E3%81%AB%E5%A4%B1%E6%95%97%E3%81%97%E3%81%BE%E3%81%97%E3%81%9F%E3%80%82-%E3%80%8Cpydotprint%E3%80%8D%E3%82%92%E6%A9%9F%E8%83%BD%E3%81%95%E3%81%9B%E3%82%8B%E3%81%AB%E3%81%AF%E3%80%81pydot%E3%81%A8graphviz%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%99%E3%82%8B%E5%BF%85%E8%A6%81%E3%81%8C%E3%81%82%E3%82%8A%E3%81%BE%E3%81%99/834870070/
```
pip install pydot
pip install pydotplus
Sudo apt-get install graphviz
```
