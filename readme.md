# 🔰SSHってなんなの？

俺は雰囲気でSSH接続を利用している！　という感じだったので理解するために情報をとりまとめてみました。

## 🔰そもそもSSHってなに？

SSHはSecure Shellの略称。

ネットワーク上で安全なリモートログインやネットワークサービスを提供するためのプロトコル。

## 🔰SSHの歴史

- 1995年07月。ヘルシンキ工科大の[Tatu J Ylonen](https://twitter.com/tjssh)さんがSSH1プロトコルと実装を開発して公開。
- 1995年12月。[Tatu J Ylonen](https://twitter.com/tjssh)さんによって[SSH Communications Security](https://www.ssh.com/)が設立。実装は商用ソフトへ。
- 1996年。[SSH Communications Security](https://www.ssh.com/)はSSH1プロトコルとは互換性のないSSH2プロトコルを開発。
- 1998年。[SSH Communications Security](https://www.ssh.com/)はSSH2プロトコルを実装した製品を公開。
- 1999年12月。[OpenBSD](https://www.openbsd.org/)がSSH2プロトコルとSSH1プロトコルを実装した[OpenSSH](https://www.openssh.com/)を公開。
- 2006年。IETF（Internet Engineering Task Force）からSSH2プロトコルについてRFC [4250](https://www.rfc-editor.org/rfc/rfc4250.txt),[4251](https://www.rfc-editor.org/rfc/rfc4251.txt),[4252](https://www.rfc-editor.org/rfc/rfc4252.txt),[4253](https://www.rfc-editor.org/rfc/rfc4253.txt),[4254](https://www.rfc-editor.org/rfc/rfc4254.txt),[4255](https://www.rfc-editor.org/rfc/rfc4255.txt),[4256](https://www.rfc-editor.org/rfc/rfc4256.txt)が発行され標準化される。

SSH2の事を知りたければ下記、RFCを参照すればよい。

- [4250 - The Secure Shell (SSH) Protocol Assigned Numbers](https://www.rfc-editor.org/rfc/rfc4250.txt)
- [4251 - The Secure Shell (SSH) Protocol Architecture](https://www.rfc-editor.org/rfc/rfc4251.txt)
- [4252 - The Secure Shell (SSH) Authentication Protocol](https://www.rfc-editor.org/rfc/rfc4252.txt)
- [4253 - The Secure Shell (SSH) Transport Layer Protocol](https://www.rfc-editor.org/rfc/rfc4253.txt)
- [4254 - The Secure Shell (SSH) Connection Protocol](https://www.rfc-editor.org/rfc/rfc4254.txt)
- [4255 - Using DNS to Securely Publish Secure Shell (SSH) Key Fingerprints](https://www.rfc-editor.org/rfc/rfc4255.txt)
- [4256 - Generic Message Exchange Authentication for the Secure Shell Protocol (SSH)](https://www.rfc-editor.org/rfc/rfc4256.txt)

## 🔰SSHのメジャーバージョンSSH1 と SSH2

SSHは歴史的な経緯からSSH1とSSH2のメジャーバージョンに分かれる。

SSH1とSSH2はそれぞれ互換性はないプロトコルとなっている。

SSH1は脆弱性が存在して、今現在ほぼ廃止な状態。SSH2のみを利用するのがよい。

## 🔰実装

SSHはプロトコルなので商用・フリー含めて色々な実装が存在する。

SSHではサーバ/クライアントとそれぞれ機能があるが、両方実装していたりクライアント機能のみ実装していたり対応はソフトによってまちまち。

- [SSH Communications Security, Inc.](https://www.ssh.com/)が提供する[Tectia SSH](https://www.ssh.com/products/tectia-ssh/)。（商用）
- [OpenBSD](https://www.openbsd.org/)が提供する[OpenSSH](https://www.openssh.com/)
- [OpenSSH](https://www.openssh.com/)をMicrosoftがWindowsに移植した[Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH)
- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/index.html)
- [Tera Term](https://ttssh2.osdn.jp/index.html.ja)
- [WinSCP](https://winscp.net/eng/docs/lang:jp)

etcetc...SSHプロトコルを実装したサーバ/クライアントソフトウェアはフリー・商用含めて色々とあるようです。

世間的にはOpenSSHとか様々なOSに標準でインストールされていたりするのでメジャーな感じらしい。

## 🔰SSHのプロトコルについて調べてみる

SSHがセキュアな通信を行うための仕組みだということが解ったので、どういう仕組でセキュアな通信を実現しているかRFCを読んだりぐぐって得た情報をまとめておく。

なお2006年に発行されたRFCはSSH2プロトコルについて記述されている。

### 🔰Protocol Architecture

[4251 - The Secure Shell (SSH) Protocol Architecture](https://www.rfc-editor.org/rfc/rfc4251.txt)

SSH2プロトコルは3つの下記の主要なサブプロトコルから構成されている。

- Transport Layer Protocol
- User Authentication Protocol
- Connection Protocol

### 🔰Transport Layer Protocol

[4253 - The Secure Shell (SSH) Transport Layer Protocol](https://www.rfc-editor.org/rfc/rfc4253.txt)

- サーバとクライアントでやりとりするパケットのフォーマットについて
- データ圧縮について
- パケットの暗号化に用いる暗号化アルゴリズムについて
- 暗号化に利用するワンタイムセッションキーをクライアントとサーバで安全に交換する方法としてDiffie-Hellman鍵交換方式を指定

Transport Layer Protocolではパケットのフォーマットだったり、圧縮・暗号また暗号に用いる鍵の安全な交換方法について規定されている。

※パケットの暗号化に使うアルゴリズムは3des-cbcを必須実装として、他のアルゴリズムは任意実装だったり推奨だったりのような事が書いてたりする。

### 🔰User Authentication Protocol

Transport Layer Protocolでセキュアな通信を確立した後に、User Authentication Protocolを用いて接続してくるユーザの正当性を判断する。

User Authentication Protocolではサーバとクライアントでどのように認証のやり取りを行うか規定しているようです。

[4252 - The Secure Shell (SSH) Authentication Protocol](https://www.rfc-editor.org/rfc/rfc4252.txt)

RFCを読むと必須実装として規定してるのは公開鍵認証方式だけで、あとの認証方式はアプリ側に委ねられているようですね。

SSHの実装をみてみると

- ホストベース認証
- パスワード認証
- 公開鍵認証
- Kerberos認証
- ワンタイムパスワード認証
- etcetc

等々、実装によって色々なユーザ認証の方式が行われいるようです。

### 🔰Connection Protocol

Transport Layer ProtocolとUser Authentication Protocolが確立された上で、Connection Protocolでサービスを提供する。

[4254 - The Secure Shell (SSH) Connection Protocol](https://www.rfc-editor.org/rfc/rfc4254.txt)

SSHのConnection Protocolでは多重化されたチャネルを利用してセッション開始、擬似端末、x11フォワーディング、tcpポートフォワーディング等々サービスをユーザに提供する。

このチャネルのやりとりについて規定しているようです。

## 🔰各プロトコルをまとめると

各プロトコルでそれぞれ下記のような事を行っているっぽい。

- Transport Layer Protocolで安全な通信の確立
- User Authentication Protocolでユーザの正当性を確立
- Connection Protocolでチャネルを利用してサービスを提供

クライアントとサーバがSSH接続を行う際に、まずはTransport Layer Protocolで安全な通信を確立し、その後にUser Authentication Protocolで接続しようとしているユーザの正当性を確認。
その上でチャネルを利用して、サーバとクライアントでサービスのやりとりを行う。

## 🔰フィンガープリント(finger print)って？

[4255 - Using DNS to Securely Publish Secure Shell (SSH) Key Fingerprints](https://www.rfc-editor.org/rfc/rfc4255.txt)

SSHではクライアントがまだ接続したことのないサーバに接続する際には、サーバ側は検証のためホスト鍵のフィンガープリントを提示すると規定されています。

- ホスト鍵はSSHサーバに格納されている秘密鍵/公開鍵の事。User Authentication Protocolで利用する鍵とはまた別。
- フィンガープリントは公開鍵からハッシュ関数を用いて生成された文字列。公開鍵より短い長さとなり視覚的に人間が比較しやすい。

予め信頼できる方法でサーバ鍵のフィンガープリントを確認しておき、初回接続時にサーバから提示されるフィンガープリントと一致することが確認できれば接続先のサーバについて同一性が確認できる。

[中間者攻撃](https://www.ssh.com/attack/man-in-the-middle)を防ぐような仕組み。

## 🔰総評

SSHを雰囲気で利用している！　という状態からは一歩抜け出せた気がするが、まだなんかゆるふわな気がする。

あとSSHの事を調べてたら、公開鍵認証を雰囲気で利用しているって事に気づいたので別記事でまとめる事にする。
