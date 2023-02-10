# LAMP環境構築メモ <a id="TOP"></a>

### **Index**

| [LAMP選択](#202301281000) | [ブートUSBの作成](#202301281748) | [ブートUSBの起動](#202302092321) | [CentOSインストール](#202302101739) | [ルーター設定](#202302102308) | [各種サーバのインストール](#202302102000) |
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


<a id="202302102308"></a>
# <b>ルーター設定</b>

* 「IODATA WN-AC583TR」について  
    * 2014年7月発売（2018年6月生産終了）
    * IEEE802.11b / 11g / 11a / 11ac 対応
    * 5GHz / 2.4GHz 帯対応（同時接続可）
    * ファームウェア：v1.07（2019年2月更新）
    * 無線接続4台まで推奨
    * [マニュアル](https://www.iodata.jp/lib/manual/pdf2/wn-ac583tr.pdf)

1. 「192.168.99.1」に接続しログイン  
    * ユーザー名：なし
    * パスワード：*********
1. 必要に応じてファームウェアの更新を行う
1. 

実行環境：Windows 10、IODATA WN-AC583TR  
作成者：夢寐郎  
作成日：2023年2月XX日  
[[TOP]](#TOP)  


<a id="202302102000"></a>
# <b>各種サーバのインストール</b>

➀SSH ②Apache ③FTP ④PHP ➄MySQL ➅Samba の順にインストール

1. [SSH](https://www.kagoya.jp/howto/it-glossary/server/ssh/)のインストール

1. [Apache]


実行環境：CentOS Stream 8  
作成者：夢寐郎  
作成日：2023年2月XX日  
[[TOP]](#TOP)  


© 2023 夢寐郎
