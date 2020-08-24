# 必要なもの
+ PC
+ Git Bashインストール
+ VS Codeインストール
+ Git hubアカウント作成

# 事前準備
## Git Bashインストール
+ Git Bashは、Windowsにてコマンドでgitの操作を利用する際に使うターミナル（いわゆる黒い画面）のこと。
  + [Git Bashインストール方法(Winsows OS)](https://eng-entrance.com/git-install)
  + [Git Bashインストール方法(Mac OS)](https://qiita.com/NorsteinBekkler/items/a0622ee6a39d08d61b72)
  
    *Mac OSはデフォルトでgit bashが入っているが、上を参照にした方がよさそう。
## Visual Studio Codeインストール
+ VS Codeとはテキストエディタのこと。機能性や拡張性などが優れており、使いこなせるようになると実装したい機能を手早くプログラムできるようになるので、便利。個人的に便利だと感じているのは、一つの画面で、スクリプトを書きながら、ターミナルを実行できること。複数のスクリプトを並べながら書けること。
  + [VS Codeインストール方法 MAC OS](https://qiita.com/watamura/items/51c70fbb848e5f956fd6)
  + [VS Codeインストール方法 Windows OS](https://qiita.com/psychoroid/items/7d85ae6bade4a67aedb1)

## Git hubアカウント作成
+ ローカルリポジトリの内容をバックアップするためには、リモートリポジトリを作成する必要があります。以下を参考にアカウントを作成してみましょう。
  + [Git hubのアカウント作成](https://techacademy.jp/magazine/6235)

# Git, Git hubの使い方
## Private Key, Public Keyの作成
1.  Git Bash上で、.sshというディレクトリがあるかを確認してください。  
```
cd; ls -la .ssh/
```
`ls: cannot access '.ssh/': No such file or directory`と表示された方は、`mkdir .ssh`と打ち込んで.sshディレクトリを作成してください。

1.  .sshディレクトリに移動してください。 
```
cd .ssh/
```
1.  引き続きGit Bashにて、以下のコマンドを打ち込んでください。    
```
ssh-keygen -t rsa -f github_test
```

1.  以下の出力が表示されたことが確認出来たら、任意のパスフレーズを入力し、Enterキーを押下してください(セキュリティ上、入力しても表示はされません)。  
```
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): (任意のpassphrase) <Enter>
```
* パスフレーズとはパスワードのようなものですが, 厳密には桁数やスペースが利用できたりするものです. 推測されないような値を設定しましょう.,
* 僕は面倒なので空パスフレーズにしています。（セキュリティを考えるとよくない）

1.  再度パスフレーズを入力し、Enterを押下してください。
```
Enter same passphrase again: (任意のpassphrase) <Enter>
```

1.  以下のような出力がされたかを確認してください。

```
The key's randomart image is:
+---[RSA 3072]----+
|            .+%BO|
|       o   . O+@E|
|      o + . +.O.+|
|       = + o. .+.|
|        S o  . . |
|     . = o    o  |
|      o o .  +   |
|         .  + .  |
|          oo =o  |
+----[SHA256]-----+
```

1.  .ssh配下にgithub_test、github_test.pubがあるかを確認してください。
```
ls -la 
```

1.  sshキーを表示し、表示された鍵(文字列)をコピーしてください。
```
cat github_test.pub
```

## Public Keyのupload

1.  以下のURLへアクセスしてください。

https://github.com/settings/keys

1.  画面右上のNew SSH Keyを押下し、先ほどコピーした鍵をKeyのテキストボックスに貼り付けます。

1.  任意のtitle名を入力し、Add SSH keyを押下します。

## SSH_Configの設定

1. `vi ~/.ssh/config`と入力し、configファイルを以下のように記載してください。
```
Host github.hpe.com
  HostName github.hpe.com
  User git
  Port 22
  IdentityFile ~/.ssh/github_test
```

1. Git Bash上で以下を実行し、パスフレーズを入力してください。
```
ssh -T git@github.hpe.com
```

1. 以下のような応答が返ってくるかを確認してください。  
```
Hi XXXXX! You've successfully authenticated,
but GitHub does not provide shell access.
```


