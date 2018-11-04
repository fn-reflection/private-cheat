## 本文書での記法
  {{}}は各々によって異なるデータが入ります。(テンプレート記法)  
  {{メールアドレス}} :example@test.com

## 秘密鍵・公開鍵ペアの作成
  ssh-keygen -t rsa -C "{{メールアドレス}}"  
    鍵の名前を変更するときは生成されるディレクトリに注意する (~/.ssh/ではなく\~に作成される)  
    以下の例ではcd ./.sshしてからid_rsa_githubに名前を変更したとする  
    パスフレーズはつける[11桁以上が好ましい]

## 公開鍵の内容をクリップボードに転写[windows]
  clip < id_rsa_github.pub
## ホスティングサービス(Github)に公開鍵を登録
  https://github.com/settings/keys  
  にて鍵の名前(アクセスする端末名が好ましい)とclipしたデータを登録する

## ~/.ssh/configファイルの作成
Host github.com  
  User git  
  Hostname ssh.github.com  
  Port 22  
  IdentityFile "c:\Users\｛｛ログインユーザー名｝｝\.ssh\id_rsa_github"  

## ~/.ssh/configファイルのシンボリックリンクを生成してローカルのGitプログラムに読み込ませる
mklink "C:\Program Files\Git\etc\ssh\ssh_config" "C:\Users\$user_profile\.ssh\config"  
　　"C:\Program Files\Git\etc\ssh\ssh_config"が存在する場合は既存のものをリネームする  

## sshプロトコルによる接続確認
  ssh -T git@github.com
