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
