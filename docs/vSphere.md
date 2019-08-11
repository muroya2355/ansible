# vSphere と Software-Defined Data Center の概要

## データセンターの概要

### サーバ仮想化

* ホスト型

	OS 上でアプリケーションとして仮想化

* ハイパーバイザ（VMkernel）

	HW上に仮想化レイヤ（ハイパーバイザ）をインストール

## VMware vSphere の概要

### VMware vSphere 製品スイート

* VMware ESXi

	ハイパーバイザ

* VMware vCenter Server

	複数のESXi ホストや仮想マシンを一元管理

* VMware vSphere vMotion

	稼働中の仮想マシンをホスト間で移行

* VMware vSphere Storage vMotion

	稼働中の仮想マシンのファイルをデータストア間で移行

* VMware vSphere Distributed Resource Scheduler (DRS)

	複数のホスト間でのロードバランシング

* VMware vSphere HA

	ホストの障害発生時に、ダウンした仮想マシンを別サーバで再起動する

* vSphere Fault Tolerance (FT)

	仮想マシンを停止することなく HA を提供

* VMware vSphere Distributed Switch (VDS)

	複数のホストにまたがった単一の論理スイッチを構成する

* VMware vSphere Data Protection

	運用中の仮想マシンへのバックアップソリューションを提供

### VMware vSphere のエディション

* vSphere Standard
* vSphere Enterprise Plus
* vSphere Enterprise with Operations Management (vSOM)
* vSphere Desktop

### VMware vSphere 管理の概要

* VMware vSphere Client

	vSphere 6.5 で廃止

* VMware vSphere Web Client

	vCenter Server へ接続し、機能・プラグインを管理

* VMware vSphere Host Client

	HTML5ベース。スタンドアロンホストの管理、障害発生時にホストに直接接続

* VMware vSphere PowerCLI

	PowerShell を使った vSphere 環境の管理

* VMware vSphere Command Line Interface

	コマンドラインから vSphere 環境を管理

## Software-Defined Data Center

### Software-Defined Data Center の概要

* Software-Defined Data Center (SDDC)

	物理データセンターで運用されているコンポーネントをソフトウェアで構成する。サーバの仮想化、仮想ネットワーク、Software Defined Storage


## VMware ESXi の導入方法

### ESXi のシステム要件

* CPU 64bit
* メモリ 4GB以上
* 起動ストレージ 1GB以上
* ネットワークアダプタ 1つ以上のギガビットイーサネットアダプタ

### ESXi のインストール

* CD/DVD メディアからのインストール

* Auto Deploy

	PXE ブートをベースとし、ネットワーク上からインストールイメージを取得する。Power CLI と vSphere Web Client で構成できる

* ステートレスキャッシュ

	初回起動時に起動イメージをブート領域にキャッシュする

* ステートフルインストール

	Auto Deploy プロセスを利用してホストイメージをブート領域にインストールする

* スクリプトインストール

	インストール時に必要な構成情報をあらかじめ KickStart ファイルに定義し、自動インストールを行う


## DCUI による初期設定

### DCUI へのログイン

* DCUI (Direct Console User Interface)

	ESXi ホストの管理画面

### 管理用IPアドレスの設定

管理IPアドレスは仮想スイッチの VMkernel ポートとして設定される

### トラブルシューティングオプションの設定

* ESXi シェルの有効化／無効化
* SSH の有効化／無効化
* ESXi シェルと SSH のタイムアウト
* DCUI タイムアウト
* 管理エージェントの再起動
	* vpa, hostd

		vCenter や VMware Host Client からの接続を制御する管理サービス

## vSphere GUI インターフェース

* VMware Host Client

	https://<IPアドレス or FQDN>/ui

* vSphere Web Client

# 仮想マシンの作成と管理

## 仮想マシンの構成要素

### 仮想マシンのファイル

| ファイル     | 説明                    |
| :---------- | :---------------------- |
| *.vmx       | 構成ファイル             |
| *.vmdk      | 仮想ディスク (定義データ) |
| *-flat.vmdk | 仮想ディスク (実データ)   |
| *.vswp      | スワップファイル          |
| *.nvram     | BIOSファイル             |
| *.vmsd, *.vmsn, *-delta.vmdk | スナップショットファイル |


### 仮想マシンハードウェアバージョン

* vSphere 6.5 以降の最大ハードウェア構成

	* 最大仮想CPU 128個
	* 最大仮想メモリ 6TB
	* 最大仮想ディスク 62TB

* NVMe (Non-Volatile Memory Express) コントローラ

	SSD 接続規格

* RDMA (Remote Direct Memory Access)

	コンピュータの主記憶から異なるコンピュータの主記憶へデータ転送を行う

## 仮想マシンの作成

