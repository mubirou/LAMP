# LAMP環境構築メモ <a id="TOP"></a>

### **Index**

| [LAMP選択](#202301281000) | [ブートUSBの作成](#202301281748) | [ブートUSBの起動](#202302092321) | [CentOSインストール](#202302101739) | [Linuxコマンド](#202302121019) | [ルーター](#202302102308) | [SSH](#202302111947) | [Apache](#202302120812) | [FTP](#202302121037) | [vi](#202302130554) | [ユーザー管理](#202302130631) | PHP | MySQL | Samba | WordPress |
***

<a id="202301281000"></a>
# <b>LAMP選択</b>

* [OS](https://ja.hostadvice.com/marketshare/os/jp/) : [**CentOS Stream**](https://www.centos.org/)  
* [Web Server](https://manuon.com/webserver-share-ranking/#index_id4) : [**Apache**](https://httpd.apache.org/)  
* [RDBMS](https://db-engines.com/en/ranking) : [**MySQL**](https://www.mysql.com/jp/) / [MariaDB](https://mariadb.com/kb/ja/mariadb/) / [SQLite](https://sqlite.org/)  
* [Server Side Programming](https://w3techs.com/technologies/overview/programming_language) : [**PHP**](https://www.php.net/) / [Python](https://www.python.jp/)  

作成者：夢寐郎  
作成日：2023年1月28日  
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
    ```
* IPアドレスを調べる
    ```
    # ip address
    ……
    inet 192.168.X.XX/24 ……
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
更新日：2023年X月XX日  
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
            [Window]-[Appearance]-[Font settigs]-[Change]：[Ricty Diminished Discord](https://github.com/edihbrandon/RictyDiminished)等
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
        public (active)
        target: default
        icmp-block-inversion: no
        interfaces: eno1
        sources:
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
        public (active)
        target: default
        icmp-block-inversion: no
        interfaces: eno1
        sources:
        services: cockpit dhcpv6-client http ssh ←httpが追加
        ……
        ```

1. 動作確認（WebブラウザでApacheが起動しているLinuxのIPアドレスにアクセス）  
    （「HTTP SERVER TEST PAGE」が表示されたら成功！）

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
        mubirou:x:1000:
        web:x:1001:mubirou ←複数登録する場合「,」を付けて追加
        ```

1. /var/www の所有権とパーミッションの変更  
    1. 所有権の確認  
        ```
        # ls -l /var/
        …
        drwxr-xr-x. ... root root ... www
        ```
    1. 所有権の変更  
        ```
        # chgrp -R web /var/www ←所有権をwebに変更
        ```
    1. パーミッションの変更  
        d(rwx)(r-x)(r-x) = d(4+2+1)(4+0+1)(4+0+1) = d(755)  
        ＝「パーミッション755のディレクトリ」  
        上記を「パーミッション775のディレクトリ」に変更する  
        ```
        # chmod -R 775 /var/www
        ```
    1. 再度、所有権とパーミッションの確認  
        ```
        # ls -l /var/
        …
        drwxrwxr-x. ... root web ... www
        ```

1. [ファイアウォール](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB)の設定
    1. 稼働状況  
        ```
        # firewall-cmd --list-all
        ……
        services: cockpit dhcpv6-client http ssh ←ftpが無い
        ```
    1. httpを追加
        ```
        # firewall-cmd --permanent --add-service=ftp
        ```
    1. ファイアウォールの再起動
        ```
        # firewall-cmd --reload
        ```
    1. 再度、稼働確認
        ```
        # firewall-cmd --list-all
        ……
        services: cockpit dhcpv6-client ftp http ssh ←ftpが追加
        ……
        ```

1. [FFFTP](https://forest.watch.impress.co.jp/library/software/ffftp/)（FTPソフト）による動作確認
    1. https://forest.watch.impress.co.jp/library/software/ffftp/ にアクセス
    1. [FFFTP（64bit版）]をダウンロード＆インストール
    1. FFFTPを起動
    1. [新規ホスト] を選択し各種設定  
        * ホストの設定名：XXX@192.168.X.XX（任意）
        * ホスト名（アドレス）：192.168.X.XX（Apacheが起動しているIPアドレス）
        * ユーザー名：mubirou
        * パスワード：XXXX
        * ローカルの初期フォルダ：（Windows上の任意のフォルダ）
        * ホストの初期フォルダ：/var/www/html
    1. [接続] を選択
    1. "ファイルの一覧の取得は正常終了しました."と表示されたら成功！
    * FTPソフトには [FileZilla](https://ja.wikipedia.org/wiki/FileZilla) 等もあります

参考：『INTRODUCTION NOTES』120頁（FTPサーバ）  
参考：『INTRODUCTION NOTES』176頁（パーミッション等）  
実行環境：CentOS Stream 8、vsftpd 3.0.3、FFFTP 5.7  
作成者：夢寐郎  
作成日：2023年2月13日  
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
    * 保存せず終了  
        [Esc] → [:q!]
    * 保存して終了  
        [Esc] → [ZZ]

参考：『INTRODUCTION NOTES』123頁（viの使い方）  
実行環境：CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月13日  
更新日：2023年2月XX日  
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
    sshd:x:74:
    apache:x:48:
    mubirou:x:1000:
    ```

参考：『INTRODUCTION NOTES』112頁（ユーザー管理）  
実行環境：CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月13日  
更新日：2023年XX月XX日  
[[TOP]](#TOP)  


© 2023 夢寐郎