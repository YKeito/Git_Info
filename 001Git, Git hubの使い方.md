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
## 1. リモートリポジトリの作成
+ 初めにローカルリポジトリのデータを保存するためのリモートリポジトリを用意しましょう。(Drop boxでフォルダを作成するイメージ)
1.  Git hubにログインし、ホームでRepositoriesのタブをクリックし、Newをクリックしてください。
![test](https://github.com/YKeito/Git_Info/blob/master/Img/2020-08-24-000005.bmp)
1.  Repository nameに任意の名前を入力し、Public(リンクさえ知っていれば誰でもアクセスできる状態), Private(リンクを知っていても、権限がない場合アクセスできない状態)を選択し、Initialize this repository with a READMEをクリックし、Create repositoryをクリックしてください。
![test](https://github.com/YKeito/Git_Info/blob/master/Img/2020-08-24-000008.bmp)
1.  以上でリモートリポジトリの作成完了です。

## 2. Git hubにssh接続設定
+ ローカルリポジトリからリモートリポジトリに接続するためにはssh接続をできるようにする必要があります。以下を参考に設定してみましょう。
### Private Key, Public Keyの作成
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

### Public Keyのupload
1.  以下のURLへアクセスしてください。

      https://github.com/settings/keys

1.  画面右上のNew SSH Keyを押下し、先ほどコピーした鍵をKeyのテキストボックスに貼り付けます。

1.  任意のtitle名を入力し、Add SSH keyを押下します。

### SSH_Configの設定
1. `vi ~/.ssh/config`と入力し、configファイルを以下のように記載してください。
    ```
    Host github.com
      HostName github.com
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
1. 以上で。Git hubにssh接続設定の完了です。

## 3. 作成したリモートリポジトリをローカルリポジトリに反映
+ Git Bashを利用し、ローカルリポジトリの内容をリモートリポジトリに反映するためには、リモートリポジトリをローカルPCのフォルダに複製する必要があります。(Drop box上のフォルダをエクスプローラーに反映させるイメージ)
1.  作成したリモートリポジトリのホーム画面から、Codeをクリックし、Clone with repositoryの内容をコピーしてください。 
![test](https://github.com/YKeito/Git_Info/blob/master/Img/2020-08-24-000009.bmp)
1.  ターミナルを開き、Documentsに移動してください。
    ```
    cd ./Documents
    ```
    
1.  リモートリポジトリをローカルリポジトリに複製してください。
    ```
    git clone Clone with repositoryの内容を貼り付け
    ```
    
1.  エクスプローラーを開き(win + e)、複製されているか確認してください。

1.  以上で、作成したリモートリポジトリをローカルリポジトリに反映の終了です。

## 4. ローカルリポジトリの変更内容をリモートリポジトリに反映
+ Git Bashを利用し、ローカルリポジトリの内容をリモートリポジトリに反映するためには、git コマンドを学ぶ必要があります。
1.  ファイル（フォルダ）をステージングに追加
    ファイル名を指定する場合
    ```
    git add ファイル名
    ```
    作業ディレクトリ(ローカルリポジトリ)のすべての内容を指定する場合
    ```
    git add .
    ```

1.  ステージングに追加した内容をリモートリポジトリに反映
    ```
    git commit -m '任意のメッセージ'
    ```
1.  リモートリポジトリに、ローカルリポジトリの内容を反映
    ```
    git push origin master
    ```
1.  ローカルリポジトリにリモートリポジトリの内容を反映
    リモートリポジトリに変更内容を保存すると、リモートリポジトリの内容が最新の状態ではないので、内容を更新する必要があります。自分以外の人間がリモートリポジトリに変更を加えたときや、コマンドを使わず、リモートリポジトリから内容を反映させた場合、`git push`の前に以下のコマンドが必要となります。
    ```
    git pull origin master
    ```

1.  以上で、ローカルリポジトリの変更内容をリモートリポジトリに反映の終了です。
## 5. git コマンドの補足
1.  ステージングの内容をリセット
    addした内容を取り消したいとき
    ```
    git reset
    ```
1.  commitのリセット
    ```
    git revert <コミットハッシュ>
    ```
1.  コミットハッシュの確認
    ```
    git log
    ```
    以下のような結果が返ってきます。
    ```
    commit c4e891ff741cc8804755aefd27000cfe9290aa9a (HEAD -> master, origin/master, origin/HEAD)
    Author: Yasue <keito.yasue@hpe.com>
    Date:   Mon Aug 24 19:08:50 2020 +0900

        test

    commit d791d5c6b84a3bce2dff47a51a8c6972e2db61d1
    Author: YKeito <47684684+YKeito@users.noreply.github.com>
    Date:   Mon Aug 24 18:58:23 2020 +0900

        Update 001Git, Git hubの使い方.md
                    :
                    :
                    :
    ```
    ***c4e891ff741cc8804755aefd27000cfe9290aa9a***や***d791d5c6b84a3bce2dff47a51a8c6972e2db61d1***がコミットハッシュです。

