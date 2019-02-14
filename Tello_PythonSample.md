# Tello EDUをPythonで動かしてみよう

##  まずはPythonをインストール
[Pythonホームページ](https://www.python.org/downloads/)から対応するものをインストールしよう('ω')ノ
[インストール参考サイト](https://www.python.jp/install/windows/install_py3.html)

インストール後コマンドプロンプトで書きコマンドを入力し、インストールしたPythonのバージョンが返ってくることを確認する。
```
$ python -V
```
##  pythonを動かしてみよう
Pythonをインストールディレクトリに適当なテキストエディターで「
```python:hello.py
print("Hello,Python")
```
」と書かれたファイルを用意し、拡張子を.pyにして保存。
※デフォルトではここにあるかも
- C:\Users\user\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Python 3.x

コマンドプロンプトのディレクトリをPythonをインストールした直下に移動し、以下コマンドでPythonファイルを実行してみる。
```
$ python <ファイル名>.py
```

これで「Hello,Python」って返事が来たらOK

##  セットアップ
- 先ほどと同様にPythonをインストールしたフォルダに[公式サンプルコード](https://dl-cdn.ryzerobotics.com/downloads/tello/20180222/Tello3.py)を置く。
  - https://dl-cdn.ryzerobotics.com/downloads/tello/20180222/Tello3.py
- Telloの電源を入れ、PCからTelloのWifiアクセスポイントへ繋ぐ。(SSIDは[TELLO-XXXX])
- TelloのIPは192.168.10.1なのでpingで接続確認を行う。
  - $ ping 192.168.10.1
