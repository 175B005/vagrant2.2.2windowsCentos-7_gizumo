# vagrant 環境構築
### 　OS：version2.2.2 Centos/7 
### 対象：Gizumo研修用windowsの方

---

## 手順

※手順通りに進めると、windowsであれば環境構築できるはずです。
できない場合はファイアーウォールの設定や、開いているディレクトリをの管理者権限など、
ポート、IPアドレスの設定などを確認してください。version も注意です。vagrant 2.2.4未確認

- まず[box package](https://drive.google.com/open?id=18jG-eFrzcU0MtUAHnrTZULm6gJXtwC8r)をダウンロードする。１GB注意（作業したいディレクトリに配置する。）
- コマンドライン（Cmnder他）を開き、作業ディレクトリに行く。

```
作業ディレクトリ>vagrant -v
Vagrant 2.2.2
作業ディレクトリ>vagrant status
default                   not created (virtualbox)

The environment has not yet been created. Run `vagrant up` to
create the environment. If a machine is not created, only the
default provider will be shown. So if a provider is not listed,
then the machine is not created for that environment.
 
```
確認ができたら、OK。 vagrant起動状態だったら、vagrant haltで切ってください。
作業ディレクトリの中に「.vagrant」「vagrantfile」などがある時は、全部消してください
```
作業ディレクトリ>ls
package.box //これ一つあればOK。
```

- 次にパッケージからboxを作成する　(任意の名前を付けて、boxを作る)
「vagrant box add 好きな名前 package.box」

```
	
作業ディレクトリ>vagrant box add ishiduki package.box
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'ishiduki' (v0) for provider:
    box: Unpacking necessary files from: file://作業ディレクトリ/package.box
    box: Progress: 100% (Rate: 213M/s, Estimated time remaining: --:--:--)
==> box: Successfully added box 'ishiduki' (v0) for 'virtualbox'!

作業ディレクトリ>vagrant box list
centos/7 (virtualbox, 1902.01 //カリキュラムでGW前に作ったやつ(バック部屋用説明ｗ)
ishiduki (virtualbox, 0)　　　//今作ったbox

```
今回はpackage.boxというexportしておいたものをあらかじめダウンロードしたので、
このbox作成はすぐに終わります。
カリキュラムでは外部リンクからロードしながらなので、時間がかかっていました。

- vagrant init で設定ファイルを作る (boxの名前はさっき作ったやつ)

```
作業ディレクトリ>vagrant init ishiduki    
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

作業ディレクトリ>
```
- 作れたら、テキストエディターを開いて、コメントイン。
```
config.vm.network "forwarded_port", guest: 80, host: 8080

config.vm.network "private_network", ip: "192.168.33.10"
//ipが使われている可能性があるので、33->44とかに変更しておく。
//一度カリキュラムで開いているので、ちゃんと消せていないと、ipは使われたままです。
```
一応、上のboxNameも、自分の作ったbox名になってるか確認してね。

- いよいよ、開始です。

```
作業ディレクトリ>vagrant up
~~~~
~~~~
~~~~

```
ここでこけたら、boxない問題か、port使われてる問題の類だと思われる。。

- 無事起動できたか確認

```
作業ディレクトリ>vagrant status

```


---
