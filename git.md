# Gitの基本

## Gitインストール（window）
https://gitforwindows.org/ よりインストール（設定は初期でOK）
## Gitの確認
ターミナル（Git Bash）を開く。以下コマンドを入力
- $ git version

## gitの初期設定
### Gitの設定ファイルは以下三種類
- system  当該マシンの全ユーザに関する設定
- global  当該ユーザに関する設定
- local   特例のディレクトリ（リポジトリ）に関する設定

### 今回はglobalの設定を行う  
- $ git config --global user.name "github user.name"  //githubのユーザネーム  
- $ git config --global use.email github@example.com  //githubのメアド  
- $ git config --global core.editor "atom --wait"     //gitで使うエディタ vs codeなら"code --wait"  

### 設定の確認
- $ git config --list （個別に見たい場合は$ git config use.emailのようにする）

### globalのgitconfigの置き場
ログインユーザのHOMEディレクトリ配下の .gitconfig（C:\Users\{username}\.gitconfig）
ターミナルから以下のコマンドでも見れる
- $ cat ~/.gitconfig

# Gitの基本操作
ローカル（自分のPC）は3つのエリアに分かれている
- ワークツリー（ファイルを変更する作業場）
- ステージ（コミットの準備をする）
 - git addで変更点をインデックスに保存
- ローカルリポジトリ（スナップショットを記録）
 - git commitでステージのインデックスに保存されているファイルのスナップショットをとる。

# Gitのデータ管理
git addやgit commitをすると、ローカルリポジトリに「圧縮ファイル」「ツリーファイル」「コミットファイル」が作成される。
Gitではこれらのファイルを「Gitオブジェクト」と呼び、「.git/object」ディレクトリの下に保存される。
各ファイルの説明は以下のとおりである。
- Gitオブジェクト
  - 圧縮ファイル（blobオブジェクト）
    - git addをした際にリポジトリに作成される。正確には「blobオブジェクト」と言い、ファイルの中身を圧縮しただけのもの。
  - ツリーファイル（treeオブジェクト）
    -  圧縮ファイルには圧縮前の元々のファイル名の情報が入っていない。ファイル名とファイルの中身の組み合わせ（ファイル構造）を保存するためのものがツリーファイルであり、正確には「treeオブジェクト」言う。
  - コミットファイル（commitオブジェクト）
    - いつ、誰が、何を、何のために、ファイルを変更したのかという情報を保存するもの。正確には「commitオブジェクト」と言う。

## 実際の動きを見てみよう(/・ω・)/
### ローカルリポジトリの作成
ターミナル（Git Bach）を開く。ホームディレクトリへ移動する。
- $ mkdir sample  //ホームディレクトリ直下に「sampleディレクトリ」を作成
- $ cd sample //sampleディレクトリに移動
- $ git init  //現在のディレクトリを（ローカル）リポジトリとして指定する（リポジトリの新規作成）

「sampleディレクトリ」内に「.gitディレクトリ」が作成されているのを確認する。隠しファイルなので注意。これが作成されていればローカルリポジトリの作成は成功だょ

### 圧縮ファイルの作成
- $ echo 'Heloo,World!' >hello  //sampleディレクトリ内にhelloファイルを作成
- $ git hash-object hello
6f1a2edf6e7ab89da8432586c326d64dcb63b37f


このような文字列が返ってくる。
返ってきた文字列は「ハッシュID」と言う。ハッシュIDのうち、先頭2文字をディレクトリ名に、その後ろをファイル名にして圧縮ファイルを保存する。
では、実際に圧縮ファイルを作成してみよう

- $ git add hello //圧縮ファイルを作成

ここで「.git/object」ディレクトリを確認すると「6fディレクトリ」内に「1a2edf6e7ab89da8432586c326d64dcb63b37f」という圧縮ファイルが作成されている。先ほどのハッシュIDと同じことを確認しよう。
※ハッシュIDはファイルの中身に対して一意なもの。中身が同じものなら何度git addしようとも圧縮ファイルが追加で作られることはない。

### ツリーファイルの作成
ファイルの中身の圧縮ファイルまでは作成出来た。次に元々のファイル名とファイルの中身の組み合わせ（ファイル構造）を保存する。
- $ git commit -m 'add hello' //コミットしてツリーオブジェクトを作成
[master (root-commit) 41d0cd8] add hello
 1 file changed, 2 insertions(+)
 create mode 100644 hello
- $ git cat-file -p master^{tree} //masterブランチ上で最後のコミットが指しているツリーファイルの中身を表示する
100644 blob 6f1a2edf6e7ab89da8432586c326d64dcb63b37f    hello

最後のコミットが指しているtreeには、blobオブジェクト「6f1a2edf6e7ab89da8432586c326d64dcb63b37f」が「hello」というファイル名だということが保存されている。

### コミットファイルの確認
ツリーファイルが作成されたことで、ファイル構造を追えるようになった。では、いつ、誰が、何を、何のために変更したのかを確認してみる。
- $ git cat-file -p HEAD  //最新のコミットファイルの中身を表示する
tree 26008fc71908884f26d82a795a9f0b9e176da561
author eiji-noguchi <github@example.com> 1549210901 +0900
committer eiji-noguchi <github@example.com> 1549210901 +0900

  add hello

まず、コミットした時点のtree「26008fc71908884f26d82a795a9f0b9e176da561」が保存されている。これはこのプロジェクトの一番上のディレクトリのツリーファイルとなる。次に作成者の情報とコミットメッセージが保存されている。

###  ファイルの変更
最後にファイルを編集し、その変更をコミットした場合の動きを確認してみよう(´・ω・｀)
- $ vim hello //helloディレクトリを編集し保存する
- $ git add hello
- $ git commit -m 'update hello'
- $ git cat-file -p HEAD
tree 9e610c2a62cae34a4d476e1d4eeb3d057a43a339
parent 41d0cd83f6b491c66fd4f009ef77052a5c8d4673
author eiji-noguchi <github@example.com> 1549211186 +0900
committer eiji-noguchi <github@example.com> 1549211186 +0900

  update hello

編集したディレクトリをコミットし、再度コミットファイルの中身を確認してみると、「**parent**」と言う親コミット情報を保存している。Gitはこのように親コミットを保存することによってコミットの履歴を辿れるようにしている。

#  備考
リポジトリの削除
- rm -rf .git
