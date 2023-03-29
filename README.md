# LAMP環境構築メモ <a id="TOP"></a>

### **Index**

| [LAMPについて](#202301281000) | [ブートUSBの作成](#202301281748) | [ブートUSBの起動](#202302092321) | [CentOSインストール](#202302101739) | [Linuxコマンド](#202302121019) | [ルーター](#202302102308) | [SSH](#202302111947) | [Apache](#202302120812) | [FTP](#202302121037) | [Vim](#202302130554) | [ユーザー管理](#202302130631) | [PHP](#202302142236) | [MariaDB](#202302162306) | [PHP+MariaDB](#202302170022) | [WordPress](#202302170208) | [Samba](#202302191517) | [SQLite](#202302232039) | [PHP+SQLite](#202302232127) | [Python](#202302232147) | [Python on Apache](#202302251334) | [Python+SQLite](#202302282229) | [Python+MariaDB](#202302282308) | [サンプル](#202303022135) | [ポート開放](#202303151240) | [ドメイン名取得](#202303262200) | [HTTPS](#202303262032) |

***

<a id="202301281000"></a>
# <b>LAMPについて</b>

1. LAMP選択
    * [OS](https://ja.hostadvice.com/marketshare/os/jp/) : [**CentOS Stream**](https://www.centos.org/)  
    * [Web Server](https://manuon.com/webserver-share-ranking/#index_id4) : [**Apache**](https://httpd.apache.org/)  
    * [RDBMS](https://db-engines.com/en/ranking) : [MySQL](https://www.mysql.com/jp/) / [**MariaDB**](https://mariadb.com/kb/ja/mariadb/) / [**SQLite**](https://sqlite.org/)  
    * [Server Side Programming](https://w3techs.com/technologies/overview/programming_language) : [**PHP**](https://www.php.net/) / [Python](https://www.python.jp/)  

1. インストール順序  
    1. [CentOS](#202302101739)
    1. [SSH](#202302111947)
    1. [Apache](#202302120812)
    1. [FTP](#202302121037)
    1. [PHP](#202302142236)
    1. [MariaDB](#202302162306)
    1. [WordPress](#202302170208)（オプション）
    1. [Samba](#202302191517)（オプション）
    1. [SQLite](#202302232039)
    1. [Python](https://www.python.jp/)

作成者：夢寐郎  
作成日：2023年1月28日  
更新日：2023年2月23日 SQLite を追加  
[[TOP]](#TOP)  


<a id="202301281748"></a>
# <b>ブートUSBの作成</b>

CentOS Stream をインストールするための「ブートUSB」を作成します  

1. CentOS の ISO ファイルをダンロード
    1. https://www.centos.org/download/ を開く
    1. 「CentOS Stream]-[8]-[x86_64] を選択
    1. http://ftp.riken.jp/Linux/centos/8-stream/isos/x86_64/ 等から選択
    1. [CentOS-Stream-8-x86_64-20230125-dvd1.iso] を選択しダウンロード（約11GB）

1. Rufus のダウンロード
    * Rufus（ルーファス）について  
        * The Reliable USB Formatting Utility, with Sourceの略
        * USBメモリを起動可能ドライブにするフリーソフトウェア
        * Windows対応
    1. https://rufus.ie/ja/ を開く
    1. [ダウンロード]-[Rufus 3.21] を選択

1. ブート USB の作成
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

1. [上記のUSB](#202301281748)をパソコンに挿入し起動（下記参照）
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

1. バージョンの確認  
    ```
    # cat /etc/system-release
    CentOS Stream release 8
    ```

実行環境：CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月10日  
更新日：2023年3月01日 バージョンの確認  
[[TOP]](#TOP)  


<a id="202302121019"></a>
# <b>Linuxコマンド</b>

* 入力の終了
    ```
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

* ファイルのバックアップ  
    ```
    # cp 元ファイル 名前を変更したバックファイル名
    ```

* コマンドの絶対パス（例）
    ```
    # which ssh
    /usr/bin/ssh
    ```

* シャットダウン
    ```
    # shutdown now
    ```
* 再起動（UEFI/BIOSの起動順位に注意）
    ```
    # reboot
    ```
##

* 接続状態を確認  
    ```
    # ping 〇〇 ←〇〇.com や IPアドレス
    ```

作成者：夢寐郎  
作成日：2023年2月12日  
更新日：2023年2月28日 which を追加  
[[TOP]](#TOP)  


<a id="202302102308"></a>
# <b>ルーター（参考）</b>

* NTT GV-ONU（PR-S300SE）
    * 光回線終端装置（ひかり電話ルータ）
    * 設定画面：http://ntt.setup/（192.168.1.1） 
        * ユーザー名：user
        * パスワード：****

* SoftBank E-WMTA2.3（光BBユニット）
    * 2020年から"2.4"を提供中（☎0800-111-2009）
    * 無線LAN規格：IEEE 802.11a/b/g/n/ac
    * 周波数帯：5GHz/2.4GHz帯
    * 設定画面：172.16.255.254（192.168.3.1）
        * ユーザー名：user
        * パスワード：****

* ASUS RT-AC68U
    * Wi-Fiルーター
    * 2013年11月発売
    * 無線LAN規格：IEEE802.11a/b/g/n/ac
    * 周波数帯：5GHz/2.4GHz帯
    * 設定画面：192.168.1.1（192.168.2.1）
        * ユーザー名：admin
        * パスワード：*****
    * 取扱説明書：http://dlcdnet.asus.com/pub/ASUS/wireless/RT-AC68U/APAC9455_RT_AC68U_QSG.pdf  
    （日本語 49～63頁）  

* IODATA WN-AC583TR（ポケットルーター）
    * 2014年7月発売（2018年6月生産終了）
    * 無線LAN規格：IEEE802.11a/b/g/ac
    * 周波数帯：5GHz/2.4GHz帯（同時接続可）
    * ファームウェア：v1.07（2019年2月公開）
    * 無線接続4台まで推奨
    * [マニュアル](https://www.iodata.jp/lib/manual/pdf2/wn-ac583tr.pdf)
    * 192.168.99.1  
        * ユーザー名：なし
        * パスワード：****  
 
作成者：夢寐郎  
作成日：2023年2月11日  
更新日：2023年3月15日  
[[TOP]](#TOP)  


<a id="202302111947"></a>
# <b>SSH</b>

1. [SSH](https://www.kagoya.jp/howto/it-glossary/server/ssh/)（Secure Shell）のバージョンを調べる
    ```
    # dnf list installed | grep openssh
    openssh.x86_64          8.0p1-17.e18  @anaconda
    openssh-clients.x86_64  8.0p1-17.e18  @anaconda
    openssh-server.x86_64   8.0p1-17.e18  @anaconda
    ```
1. [root](https://eng-entrance.com/linux-root) ユーザーのログイン禁止
    1. 設定ファイルの変更
        ```
        # vi -R /etc/ssh/sshd_config
        ……
        43行目 PermitRootLogin no ←「yes」から変更
        ```
    1. SSH の再起動
        ```
        # systemctl restart sshd
        ```
1. 任意のユーザーに全てのコマンドの実行権限を与える  
    （$ sudo shutdown now 等のコマンドが可能になる）
    ```
    # visudo /etc/sudoers ←sudoの設定ファイル
    ……
    最終行 mubirou ALL=(ALL) ALL ←追加
    ```

1. [新規ユーザーの登録](#202302130631)

1. Windows からの操作
    1. https://www.putty.org/ の [Download PuTTy] を選択
    1. [putty-64bit-0.78-installer.msi] をダウンロード＆インストール
    1. PuTTy（パティ）を起動し各種設定＆接続  
        1. フォントの変更  
            [Window]-[Appearance]-[Font settigs]-[Change]：[Ricty Diminished Discord](https://github.com/edihbrandon/RictyDiminished)（サイズ: 18）
        1. 接続先の指定  
            [Session]-[Host Name(or IP address)]：192.168.X.XX（LinuxパソコンのIPアドレス）
        1. 上記の設定の保存  
            [Session]-[Saved Sessions]：CentOS@192.168.X.XX 等に命名して [Save]
    1. Linux に接続  
        1. [Session] で上記の [CentOS@192.168.X.XX] を選択し [Load]-[Open]  
            * [PuTTy Security Alert] が表示されたら [Accept] し再度接続
        1. [login as:] と表示されたらログイン  
            ```
            login as: mubirou ←任意
            mubirou@192.168.X.XX's password:
            ……
            [mubirou@centos ~]$ su ←root権限にする
            パスワード:
            [root@centos ~]#
            ```

参考：『INTRODUCTION NOTES』110頁（SSH）  
実行環境：CentOS Stream 8、OpenSSH 8.0、PuTTy 0.78  
作成者：夢寐郎  
作成日：2023年2月11日  
更新日：2023年3月02日 root ユーザーのログイン禁止等の追加  
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
    1. http と https を追加
        ```
        # firewall-cmd --add-service=http --zone=public --permanent
        # firewall-cmd --add-service=https --zone=public --permanent
        ```
        * 削除方法  
            ```
            # firewall-cmd --remove-service=〇〇 ...
            # firewall-cmd --reload
            ```
    1. firewalld の再起動
        ```
        # systemctl restart firewalld
        ```
    1. 再度、稼働確認
        ```
        # firewall-cmd --list-all
        ……
        services: cockpit dhcpv6-client http https ssh ←http,httpsが追加
        ……
        ```

1. [SELinux](https://ja.wikipedia.org/wiki/Security-Enhanced_Linux)の設定  
    1. SELinux の確認  
        ```
        # getsebool -a | grep httpd
        httpd_anon_write --> off
        httpd_builtin_scripting --> on
        httpd_can_check_spam --> off
        httpd_can_connect_ftp --> off
        httpd_can_connect_ldap --> off
        httpd_can_connect_mythtv --> off
        httpd_can_connect_zabbix --> off
        httpd_can_network_connect --> off ←onにする必要ある
        httpd_can_network_connect_cobbler --> off
        httpd_can_network_connect_db --> off
        httpd_can_network_memcache --> off
        httpd_can_network_relay --> off
        httpd_can_sendmail --> off
        httpd_dbus_avahi --> off
        httpd_dbus_sssd --> off
        httpd_dontaudit_search_dirs --> off
        httpd_enable_cgi --> on
        httpd_enable_ftp_server --> off
        httpd_enable_homedirs --> off ←onにする必要ある
        httpd_execmem --> off
        httpd_graceful_shutdown --> off  ←onにする必要ある
        httpd_manage_ipa --> off
        httpd_mod_auth_ntlm_winbind --> off
        httpd_mod_auth_pam --> off
        httpd_read_user_content --> off
        httpd_run_ipa --> off
        httpd_run_preupgrade --> off
        httpd_run_stickshift --> off
        httpd_serve_cobbler_files --> off
        httpd_setrlimit --> off
        httpd_ssi_exec --> off
        httpd_sys_script_anon_write --> off
        httpd_tmp_exec --> off
        httpd_tty_comm --> off ←onにする必要ある
        httpd_unified --> off
        httpd_use_cifs --> off
        httpd_use_fusefs --> off
        httpd_use_gpg --> off
        httpd_use_nfs --> off
        httpd_use_opencryptoki --> off
        httpd_use_openstack --> off
        httpd_use_sasl --> off
        httpd_verify_dns --> off
        ```
    1. SELinux は有効のまま apache 関連のアクセスを可能にする  
        ```
        # setsebool -P httpd_can_network_connect on
        # setsebool -P httpd_tty_comm on
        # setsebool -P httpd_graceful_shutdown on
        # setsebool -P httpd_enable_homedirs on
        ```
    1. 再度 SELinux の確認  
        ```
        # getsebool -a | grep httpd | grep 'on$'
        httpd_builtin_scripting --> on
        httpd_can_network_connect --> on
        httpd_enable_cgi --> on
        httpd_enable_homedirs --> on
        httpd_graceful_shutdown --> on
        httpd_tty_comm --> on
        ```

1. 動作確認（WebブラウザでApacheが起動しているLinuxのIPアドレスにアクセス）  

1. 「HTTP SERVER TEST PAGE」と表示されたら成功！  
    （/var/www/html/index.html がない場合）  

[**⚠ディレクトリ一覧表示対策**](#202302121037-htaccess)が必要  

（参考）エラーログの表示  
```
# cat /var/log/httpd/error_log
```

実行環境：CentOS Stream 8、Apache 2.4.37  
作成者：夢寐郎  
作成日：2023年2月12日  
更新日：2023年3月26日 ディレクトリ一覧表示対策の追加  
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

1. [Vim](#202302130554) を使いWebページ管理者グループを作成  
    1. [ユーザー管理](#202302130631)を参考に登録ユーザーを確認
        ```
        # cat /etc/group
        ……
        mubirou:x:1000:
        ```
    1. Web ページ管理者グループを作成
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
    1. ftp を追加
        ```
        # firewall-cmd --add-service=ftp --permanent
        ```
    1. firewalld の再起動
        ```
        # firewall-cmd --reload
        ```
    1. 再度、稼働確認
        ```
        # firewall-cmd --list-services --permanent
        cockpit dhcpv6-client ftp http ssh ←ftpが追加
        ```

1. [SELinux](https://ja.wikipedia.org/wiki/Security-Enhanced_Linux) の設定  
    1. SELinux の確認  
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
    1. SELinux は有効のまま vsftpd のデータ転送を可能にする  
        ```
        # setsebool -P ftpd_full_access on
        ```
    1. 再度 SELinux の確認  
        ```
        # getsebool -a | grep ftpd_full_access
        ftpd_full_access --> on
        ```

<a id="202302121037-FileZilla"></a>

1. [FileZilla](https://ja.wikipedia.org/wiki/FileZilla) によるファイル転送
    1. https://filezilla-project.org/download.php?type=client にアクセス
    1. [Download FileZilla Client] を選択しダウンロード＆インストール
    1. FileZilla を起動
    1. [ファイル]-[サイトマネージャー]-[新しいサイト] で各種設定  
        * [一般] タブ
            * [新規サイト] → XXX@192.168.X.XX（任意）
            * ホスト：192.168.X.XX（Apacheが起動しているIPアドレス）
            * ポート：（とりあず空白）
            * ログオンタイプ：パスワードを尋ねる
            * ユーザー：mubirou
        * [詳細] タブ
            * デフォルトのローカルディレクトリ：（Windows 上の任意のフォルダ）
            * デフォルトのリモートディレクトリ：/var/www/html（Webサーバ上）
    1．[接続] を選択

    1. [新規ホスト] を選択し各種設定  
        * ホストの設定名：XXX@192.168.X.XX（任意）
        * ホスト名（アドレス）：192.168.X.XX（Apacheが起動しているIPアドレス）
        * ユーザー名：mubirou
        * パスワード：XXXX
        * ポート：（空白）
    1. "ログインしました"と表示されたら接続成功！
    1. [デフォルトのローカルディレクトリ] に以下の index.html ファイルを作成  
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
    1. [ローカルサイト] の上記の [index.html] を [リモートサイト] にドラッグ＆ドロップできたら成功！（アップロード方法は他にもあり）
    1. Web ブラウザ上で 192.168.X.XX にアクセスして Hello,world! と表示されたら大成功！

<a id="202302121037-htaccess"></a>

**⚠ ディレクトリ一覧表示対策**  

1. [httpd.conf](https://e-words.jp/w/httpd.conf.html) の編集  
    ```
    # vi /etc/httpd/conf/httpd.conf
    136行目 <Directory "/var/www/html">
    ……
    156行目 AllowOverride All ←「None」から変更
    ……
    162行目 </Directory>
    ```
1. httpd の再起動  
    ```
    # systemctl restart httpd ←再起動
    ```
1. 以下の内容の [.htaccess](https://ja.wikipedia.org/wiki/.htaccess) ファイルを作成
    ```
    Options -Indexes
    ```
1. /var/www/htmlに上記の [.htaccess](https://ja.wikipedia.org/wiki/.htaccess) ファイルを配置（配下のディレクトリにも適応される）  

1. index.html が配置されていないディレクトリにアクセスし以下の表示がされたら成功！  
    ```
    Forbidden
    You don't have permission to access this resource.
    
    禁じ手
    このリソースにアクセスする権限がありません。
    ```

参考：『INTRODUCTION NOTES』120頁（FTPサーバ）  
参考：『INTRODUCTION NOTES』176頁（パーミッション等）  
実行環境：CentOS Stream 8、vsftpd 3.0.3、FileZilla 3.63.2  
作成者：夢寐郎  
作成日：2023年2月14日  
更新日：2023年3月26日 .htaccess を追加  
[[TOP]](#TOP)  


<a id="202302130554"></a>
# <b>Vim</b>

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
実行環境：CentOS Stream 8、vim 8.0  
作成者：夢寐郎  
作成日：2023年2月13日  
更新日：2023年2月23日 vi → [Vim](https://ja.wikipedia.org/wiki/Vim)（ヴィム）に変更  
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

1. PHP のインストール  
    1. インストール可能な PHP の確認  
        ```
        # dnf module list php
        ……
        Name  Stream   ...
        php   7.2 [d]  ... ←初期値
        php   7.3      ...
        php   7.4      ...
        php   8.0      ...
        ```
    1. PHP パッケージ情報の確認（概要）  
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
    1. PHP のインストール  
        ```
        # dnf -y update ←インストール済パッケージをアップデート
        # dnf install -y php php-mysqlnd php-json php-mbstring php-gd php-pdo php-xml php-xmlrpc
        ```
    1. インストール済の PHP パッケージの確認  
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
    1. [Apache](#202302120812) の再起動（Apache モジュールとして動作しているため）  
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

##

* PHP のエラー表示（参考）  
    ```
    <?php
        ini_set('display_errors', 1 );
        error_reporting(E_ALL);

        echo $a;
    ?>
    ```
    ```
    Notice: Undefined variable: a in /var/www/html/test.php on line 4
    ```
## 

* 7.2 → 7.4 へアップグレード  
    1. インストール済の PHP のバージョン確認  
        ```
        # php -v
        PHP 7.2.24 (cli) ...
        ```
    1. インストール可能な PHP の確認  
        ```
        # dnf module list php
        ……
        Name  Stream      ...
        php   7.2 [d][e]  ...
        php   7.3         ...
        php   7.4         ...
        php   8.0         ...
        ```
    1. インストール  
        ```
        # dnf -y update ←インストール済パッケージをアップデート
        # dnf module -y reset php  ←既存のPHPのモジュールをリセット
        # dnf module -y install php:7.4 ←インストール
        # systemctl restart httpd ←Apacheの再起動
        ```
    1. インストール済の PHP パッケージの確認  
        ```
        # dnf list installed | grep php
        php.x86_64           7.4.30-1. ...
        php-cli.x86_64       7.4.30-1. ...
        php-common.x86_64    7.4.30-1. ...
        php-fpm.x86_64       7.4.30-1. ...
        php-gd.x86_64        7.4.30-1. ...
        php-json.x86_64      7.4.30-1. ...
        php-mbstring.x86_64  7.4.30-1. ...
        php-mysqlnd.x86_64   7.4.30-1. ...
        php-opcache.x86_64   7.4.30-1. ... ←新たに追加
        php-pdo.x86_64       7.4.30-1. ...
        php-xml.x86_64       7.4.30-1. ...
        php-xmlrpc.x86_64    7.4.30-1. ...
        ```

参考：『INTRODUCTION NOTES』121頁（PHP）  
実行環境：CentOS Stream 8、PHP 7.2.24  
作成者：夢寐郎  
作成日：2023年2月15日  
更新日：2023年3月04日 エラー表示を追加  
[[TOP]](#TOP)  


<a id="202302162306"></a>
# <b>MariaDB</b>

1. [MariaDB](https://ja.wikipedia.org/wiki/MariaDB) のインストール  
    1. インストール可能な PHP の確認  
        ```
        # dnf list maria*
        mariadb.x86_64         3:10.3.28...
        ……
        mariadb-server.x86_64  3:10.3.28...
        ……
        ```
    1. MariaDB のインストール  
        ```
        # dnf -y update ←インストール済パッケージをアップデート
        # dnf install -y mariadb-server
        ```
    1. インストール済の MariaDB パッケージの確認  
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
    1. mysql を追加
        ```
        # firewall-cmd --add-service=mysql --permanent
        ```
    1. firewalld の再起動
        ```
        # systemctl restart firewalld
        ```
    1. 再度、稼働確認
        ```
        # firewall-cmd --list-services --permanent
        cockpit dhcpv6-client ftp http mysql ssh ←mysqlが追加
        ```

1. [SELinux](https://ja.wikipedia.org/wiki/Security-Enhanced_Linux) の設定  
    1. SELinux の確認  
        ```
        # getsebool -a | grep mysql_connect
        mysql_connect_any --> off ←これをonにする
        mysql_connect_http --> off
        selinuxuser_mysql_connect_enabled --> off ←これをonにする
        ```
    1. SELinux は有効のまま mysql のデータ転送を可能にする
        ```
        # setsebool -P mysql_connect_any on
        # setsebool -P selinuxuser_mysql_connect_enabled on
        ```
    1. 再度 SELinux の確認  
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
参考：[MySQL / MariaDB 基礎文法](https://github.com/mubirou/HelloWorld/blob/master/languages/MySQL/MySQL_reference.md)  
参考：『INTRODUCTION NOTES』122頁（MariaDB）  
実行環境：CentOS Stream 8、MariaDB 10.3.28  
作成者：夢寐郎  
作成日：2023年2月17日  
更新日：2023年2月19日  
[[TOP]](#TOP)  


<a id="202302170022"></a>
# <b>PHP+MariaDB</b>

1. [PHP をインストール](#202302142236)

1. 動作確認  
    1. mysqltest.php を作成
        ```
        <?php
            $con = new PDO('mysql::memory:', 'root', '');
            $statement = $con->prepare("SELECT VERSION()");
            $statement->execute();
            echo $statement->fetchColumn(); //-> 10.3.28-MariaDB
        ?>
        ```
    1. FTP クライアントソフトを使って /var/www/html/ にアップロード
    1. Web ブラウザで http://192.168.X.XX/mysqltest.php にアクセス
    1. "10.3.28-MariaDB" と表示されれば成功！

参考：[Godot+PHP+MySQL](https://github.com/mubirou/Godot-Study-Notes#phpmysql)  
参考：[MySQL / MariaDB 基礎文法](https://github.com/mubirou/HelloWorld/blob/master/languages/MySQL/MySQL_reference.md)  
実行環境：CentOS Stream 8、PHP 7.2.24、MariaDB 10.3.28  
作成者：夢寐郎  
作成日：2023年2月17日  
[[TOP]](#TOP)  


<a id="202302170208"></a>
# <b>WordPress</b>

1. [WordPress](https://ja.wikipedia.org/wiki/WordPress) のダウンロード＆設定  
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
    1. http://192.168.X.XX/wordpress/wp-admin/install.php にアクセス
    1. [ようこそ] 画面で各種設定  
        * サイトのタイトル：TODAY'S QUESTION（例）
        * ユーザー名：mubirou（任意）
        * パスワード：********（初期値を保管）
        * メールアドレス：mubirou.info@gmail.com（任意）
    1. [WordPress をインストール] を選択
    1. "成功しました！"と表示されたら [ログイン]
    1. パスワードの変更  
        1. [ユーザー]-[mubirou]-[編集]-[新しいパスワード]
        1. [プロフィールを更新]
        1. [ログアウト] 後に http://192.168.X.XX/wordpress/wp-admin/ にアクセスして接続確認

1. [SELinux](https://ja.wikipedia.org/wiki/Security-Enhanced_Linux) 関連の設定  
    （WordPress 上でファイルをアップロード可能にする）  
    1. パーミッションの変更  
        ```
        # chmod -R 777 /var/www/html/wordpress
        ```
    1. Apache による書き込み権限を与える  
        ```
        # chcon -R -t httpd_sys_rw_content_t /var/www/html/wordpress
        ```

参考：『INTRODUCTION NOTES』215頁（WordPress）  
参考：[WordPress の要件](https://ja.wordpress.org/about/requirements/)  
実行環境：CentOS Stream 8、[MariaDB](#202302162306) 10.3.28、[PHP](#202302142236) 7.4.30、WordPress 6.1.1  
作成者：夢寐郎  
作成日：2023年2月19日  
更新日：2023年3月02日 Apache による書き込み権限を追加  
[[TOP]](#TOP)  


<a id="202302191517"></a>
# <b>Samba</b>

1. [Samba](http://www.samba.gr.jp/doc/samba2.0_and_linux.html) パッケージ情報の確認（概要）  
    ```
    # dnf info samba samba-client samba-winbind samba-winbind-clients cifs-utils
    ……
    名前         : cifs-utils
    ……
    名前         : samba
    ……
    名前         : samba-client
    ……
    名前         : samba-winbind
    ………
    名前         : samba-winbind-clients
    ```

1. インストール  
    ```
    # dnf -y update ←インストール済パッケージをアップデート
    # dnf -y install samba samba-client samba-winbind samba-winbind-clients cifs-utils
    ```
    ```
    # dnf list installed | grep samba
    python3-samba.x86_64           4.17.5...
    samba.x86_64                   4.17.5... ←Samba
    samba-client.x86_64            4.17.5... ←samba-client
    samba-client-libs.x86_64       4.17.5...
    samba-common.noarch            4.17.5...
    samba-common-libs.x86_64       4.17.5...
    samba-common-tools.x86_64      4.17.5...
    samba-dc-libs.x86_64           4.17.5...
    samba-dcerpc.x86_64            4.17.5...
    samba-ldb-ldap-modules.x86_64  4.17.5...
    samba-libs.x86_64              4.17.5...
    samba-winbind.x86_64           4.17.5... ←samba-winbind
    samba-winbind-clients.x86_64   4.17.5... ←samba-winbind-clients
    samba-winbind-modules.x86_64   4.17.5...
    ```

1. [ファイアウォール](https://ja.wikipedia.org/wiki/%E3%83%95%E3%82%A1%E3%82%A4%E3%82%A2%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB)の設定
    1. 稼働状況  
        ```
        # firewall-cmd --list-all
        ……
        services: cockpit dhcpv6-client ftp http mysql ssh ←samba通信がない
        ……
    1. samba を追加
        ```
        # firewall-cmd --add-service=samba --permanent
        ```
    1. firewalld の再起動
        ```
        # systemctl restart firewalld
        ```
    1. 再度、稼働確認
        ```
        # firewall-cmd --list-all
        ……
        services: cockpit dhcpv6-client ftp http mysql samba ssh ←samba!
        ……
        ```

1. [SELinux](https://ja.wikipedia.org/wiki/Security-Enhanced_Linux) の設定  
    1. SELinux の確認  
        ```
        # getsebool -a | grep samba
        samba_create_home_dirs --> off  ←これをonにする
        samba_domain_controller --> off
        samba_enable_home_dirs --> off  ←これをonにする
        samba_export_all_ro --> off
        samba_export_all_rw --> off  ←これをonにする
        samba_load_libgfapi --> off
        samba_portmapper --> off
        samba_run_unconfined --> off
        samba_share_fusefs --> off
        samba_share_nfs --> off
        sanlock_use_samba --> off
        tmpreaper_use_samba --> off
        use_samba_home_dirs --> off
        virt_use_samba --> off
        ```
    1. SELinux は有効のままの Samba データ転送を可能にする
        ```
        # setsebool -P samba_create_home_dirs on
        # setsebool -P samba_enable_home_dirs on
        # setsebool -P samba_export_all_rw on
        ```
    1. 再度 SELinux の確認（概要）  
        ```
        # getsebool -a | grep samba
        samba_create_home_dirs --> on ←onに変更された
        samba_domain_controller --> off
        samba_enable_home_dirs --> on ←onに変更された
        samba_export_all_ro --> off
        samba_export_all_rw --> on ←onに変更された
        samba_load_libgfapi --> off
        samba_portmapper --> off
        samba_run_unconfined --> off
        samba_share_fusefs --> off
        samba_share_nfs --> off
        sanlock_use_samba --> off
        tmpreaper_use_samba --> off
        use_samba_home_dirs --> off
        virt_use_samba --> off
        ```

1. Samba ユーザーの追加  
    ⚠ 既存の [CentOS ユーザー](#202302130631)である必要がある  
    ```
    # pdbedit -a mubirou
    new password:
    ```
    * その他の操作
        ```
        # pdbedit -x 〇〇 ←任意の Samba ユーザーの削除
        # pdbedit -L ←Sambaユーザーの一覧表示
        # pdbedit -v 〇〇 ←任意の Samba ユーザーの詳細表示
        ```

1. [smb.conf](http://www.samba.gr.jp/project/translation/current/htmldocs/manpages/smb.conf.5.html) ファイルの編集  
    1. 共有ディレクトリの作成
        ```
        # mkdir /home/samba
        # chmod -R 777 /home/samba
        ```
    1. smb.conf のバックアップ  
        ```
        # cp /etc/samba/smb.conf /etc/samba/smb.conf.org
        ```
    1. smb.conf を以下の通りに編集
        ```
        # vi /etc/samba/smb.conf
        [global]
            workgroup = WORKGROUP
            security = user
            netbios name = centos
            server string = SAMBA SERVER Version %v
            passdb backend = tdbsam

        [share]
            path = /home/samba
            comment = Share Directory
            writable = yes
            guest ok = yes
        ```
    1. 文法チェック
        ```
        # testparm
        ……
        Loaded services file OK. →問題なし
        ……
        ```

1. [Samba](http://www.samba.gr.jp/doc/samba2.0_and_linux.html) の起動＆自動起動  
    ```
    # systemctl enable smb.service
    # systemctl enable nmb.service
    # systemctl start smb.service
    # systemctl start nmb.service
    ```
    * その他の操作  
        ```
        # systemctl restart smb.service
        # systemctl restart nmb.service
        ```

1. 動作確認  
    1. SMB 1.0 の有効化  
        1. Windows で以下を検索して開く  
            🔎**windows の機能の有効化または無効化**
        1. 以下の項目に✓を入れ Windows を再起動  
            * **SMB 1.0/CIFS ファイル共有のサポート**
                * SMB 1.0/CIFS クライアント
                * SMB 1.0/CIFS サーバー
                * SMB 1.0/CIFS 自動削除
            * SMB ダイレクト
    1. 共有ディレクトリに接続  
        1. Windows 上で [エクスプローラー]-[**ネットワーク**] を選択
        1. アドレスに **&yen;&yen;192.168.X.XX** と入力  
            （Samba が起動している CentOS の IP アドレス）
        1. [**ネットワーク資格情報の入力**] を入力し [OK]  
            * ユーザー名：mubirou ← Samba ユーザー
            * パスワード：*******
        1. 上記で設定した "share" ディレクトリが表示されるのを確認（[右クリック]→[クイックアクセスにピン留めする] 等すると良い）
        1. 任意のファイルをドラッグ＆ドロップできれば成功！  

参考：『INTRODUCTION NOTES』177頁（Samba）  
実行環境：CentOS Stream 8、Samba 4.17.5、Windows 11（22H2）  
作成者：夢寐郎  
作成日：2023年2月23日  
[[TOP]](#TOP)  


<a id="202302232039"></a>
# <b>SQLite</b>

1. [SQLite](https://ja.wikipedia.org/wiki/SQLite) のインストール  
    ```
    # dnf -y update ←インストール済パッケージをアップデート
    # dnf -y install epel-release ←EPEL リポジトリの有効化
    # dnf -y install sqlite
    ```

1. SQLite のバージョンを調べる
    ```
    # sqlite3 -version
    3.26.0 2018-12-01 12:34:55 ...
    ```
実行環境：CentOS Stream 8、SQLite 3.26  
作成者：夢寐郎  
作成日：2023年2月23日  
[[TOP]](#TOP)  


<a id="202302232127"></a>
# <b>PHP+SQLite</b>

1. [PHP のインストール](#202302142236)

1. [SQLite のインストール](#202302232039)

1. 動作確認  
    1. sqlitetest.php を作成
        ```
        <?php
            $con = new PDO('sqlite::memory:', null, null);
            $statement = $con->prepare('SELECT sqlite_version()');
            $statement->execute();
            echo $statement->fetchColumn(); //-> 3.26.0
        ?>
        ```
    1. FTP クライアントソフトを使って /var/www/html/ にアップロード
    1. Web ブラウザで http://192.168.X.XX/sqlitetest.php にアクセス
    1. "3.26.0" と表示されれば成功！

参考：[Godot+PHP+SQLite](https://github.com/mubirou/Godot-Study-Notes#phpsqlite)  
参考：[SQLite 基礎文法](https://bit.ly/41kaJsS)  
参考：『INTRODUCTION NOTES』111頁（PHP→SQLite→PHP）  
実行環境：CentOS Stream 8、[PHP](#202302142236) 7.4.30、SQLite 3.26  
作成者：夢寐郎  
作成日：2023年2月23日  
[[TOP]](#TOP)  


<a id="202302232147"></a>
# <b>Python</b>

1. [Python](https://www.python.jp/) のインストール  
    ```
    # dnf -y update ←インストール済パッケージをアップデート
    # dnf module -y install python39 ←Ver.3.9の場合
    ```
1. バージョンを調べる  
    ```
    # python3 --version
    Python 3.9.16
    ```
1. 動作確認  
    1. test.py を新規作成  
        ```
        # vi test.py
        ```
    1. [Vim](#202302130554) で編集＆保存
        ```
        print("Hello,world!")
        ```
    1. 実行  
        ```
        # python3 test.py
        Hello,world! ←と表示されたら成功！
        ```
        ※確認後[ファイルを削除](#202302121019)しておく  
        ```
        # rm test.py
        ```

実行環境：CentOS Stream 8、Python 3.9.16  
作成者：夢寐郎  
作成日：2023年2月25日  
[[TOP]](#TOP)  


<a id="202302251334"></a>
# <b>Python on Apache</b>

[Python](#202302232147) を [Apache](#202302120812) 上で [CGI](https://ja.wikipedia.org/wiki/Common_Gateway_Interface) として動作させる方法  

1. [Apache の準備](#202302120812)

1. [Python の準備](#202302232147)

1. Python スクリプトの用意
    1. test.py を作成（[参考](https://github.com/mubirou/Godot-Study-Notes#pythonsqlite)）  
        ```
        #!/usr/bin/python3
        # -*- coding: utf-8 -*-
        print("Content-Type: text/html\n")

        import platform
        print(platform.python_version()) #-> 3.9.16
        ```
        > ⚠ [VSCode](https://code.visualstudio.com/) で編集時に [**改行コード**](https://atmarkit.itmedia.co.jp/ait/articles/1809/14/news025.html) を CRLF → **LF** に変更しないと **Internal Server Error** が発生する  
    1. [FileZilla](#202302121037-FileZilla) を使って以下の通りになるようにアップロード  
        **/var/www/cgi-bin/test.py**
    1. パーミッション変更  
        （[FileZilla](https://ja.wikipedia.org/wiki/FileZilla) 上でも [[パーミッションの変更](https://bit.ly/3EJeZss)] で可能）  
        * 確認  
        ```
        # ls -l /var/www/cgi-bin/test.py
        -rw-r--r--. 1 mubirou mubirou ... ←644
        ```
        * 変更
        ```
        # chmod 755 /var/www/cgi-bin/test.py
        ```
        * 再度確認  
        ```
        # ls -l /var/www/cgi-bin/test.py
        -rwxr-xr-x. 1 mubirou mubirou ... ←755
        ```

1. [httpd.conf](https://e-words.jp/w/httpd.conf.html) の変更  
    1. [Vim](#202302130554) で **/etc/httpd/conf/httpd.conf** を開く  
        ```
        # vi /etc/httpd/conf/httpd.conf
        ```
        105行目以降  
        ```
        <Directory "/var/www/cgi-bin"> ←「/>」から変更
            AllowOverride none
            Require all granted ←「all denied」から変更
            Options ExecCGI ←追加
            AddHandler cgi-script .py ←追加
        </Directory>
        ```
        250行目以降  
        ```
        ScriptAlias /cgi-bin/ "/var/www/cgi-bin/" ←確認
        ```
        297行目以降  
        ```
        #AddHandler cgi-script .cgi ←確認
        ```
    1. [Apache](#202302120812) の再起動  
        ```
        # systemctl restart httpd
        ```

1. 動作確認  
    1. Web ブラウザで以下にアクセス  
        http://192.168.X.XX/cgi-bin/test.py
    1. Python のバージョンが表示されたら成功！

実行環境：CentOS Stream 8、Python 3.9.16、Apache 2.4.37、FileZilla 3.63.2  
作成者：夢寐郎  
作成日：2023年2月28日  
[[TOP]](#TOP)  


<a id="202302282229"></a>
# <b>Python+SQLite</b>

1. [Python の準備](#202302251334)

1. [SQLite のインストール](#202302232039)

1. 動作確認  
    1. sqlitetest.py を作成
        ```
        #!/usr/bin/python3
        # -*- coding: utf-8 -*-
        print("Content-Type: text/html\n")

        import sqlite3

        _con = sqlite3.connect(':memory:')
        _cur = _con.cursor()
        _cur.execute('SELECT sqlite_version()')
        _result = _cur.fetchall()
        print(_result[0][0]) #-> 3.26.0
        ```
        > ⚠ [VSCode](https://code.visualstudio.com/) で編集時に [**改行コード**](https://atmarkit.itmedia.co.jp/ait/articles/1809/14/news025.html) を CRLF → **LF** に変更しないと **Internal Server Error** が発生する  
    1. [FileZilla](#202302121037-FileZilla) を使って以下の通りになるようにアップロード  
        **/var/www/html/cgi-bin/sqlitetest.py**
    1. [[パーミッションの変更](https://bit.ly/3EJeZss)] で **755** にする  
    1. Web ブラウザで以下にアクセス  
        http://192.168.X.XX/cgi-bin/sqlitetest.py
    1. SQLite のバージョンが表示されれば成功！

参考：[Godot+Python+SQLite](https://github.com/mubirou/Godot-Study-Notes#220624)  
参考：[SQLite 基礎文法](https://bit.ly/41kaJsS)  
実行環境：CentOS Stream 8、Python 3.9.16、Apache 2.4.37、SQLite 3.26、FileZilla 3.63.2  
作成者：夢寐郎  
作成日：2023年2月28日  
[[TOP]](#TOP)  


<a id="202302282308"></a>
# <b>Python+MariaDB</b>

1. [Python の準備](#202302251334)

1. [MariaDB のインストール](#202302162306)

1. [MySQL Connector/Python](https://dev.mysql.com/doc/connector-python/en/) のインストール
    ```
    # dnf -y update ←インストール済パッケージをアップデート
    # # pip3 install mysql-connector-python
    ```

1. データベース（test_db）を作成（[参考](https://bit.ly/3m62yAi)）  
    ```
    # mysql -u root
    MariaDB [(xxx)]> CREATE DATABASE test_db; ←作成
    MariaDB [(xxx)]> show databases; ←確認
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | test_db            | ←新しく作成
    | wordpress_db       |
    +--------------------+
    ```

1. 動作確認  
    1. mysqltest.py を作成
        ```
        #!/usr/bin/python3
        # -*- coding: utf-8 -*-
        print("Content-Type: text/html\n")

        import mysql.connector

        _con = mysql.connector.connect(user='root', passwd='', host='localhost', db='test_db')
        _cur = _con.cursor()
        _cur.execute('SELECT VERSION()')
        _result = _cur.fetchall()
        print(_result[0][0]) #-> 10.3.28-MariaDB
        ```
        > ⚠ [VSCode](https://code.visualstudio.com/) で編集時に [**改行コード**](https://atmarkit.itmedia.co.jp/ait/articles/1809/14/news025.html) を CRLF → **LF** に変更しないと **Internal Server Error** が発生する  
    1. [FileZilla](#202302121037-FileZilla) を使って以下の通りになるようにアップロード  
        **/var/www/html/cgi-bin/mysqltest.py**
    1. [[パーミッションの変更](https://bit.ly/3EJeZss)] で **755** にする  
    1. Web ブラウザで以下にアクセス  
        http://192.168.X.XX/cgi-bin/mysqltest.py
    1. MariaDB のバージョンが表示されれば成功！

参考：[Godot+Python+MySQL](https://github.com/mubirou/Godot-Study-Notes#pythonmysql)  
参考：[MySQL / MariaDB 基礎文法](https://github.com/mubirou/HelloWorld/blob/master/languages/MySQL/MySQL_reference.md)  
実行環境：CentOS Stream 8、Python 3.9.16、Apache 2.4.37、MariaDB 10.3.28、FileZilla 3.63.2  
作成者：夢寐郎  
作成日：2023年3月1日  
[[TOP]](#TOP)  


<a id="202303022135"></a>
# <b>サンプル</b>

* サンプルのコンセプト  
    * 4択問題を2ページ作成
    * ラジオボタンによる選択と [送信] ボタンで解答
    * [送信] ボタンはクリック後は押せないようにする
    * データベースに保存されている正答と比較
    * 正答か誤答かをページ遷移することなく表示する
    * [HTML](#202303022135_html) + [JavaScript](#202303022135_js) + [PHP](#202303022135_php) + [MariaDB](#202303022135_db) を利用する

<a id="202303022135_html"></a>

### 👉 HTML の記述  
1. 問題 001 のページ  
![image](https://github.com/mubirou/LAMP/blob/master/jpg/202303091627.jpg)  
    ```html
    <!DOCTYPE html>
    <html lang="ja">
    <head>
        <meta charset="UTF-8">
        <title>xxx</title>
        <script src="question.js"></script>
    </head>
    <body>
        <form id="form1">
            (001)<br>
            virtualの意味は？<br>
            <input type="radio" name="radio1" value="1"><label id="label1">仮想</label><br>
            <input type="radio" name="radio1" value="2"><label id="label2">名目</label><br>
            <input type="radio" name="radio1" value="3"><label id="label3">実質</label><br>
            <input type="radio" name="radio1" value="4"><label id="label4">虚</label><br>
            <input type="button" id="btn1" value="送信" onclick="onclick_btn1('001')"/>
        </form>
    </body>
    </html>
    ```

1. 問題 002 のページ  
![image](https://github.com/mubirou/LAMP/blob/master/jpg/202303091636.jpg)  
    ```html
    <!DOCTYPE html>
    <html lang="ja">
    <head>
        <meta charset="UTF-8">
        <title>xxx</title>
        <script src="question.js"></script>
    </head>
    <body>
        <form id="form1">
            (002)<br>
            VRの適切な日本語訳は？<br>
            <input type="radio" name="radio1" value="1"><label id="label1">複合現実感</label><br>
            <input type="radio" name="radio1" value="2"><label id="label2">拡張現実感</label><br>
            <input type="radio" name="radio1" value="3"><label id="label3">仮想現実感</label><br>
            <input type="radio" name="radio1" value="4"><label id="label4">人工現実感</label><br>
            <input type="button" id="btn1" value="送信" onclick="onclick_btn1('002')"/>
        </form>
    </body>
    </html>
    ```

<a id="202303022135_db"></a>

### 👉 MariaDB の処理  
1. [MariaDB の準備](#202302162306)
1. [データベースの作成](https://bit.ly/3kT7h8x)
    ```
    # mysql -uroot
    > CREATE DATABASE mubirou_db; ←既存の場合はERROR
    Query OK, 1 row affected (0.000 sec)
    ```
    ```
    > show databases; ←既存のデータベースの確認
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mubirou_db         | ←新規作成されたデータベース
    | ……                 |
    ```
1. [テーブルの作成](https://bit.ly/3F8tVk1)
    ```
    > USE mubirou_db ←既存のデータベースを開く
    Database changed
    > CREATE TABLE question_tb (
    ->  id VARCHAR(4),
    ->  answer INT
    ->  );
    Query OK, 0 rows affected (0.125 sec)
    ```
    ```
    > SHOW FIELDS FROM question_tb; ←テーブルの確認
    +--------+------------+------+-----+---------+-------+
    | Field  | Type       | Null | Key | Default | Extra |
    +--------+------------+------+-----+---------+-------+
    | id     | varchar(4) | YES  |     | NULL    |       |
    | answer | int(11)    | YES  |     | NULL    |       |
    +--------+------------+------+-----+---------+-------+
    ```
1. [データの追加](https://bit.ly/3J1vz88)
    ```
    > INSERT INTO question_tb VALUES ("0001", 3);
    > INSERT INTO question_tb VALUES ("0002", 4);
    ```
    ```
    >  SELECT * FROM question_tb;
    +------+--------+
    | id   | answer |
    +------+--------+
    | 0001 |      3 |
    | 0002 |      4 |
    +------+--------+
    ```

<a id="202303022135_js"></a>

### 👉 JavaScript の記述  
```javascript
// question.js
function onclick_btn1(_id) {
    // Get the selected value
    let _form1 = document.getElementById("form1");
    let _value = _form1.elements["radio1"].value;

    // Send to PHP
    let _request = new XMLHttpRequest();
    _request.onload = function() {
        let _json = JSON.parse(this.responseText);
        if (_json.bool) {
            alert("正解");
        } else {
            let _correctAnswerNum = _json.correctAnswer;
            let _correctAnswerText = document.getElementById("label" + _correctAnswerNum).innerText;
            alert("不正解\n" + "正解は「" + _correctAnswerText + "」です");
        }
    }
    _request.open("POST", "question.php");
    _request.setRequestHeader('content-type', 'application/x-www-form-urlencoded');
    let _data = "id=" + _id + "&" + "value=" + _value;
    _request.send(_data); // Argument is a string (ex."id=001&value=3")

    // Disable submit button
    const _btn1 = document.getElementById("btn1");
    _btn1.disabled = true;
}
```
※ [XMLHttpRequest](https://developer.mozilla.org/ja/docs/Web/API/XMLHttpRequest) ではなく [fetch](https://developer.mozilla.org/ja/docs/Web/API/Fetch_API/Using_Fetch) を使う方法もあり  

<a id="202303022135_php"></a>

### 👉 PHP の記述    
```php
<?php // question.php
    // Get data from JavaScript
    $id = $_POST["id"];
    $value = $_POST["value"];

    // Open database
    $dsn = "mysql:dbname=mubirou_db;host=localhost";
    $user = "root";
    $password = "";
    $pdo = new PDO($dsn, $user, $password);

    // Extract data matching the condition
    $sql = "SELECT * FROM question_tb WHERE id = {$id}";
    $statement = $pdo->query($sql);
    $result = $statement->fetch();

    // Create JSON Data
    $array = array();
    $array["bool"] = ($value == $result["answer"]);
    $array["correctAnswer"] = $result["answer"];
    $json = json_encode($array);
    echo $json;
?>
```

実行環境：CentOS Stream 8、Apache 2.4.37、PHP 7.4.30、MariaDB 10.3.28  
作成者：夢寐郎  
作成日：2023年3月13日  
[[TOP]](#TOP)  


<a id="202303151240"></a>
# <b>ポート開放</b>

👉 **光回線終端装置の設定（UNI接続）**  
> [**PR-S300SE/GV-ONU**](http://nttwest.ssdl1.smartstream.ne.jp/nttwest/flets/kiki/flets/prs300se/PRS300SE_man1409.pdf) について  
➀ 光回線終端装置（ONU＝Optical Network Unit）  
➁ ホームゲートウェイ（光電話対応のルーター）  
➂ 映像用回線終端装置（V-ONU）  
が一体になったもの
1. 本体上部のルータ機能部と接続されている UNI（User-Network Interface）ポートのケーブル（青）外す
1. UNIポート → LANケーブル → ルータ（光BBユニット）→ Linuxサーバ に接続

👉 **Linux の IP アドレスの固定化**

1. Linux の IP アドレス等を調べる  
    ```
    # ip address
    1: lo: ...
        ...
    2: eno1: ... ←デバイス名
        link/ether XX:XX:XX:XX:XX:XX ... ←MACアドレス
        ...
        inet 192.168.X.XX/24 brd 192.168.X.255 ... ←IPアドレス等
        ...
    ```
1. ネットワーク･インターフェイスの設定変更  
    ```
    # vi /etc/sysconfig/network-scripts/ifcfg-eno1 ←デバイス名
    TYPE=Ethernet
    PROXY_METHOD=none
    BROWSER_ONLY=no
    BOOTPROTO=none ←「dhcp」から変更
    DEFROUTE=yes
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_FAILURE_FATAL=no
    IPV6_ADDR_GEN_MODE=eui64
    NAME=eno1 ←デバイス名（初期値）
    UUID=21a0bf1c-2195-43fe-971d-XXXXXXXXXXXX
    DEVICE=eno1 ←デバイス名（初期値）
    ONBOOT=yes ←起動時に有効にするか否か（初期値）
    IPADDR=192.168.X.XX ←IPアドレス（追加）
    PREFIX=24 ←サブネットマスク（追加）
    GATEWAY=192.168.X.XX ←ルータのアドレス（追加） 
    DNS1=192.168.X.XX ←IPアドレス（追加）
    ```
    ※ 設定終了後「reboot now」で OS を再起動  

👉 **ルータ（光BBユニット）のポート開放**  

1. Web ブラウザで 172.16.255.254（192.168.X.1）を開く  
    * ユーザー名：user
    * パスワード：****（初期値 user）
1. [ルーター機能の設定]-[ポート転送設定] で以下の通り設定
    * 1 / 有効 / TCP / **20-21** / 20-21 / 192.168.X.XX ←[FTP](#202302121037)
    * 2 / 有効 / TCP/UDP / **22-22** / 22-22 / 192.168.X.XX ←[SSH](#202302111947)
    * 3 / 有効 / TCP / **80-80** / 80-80 / 192.168.X.XX ←[HTTP](#202302120812)
1. [設定を保存する] を選択、ルーターの再起動

👉 **ポート開放の確認**  

1. Linux サーバーのポート開放確認  
    ```
    # dnf -y update ←インストール済パッケージをアップデート
    # dnf -y install nmap ←nmapのインストール
    # nmap 192.168.x.xx ←LinuxサーバのIPアドレス
    PORT     STATE SERVICE
    21/tcp   open  ftp
    22/tcp   open  ssh
    80/tcp   open  http
    ```
1. ルータ（光BBユニット）のポート開放確認  
    ```
    # nmap 192.168.X.1 ←ルータ（光BBユニット）のIPアドレス
    ……
    PORT      STATE    SERVICE
    ……
    80/tcp    open     http
    ……
    ```
👉 **動作確認**

1. [グローバルIPアドレス](https://atmarkit.itmedia.co.jp/aig/06network/globalip.html)の確認
    1. ルータ（光BBユニット）の設定画面を開く  
        * 172.16.255.254（192.168.X.1）
        * ユーザー名：user
        * パスワード：****（初期値 user）
    1. [設定／接続情報]-[**WAN側IPアドレス**]-[IPアドレス] で確認
1. ルーター外から上記の[グローバルIPアドレス](https://atmarkit.itmedia.co.jp/aig/06network/globalip.html)にアクセスできたら成功！
1. 同様にルータ外から [SSH](#202302111947) と [FTP](#202302121037) に接続できたら成功！

参考：『INTRODUCTION NOTES 6』2頁（ポートの開放）  
参考：[ソフトバンク光のポート開放](https://naruhodo-wifi.com/softbank_hikari_port/)  
実行環境：[PR-S300SE/GV-ONU](http://nttwest.ssdl1.smartstream.ne.jp/nttwest/flets/kiki/flets/prs300se/PRS300SE_man1409.pdf)、[SoftBank E-WMTA2.3](https://torisetsu.biz/products/0000199609/)、CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年3月26日  
[[TOP]](#TOP)  


<a id="202303262200"></a>
# <b>ドメイン名取得</b>

> 【[お名前.com](https://www.onamae.com/)】  
    ・[GMOインターネットグループ(株)](https://www.gmo.jp/)が運営するドメイン登録サービス  
    ・アジアで初めて認定（現在は19社）を受け1999年からサービス開始された [ICANN](https://www.nic.ad.jp/ja/basics/terms/icann.html) 公認の[レジストラ](https://bit.ly/40mPd6b)  

1. https://www.onamae.com/ にアクセス
1. 取得したいドメイン名を検索
1. 〇の付いた取得したいドメインを選択 
1. レンタルサーバーは [利用しない] を選択
1. [料金確認へ進む] を選択
1. [お申込み内容] で [.com 1年登録] を選択
1. [⦿ 初めてご利用の方] を選び以下の通り設定し [次へ]  
    * メールアドレス：mubirou.info@gmail.com
    * パスワード：******
1. [続いて必要事項をご入力下さい。]で住所等を入力
1. [クレジットカード] 情報を入力
1. 各種設定
    * https://www.onamae.com/ にアクセスしログイン  
        * お名前ID（会員ID）：XXXXXXXX（8桁の数字）
        * パスワード：XXXXX
1. 半日ほど経過するとメールおよび LINE に「ドメイン登録完了」の連絡あり

参考：[ドメイン名とIPアドレスを紐づける手順](https://www.tagtec.net/onamaecom-dns-setting)  
作成者：夢寐郎  
作成日：2023年3月28日  
[[TOP]](#TOP)  


<a id="202303262032"></a>
# <b>HTTPS</b>

### この項目は書きかけです

👉 [Apache](#202302120812) で [HTTPS](https://ja.wikipedia.org/wiki/HTTPS) 化（常時 [SSL](https://bit.ly/3JRqfVu) 化）するのに必要なソフトウェアの準備

1. [OpenSSL](https://ja.wikipedia.org/wiki/OpenSSL) のインストール  
    ```
    # openssl version ←バージョン確認
    OpenSSL 1.1.1k  FIPS 25 Mar 2021 ←インストール済
    ```
1. **mod_ssl** のインストール  
    ```
    # dnf -y update ←インストール済パッケージをアップデート
    # dnf -y install mod_ssl ←インストール
    ```
    ```
    # rpm -qa | grep mod_ssl ←バージョン確認
    mod_ssl-2.4.37-54.module_el8.8.0+1256+e1598b50.x86_64
    ```

👉 南京錠（[公開鍵と秘密鍵](https://bit.ly/40O0MTH)）の購入と上司（[CA](https://bit.ly/3LWrWnb)）への報告書（[CSR](https://jp.globalsign.com/support/ssl/certificates/about-csr.html)＝[SSLサーバ証明書](https://bit.ly/3Kgsxyy)発行の署名要求）の作成  

1. 各作業
```
# cd /etc/pki/tls/certs
# cp /usr/share/doc/openssl/Makefile.certificate Makefile
# dnf -y install make ←makeコマンドのインストール
# make /etc/pki/tls/certs/server.csr
……
Enter pass phrase:**** ←パスワードの入力（3回繰返す）
Country Name (2 letter code) [XX]:JP
State or Province NAme (full name) []:Tokyo
Locality Name (eg, city) [Default City]:Setagaya
Organization Name (eg, company) [Default Company Ltd]:mubirou
Organization Unit (eg, section) []:Network
Commmon Name (eg, your name or your server's hostname) []:mubirou.com
Email Address []:mubirou.info@gmail.com
A challenge password []: ↲
An optional company name []: ↲
```

1. 報告書([CSR](https://jp.globalsign.com/support/ssl/certificates/about-csr.html)＝[SSLサーバ証明書](https://bit.ly/3Kgsxyy)の内容の確認
```
# openssl req -in server.csr -text
Certificate Request:
    ……
    Subject: C = JP, ST = Tokyo, L = Setagaya, O = mubirou, OU = Network, CN = mubirou.com, emailAddress = mubirou.info@gmail.com
    ……
    Public Key Algorithm: rsaEncryption ←公開鍵（≒南京錠本体）
        RSA Public-Key: (2048 bit) ←RSA暗号
    ……
```

***


```
# cat /etc/httpd/conf.d/ssl.conf ←HTTPSの設定ファイルの確認
```




1. CAに公開鍵証明書の発行申請  
    1. XXXX

参考：[無料のSSL証明書を作成する方法](https://webree.jp/article/letsencrypt-install)  
参考：[SSL/TLSサーバ証明書を取得する方法](https://e-penguiner.com/how-to-get-ssl-tls-certificate-in-letsencrypt-and-update/)  
参考：[SSL証明書の導入手順](https://blog.senseshare.jp/encrypt.html)  
作成者：夢寐郎  
作成日：2023年X月XX日  
[[TOP]](#TOP)  


© 2023 夢寐郎  


***
### メモ📝

1. **ホームネットワークセキュリティ機能**をオフにする
    1. http://aterm.me/（192.168.1.210）にアクセス  
        * ユーザー名：admin
        * パスワード：*****
    1. [詳細設定]-[ホームネットワークセキュリティ機能]-[OFF]

1. **メッシュ機能**をオフにする  
    [Wi-Fi基本設定]-[メッシュWi-Fi機能]-[OFF]

1. **SSID名**等の変更  
    1. [Wi-Fi詳細設定（2.4GHz）] を選択して各種設定  
        * 対象ネットワークを選択：プライマリSSID：XXXX
        * ネットワーク名（SSID）：aterm-xxxxxx-5p（初期値）
        * 暗号化キー：（任意の値に変更）  
        ※[セカンダリSSID] は [ESS-IDステルス機能] を [ON]
    1. [Wi-Fi詳細設定（5GHz）] を選択して各種設定  
        * 対象ネットワークを選択：プライマリSSID：XXXX
        * ネットワーク名（SSID）：aterm-xxxxxx-5p（初期値）
        * 暗号化キー：（任意の値に変更）  
        ※[セカンダリSSID] は [ESS-IDステルス機能] を [ON]

1. **動作モード**の変更
    * 電源を抜く
    * 本体の [MODE] スイッチを [RT]（ルーターモード）→ [BR]（ブリッジモード）に変更
    * 電源を入れる