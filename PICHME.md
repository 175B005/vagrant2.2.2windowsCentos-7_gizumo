# vagrant 環境構築
### 　OS：version2.2.2 Centos/7 
### 対象：Gizumo研修用windowsの方

### 掲載期間：GW明けまで。（研修ではあるが、一応
（バック部屋の方への説明用）（一般説明はないと思うが、あればこれを改良して出す。）

---

## 手順

※手順通りに進めると、windowsであれば環境構築できるはずです。

できない場合はファイアーウォールの設定や、開いているディレクトリをの管理者権限など、

ポート、IPアドレスの設定などを確認してください。version も注意です。vagrant 2.2.4未確認

#### まず[box package](https://drive.google.com/open?id=18jG-eFrzcU0MtUAHnrTZULm6gJXtwC8r)をダウンロードする。１GB注意（作業したいディレクトリに配置する。）
- コマンドライン（Cmnder他）を開き、作業ディレクトリに行く。

---

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
---

#### 次にパッケージからboxを作成する　(任意の名前を付けて、boxを作る)

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

---

#### vagrant init で設定ファイルを作る (boxの名前はさっき作ったやつ)

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

---

#### いよいよ、開始です。

```
作業ディレクトリ>vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ishiduki'...
==> default: Matching MAC address for NAT networking...
==> default: Setting the name of the VM: ***
==> default: Vagrant has detected a configuration issue which exposes a
==> default: vulnerability with the installed version of VirtualBox. The
==> default: current guest is configured to use an E1000 NIC type for a
==> default: network adapter which is vulnerable in this version of VirtualBox.
==> default: Ensure the guest is trusted to use this configuration or update
==> default: the NIC type using one of the methods below:
==> default:
==> default:   https://www.vagrantup.com/docs/virtualbox/configuration.html#default-nic-type
==> default:   https://www.vagrantup.com/docs/virtualbox/networking.html#virtualbox-nic-type
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 80 (guest) => 8080 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.***********
    default: SSH username: v*******
    default: SSH auth method: private key
    default: Warning: Connection reset. Retrying...
    default: Warning: Connection aborted. Retrying...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: No guest additions were detected on the base box for this VM! Guest
    default: additions are required for forwarded ports, shared folders, host only
    default: networking, and more. If SSH fails on this machine, please install
    default: the guest additions and repackage the box to continue.
    default:
    default: This is not an error message; everything may continue to work properly,
    default: in which case you may ignore this message.
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /vagrant => C:/Users/runashi-hosi/Desktop/Gizumo/vagrant_direction
Vagrant was unable to mount VirtualBox shared folders. This is usually
because the filesystem "vboxsf" is not available. This filesystem is
made available via the VirtualBox Guest Additions and kernel module.
Please verify that these guest additions are properly installed in the
guest. This is not a bug in Vagrant and is usually caused by a faulty
Vagrant box. For context, the command attempted was:

mount -t vboxsf -o uid=1000,gid=1000 vagrant /vagrant

The error output from the command was:

mount: unknown filesystem type 'vboxsf'

```
ここでこけたら、boxない問題か、port使われてる問題の類だと思われる。。

---

#### 無事起動できたか確認

```
作業ディレクトリ>vagrant status
Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.

作業ディレクトリ>

```

---

#### よっしゃ。では接続...
```
作業ディレクトリ>vagrant ssh
Last login: Mon Apr 29 10:59:01 2019 from 10.0.2.2
[vagrant@localhost ~]$
```
このコマンドが打てない人は、rloginなどで,

ipadress:192.168.33.10(up時に書いてあるSSH address: *******)

username:vagrant(up時に書いてあるSSH username: *******)

通信プロトコル:ssh

で接続ができる。ハズ。。（パスワードは初期はvagrantだったと思う。）

---

#### ここで、ちゃんとプロジェクトが入っているか確認。
```
[vagrant@localhost ~]$ ls
giz  remi-release-7.rpm
[vagrant@localhost ~]$
```
入ってないときは、boxが名前違ってる問題なので、もう一度、box作成からやり直し。

gizlog環境構築の段階まで終わっているので、

何がインストールされてるかだけ、しっかりとチケットを見ておく

---

#### では最後に、サービスを起動して終了。
```
sudo systemctl start nginx
sudo systemctl start php-fpm
mysql -u root -p
Password: slackで送ったpassword
~~~
~~~
mysql>
mysql>exit
Bye
[vagrant@localhost ~]$php artisan migrate --seed

```
- (http://localhst:8080)にアクセス。
- slackにログイン。
- gizlogのhome にリダイレクトで遷移したら、起動完了！！

---

migrateでこける場合。
```
[vagrant@localhost ~]composer install
//でもダメなら
[vagrant@localhost ~]php artisan dump-autoload

```
アクセスができない場合
- シークレットウィンドウで試す。
- sudo systemctl restart nginx

---

#### 最後に

これで環境構築は終わりです。エラーの場合はその情報とともに連絡を。
チケットで構築する内容は完成ですが、何をしたかわからないと思いますので、
GW開けに構築の流れを復習します。
今回のうまくいかない主な原因は、アクセス権限です。
中に入っているファイルのうち、
主に３つのファイル（ディレクトリ含むと大量に権限を変更しましたが、必要なのを抜粋）の権限を変更しました。

- session関係
- AuthUser関係
- laravel.logログファイル


---
