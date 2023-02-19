# LAMP環境構築メモ <a id="TOP"></a>

### **Index**

| [LAMPについて](#202301281000) | [ブートUSBの作成](#202301281748) | [ブートUSBの起動](#202302092321) | [CentOSインストール](#202302101739) | [Linuxコマンド](#202302121019) | [ルーター](#202302102308) | [SSH](#202302111947) | [Apache](#202302120812) | [FTP](#202302121037) | [vi](#202302130554) | [ユーザー管理](#202302130631) | [PHP](#202302142236) | [MariaDB](#202302162306) | [PHP+MariaDB](#202302170022) | [WordPress](#202302170208) | Samba |
***

<a id="202301281000"></a>
# <b>LAMPについて</b>

1. LAMP選択
    * [OS](https://ja.hostadvice.com/marketshare/os/jp/) : [**CentOS Stream**](https://www.centos.org/)  
    * [Web Server](https://manuon.com/webserver-share-ranking/#index_id4) : [**Apache**](https://httpd.apache.org/)  
    * [RDBMS](https://db-engines.com/en/ranking) : [MySQL](https://www.mysql.com/jp/) / [**MariaDB**](https://mariadb.com/kb/ja/mariadb/) / [SQLite](https://sqlite.org/)  
    * [Server Side Programming](https://w3techs.com/technologies/overview/programming_language) : [**PHP**](https://www.php.net/) / [Python](https://www.python.jp/)  

1. インストール順序  
    1. [CentOS](#202302101739)
    1. [SSH](#202302111947)
    1. [Apache](#202302120812)
    1. [FTP](#202302121037)
    1. [PHP](#202302142236)
    1. [MariaDB](#202302162306)
    1. [WordPress](#202302170208)（オプション）
    1. Samba（オプション）

作成者：夢寐郎  
作成日：2023年1月28日  
更新日：2023年2月16日  
[[TOP]](#TOP)  


<a id="202301281748"></a>
# <b>ブートUSBの作成</b>

CentOS Streamをインストールするための「ブートUSB」を作成します  

1. CentOSのISOファイルをダンロード
    1. https://www.centos.org/download/ を開く
    1. 「CentOS Stream]-[8]-[x86_64] を選択
    1. http://ftp.riken.jp/Linux/centos/8-stream/isos/x86_64/ 等から選択
    1. [CentOS-Stream-8-x86_64-20230125-dvd1.iso] を選択しダウンロード（約11GB）

1. Rufusのダウンロード
    * Rufus（ルーファス）について  
        * The Reliable USB Formatting Utility, with Sourceの略
        * USBメモリを起動可能ドライブにするフリーソフトウェア
        * Windows対応
    1. https://rufus.ie/ja/ を開く
    1. [ダウンロード]-[Rufus 3.21] を選択

1. ブートUSBの作成
    1. 上記でダウンロードした [rufus-3.2.1.exe] を起動し各種設定  
        * デバイス：USBメモリ（16GB以上）
        * ブートの種類：ダウンロード済の上記のISOファイル
        * パーティション構成：MBR（初期値）
        * ターゲットシステム：BIOSまたはUEFI（初期値）
        * ボリュームラベル：CentOS-Stream-8-x86_64-dvd（初期値）
        * ファイルシステム：FAT32（規定）
        * クラスターサイズ：16キロバイト（規定）
        * クイックフォーマット：✓
        * 機能拡張されたラベルとアイコンファイルを作成：なし
        * 不良ブロックを検出：なし
        * 1パス
    1. [スタート] を選択
    1. [ISOイメージモードで書き込む(推奨)] を選択
    1. [CENTOS-STRE] という名前のブートUSBが完成（約15分程度）

参考：[CentOS 8をインストールする手順](https://nwengblog.com/centos8install/#toc1)  
実行環境：Windows 10、Rufus 3.21、CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月9日  
[[TOP]](#TOP)  


<a id="202302092321"></a>
# <b>ブートUSBの起動</b>

＜検証パソコン＞
* 自作PC（2015年3月購入／TSUKUMO eX.）
    * マザーボード：ASUS H97M-PLUS（MicroATX／2014年5月発売）
    * CPU：Intel Core i3-4160（2コア･3.6GHz／2014年7月発売）
    * メモリ：16GB
    * HDD：3TB

* NEC VersaPro VK27MD-G（2013年5月発売）
    * CPU：Intel Core i5-3360M（2012年7月発売）
    * メモリ：8GB
    * HDD：128GB

1. [上記のUSB](#202301281748)をパソコンに挿入し起動
1. [BIOS（UEFI）](https://www.pc-master.jp/jisaku/bios-uefi.html)を起動  
1. USBメモリを優先的に起動させる  
    * [ASUS UEFI BIOS Utility - Ez Mode] の場合  
        (1)「F2」キーで起動
        (1) [Boot Menu(F8)] を選択
        (2) 上記のUSBを選択
    * [NEC（InsydeH20 Setup Utility] の場合
        (1)「F2」キーで起動
        (2) [Boot]-[USB Memory]（上記のUSB）を選択
        (3) [F6]キーで優先順位を最上位に移動
        (4) [F10]キー（Save and Exit）を選択
1. CentOSのインストール画面が表示されたら成功！

実行環境：CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月10日  
[[TOP]](#TOP)  


<a id="202302101739"></a>
# <b>CentOSインストール</b>

1. [ブートUSBの起動](#202302092321)する
1. インストール方法：Install CentOS Stream 8-stream
1. インストール時に使用する言語：日本語 Japanese
1. インストール概要で各種設定  
    * 地域設定
        * キーボード：英語(US),日本語
        * 言語サポート：日本語,English(United States)
        * 時刻と日付：アジア/東京
    * ソフトウェア
        * インストールソース：LABELCE＝CENTOS-STRE（初期値）
        * ソフトウェアの選択：最小限のインストール
    * システム  
        * インストール先：任意（自動構成）
        * KDUMP：有効（初期値）
        * ネットワークとホスト名：オン
            * ホスト名：**centos**.mubirou.com
        * セキュリティーポリシー：未設定（初期値）
    * ユーザーの設定
        * rootパスワード：********
        * ユーザーの設定：未設定（初期値）
1. [インストールの開始] を選択  
（5分程度でインストール完了）

実行環境：CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月10日  
[[TOP]](#TOP)  


<a id="202302121019"></a>
# <b>Linuxコマンド</b>

* 入力の終了
    ```
    > [Ctrl]+[d]
    > [Ctrl]+[z]
    ```
* IPアドレスを調べる
    ```
    # ip address
    ……
    inet 192.168.X.XX/24 ……
    ```

* ファイルの削除  
    ```
    # rm -f xxxx
    ```

* シャットダウン
    ```
    # shutdown now
    ```
* 再起動（UEFI/BIOSの起動順位に注意）
    ```
    # reboot
    ```

作成者：夢寐郎  
作成日：2023年2月12日  
更新日：2023年2月16日  
[[TOP]](#TOP)  


<a id="202302102308"></a>
# <b>ルーター</b>

* NTT GV-ONU（PR-S300SE）
    * 光回線終端装置（ひかり電話ルータ）
    * 設定画面：http://ntt.setup/（192.168.1.1） 
        * ユーザー名：user
        * パスワード：****

* SoftBank E-WMTA2.3（光BBユニット）
    * 2020年から"2.4"を提供中（☎0800-111-2009）
    * 無線LAN規格：IEEE 802.11a/b/g/n/ac
    * 周波数帯：2.4GHz/2.4GHz帯
    * 設定画面：172.16.255.254（192.168.3.1）
        * ユーザー名：user
        * パスワード：****

* ASUS RT-AC68U
    * Wi-Fiルーター
    * 2013年11月発売
    * 無線LAN規格：IEEE802.11a/b/g/n/ac
    * 周波数帯：2.4GHz/2.4GHz帯
    * 設定画面：192.168.1.1（192.168.2.1）
        * ユーザー名：admin
        * パスワード：*****

* IODATA WN-AC583TR（ポケットルーター）
    * 2014年7月発売（2018年6月生産終了）
    * 無線LAN規格：IEEE802.11a/b/g/ac
    * 周波数帯：2.4GHz/2.4GHz帯（同時接続可）
    * ファームウェア：v1.07（2019年2月公開）
    * 無線接続4台まで推奨
    * [マニュアル](https://www.iodata.jp/lib/manual/pdf2/wn-ac583tr.pdf)
    * 192.168.99.1  
        * ユーザー名：なし
        * パスワード：****  
 
作成者：夢寐郎  
作成日：2023年2月11日  
[[TOP]](#TOP)  


<a id="202302111947"></a>
# <b>SSH</b>

* [SSH](https://www.kagoya.jp/howto/it-glossary/server/ssh/)（Secure Shell）のバージョンを調べる
    ```
    # dnf list installed | grep openssh
    openssh.x86_64          8.0p1-17.e18  @anaconda
    openssh-clients.x86_64  8.0p1-17.e18  @anaconda
    openssh-server.x86_64   8.0p1-17.e18  @anaconda
    ```
* 設定ファイルの確認＆変更
    ```
    # vi /etc/ssh/sshd_config
    ```
* Windowsからの操作
    1. https://www.putty.org/ の [Download PuTTy] を選択
    1. [putty-64bit-0.78-installer.msi] をダウンロード＆インストール
    1. PuTTy（パティ）を起動し各種設定＆接続  
        1. フォントの変更  
            [Window]-[Appearance]-[Font settigs]-[Change]：[Ricty Diminished Discord](https://github.com/edihbrandon/RictyDiminished)（サイズ: 18）
        1. 接続先の指定  
            [Session]-[Host Name(or IP address)]：192.168.X.XX（LinuxパソコンのIPアドレス）
        1. 上記の設定の保存  
            [Session]-[Saved Sessions]：CentOS@192.168.X.XX 等に命名して [Save]
    1. Linuxに接続  
        1. [Session] で上記の [CentOS@192.168.X.XX] を選択し [Load]-[Open]  
            * [PuTTy Security Alert] が表示されたら [Accept] し再度接続
        1. [login as:] と表示されたらログイン  
            （rootログイン拒否設定も可）  
            ```
            login as: root
            root@192.168.X.XX's password:
            Last login: Sat Feb 11 13:08:13 2023
            [root@centos ~]#
            ```

参考：『INTRODUCTION NOTES』110頁（2007.7.30）  
実行環境：CentOS Stream 8、OpenSSH 8.0、PuTTy 0.78  
作成者：夢寐郎  
作成日：2023年2月11日  
更新日：2023年2月12日 「フォントの変更」の更新  
[[TOP]](#TOP)  


<a id="202302120812"></a>
# <b>Apache</b>

1. [Apache](https://ja.wikipedia.org/wiki/Apache_HTTP_Server)（Apache HTTP Server）のインストール
    ```
    # dnf -y update ←インストール済パッケージをアップデート
    # dnf -y install httpd
    ```
1. バージョンを調べる  
    ```
    # dnf list installed | grep httpd
    ……
    httpd.x86_64             2.4.37-54.module_el8.8.0+1256...
    httpd-filesystem.noarch  2.4.37-54.module_el8.8.0+1256...
    httpd-tools.x86_64       2.4.37-54.module_el8.8.0+1256...
    ```
    ```
    # httpd -v
    Server version: Apache/2.4.37 (centos)
    Server built:   Jan 31 2023 21:56:20
    ```

1. 各種操作  
    ```
    # systemctl status httpd ←状態の確認
    # systemctl start httpd ←起動
    # systemctl stop httpd ←停止
    # systemctl restart httpd ←再起動
    # systemctl enable httpd ←OS起動時に自動起動オン
    # systemctl disable httpd ←自動起動のオフ
    ```

1. [ファイアウォール](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB)の設定
    1. 稼働状況  
        ```
        # firewall-cmd --list-all
        ……
        services: cockpit dhcpv6-client ssh ←http通信がない
        ……
    1. httpを追加
        ```
        # firewall-cmd --permanent --add-service=http
        ```
    1. ファイアウォールの再起動
        ```
        # firewall-cmd --reload
        ```
    1. 再度、稼働確認
        ```
        # firewall-cmd --list-all
        ……
        services: cockpit dhcpv6-client http ssh ←httpが追加
        ……
        ```

1. 動作確認（WebブラウザでApacheが起動しているLinuxのIPアドレスにアクセス）  

1. 「HTTP SERVER TEST PAGE」と表示されたら成功！  
    （/var/www/html/index.html がない場合）

実行環境：CentOS Stream 8、Apache 2.4.37  
作成者：夢寐郎  
作成日：2023年2月12日  
[[TOP]](#TOP)  


<a id="202302121037"></a>
# <b>FTP</b>

1. [vsftpd](https://linuc.org/study/knowledge/487/)（Very Secure FTP Daemon）のインストール
    ```
    # dnf -y update ←インストール済パッケージをアップデート
    # dnf -y install vsftpd
    ```
1. バージョンを調べる  
    ```
    # dnf list installed | grep vsftpd
    vsftpd.x86_64  3.0.3-35.el8  @appstream
    ```
    ```
    # vsftpd -v
    vsftpd: version 3.0.3
    ```

1. 各種操作  
    ```
    # systemctl start vsftpd ←起動
    # systemctl stop vsftpd ←停止
    # systemctl enable vsftpd ←OS起動時に自動起動オン
    # systemctl disable vsftpd ←自動起動のオフ
    # systemctl is-active vsftpd ←起動しているか確認
    ```

1. 「[vi](#202302130554)」を使いWebページ管理者グループを作成  
    1. [ユーザー管理](#202302130631)を参考に登録ユーザーを確認
        ```
        # cat /etc/group
        ……
        mubirou:x:1000:
        ```
    1. Webページ管理者グループを作成
        ```
        # vi /etc/group
        ……
        apache:x:48:mubirou ←追加（複数登録する場合「,」を付ける）
        mubirou:x:1000:
        ```

1. /var/www の所有者とグループの変更
    1. 所有者とグループ、パーミッションの確認  
        ```
        # ls -l /var/www
        合計 0
        drwxr-xr-x. 2 root root ... cgi-bin
        drwxr-xr-x. 2 root root ... html
        ```
    1. 所有者とグループの変更  
        ```
        # chown -R apache.apache /var/www
        ```
    
1. /var/www のパーミッションの変更
    1. パーミッションの変更  
        d(rwx)(r-x)(r-x) = d(4+2+1)(4+0+1)(4+0+1) = d(755)  
        ＝「パーミッション755のディレクトリ」  
        上記を「パーミッション775のディレクトリ」に変更する  
        ```
        # chmod -R 775 /var/www
        ```
    1. 再度、所有者とグループ、パーミッションの確認  
        ```
        # ls -l /var/www
        …
        drwxrwxr-x. 2 apache apache ... cgi-bin
        drwxrwxr-x. 2 apache apache ... html
        ```

1. [ファイアウォール](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB)の設定
    1. 稼働状況  
        ```
        # firewall-cmd --list-services --permanent
        cockpit dhcpv6-client http ssh ←ftpが無い
        ```
    1. ftpを追加
        ```
        # firewall-cmd --add-service=ftp --permanent
        ```
    1. ファイアウォールの再起動
        ```
        # firewall-cmd --reload
        ```
    1. 再度、稼働確認
        ```
        # firewall-cmd --list-services --permanent
        cockpit dhcpv6-client ftp http ssh ←ftpが追加
        ```

1. [SELinux](https://ja.wikipedia.org/wiki/Security-Enhanced_Linux)の設定  
    1. SELinuxの確認  
        ```
        # getsebool -a | grep ^ftp
        ftpd_anon_write --> off
        ftpd_connect_all_unreserved --> off
        ftpd_connect_db --> off
        ftpd_full_access --> off ←これをonにする必要がある
        ftpd_use_cifs --> off
        ftpd_use_fusefs --> off
        ftpd_use_nfs --> off
        ftpd_use_passive_mode --> off
        ```
    1. SELinuxは有効のままvsftpdのデータ転送を可能にする  
        ```
        # setsebool -P ftpd_full_access on
        ```
    1. 再度、SELinuxの確認  
        ```
        # getsebool -a | grep ftpd_full_access
        ftpd_full_access --> on
        ```

1. [FFFTP](https://forest.watch.impress.co.jp/library/software/ffftp/)（他にも [FileZilla](https://ja.wikipedia.org/wiki/FileZilla) 等あり）によるファイル転送
    1. https://forest.watch.impress.co.jp/library/software/ffftp/ にアクセス
    1. [FFFTP（64bit版）]をダウンロード＆インストール
    1. FFFTPを起動
    1. [新規ホスト] を選択し各種設定  
        * ホストの設定名：XXX@192.168.X.XX（任意）
        * ホスト名（アドレス）：192.168.X.XX（Apacheが起動しているIPアドレス）
        * ユーザー名：mubirou
        * パスワード：XXXX
        * ローカルの初期フォルダ：（Windows上の任意のフォルダ）
        * ホストの初期フォルダ：/var/www/html（Webサーバ上）
    1. [接続] を選択
    1. "ファイルの一覧の取得は正常終了しました"と表示されたら接続成功！
    1. [ローカルの初期フォルダ] に以下の index.html ファイルを作成  
        ```
        <!DOCTYPE html>
        <html lang="ja">
        <head>
            <meta charset="UTF-8">
            <title>xxx</title>
        </head>
        <body>
            Hello,world!
        </body>
        </html>
        ```
    1. [index.html] を選択し [コマンド]-[アップロード] を選択
    1. リモートサイト側に上記ファイルがアップロードされたらファイル転送成功！
    1. Webブラウザ上で 192.168.X.XX にアクセスして Hello,world! と表示されたら大成功！

参考：『INTRODUCTION NOTES』120頁（FTPサーバ）  
参考：『INTRODUCTION NOTES』176頁（パーミッション等）  
実行環境：CentOS Stream 8、vsftpd 3.0.3、FFFTP 5.7  
作成者：夢寐郎  
作成日：2023年2月14日  
[[TOP]](#TOP)  


<a id="202302130554"></a>
# <b>vi</b>

* 起動方法  
    ```
    # vi xxxx ←ファイル不在の場合は新規作成
    # vi -R xxxx ←書込不可で開く（≒ # cat xxxx）
    ```

* 操作方法  
    * モード切替  
        [i]：入力モード（文字の入力）
        [Esc]：編集モード（ファイル保存など）
    * 行番号表示  
        [Esc] → [:set number]
    * 指定行へジャンプ  
        [Esc] → [:行番号]
    * 検索＆検索繰返し  
        [Esc] → [/〇〇]（引続き [n] で検索繰返し）  
    * 保存せず終了  
        [Esc] → [:q!]
    * 保存して終了  
        [Esc] → [ZZ]

参考：『INTRODUCTION NOTES』123頁（viの使い方）  
実行環境：CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月13日  
[[TOP]](#TOP)  


<a id="202302130631"></a>
# <b>ユーザー管理</b>

* 新規ユーザー（一般ユーザー）登録  
    ```
    # useradd 新規ユーザー名
    # passwd 新規ユーザー名
    ```

* ユーザーの削除  
    ```
    # userdel -r 既存ユーザー名
    ```

* ユーザー情報の確認（例）
    ```
    # id mubirou
    uid=1000(mubirou) gid=1000(mubirou) groups=1000(mubirou)
    ```

* グループ管理（例）
    ```
    # cat /etc/group
    ……
    apache:x:48:mubirou,ichiro
    mubirou:x:1000:
    ```

参考：『INTRODUCTION NOTES』112頁（ユーザー管理）  
実行環境：CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月13日  
[[TOP]](#TOP)  


<a id="202302142236"></a>
# <b>PHP</b>

1. PHPのインストール  
    1. インストール可能なPHPの確認  
        ```
        # dnf module list php
        ……
        Name  Stream   ...
        php   7.2 [d]  ... ←初期値
        php   7.3      ...
        php   7.4      ...
        php   8.0      ...
        ```
    1. PHPパッケージ情報の確認（概要）  
        ```
        # dnf info php php-mysqlnd php-json php-mbstring php-gd php-pdo php-xml php-xmlrpc
        ……
        名前  : php
        概要  : PHP scripting language for creating dynamic web sites
        ……
        名前  : php-gd
        概要  : A module for PHP applications for using the gd graphics
        ……
        名前  : php-json
        概要  : JavaScript Object Notation extension for PHP
        ……
        名前  : php-mbstring
        概要  : A module for PHP applications which need multi-byte string handling
        ……
        名前  : php-mysqlnd
        概要  : A module for PHP applications that use MySQL databases
        ……
        名前  : php-pdo
        概要  : A database access abstraction module for PHP applications
        ……
        名前  : php-xml
        概要  : A module for PHP applications which use XML
        ……
        名前  : php-xmlrpc
        概要  : A module for PHP applications which use the XML-RPC
        ```
    1. PHPのインストール  
        ```
        # dnf -y update ←インストール済パッケージをアップデート
        # dnf install -y php php-mysqlnd php-json php-mbstring php-gd php-pdo php-xml php-xmlrpc
        ```
    1. インストール済のPHPパッケージの確認  
        ```
        # dnf list installed | grep php
        php.x86_64           7.2.24-1. ...
        php-cli.x86_64       7.2.24-1. ...
        php-common.x86_64    7.2.24-1. ...
        php-fpm.x86_64       7.2.24-1. ...
        php-gd.x86_64        7.2.24-1. ...
        php-json.x86_64      7.2.24-1. ...
        php-mbstring.x86_64  7.2.24-1. ...
        php-mysqlnd.x86_64   7.2.24-1. ...
        php-pdo.x86_64       7.2.24-1. ...
        php-xml.x86_64       7.2.24-1. ...
        php-xmlrpc.x86_64    7.2.24-1. ...
        ```
    1. Apacheの再起動（Apacheモジュールとして動作しているため）  
        ```
        # systemctl restart httpd
        ```
    1. 動作確認  
        1. test.php を作成
            ```
            <?php
            phpinfo();
            ?>
            ```
        1. FTPソフトを使って /var/www/html/ にアップロード
        1. Webブラウザで http://192.168.X.XX/test.php にアクセス
        1. 各種データが表示されれば成功！

参考：『INTRODUCTION NOTES』121頁（PHP）  
実行環境：CentOS Stream 8、PHP 7.2.24  
作成者：夢寐郎  
作成日：2023年2月15日  
[[TOP]](#TOP)  


<a id="202302162306"></a>
# <b>MariaDB</b>

1. [MariaDB](https://ja.wikipedia.org/wiki/MariaDB)のインストール  
    1. インストール可能なPHPの確認  
        ```
        # dnf list maria*
        mariadb.x86_64         3:10.3.28...
        ……
        mariadb-server.x86_64  3:10.3.28...
        ……
        ```
    1. MariaDBのインストール  
        ```
        # dnf -y update ←インストール済パッケージをアップデート
        # dnf install -y mariadb-server
        ```
    1. インストール済のMariaDBパッケージの確認  
        ```
        # dnf list installed | grep mariadb
        mariadb.x86_64                     3:10.3.28...
        mariadb-backup.x86_64              3:10.3.28...
        mariadb-common.x86_64              3:10.3.28.module_el8.3.0
        mariadb-connector-c.x86_64         3.1.11-2...
        mariadb-connector-c-config.noarch  3.1.11-2...
        mariadb-errmsg.x86_64              3:10.3.28...
        mariadb-gssapi-server.x86_64       3:10.3.28...
        mariadb-server.x86_64              3:10.3.28...
        mariadb-server-utils.x86_64        3:10.3.28...
        ```

1. [ファイアウォール](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB)の設定
    1. 稼働状況  
        ```
        # firewall-cmd --list-services --permanent
        cockpit dhcpv6-client ftp http ssh ←mysqlが無い
        ```
    1. mysqlを追加
        ```
        # firewall-cmd --add-service=mysql --permanent
        ```
    1. ファイアウォールの再起動
        ```
        # systemctl restart firewalld
        ```
    1. 再度、稼働確認
        ```
        # firewall-cmd --list-services --permanent
        cockpit dhcpv6-client ftp http mysql ssh ←mysqlが追加
        ```

1. [SELinux](https://ja.wikipedia.org/wiki/Security-Enhanced_Linux)の設定  
    1. SELinuxの確認  
        ```
        # getsebool -a | grep mysql_connect
        mysql_connect_any --> off ←これをonにする
        mysql_connect_http --> off
        selinuxuser_mysql_connect_enabled --> off ←これをonにする
        ```
    1. SELinuxは有効のままmysqlのデータ転送を可能にする
        ```
        # setsebool -P mysql_connect_any on
        # setsebool -P selinuxuser_mysql_connect_enabled on
        ```
    1. 再度、SELinuxの確認  
        ```
        # getsebool -a | grep mysql_connect
        mysql_connect_any --> on
        mysql_connect_http --> off
        selinuxuser_mysql_connect_enabled --> on
        ```
1. 各種操作  
    ```
    # systemctl status mariadb ←状態の確認
    # systemctl start mariadb ←起動
    # systemctl stop mariadb ←停止
    # systemctl restart mariadb ←再起動
    # systemctl enable mariadb ←OS起動時に自動起動オン
    # systemctl disable mariadb ←自動起動のオフ
    ```

1. 動作確認  
    ```
    # mysql -uroot
    ……
    MariaDB [(none)]> ←これが表示されたら成功！
    MariaDB [(none)]> select user,host from mysql.user;
    +------+--------------------+
    | user | host               |
    +------+--------------------+
    | root | 127.0.0.1          |
    | root | ::1                |
    | root | centos.mubirou.com |
    | root | localhost          |
    +------+--------------------+
    MariaDB [(none)]> quit ←MariaDBからの切断し端末に戻る
    Bye
    ```

参考：『INTRODUCTION NOTES』122頁（MariaDB）  
実行環境：CentOS Stream 8、MariaDB 10.3.28  
作成者：夢寐郎  
作成日：2023年2月17日  
[[TOP]](#TOP)  


<a id="202302170022"></a>
# <b>PHP+MariaDB</b>

1. [PHPをインストール](#202302142236)

1. 動作確認  
    1. sqltest.php を作成
        ```
        <?php
            $con = new PDO('mysql::memory:', 'root', '');
            $statement = $con->prepare("SELECT VERSION()");
            $statement->execute();
            echo $statement->fetchColumn(); //-> 10.3.28-MariaDB
        ?>
        ```
    1. FTPソフトを使って /var/www/html/ にアップロード
    1. Webブラウザで http://192.168.X.XX/sqltest.php にアクセス
    1. "10.3.28-MariaDB" と表示されれば成功！

参考：[Godot+PHP+MySQL](https://github.com/mubirou/Godot-Study-Notes#phpmysql)  
実行環境：CentOS Stream 8、PHP 7.2.24、MariaDB 10.3.28  
作成者：夢寐郎  
作成日：2023年2月17日  
[[TOP]](#TOP)  


<a id="202302170208"></a>
# <b>WordPress</b>

## この項目は書きかけです

1. [WordPress](https://ja.wikipedia.org/wiki/WordPress)のダウンロード＆設定  
    1. https://ja.wordpress.org/ にアクセス
    1. [WordPressを入手]-[ダウンロードしてインストール]-[WordPress 6.1.1をダウンロード] を選択
    1. ダウンロードした wordpress-6.1.1-ja.zip を解凍
    1. **wordpress** フォルダ内の wp-config-sample.php をコピーして名前を **wp-config.php** に変更し以下の通り編集  
        ```
        <?php
        ……
        define( 'DB_NAME', 'wordpress_db' ); ←'database_name_here'から変更
        ……
        define( 'DB_USER', '' ); ←'username_here'から変更（後で変更）
        ……
        define( 'DB_PASSWORD', '〇〇〇' ); ←'password_here'から変更
        ```
    1. **wordpress** フォルダを [FTP](#202302121037) ソフトを使って **/var/www/html** フォルダ内にアップロード  

1. [MariaDB](#202302162306) でデータベースを作成  
    ```
    # mysql -u root -p
    ……
    MariaDB [(none)]> CREATE DATABASE wordpress_db; ←上記と同じDB名
    ```

1. ブログの作成  
    1. http://192.168.3.11/wordpress/wp-admin/install.php にアクセス

参考：『INTRODUCTION NOTES』215頁（WordPress）  
実行環境：CentOS Stream 8、MariaDB 10.3.28、PHP 7.2.24、WordPress 6.1.1  
作成者：夢寐郎  
作成日：2023年2月XX日  
[[TOP]](#TOP)  


© 2023 夢寐郎
