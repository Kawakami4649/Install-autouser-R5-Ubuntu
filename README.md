# Install-autouser-R5-Ubuntu README

## 概要

`Install-autouser-R5-Ubuntu` は、E-POST Mail Server 上で DeltaChat 用のユーザーアカウントをブラウザから自動作成する CGI システムです。

Web画面からユーザー名・パスワードを入力すると、

* メールアカウント作成
* DeltaChat 用設定情報生成
* 招待コード（Invitation Code）生成
* QRコード表示

を行い、DeltaChat に簡単に登録できます。

---

## 構成

```
Install-autouser-R5-Ubuntu/
├── install.sh
└── autouser.tar.gz
```

展開後は以下のように配置されます。

```
/usr/lib/cgi-bin/
    autouser.cgi
    autouser.ini

/var/www/html/autouser/
    index.html
    result.html
    setup.jpg
    logo-mini.gif
```

---

## 動作環境

* Ubuntu
* Apache2 + CGI有効
* E-POST Mail Server
* DeltaChat

---

## インストール

### 1. パッケージ展開

```bash
sudo tar xfvz autouser.tar.gz -C /
```

または

```bash
sudo ./install.sh
```

---

### 2. CGI実行権限付与

必要に応じて

```bash
sudo chmod 755 /usr/lib/cgi-bin/autouser.cgi
```

---

### 3. Apache CGI有効化

```bash
sudo a2enmod cgi
sudo systemctl restart apache2
```

---

## 設定ファイル

```
/usr/lib/cgi-bin/autouser.ini
```

### htmlpath

生成ページの配置先

```ini
[htmlpath]
/var/www/html/autouser
```

---

### accesscode

アクセス認証コード

```ini
[accesscode]
secret
```

---

### profile

DeltaChat 用プロファイルテンプレート

例

```ini
DCLOGIN://
%s@%s
v=1
p=%s
ih=192.168.10.1
ip=143
iu=%s
ipw=%s
is=
ic=3
sh=192.168.10.1
sp=25
su=%s
spw=%s
ss=starttls
sc=3
```

各 `%s` には

* メールアドレス
* ドメイン
* パスワード

などが自動挿入されます。

---

## 利用方法

ブラウザで

```
http://サーバー/autouser/
```

へアクセスします。

### 入力項目

#### アカウント作成

新規ユーザー作成を行います。

#### ユーザー名

英数字で入力します。

空欄の場合はランダム生成されます。

#### パスワード

12文字以内。

#### QRコード

生成用文字列。

---

## 実行後

以下が表示されます。

* 作成されたメールアドレス
* DeltaChat用招待コード
* QRコード
* コピー用ボタン

スマートフォン版 DeltaChat で

```
Create New Profile
↓
Use Other Server
↓
Scan Invitation Code
```

を選択し、表示された QR コードを読み込むことで設定できます。

---

## Webファイル

### index.html

アカウント作成画面

```
/var/www/html/autouser/index.html
```

### result.html

作成結果表示画面

```
/var/www/html/autouser/result.html
```

### setup.jpg

DeltaChat設定手順画像

```
/var/www/html/autouser/setup.jpg
```

---

## CGI

```
/usr/lib/cgi-bin/autouser.cgi
```

機能

* ユーザー作成
* パスワード設定
* DeltaChat接続情報生成
* 招待コード生成
* HTML結果出力

---

## 使用例

1. ブラウザでアクセス

```
http://mailserver/autouser/
```

2. ユーザー名

```
user01
```

3. パスワード

```
pass1234
```

4. 作成

↓

生成される内容

```
user01@example.com

DCLOGIN://
...
```

↓

DeltaChat の「Scan Invitation Code」で読み込むだけで利用開始できます。

---

## ライセンス

```
© 2026 e-POST Inc.
All rights reserved.
```

## 用途

本パッケージは、E-POST Mail Server 上で DeltaChat アカウントの配布・初期設定を簡略化し、利用者が QR コードを読み取るだけでメッセージ環境を利用開始できるようにするための自動ユーザー登録システムです。

