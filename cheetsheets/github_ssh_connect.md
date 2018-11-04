# githubのssh接続手順 [windows]
## 本文書でのテンプレート記法
  ユーザーによって記述が異なる部分を{{}}で表現します。  
  {{メールアドレス}}の例:example@test.com  
  {{windowsユーザー名}}の例:tanakatarou

## 秘密鍵・公開鍵ペアの作成

```console
ssh-keygen -t rsa -C "{{メールアドレス}}"  
```

鍵の名前を変更するときは生成されるディレクトリに注意する (~/.ssh/ではなく\~に作成される)  
以下の例ではcd ./.sshしてからid_rsa_githubに名前を変更したとする  
パスフレーズはつける[11桁以上が好ましい]

## 公開鍵の内容をクリップボードに転写[windows]

```console
clip < id_rsa_github.pub
```
## ホスティングサービス(Github)に公開鍵を登録
  https://github.com/settings/keys  
  にて鍵の名前(アクセスする端末名が好ましい)とclipしたデータを登録する

## ~/.ssh/configファイルの作成

```config:~/.ssh/config
Host github.com  
  User git  
  Hostname ssh.github.com  
  Port 22  
  IdentityFile "c:\Users\{{windowsユーザー名}}\.ssh\id_rsa_github"  
```

## ~/.ssh/configファイルのシンボリックリンクを生成してローカルのGitプログラムに読み込ませる
```console
mklink "C:\Program Files\Git\etc\ssh\ssh_config" "C:\Users\{{windowsユーザー名}}\.ssh\config"  
```
"C:\Program Files\Git\etc\ssh\ssh_config"が存在する場合は既存のものをリネームする  

## sshプロトコルによる接続確認
```console
  ssh -T git@github.com
```

