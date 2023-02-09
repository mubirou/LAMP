# LAMP環境構築メモ <a id="TOP"></a>

### **Index**

| [選択肢](#202301281000) | [インストール用USBメモリ](#202301281748) |

<a id="202301281000"></a>
# <b>選択肢</b>

* [OS](https://ja.hostadvice.com/marketshare/os/jp/) : [**CentOS**](https://www.centos.org/)  
* [Web Server](https://manuon.com/webserver-share-ranking/#index_id4) : [**Apache**](https://httpd.apache.org/)  
* [RDBMS](https://db-engines.com/en/ranking) : [**MySQL**](https://www.mysql.com/jp/) / [MariaDB](https://mariadb.com/kb/ja/mariadb/) / [SQLite](https://sqlite.org/)  
* [Server Side Programming](https://w3techs.com/technologies/overview/programming_language) : [**PHP**](https://www.php.net/) / [Python](https://www.python.jp/)  

作成者：夢寐郎  
作成日：2023年1月28日  
[[TOP]](#TOP)  


<a id="202301281748"></a>
# <b>インストール用USBメモリ</b>

1. CentOSのISOファイルをダンロード
    1. https://www.centos.org/download/ を開く
    1. 「CentOS Stream]-[8]-[x86_64] を選択
    1. http://ftp.riken.jp/Linux/centos/8-stream/isos/x86_64/ 等から選択
    1. CentOS-Stream-8-x86_64-20230125-dvd1.iso を選択しダウンロード（約11GB）

1. [Ubuntu Software] で [ブータブルUSBの作成] アプリをインストール

1. [ブータブルUSBの作成] を起動し設定＆確認
    * 書き込み元のディスクイメージ（.iso）：

1. Universal USB Installerのダウンロード
    1. https://bit.ly/3HoSQQz を開く
    1. [Download UUI]（Universal-USB-Installer-2.0.1.41.exe）を選択

1. ブータブルUSBの作成
    1. Universal USB Installe を起動し各種設定
        1. Step 1：CentOS Installer
        1. Step 2：上記でダウンロードしたISOファイルを指定
        1. Step 3：USBメモリ（16GB以上）を選択
    1. [Create]ボタンを押す


1. Rufusのダウンロード
    * Rufus（ルーファス）について  
        * The Reliable USB Formatting Utility, with Sourceの略
        * USBメモリを起動可能ドライブにするフリーソフトウェア
        * Windows対応
    1. https://rufus.ie/ja/ を開く
    1. [ダウンロード]-[Rufus 3.21] を選択
    1. ダウンロードした [rufus-3.2.1.exe] を起動し各種設定  
        * デバイス：USBメモリ（16GB以上）
        * ブートの種類：ダウンロード済の上記のISOファイル
        * パーティション構成：MBR（初期値）
        * ターゲットシステム：BIOSまたはUEFI（初期値）
        * ボリュームラベル：CentOS-Stream-8-x86_64（任意）
        * ファイルシステム：FAT32（規定）
        * クラスターサイズ：16キロバイト（規定）
        * クイックフォーマット：なし
        * 機能拡張されたラベルとアイコンファイルを作成：なし
        * 不良ブロックを検出：なし
        * 1パス
    1. [スタート]（⦿ISOイメージモードで書き込む）を選択

参考：[CentOS 8をインストールする手順](https://nwengblog.com/centos8install/#toc1)  
実行環境：Windows 10  
作成者：夢寐郎  
作成日：2023年X月XX日  
[[TOP]](#TOP)  

© 2023 夢寐郎
