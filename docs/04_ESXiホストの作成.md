# ESXiホスト作成

<img width=60% src="./figure/Overview-1次仮想マシン.png">

実施内容

* 仮想マシンの作成

* ESXiインストール

* ESXi設定
	* IPアドレス設定
	* シェル・SSH有効化
	* vSphere Web Client へログイン
	* ライセンス登録

## マシン構成

| 物理設定 |  |
| :- | :- |
| 名前 | ESXi |
| ディスク容量 | 30GB |
| メモリ | 4GB |
| CPU | 2コア |
| NW | ホストオンリー, ブリッジ接続 |

<br>

| ESXi設定 |  |
| :- | :- |
| ホスト名 | ESXI-HOST |
| rootパスワード | sN$87fzS |
| ネットワークアダプタ | vmnic0 (ホストオンリー), vmnic1 (ブリッジ接続) |
| IPアドレス(管理用) | 192.168.153.126 |
| デフォルトGW(管理用) | 未設定 |
| シェル | 有効 |
| SSHログイン | 有効 |


## 仮想マシンの作成

### isoイメージのダウンロード

<b>※ My VMWare のアカウント登録が必要</b>

* VMware の公式サイトにアクセス

	https://www.vmware.com/jp.html

* [ダウンロード] > [無償製品のダウンロード] > [vSphere Hypervisor] をクリック

	<img width=70% src="./figure/04/vsphere無償版ダウンロード.png">

* [アカウントの作成] をクリックして、アカウント登録した後にログインしなおす

	<img width=80% src="./figure/04/Screenshot_2020-01-04 VMware vSphere Hypervisor の無償ダウンロード.png">

* ライセンスキーを確認しておく<br>isoファイルがダウンロード可能になるので、[手動ダウンロード] をクリックしてダウンロード

	<img width=80% src="./figure/04/Screenshot_2020-01-04 VMware vSphere Hypervisor の無償ダウンロード_2.png">

	※本書では「6.7 Update 3- バイナリ」ではなく、「6.7 Update 2- Binaries」を用いている

### VMWare Player設定

* 新規仮想マシンの設定を入力<br>特に注意すべきポイントは以下

	* [プロセッサ] > [仮想エンジン] の「Intel VT-x/EPTまたは...」にチェックが入っていることを確認

		<img width=60% src="./figure/04/VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 1_22_58.png">

	* [ネットワークアダプタ] にホストオンリー、[ネットワークアダプタ 2] にブリッジを設定

		<img width=60% src="./figure/04/VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 1_23_20.png">

	* 最終的な設定は下のようになる

		<img width=60% src="./figure/04/VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 1_23_30.png">
	
* 設定を完了させて仮想マシンを起動

## ESXiインストール

<b>※Alt + Ctrl キーでマウスカーソルがホストOSに戻る</b>

* ライセンス同意画面。F11（同意）を押下

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_21_12.png">

* ディスク選択画面。そのまま Enterを押下

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_21_29.png">

* 言語は [Japanese] を選択

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_21_41.png">

* rootパスワード [sN$87fzS] を入力

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_22_07.png">

* インストール確認画面が出るので、F11を押下してインストール開始

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_22_18.png">

* インストール完了画面。Enterキーを押して再起動

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_23_20.png">

* 下のようなホーム画面が出てきたらOK
	
	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_25_26.png">


## ESXi設定

### ネットワーク設定

* ホーム画面でF2キーを押して設定画面に

* 途中でユーザ認証を求められるので、rootユーザのパスワード [sN$87fzS] を入力してEnter

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_27_41.png">

* 設定画面から [Configure Management Network] を選択

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_28_34.png">

* [Network Adapters] を選択

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_05 0_31_35.png">

* 管理用（ホストオンリー）のネットワークアダプタ [vmnic0] が選択されていることを確認してEnter

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_30_33.png">

	* どちらがホストオンリーのアダプタかは、仮想マシン設定画面でMACアドレスを確認して判断する

		<img width=60% src="./figure/04/MACアドレス確認.png">

* [IPv4 Configuration] を選択

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_30_45.png">

* 管理用のIPアドレスを設定してEnter

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_31_24.png">

* [IPv6 Configuration] を選択し、[Disable IPv6] を選択してEnter

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_31_24.png">

* ホスト名 [ESXI-HOST] を入力してEnter

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_35_03.png">

* Escキーを押して設定画面を抜ける

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_05 0_47_09.png">

* 設定反映のためにホストの再起動を求められるので [Yes] を選択

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_32_40.png">

* 再起動後のホーム画面で、IPアドレスとホスト名を確認

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_35_33.png">

### シェル・SSH有効化

* 設定画面から [Troubleshooting Options] を選択

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_36_11.png">

* [Enable ESXi Shell] で Enterを押下して有効化

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_36_17.png">

* 画面右側で [ESXi Shell is Enabled] となっていることを確認

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_36_26.png">

* 同様にSSHも有効化して設定画面を抜ける

	<img width=70% src="./figure/04/ESXi - VMware Workstation 15 Player (非営利目的の使用のみ) 2020_01_04 23_36_38.png">


### vSphere Web Client へログイン

* ホストOSのブラウザから、vSphere Web Clientにアクセス<br>途中 HTTPS 証明書の警告が出るが無視

	https://192.168.153.126/

* ログイン画面で、ユーザ [root]、パスワード [sN$87fzS] を入力してログイン

	<img width=70% src="./figure/04/Screenshot_2020-01-04 ログイン VMware ESXi.png">

* ホーム画面が表示されることを確認

	<img width=80% src="./figure/04/Screenshot_2020-01-04 ESXI-HOST localdomain VMware ESXi.png">

### ライセンス登録

* ホーム画面左側のメニューから、[ホスト] > [管理] をクリックしてホスト管理画面を表示

	<img width=80% src="./figure/04/Screenshot_2020-01-04 ESXI-HOST localdomain VMware ESXi.png">

* ホスト管理画面の、[ライセンス] タブをクリック

	<img width=80% src="./figure/04/Screenshot_2020-01-04 ESXI-HOST localdomain VMware ESXi_2.png">

* ライセンスキーを入力して [ライセンスの確認] をクリック

	<img width=60% src="./figure/04/ESXI-HOST.localdomain_ VMware ESXi - Mozilla Firefox 2020_01_04 23_51_55.png">

* [ライセンスの割り当て] をクリック

	<img width=60% src="./figure/04/ESXI-HOST.localdomain_ VMware ESXi - Mozilla Firefox 2020_01_04 23_52_23.png">

* ライセンスキーが登録されていればOK

	<img width=70% src="./figure/04/Screenshot_2020-01-05 ESXI-HOST localdomain VMware ESXi.png">
