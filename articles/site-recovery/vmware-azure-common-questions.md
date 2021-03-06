---
title: よくある質問 - Azure Site Recovery を使用した VMware から Azure へのディザスター リカバリー | Microsoft Docs
description: この記事では、Azure Site Recovery を使用してオンプレミスの VMware VM の Azure へのディザスター リカバリーを設定する場合のよくある質問をまとめています
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
services: site-recovery
ms.date: 02/13/2019
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 83c9a0baa4d853c8afcb5afe1c4e5cc4ed1e0073
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/13/2019
ms.locfileid: "56235226"
---
# <a name="common-questions---vmware-to-azure-replication"></a>よくある質問 - VMware から Azure へのレプリケーション

この記事では、オンプレミスの VMware VM の Azure へのディザスター リカバリーをデプロイするときによくある質問に回答します。 この記事の内容について質問がある場合は、 [Azure Recovery Services フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)に投稿してください。


## <a name="general"></a>全般
### <a name="how-is-site-recovery-priced"></a>Site Recovery の価格
詳しくは、「[Site Recovery の価格](https://azure.microsoft.com/pricing/details/site-recovery/)」をご覧ください。

### <a name="how-do-i-pay-for-azure-vms"></a>Azure VM の支払い方法
レプリケーションの間に、データは Azure ストレージにレプリケートされ、VM 変更の料金は発生しません。 Azure へのフェールオーバーを実行すると、Site Recovery は Azure IaaS 仮想マシンを自動的に作成します。 その後は、Azure で消費するコンピューティング リソースに対して課金されます。

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>VMware から Azure へのレプリケーションによってできること
- **ディザスター リカバリー** :完全なディザスター リカバリーを設定することができます。 このシナリオでは、オンプレミスの VMware VM を Azure ストレージにレプリケートします。 その後、オンプレミスのインフラストラクチャが使用できなくなった場合は、Azure にフェールオーバーできます。 フェールオーバーの際は、レプリケートされたデータを使って Azure VM が作成されます。 オンプレミスのデータ センターが再び使用可能になるまでは、Azure VM のアプリとワークロードにアクセスできます。 その後、Azure からオンプレミスのサイトにフェールバックできます。
- **移行**:Site Recovery を使って、オンプレミスの VMware VM を Azure に移行できます。 このシナリオでは、オンプレミスの VMware VM を Azure ストレージにレプリケートします。 その後、オンプレミスから Azure にフェールオーバーします。 フェールオーバーの後は、Azure VM で実行しているアプリとワークロードを利用できます。

## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>Azure で必要なものは何ですか?
Azure サブスクリプション、Recovery Services コンテナー、ストレージ アカウント、仮想ネットワークが必要です。 コンテナー、ストレージ アカウント、ネットワークは、同じリージョンに存在する必要があります。

### <a name="what-azure-storage-account-do-i-need"></a>どの Azure ストレージ アカウントが必要ですか?
LRS または GRS ストレージ アカウントが必要です。 地域的障害が発生した場合やプライマリ リージョンが復旧できない場合にデータの復元性を確保できるように、GRS をお勧めします。 Premium Storage はサポートされています。

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Azure アカウントには VM を作成するアクセス許可が必要ですか?
サブスクリプション管理者の場合は、必要なレプリケーション アクセス許可を持っています。 サブスクリプション管理者ではない場合は、Site Recovery を構成するときに指定するリソース グループと仮想ネットワークに Azure VM を作成するアクセス許可と、選んだストレージ アカウントに書き込むアクセス許可が必要です。 [詳細情報](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)。

### <a name="can-i-use-guest-os-server-license-on-azure"></a>Azure でゲスト OS のサーバー ライセンスを使用できますか?
はい、マイクロソフト ソフトウェア アシュランスのお客様は、[Azure ハイブリッド特典](https://azure.microsoft.com/en-in/pricing/hybrid-benefit/)を使用して、Azure に移行する **Windows Server マシン**のライセンス コストを節約したり、Azure をディザスター リカバリーに使用したりできます。

## <a name="pricing"></a>価格

### <a name="how-are-licensing-charges-handled-during-replication-after-failover"></a>フェールオーバー後、レプリケーションの間にライセンス料金はどのように処理されますか?

詳しくは、[こちら](https://aka.ms/asr_pricing_FAQ)のラインセンスに関する FAQ をご覧ください。

### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>Site Recovery の使用中は、どのようにして概算料金を計算できますか?

Azure Site Recovery を使用している間は、[料金計算ツール](https://aka.ms/asr_pricing_calculator)を利用してコストを見積もることが可能です。 コストの詳細な見積のためには、デプロイ プランナー ツールを実行して (https://aka.ms/siterecovery_deployment_planner))、[コスト見積レポート](https://aka.ms/asr_DP_costreport)を分析します。

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>Azure Site Recovery のユーザーになって 1 か月以上が経過しました。 保護されたインスタンスはすべて、引き続き最初の 31 日間無料の対象になりますか?

はい。どのくらい長く Azure Site Recovery を利用しているかは関係ありません。 保護されたインスタンスはすべて、最初の 31 日間は Azure Site Recovery の料金が発生しません。 たとえば、過去 6 か月間に 10 個のインスタンスを保護していて、11 個目のインスタンスを Azure Site Recovery に接続した場合、11 個目のインスタンスに対する Azure Site Recovery の料金は、最初の 31 日間は発生しません。 最初の 10 個のインスタンスは、既に 31 日間を超えて保護されているので、継続して Azure Site Recovery の料金が発生します。

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>最初の 31 日間に、その他の Azure 料金は発生しますか?

はい。Azure Site Recovery では、保護されたインスタンスは最初の 31 日間無料ですが、Azure Storage、ストレージ トランザクション、およびデータ転送については料金が発生する場合があります。 また、復旧された仮想マシンも、Azure のコンピューティングの料金が発生する場合があります。

### <a name="what-charges-do-i-incur-while-using-azure-site-recovery"></a>Azure Site Recovery を使うと、どのような料金が発生しますか?

詳しくは、[発生するコストに関する FAQ ](https://aka.ms/asr_pricing_FAQ)に関するページをご覧ください。

### <a name="is-there-a-cost-associated-to-perform-dr-drillstest-failover"></a>DR ドリル/テスト フェールオーバーの実行に関連するコストはありますか?

DR ドリルに対しては、別個のコストはかかりません。 テスト フェールオーバー後に仮想マシンが作成されると、以降はコンピューティングの料金がかかります。

## <a name="azure-site-recovery-components-upgrade"></a>Azure Site Recovery コンポーネントのアップグレード

### <a name="my-mobility-agentconfiguration-serverprocess-server-version-is-very-old-and-my-upgrade-has-failed-how-should-i-upgrade-to-latest-version"></a>モビリティ エージェント/構成サーバー/プロセス サーバーのバージョンが非常に古く、、アップグレードに失敗しました。 最新バージョンにアップグレードするにはどうすればよいですか?

Azure Site Recovery は N-4 サポート モデルに従っています。 非常に古いバージョンからアップグレードする方法の詳細については、[サポートに関する声明](https://aka.ms/asr_support_statement)を参照してください。

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-azure-site-recovery"></a>Azure Site Recovery のリリース ノートと更新プログラムのロールアップはどこで入手できますか?

リリース ノート情報については、この[ドキュメント](https://aka.ms/asr_update_rollups)を参照してください。 各更新プログラムのロールアップでは、それぞれのコンポーネントのインストールのリンクがあります。

### <a name="how-should-i-upgrade-site-recovery-components-for-on-premises-vmware-or-physical-site-to-azure"></a>オンプレミスの VMware または物理サイトの Site Recovery コンポーネントを Azure をアップグレードするには、どうすればよいですか?

コンポーネントをアップグレードするには、[こちら](https://aka.ms/asr_vmware_upgrades)に記載されているガイダンスを参照してください。

## <a name="is-reboot-of-source-machine-mandatory-for-each-upgrade"></a>ソース マシンの再起動は、アップグレードごとに必須ですか?

アップグレードごとをお勧めしますが、必須ではありません。 明確なガイダンスについては、[こちら](https://aka.ms/asr_vmware_upgrades)を参照してください。

## <a name="on-premises"></a>オンプレミス

### <a name="what-do-i-need-on-premises"></a>オンプレミスには何が必要ですか?

オンプレミスには、次のものが必要です。
- 1 つの VMware VM にインストールされた Site Recovery のコンポーネント。
- 少なくとも 1 つの ESXi ホストを含む VMware インフラストラクチャ。vCenter サーバーをお勧めします。
- レプリケートする 1 つ以上の VMware VM。

VMware から Azure へのアーキテクチャについて詳しくは、[こちら](vmware-azure-architecture.md)をご覧ください。

オンプレミスの構成サーバーは次のように展開できます。

- 構成サーバーが事前インストールされている OVA テンプレートを使用し、VMware VM として構成サーバーを展開することをお勧めします。
- 何らかの理由でテンプレートを使用できない場合は、構成サーバーを手動で設定できます。 [詳細情報](physical-azure-disaster-recovery.md#set-up-the-source-environment)。



### <a name="where-do-on-premises-vms-replicate-to"></a>オンプレミスの VM のレプリケート先はどこですか?
Azure ストレージにデータがレプリケートされます。 フェールオーバーを実行すると、Site Recovery はストレージ アカウントから Azure VM を自動的に作成します。

## <a name="replication"></a>レプリケーション

### <a name="what-applications-can-i-replicate"></a>どのアプリケーションをレプリケートできますか?
[レプリケーション要件](vmware-physical-azure-support-matrix.md##replicated-machines)に準拠する VMware VM で実行しているすべてのアプリまたはワークロードをレプリケートできます。 また、アプリケーションに対応したレプリケーションもサポートしているため、アプリをインテリジェントな状態にフェールオーバーおよびフェールバックできます。 Site Recovery は、SharePoint、Exchange、Dynamics、SQL Server、Active Directory などの Microsoft アプリケーションと統合し、Oracle、SAP、IBM、Red Hat などの主要なベンダーと緊密に連携します。 [詳細情報](site-recovery-workload.md) を参照してください。

### <a name="can-i-protect-a-virtual-machine-that-has-docker-disk-configuration"></a>Docker ディスク構成を備えた仮想マシンを保護できますか?

いいえ、これはサポートされていないシナリオです。

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>サイト間 VPN で Azure にレプリケートできますか?
Site Recovery は、パブリック エンドポイントまたは ExpressRoute のパブリック ピアリングを使って、オンプレミスから Azure ストレージにデータをレプリケートします。 サイト間 VPN ネットワーク経由のレプリケーションはサポートされていません。

### <a name="can-i-replicate-to-azure-with-expressroute"></a>ExpressRoute で Azure にレプリケートできますか?
はい。ExpressRoute を使って Azure に VM をレプリケートできます。 Site Recovery は、パブリック エンドポイント経由で Azure Storage アカウントにデータをレプリケートします。 Site Recovery のレプリケーションに ExpressRoute を使うには、[パブリック ピアリング](../expressroute/expressroute-circuit-peerings.md#publicpeering)または [Microsoft ピアリング](../expressroute/expressroute-circuit-peerings.md#microsoftpeering)を設定する必要があります。 Microsoft ピアリングは、レプリケーション用に推奨されるルーティング ドメインです。 レプリケーションのための[ネットワークの要件](vmware-azure-configuration-server-requirements.md#network-requirements)も満たしていることを確認してください。 VM が Azure 仮想ネットワークにフェールオーバーした後は、[プライベート ピアリング](../expressroute/expressroute-circuit-peerings.md#privatepeering)を使ってアクセスできます。

### <a name="how-can-i-change-storage-account-after-machine-is-protected"></a>コンピューターが保護された後は、どのようにストレージ アカウントを変更できますか?

ストレージ アカウントは、Premiun にのみアップグレードできます。 別のストレージ アカウントを使用したい場合、ソース コンピューターのレプリケーションを無効にしてから、新しいストレージ アカウントを使用して保護を再度有効にする必要があります。 これ以外に、保護が有効になった後にストレージ アカウントを変更する方法はありません。

### <a name="why-cant-i-replicate-over-vpn"></a>VPN 経由でレプリケートできないのはなぜですか?

Azure にレプリケートする場合、レプリケーション トラフィックは Azure Storage アカウントのパブリック エンドポイントに到達するので、ExpressRoute (パブリック ピアリング) のパブリック インターネットによってのみレプリケートでき、VPN は動作しません。

### <a name="what-are-the-replicated-vm-requirements"></a>レプリケートされる VM にはどのような要件がありますか?

レプリケーションの場合、VMware VM はサポートされているオペレーティング システムを実行している必要があります。 さらに、VM は Azure VM に対する要件を満たす必要があります。 サポート マトリックスについて詳しくは、[こちら](vmware-physical-azure-support-matrix.md##replicated-machines)をご覧ください。

### <a name="how-often-can-i-replicate-to-azure"></a>どのくらいの頻度で Azure にレプリケートできますか?
VMware VM を Azure にレプリケートするときは、レプリケーションは継続的に行われます。

### <a name="can-i-retain-the-ip-address-on-failover"></a>フェールオーバーの際に IP アドレスを保持することはできますか?
はい、フェールオーバーの際に IP アドレスを保持することは可能です。 フェールオーバー前に [コンピューティングとネットワーク] ブレード上で対象となる IP アドレスを必ず指定してください。 また、フェールオーバー時には必ずコンピューターをシャットダウンして、フェールバック時の IP の競合を避けてください。

### <a name="can-i-extend-replication"></a>レプリケーションを拡張することはできますか?
拡張またはチェーン レプリケーションはサポートされていません。 [フィードバック フォーラム](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959)でこの機能を要求してください。

### <a name="can-i-do-an-offline-initial-replication"></a>オフラインの初期レプリケーションを行うことはできますか?
これはサポートされていません。 [フィードバック フォーラム](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)でこの機能を要求してください。

### <a name="can-i-exclude-disks"></a>ディスクを除外することはできますか?
はい、レプリケーションからディスクを除外できます。

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>ダイナミック ディスクを含む VM をレプリケートできますか?
ダイナミック ディスクをレプリケートすることができます。 オペレーティング システム ディスクはベーシック ディスクである必要があります。

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>マルチ VM 整合性用のレプリケーション グループを使用する場合、新しい VM を既存のレプリケーション グループに追加できますか?
はい、既存のレプリケーション グループのレプリケーションを有効にするときに、そのグループに新しい VM を追加できます。 レプリケーションが開始された後で既存のレプリケーション グループに VM を追加することはできません。既存の VM 用のレプリケーション グループを作成することもできません。

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>レプリケートされる VM を、ディスクの追加またはサイズ変更によって変更することはできますか?

Azure への VMware のレプリケーションでは、ディスクのサイズを変更できます。 新しいディスクを追加する場合は、ディスクを追加し、VM の保護を再度有効にする必要があります。

### <a name="can-i-migrate-on-prem-machines-to-a-new-vcenter-without-impacting-ongoing-replication"></a>進行中のレプリケーションに影響を与えることなく、オンプレミスのマシンを新しい Vcenter に移行することはできますか?
いいえ、Vcenter を変更したり、移行を行ったりすると、進行中のレプリケーションに影響が及びます。 新しい Vcenter を使用して ASR を設定し、マシンのレプリケーションを有効にする必要があります。

### <a name="can-i-replicate-to-cachetarget-storage-account-which-has-a-vnet-with-azure-storage-firewalls-configured-on-it"></a>(Azure Storage ファイアウォールを使用して) Vnet が構成されているキャッシュ/ターゲット ストレージ アカウントにレプリケートすることはできますか?
いいえ、Azure Site Recovery では、Vnet 上のストレージへのレプリケーションはサポートされていません。

## <a name="configuration-server"></a>構成サーバー

### <a name="what-does-the-configuration-server-do"></a>構成サーバーは何を行いますか?
構成サーバーは、次のようなオンプレミスの Site Recovery コンポーネントを実行します。
- 構成サーバー: オンプレミスと Azure の間の通信を調整し、データのレプリケーションを管理します。
- プロセス サーバー: レプリケーション ゲートウェイとして機能します。 レプリケーション データを受信し、そのデータをキャッシュ、圧縮、暗号化によって最適化して、Azure Storage に送信します。また、プロセス サーバーは、レプリケートする VM へのモビリティ サービスのインストールや、オンプレミスの VMware VM の自動検出も行います。
- マスター ターゲット サーバー: Azure からのフェールバック中にレプリケーション データを処理します。

構成サーバーのコンポーネントとプロセスの[詳細をご確認ください](vmware-azure-architecture.md)。

### <a name="where-do-i-set-up-the-configuration-server"></a>構成サーバーはどこに設定しますか?
構成サーバーには、高可用性のオンプレミス VMware VM が 1 つ必要です。

### <a name="what-are-the-requirements-for-the-configuration-server"></a>構成サーバーにはどのような要件がありますか?

前提条件については、[こちら](vmware-azure-deploy-configuration-server.md#prerequisites)をご覧ください。

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>テンプレートを使う代わりに、手動で構成サーバーを設定できますか?
最新バージョンの OVF テンプレートを使って、[構成サーバーの VM を作成する](vmware-azure-deploy-configuration-server.md)ことをお勧めします。 何らかの理由でそれができない場合は (VMware サーバーにアクセスできない場合など)、ポータルから[統合セットアップ ファイルをダウンロード](physical-azure-set-up-source.md)して、VM でそれを実行できます。

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>構成サーバーは複数のリージョンにレプリケートできますか?
いいえ。 これを行うには、リージョンごとに構成サーバーを設定する必要があります。

### <a name="can-i-host-a-configuration-server-in-azure"></a>Azure で構成サーバーをホストできますか?
可能ですが、構成サーバーを実行している Azure VM は、オンプレミスの VMware のインフラストラクチャや VM と通信する必要があります。 これで待機時間が長くなり、進行中のレプリケーションに影響する可能性があります。

### <a name="how-do-i-update-the-configuration-server"></a>構成サーバーはどのようにして更新できますか?
構成サーバーの更新の[詳細](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)をご覧ください。 最新の更新情報については、[Azure の更新情報のページ](https://azure.microsoft.com/updates/?product=site-recovery)をご覧ください。 構成サーバーの最新バージョンは、[Microsoft ダウンロード センター](https://aka.ms/asrconfigurationserver)から直接ダウンロードすることもできます。 お使いのバージョンが現在のバージョンより 5 以上古い場合は、[サポートに関する声明](https://aka.ms/asr_support_statement)でアップグレードのガイダンスについて参照してください。

### <a name="should-i-backup-the-deployed-configuration-server"></a>デプロイした構成サーバーをバックアップする必要はありますか?
構成サーバーの定期的なスケジュールされたバックアップを実行することをお勧めします。 フェールバックが成功するには、フェールバックされる仮想マシンが構成サーバー データベース内に存在し、構成サーバーが実行中で接続状態である必要があります。 構成サーバーの一般的な管理タスクの詳細については、[こちら](vmware-azure-manage-configuration-server.md)を参照してください。

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>構成サーバーを設定しているときに、MySQL を手動でダウンロードしてインストールできますか?
はい。 MySQL をダウンロードし、**C:\Temp\ASRSetup** フォルダーに配置します。 その後、手動でインストールします。 構成サーバー VM を設定し、ご契約条件に同意すると、MySQL は **[Download and install]\(ダウンロードとインストール\)** に **"インストール済み"** と表示されます。

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>MySQL をダウンロードせず、Site Recovery によってインストールすることはできますか?
はい。 MySQL インストーラーをダウンロードし、**C:\Temp\ASRSetup** フォルダーに配置します。  構成サーバーの VM を設定する際に、ご契約条件に同意し、**[Download and install]\(ダウンロードとインストール\)** をクリックすると、ポータルでは追加したインストーラーが MySQL のインストールに使用されます。
 
### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>構成サーバー VM を他の目的で使用することはできますか?
いいえ、VM は構成サーバーにのみ使用する必要があります。 

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>構成サーバーを複製してオーケストレーションに使用することはできますか?
いいえ、登録に関する問題を避けるために、新しい構成サーバーを設定する必要があります。

### <a name="can-i-change-the-vault-registered-in-the-configuration-server"></a>構成サーバーに登録されている資格情報コンテナーを変更することはできますか?
いいえ。 コンテナーは、構成サーバーに登録した後に変更することはできません。 再登録の手順については、[この記事](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault)を確認してください。

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>VMware VM と物理サーバーの両方のディザスター リカバリーに同じ構成サーバーを使用できますか?
はい。ただし、物理マシンは VMware VM にのみフェールバックできることにご注意ください。

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>構成サーバーのパスフレーズはどこでダウンロードできますか?
パスフレーズのダウンロードについては、[こちらの記事をご覧ください](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase)。

### <a name="where-can-i-download-vault-registration-keys"></a>コンテナー登録キーはどこでダウンロードできますか?

**[Recovery Services コンテナー]** で、**[管理]** > **[Site Recovery インフラストラクチャ]** > **[構成サーバー]** の順に移動します。 **[サーバー]** で **[登録キーのダウンロード]** を選択して、コンテナーの資格情報ファイルをダウンロードします。



## <a name="mobility-service"></a>モビリティ サービス

### <a name="where-can-i-find-the-mobility-service-installers"></a>モビリティ サービスのインストーラーはどこにありますか?
インストーラーは、構成サーバーの **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** フォルダーにあります。

## <a name="how-do-i-install-the-mobility-service"></a>モビリティ サービスはどのようにしてインストールしますか?
[プッシュ インストール](vmware-azure-install-mobility-service.md)を使うか、UI または PowerShell からの[手動インストール](vmware-physical-mobility-service-install-manual.md)を使って、レプリケートする各 VM にインストールします。 または、[System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md) などのデプロイ ツールを使って、デプロイすることもできます。



## <a name="security"></a>セキュリティ

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Site Recovery では、VMware サーバーに対してどのようなアクセスが必要ですか?
Site Recovery では、次のことを実行するために、VMware サーバーへのアクセスが必要です。

- 構成サーバーおよび他のオンプレミスの Site Recovery コンポーネントを実行する VMware VM を設定します。 [詳細情報](vmware-azure-deploy-configuration-server.md)
- レプリケーション対象の VM を自動的に検出します。 自動検出用のアカウントの準備方法を確認してください。 [詳細情報](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Site Recovery では、VMware VM に対してどのようなアクセスが必要ですか?

- レプリケートのためには、VMware VM に Site Recovery のモビリティ サービスがインストールされて実行していることが必要です。 ツールを手動でデプロイすることも、VM のレプリケーションを有効にするときにサービスのプッシュ インストールを行うよう Site Recovery に指定することもできます。 プッシュ インストールの場合、Site Recovery には、サービス コンポーネントのインストールに使用できるアカウントが必要です。 [詳細情報](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation)。 プロセス サーバー (構成サーバーで既定で実行しています) がこのインストールを実行します。 モビリティ サービスのインストールについて詳しくは、[こちら](vmware-azure-install-mobility-service.md)をご覧ください。
- レプリケーションの間に、VM は Site Recovery と次のように通信します。
    - VM は、レプリケーション管理のために、ポート HTTPS 443 で構成サーバーと通信します。
    - VM は、ポート HTTPS 9443 でレプリケーション データをプロセス サーバーに送信します (変更可能)。
    - マルチ VM 整合性を有効にすると、VM はポート 20004 で相互に通信します。


### <a name="is-replication-data-sent-to-site-recovery"></a>レプリケーション データは Site Recovery に送信されますか?
いいえ。Site Recovery は、レプリケートされたデータをインターセプトすることも、VM での実行内容に関するどのような情報を持つこともありません。 レプリケーション データは、VMware ハイパーバイザーと Azure ストレージの間で交換されます。 Site Recovery には、これらのデータをインターセプトする能力はありません。 レプリケーションとフェールオーバーを調整するために必要なメタデータのみが、Site Recovery サービスに送信されます。  

Site Recovery は ISO 27001:2013、27018、HIPAA、DPA の認証を受けており、SOC2 および FedRAMP JAB の評価が進行中です。

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>オンプレミスのメタデータを地理的領域内に保つことができますか?
はい。 リージョンにコンテナーを作成するとき、Site Recovery によって使われるすべてのメタデータは、そのリージョンの地理的な境界内に維持されます。

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery はレプリケーションを暗号化しますか。
はい、転送中の暗号化と [Azure での暗号化](https://docs.microsoft.com/azure/storage/storage-service-encryption)の両方がサポートされています。


## <a name="failover-and-failback"></a>フェールオーバーとフェールバック
### <a name="how-far-back-can-i-recover"></a>過去のどの時点まで遡って復旧できますか?
VMware から Azure の場合、使うことができる最も古い復旧ポイントは 72 時間です。

### <a name="how-do-i-access-azure-vms-after-failover"></a>フェールオーバー後にはどのようにして Azure VM にアクセスできますか?
フェールオーバー後、Azure VM には、セキュリティで保護されたインターネット接続、サイト間 VPN、または Azure ExpressRoute 経由でアクセスできます。 接続するにはさまざまこと準備する必要があります。 [詳細情報](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>フェールオーバーされたデータに回復力はありますか?
Azure は復元するように設計されています。 Site Recovery は、Azure SLA に従ってセカンダリ Azure データセンターにフェールオーバーする機能を備えています。 フェールオーバーが発生した場合、お客様のメタデータとコンテナーは、お客様が選択したコンテナーと同じリージョン内に保持されます。

### <a name="is-failover-automatic"></a>フェールオーバーは自動で行われますか。
[フェールオーバー](site-recovery-failover.md)は自動では行われません。 ポータルで 1 回クリックするだけでフェールオーバーを開始できます。または [PowerShell](/powershell/module/azurerm.siterecovery) を使ってフェールオーバーをトリガーすることもできます。

### <a name="can-i-fail-back-to-a-different-location"></a>異なる場所にフェールバックすることはできますか?
はい、Azure にフェールオーバーした場合、元の場所が利用できないときは、別の場所にフェールバックすることができます。 [詳細情報](concepts-types-of-failback.md#alternate-location-recovery-alr)。

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>フェールバックに VPN または ExpressRoute が必要なのはなぜですか?
Azure からフェールバックするときは、Azure からオンプレミスの VM にデータがコピーされるので、プライベート アクセスが必要です。

### <a name="can-i-resize-the-azure-vm-after-failover"></a>フェールオーバー後に Azure VM をサイズ変更することはできますか?
いいえ、フェールオーバー後にターゲット VM のサイズや種類を変更することはできません。


## <a name="automation-and-scripting"></a>自動化とスクリプト

### <a name="can-i-set-up-replication-with-scripting"></a>スクリプトでレプリケーションを設定できますか?
はい。 Rest API、PowerShell、Azure SDK のいずれかを使って、Site Recovery ワークフローを自動化することができます。[詳細情報](vmware-azure-disaster-recovery-powershell.md)

## <a name="performance-and-capacity"></a>パフォーマンスと容量
### <a name="can-i-throttle-replication-bandwidth"></a>レプリケーションの帯域幅を調整することができますか?
はい。 [詳細情報](site-recovery-plan-capacity-vmware.md)。


## <a name="next-steps"></a>次の手順
* サポート要件を[確認する](vmware-physical-azure-support-matrix.md)。
* VMware から Azure へのレプリケーションを[設定する](vmware-azure-tutorial.md)。