### 仮想マシンの作成方法

* 新規仮想マシンの作成
* 仮想アプライアンス展開

	事前に構成済みの仮想マシンのファイル（仮想アプライアンス）を入手して展開する

* クローン

	ベースとなる仮想マシンファイルを別の仮想マシンへコピーする

* テンプレート

	ベースとなる仮想マシンからテンプレートファイル（.vmtx）を作成し、テンプレートファイルから新規の仮想マシンを展開する

* テンプレートやクローンから仮想マシンを展開する場合は、設定情報のカスタマイズが必要

### 仮想マシン作成ウィザード

* 仮想マシンハードウェアバージョン

	vSphere 6.5：バージョン13

* 仮想CPU
* 仮想メモリ
* 仮想ディスク
	* SCSI (Small Computer System Interface) アダプタ

		コンピュータと周辺機器とのハードウェア間でデータのやり取りを行うインターフェース規格

	* シンプロビジョニング

		使用率に応じた可変長サイズディスク

	* シックプロビジョニング

		固定長サイズディスクファイル

	* Eager Zeroed

		仮想ディスクファイル作成時にゼロ初期化を行う。ファイル作成時に時間がかかるが、以降の書き込み時のパフォーマンスが向上する

	* Lazy Zeroed

		仮想ディスクファイル作成時にゼロ初期化を行わない。短時間でファイル作成できるが、書き込みが低速となる

* 仮想ネットワークアダプタ

	* 完全仮想化アダプタ

		仮想マシン内で物理NICのエミュレーションドライバを使用する。e1000, e1000e

	* 準仮想化アダプタ

		VMware Tools 内に含まれる仮想マシン向けの専用ドライバ。vmxnet3

	* SR-IOV パススルー (Single Root I/O Virtualization)

		仮想マシンの仮想ネットワークアダプタを、ハイパーバイザーを介さず直接物理ネットワークアダプタにマッピングする

* その他の仮想ハードウェア

	CDドライブ、USBデバイス、SCSIアダプタ等

## VMware Tools

### VMware Tools の機能

* VMware Tools

	仮想マシン上にインストールし、仮想マシンの機能を最適に利用するための仮想化専用のドライバや管理機能を提供する

* ハイパーバイザーとの時刻同期
* 仮想化専用ドライバ
* マウス操作の最適化
* 仮想マシンハートビート
* メモリ管理ドライバ

	ゲストOS内のアイドルメモリを解放する等の機能を提供する

* デジタル署名の検証

	仮想マシンの UEFI セキュアブートを有効にした場合、仮想マシンにロードできるのはデジタル署名が付与されたドライバのみとなる

### VMware Tools のイメージ

## 仮想マシンの管理

### 仮想ハードウェアの設定変更

* 仮想CPU、仮想メモリの構成

	* ホットプラグ機能

		仮想マシンの実行中に仮想ハードウェアを追加可能

	* RDM 仮想ディスクの構成

		* RDM (Raw Device Mapping)

			仮想マシンに対して直接物理デバイスを割り当てる

		* LUN (Logical Unit Number)

			OS から1台の物理HDDとして認識される単位

		* VMFS (Virtual Machine File Systems)

		* 仮想互換モード

		* 物理互換モード (パススルーモード)
		
	* vFRC の構成

	* ファイバチャネル NPIV の構成

	* VMware Tools の構成

## スナップショット

## コンテンツライブラリ

## VMware vSphere vApp

## 仮想マシンの移行

## 仮想マシンのバックアップ

## vCenter Converter



# vCenter Server の構成と管理

## vCenter Server の構成

## vCenter HA の構成

## vCenter Server Appliance のバックアップとリストア

## vSphere 環境のアップグレード



# ネットワーク管理

## 仮想ネットワークの概要

## 仮想スイッチ

## 仮想スイッチのコンポーネント

## NIC チーミング

## 仮想スイッチポリシーの構成



# ストレージ管理

## ストレージの構成

## FC-SAN と FCoE

## iSCSI

## NFS

## vSphere VMFS

## ストレージの最適化

## VMware vSAN

## vSphre Virtual Volumes

## Storage DRS

## vSphere Storage API



# リソース管理とパフォーマンスチューニング

## リソース最適化の概要

## リソースプール

## パフォーマンスモニタリング

## アラームによる監視



# クラスタの設定と管理

## vSphere HA

## vSphere FT

## vSphere DRS

## Enhanced vMotion Compatibility



# vSphere のセキュリティ管理とコンプライアンス管理

## ESXi ホストのセキュリティ設定

## アクセス権の設定

## ESXi ホストの UEFI セキュアブート

## 仮想マシンのセキュリティ

## vSphere Update Manager の概要

## ホストプロファイル