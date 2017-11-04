---
title: "SAP NetWeaver HA instalace v clusteru převzetí služeb při selhání se systémem Windows a soubor sdílet pro SAP (A) SCS instanci v Azure | Microsoft Docs"
description: "SAP NetWeaver HA instalace v systému Windows převzetí služeb při selhání clusteru a sdílené složky SCS Instance SAP (A)"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 71296618-673b-4093-ab17-b7a80df6e9ac
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ec7888cfb9b0d288b5b25f580c852ee32306684c
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="sap-netweaver-ha-installation-on-windows-failover-cluster-and-file-share-for-sap-ascs-instance-on-azure"></a>SAP NetWeaver HA instalace v systému Windows převzetí služeb při selhání clusteru a sdílené složky SCS Instance SAP (A) v Azure

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1596496]:https://launchpad.support.sap.com/#/notes/1596496

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[s2d-in-win-2016]:https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[sofs-overview]:https://technet.microsoft.com/library/hh831349(v=ws.11).aspx
[new-in-win-2016-storage]:https://docs.microsoft.com/windows-server/storage/whats-new-in-storage

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[sap-blog-new-sap-cluster-resource-dll]:https://blogs.sap.com/2017/08/28/new-sap-cluster-resource-dll-is-available/
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host]:sap-high-availability-installation-wsfc-shared-disk.md#a97ad604-9094-44fe-a364-f89cb39bf097
[sap-high-availability-installation-wsfc-shared-disk-add-probe-port]:sap-high-availability-installation-wsfc-shared-disk.md#10822f4f-32e7-4871-b63a-9b86c76ce761

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-shared-disk-vpn]:sap-high-availability-infrastructure-wsfc-shared-disk.md#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-high-availability-infrastructure-wsfc-shared-disk-change-def-ilb]:sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-wsfc]:sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-high-availability-infrastructure-wsfc-shared-disk-collect-cluster-config]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-high-availability-infrastructure-wsfc-shared-disk-add-dot-net]:sap-high-availability-infrastructure-wsfc-shared-disk.md#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-high-availability-infrastructure-wsfc-shared-disk-install-sios-both-nodes]:sap-high-availability-infrastructure-wsfc-shared-disk.md#dd41d5a2-8083-415b-9878-839652812102
[sap-high-availability-infrastructure-wsfc-shared-disk-setup-sios]:sap-high-availability-infrastructure-wsfc-shared-disk.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006

[sap-official-ha-file-share-document]:https://www.sap.com/documents/2017/07/f453332f-c97c-0010-82c7-eda71af511fa.html

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)


[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/1002-wsfc-sios-on-azure-ilb.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png


[sap-ha-guide-figure-8001]:./media/virtual-machines-shared-sap-high-availability-guide/8001.png
[sap-ha-guide-figure-8002]:./media/virtual-machines-shared-sap-high-availability-guide/8002.png
[sap-ha-guide-figure-8003]:./media/virtual-machines-shared-sap-high-availability-guide/8003.png
[sap-ha-guide-figure-8004]:./media/virtual-machines-shared-sap-high-availability-guide/8004.png
[sap-ha-guide-figure-8005]:./media/virtual-machines-shared-sap-high-availability-guide/8005.png
[sap-ha-guide-figure-8006]:./media/virtual-machines-shared-sap-high-availability-guide/8006.png
[sap-ha-guide-figure-8007]:./media/virtual-machines-shared-sap-high-availability-guide/8007.png
[sap-ha-guide-figure-8008]:./media/virtual-machines-shared-sap-high-availability-guide/8008.png
[sap-ha-guide-figure-8009]:./media/virtual-machines-shared-sap-high-availability-guide/8009.png
[sap-ha-guide-figure-8010]:./media/virtual-machines-shared-sap-high-availability-guide/8010.png
[sap-ha-guide-figure-8011]:./media/virtual-machines-shared-sap-high-availability-guide/8011.png
[sap-ha-guide-figure-8012]:./media/virtual-machines-shared-sap-high-availability-guide/8012.png
[sap-ha-guide-figure-8013]:./media/virtual-machines-shared-sap-high-availability-guide/8013.png
[sap-ha-guide-figure-8014]:./media/virtual-machines-shared-sap-high-availability-guide/8014.png
[sap-ha-guide-figure-8015]:./media/virtual-machines-shared-sap-high-availability-guide/8015.png
[sap-ha-guide-figure-8016]:./media/virtual-machines-shared-sap-high-availability-guide/8016.png
[sap-ha-guide-figure-8017]:./media/virtual-machines-shared-sap-high-availability-guide/8017.png
[sap-ha-guide-figure-8018]:./media/virtual-machines-shared-sap-high-availability-guide/8018.png
[sap-ha-guide-figure-8019]:./media/virtual-machines-shared-sap-high-availability-guide/8019.png
[sap-ha-guide-figure-8020]:./media/virtual-machines-shared-sap-high-availability-guide/8020.png
[sap-ha-guide-figure-8021]:./media/virtual-machines-shared-sap-high-availability-guide/8021.png
[sap-ha-guide-figure-8022]:./media/virtual-machines-shared-sap-high-availability-guide/8022.png
[sap-ha-guide-figure-8023]:./media/virtual-machines-shared-sap-high-availability-guide/8023.png
[sap-ha-guide-figure-8024]:./media/virtual-machines-shared-sap-high-availability-guide/8024.png
[sap-ha-guide-figure-8025]:./media/virtual-machines-shared-sap-high-availability-guide/8025.png

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

Tento dokument je popisující, jak nainstalovat a nakonfigurovat vysokou dostupné systémové SAP v Azure, s **Windows Failover Cluster (WSFC)** a **sdílené složky se Škálováním** jako možnost pro clustering SAP (A) SCS instance.

## <a name="prerequisites"></a>Požadavky

Ujistěte se, že jste si tyto dokumenty, než začnete s instalací:

* [Průvodce architekturou – Clustering SAP (A) SCS Instance na **clusteru převzetí služeb při selhání se systémem Windows** pomocí **sdílení souborů**][sap-high-availability-guide-wsfc-file-share]

* [Azure Příprava infrastruktury pro používání SAP HA **clusteru převzetí služeb při selhání se systémem Windows** a **soubor sdílený** pro instanci SCS SAP (A)][sap-high-availability-infrastructure-wsfc-file-share]


Budete potřebovat následující spustitelné soubory nebo knihovny DLL z SAP:
* SAP **správce zřizování softwaru** (**SWPM**) verze nástroje instalace **SPS21 (nebo vyšší)**.
* Stažení **nejnovější NTCLUST. Zvláštní správní oblast ČLR** archivu s novou SAP DLL prostředků clusteru. Nové knihovny DLL SAP clusteru podporuje vysokou dostupnost SCS SAP (A) s sdílenou složku na clusteru pro převzetí služeb při selhání systému Windows Server.

  Další založení na nový SAP prostředek clusteru knihovnu DLL, vyhledejte v tomto blogu: [je k dispozici nové SAP prostředek clusteru knihovnu DLL!][sap-blog-new-sap-cluster-resource-dll]

Instalace databázového systému jsme popisují není nastavení lišit v závislosti na tom, které můžete použít systém databázového systému. Ale předpokládáme, že jsou aspekty vysoké dostupnosti s databázového systému řešit pomocí funkce, které různých výrobců databázového systému podpora pro Azure. Například Always On nebo zrcadlení databáze systému SQL Server a Oracle Data Guard pro databáze Oracle. Ve scénáři, které používáme v tomto článku jsme nebyla přidat další ochranu do databázového systému.

Když různé služby databázového systému interakci s tímto typem Clusterované konfigurace SAP ASC nebo SCS v Azure neexistují žádné zvláštní požadavky.

> [!NOTE]
> Postupy instalace systémů SAP NetWeaver ABAP, Java systémy a systémy ABAP + Java jsou téměř shodné. Nejdůležitější rozdíl je, že systému SAP ABAP má jednu instanci ASC. V systému SAP Java má jednu instanci SCS. V systému SAP ABAP + Java obsahuje jednu instanci ASC a jednu instanci SCS spuštěné ve stejné skupině clusteru převzetí služeb při selhání Microsoft. Případné rozdíly instalace pro každou SAP NetWeaver instalace zásobníku se výslovně uveden. Můžete předpokládat, že všechny ostatní části jsou stejné.  
>
>

## <a name="install-ascs-instance-on-ascs-cluster"></a>Nainstalovat (A) SCS instanci v clusteru (A) SCS

> [!IMPORTANT]
>
>V současné době nepodporuje nastavení HA s konfigurací sdílené složky souboru SAP nástroj pro instalaci softwaru zřizování Manager (SWPM). Proto je nutné pro instalaci systému SAP, například, instalace a cluster instance SCS SAP (A) a konfigurace samostatné SAP GLOBALHOST některé ruční přijetí.  
>
>Neexistuje žádná změna v další kroky instalace a instalace (a cluster) instance databázového systému SAP aplikačních serverů.
>

### <a name="install-ascs-instance-on-local-drive"></a>Instance SCS instalace (A) na místní jednotce

Instalace SAP (A) SCS instance na **i** uzly clusteru (A) SCS. Nainstalujte na **místní** jednotky. V našem příkladu jsme zvolili, je místní disk C:\\. Můžete použít jakékoli místní jednotce.  

Chcete-li nainstalovat přejděte v instalaci SAP nástroj SWPM na:

&lt;Produkt&gt; -> &lt;databázového systému&gt; -> instalace -> aplikace Server ABAP (nebo Java) -> distribuované systém -> (A) SCS Instance

> [!IMPORTANT]
>V současné době scénář sdílené složky souboru ještě není podporovaný nástroj pro instalaci SAP SWPM **nelze použít** Instalační cesta:
>
>&lt;Produkt&gt; -> &lt;databázového systému&gt; -> instalace -> aplikace Server ABAP (nebo Java) -> vysokou dostupnost systém ->...
>

### <a name="remove-sapmnt-and-create-saploc-file-share"></a>Odeberte SAPMNT a vytvoření SAPLOC sdílené složky

SWMP vytvořit SAPMNT místní sdílené složky na jednotce C:\\usr\\sap složky.

Odebrat sdílenou složku souborů SAPMNT na **i** (A) SCS uzly clusteru:

Spusťte následující skript prostředí PowerShell:

```PowerShell
Remove-SmbShare sapmnt -ScopeName * -Force
 ```

Pokud SAPLOC sdílená složka neexistuje, vytvořte ji v uzlech clusteru jak ASC.

Spusťte následující skript prostředí PowerShell:

```PowerShell
#Create SAPLOC share and set security
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"
$HostName = $env:computername
$SAPLocalAdminGroupName = "$HostName\SAP_LocalAdmin"
$SAPDisk = "C:"
$SAPusrSapPath = "$SAPDisk\usr\sap"

New-SmbShare -Name saploc -Path c:\usr\sap -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName , $SAPLocalAdminGroupName  
 ```

## <a name="prepare-sap-global-host-on-sofs-cluster"></a>Příprava SAP globální hostitele v clusteru serveru SOFS

V tomto kroku vytvoření následující složky svazku a soubor, na serveru SOFS clusteru:

* Soubor SAP GLOBALHOST C:\ClusterStorage\Volume1\usr\sap\\&lt;SID&gt;\SYS\ strukturu na serveru SOFS clusteru sdílený svazek (CSV)

* Vytvoření sdílené složky SAPMNT

* Nastavení zabezpečení na SAPMNT sdílené složky a složky s úplným řízením pro:
    * **&lt;DOMÉNY&gt;\SAP_&lt;SID&gt;_GlobalAdmin** skupiny uživatelů
    * SAP (A) počítače uzly clusteru SCS **objekty &lt;domény&gt;\ClusterNode1$ a &lt;domény&gt;\ClusterNode2$**

K vytvoření Sdíleného svazku clusteru s zrcadlové svazky, jak jsou definovány v kapitole **SAP požadavky pro SOFS ve službě Azure - přidat odkaz**, provést následující rutiny prostředí PowerShell na jednom z uzlů clusteru SOFS:


```PowerShell
New-Volume -StoragePoolFriendlyName S2D* -FriendlyName SAPPR1 -FileSystem CSVFS_ReFS -Size 5GB -ResiliencySettingName Mirror
```
Pokud chcete vytvořit SAPMNT a nastavit zabezpečení složky a sdílení, spusťte následující skript prostředí PowerShell na jednom z uzlů clusteru SOFS:

```PowerShell
# Create SAPMNT on file share
$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# SAP (A)SCS cluster nodes
$ASCSClusterNode1 = "ascs-1"
$ASCSClusterNode2 = "ascs-2"

# Define SAP (A)SCS cluster node computer objects
$ASCSClusterObjectNode1 = "$DomainName\$ASCSClusterNode1$"
$ASCSClusterObjectNode2 = "$DomainName\$ASCSClusterNode2$"

# Create usr\sap\.. folders on CSV
$SAPGlobalFolder = "C:\ClusterStorage\Volume1\usr\sap\$SAPSID\SYS"
New-Item -Path $SAPGlobalFOlder -ItemType Directory

$UsrSAPFolder = "C:\ClusterStorage\Volume1\usr\sap\"

# Create SAPMNT file share and set share security
New-SmbShare -Name sapmnt -Path $UsrSAPFolder -FullAccess "BUILTIN\Administrators", $SAPSIDGlobalAdminGroupName, $ASCSClusterObjectNode1, $ASCSClusterObjectNode2 -ContinuouslyAvailable $false -CachingMode None -Verbose

# Get SAPMNT file share security settings
Get-SmbShareAccess sapmnt

# Set files & folder security
$Acl = Get-Acl $UsrSAPFolder

# Add file security object of SAP_<sid>_GlobalAdmin group
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($SAPSIDGlobalAdminGroupName,"FullControl", 'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode1$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode1,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Add security object of clusternode2$ computer object
$Ar = New-Object  system.security.accesscontrol.filesystemaccessrule($ASCSClusterObjectNode2,"FullControl",'ContainerInherit,ObjectInherit', 'None', 'Allow')
$Acl.SetAccessRule($Ar)

# Set security
Set-Acl $UsrSAPFolder $Acl -Verbose
 ```
## <a name="stop-ascs-instances-and-sap-services"></a>Instance SCS zastavení (A) a služby SAP

Provést následující kroky:
* Zastavit instancí SCS SAP (A) na obou (A) uzlů clusteru SCS
* Zastavení služeb Windows SCS SAP (A) **SAP&lt;SID&gt;_&lt;InstanceNumber&gt;**  na obou uzlů clusteru

## <a name="move-sys-folder-to-sofs-cluster"></a>Přesunout \SYS\.... Složka, v níž SOFS clusteru

Provést následující kroky:
* Kopírovat SYS složky (například C:\usr\sap\\&lt;SID&gt;\SYS) z jednoho z SCS (A) uzly clusteru na clusteru SOFS například C:\ClusterStorage\Volume1\usr\sap\\&lt;SID&gt;\SYS
* Odstranit C:\usr\sap\\&lt;SID&gt;\SYS složky z obou uzlů clusteru (A) SCS

## <a name="update-cluster-security-setting-on-sap-ascs-cluster"></a>Aktualizovat nastavení zabezpečení clusteru v clusteru SCS SAP (A)

Spusťte následující skript prostředí PowerShell na jednom z uzlů clusteru SCS SAP (A):

```PowerShell
# Grant <DOMAIN>\SAP_<SID>_GlobalAdmin group access to cluster

$SAPSID = "PR1"
$DomainName = "SAPCLUSTER"
$SAPSIDGlobalAdminGroupName = "$DomainName\SAP_" + $SAPSID + "_GlobalAdmin"

# Set full access for <DOMAIn>\SAP_<SID>_GlobalAdmin group
Grant-ClusterAccess -User $SAPSIDGlobalAdminGroupName -Full

#Check security settings
Get-ClusterAccess
```

## <a name="create-a-virtual-host-name-for-the-clustered-sap-ascs-instance"></a>Vytvořte název virtuálního hostitele pro instanci SCS Clusterové SAP (A)

Jak je popsáno v kapitole [vytvořte název virtuálního hostitele pro skupinu prostředků clusteru SAP ASC nebo SCS] [ sap-high-availability-installation-wsfc-shared-disk-create-ascs-virt-host] , například vytvořit síťový název clusteru SCS SAP (A) **pr1-ASC [10.0.6.7]**

## <a name="update-default-and-sap-ascs-instance-profile"></a>Aktualizujte výchozí a SAP (A) SCS Instance profilu

Výchozí hodnota je nutné aktualizovat a SAP (A) SCS instanci profil &lt;SID&gt;_(A) SCS<Nr>_  <Host> používat:

* Nový název virtuálního hostitele SCS SAP (A)

* Nový název hostitele globální SAP


| Původní hodnoty |  |
| --- | --- |
| Název hostitele (A) SCS SAP = globální hostitele SAP | ASC-1 |
| Název profilu (A) SCS Instance SAP | PR1_ASCS00_ascs-1 |

| Nové hodnoty |  |
| --- | --- |
| Název hostitele (A) SCS SAP | **PR1 ASC** |
| Globální hostitele SAP | **sapglobal** |
| Název profilu (A) SCS Instance SAP | PR1\_ASCS00\_**pr1 ASC** |

### <a name="update-sap-default-profile"></a>SAP výchozí profil aktualizovat.


| Název parametru | Hodnota parametru |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| rdisp/mshost | **PR1 ASC** |
| enque/serverhost | **PR1 ASC** |

### <a name="update-sap-ascs-instance-profile"></a>Aktualizovat profil Instance SCS SAP (A)

| Název parametru | Hodnota parametru |
| --- | --- |
| SAPGLOBALHOST | **sapglobal** |
| DIR_PROFILE | \\\\**sapglobal**\sapmnt\PR1\SYS\profile |
| _PF | $(DIR_PROFILE) \PR1\_ASCS00_ pr1-ASC |
| Restart_Program_02 = local$(_MS) pf=$(_PF) | **Spustit**_Program_02 = local$(_MS) pf=$(_PF) |
| SAPLOCALHOST | **PR1 ASC** |
| Restart_Program_03 = local$(_EN) pf=$(_PF) | **Spustit**_Program_03 = local$(_EN) pf=$(_PF) |
| GW/netstat_once | **0** |
| enque/encni/set_so_keepalive  | **Hodnota TRUE** |
| Služba nebo ha_check_node | **1** |

> [!IMPORTANT]
>Můžete použít **aktualizace SAPASCSSCSProfile** rutiny prostředí PowerShell k automatizaci aktualizace profilu
>
>Rutiny prostředí PowerShell podporuje SAP ABAP ASC a SAP Java SCS instance.
>

Kopírování **SAPScripts.ps1** do místní jednotky C:\tmp a spusťte následující rutiny prostředí PowerShell:

```PowerShell
Import-Module C:\tmp\SAPScripts.ps1

Update-SAPASCSSCSProfile -PathToAscsScsInstanceProfile \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_ascs-1 -NewASCSHostName pr1-ascs -NewSAPGlobalHostName sapglobal -Verbose  
```

![Obrázek 1: SAPScripts.ps1 výstup][sap-ha-guide-figure-8012]

_**Obrázek 1:** SAPScripts.ps1 výstup_

## <a name="update-ltsidgtadm-user-environment-variable"></a>Aktualizace &lt;sid&gt;adm proměnné prostředí uživatele

Aktualizace &lt;sid&gt;adm uživatelské prostředí nové GLOBALHOST cestu UNC na uzlech clusteru SCS oba (A).
Přihlaste se jako &lt;sid&gt;adm uživatele a spusťte nástroj Regedit.exe.
Přejděte na **HKEY_CURRENT_USER** -> **prostředí** a aktualizovat na novou hodnotu proměnné:

| Proměnná | Hodnota |
| --- | --- |
| RSEC_SSFS_DATAPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\data |
| RSEC_SSFS_KEYPATH | \\\\**sapglobal**\sapmnt\PR1\SYS\global\security\rsecssfs\key |
| SAPEXE | \\\\**sapglobal**\sapmnt\PR1\SYS\exe\uc\NTAMD64 |
| SAPLOCALHOST  | **PR1 ASC** |


## <a name="install-new-saprcdll"></a>Nainstalujte nový SAPRC. KNIHOVNY DLL

Musíte nainstalovat novou verzi SAP prostředek clusteru, který podporuje scénář sdílené složky souborů.

Stáhněte si nejnovější **NTCLUST. Zvláštní správní oblast ČLR** balíček z SAP služby Marketplace.

Rozbalte NTCLUS. Zvláštní správní oblast na jednom z SCS (A) uzly clusteru a spusťte následující příkaz z příkazového řádku pro instalaci nové saprc.dll:

```
.\NTCLUST\insaprct.exe -yes -install
```

Nové saprc.dll bude nainstalována na obou (A) SCS uzlů clusteru.

Další informace najdete v tématu [1596496 Poznámka SAP – jak se bude aktualizovat SAP prostředků typu knihovny DLL pro sledování prostředků clusteru][1596496].

## <a name="create-sap-sid-cluster-group-network-name-and-ip"></a>Vytvoření SAP <SID> skupina clusteru, název sítě a IP

Je třeba vytvořit:

* SAP &lt;SID&gt; skupiny clusteru
* < (a) SCSNetworkName >
* a odpovídající IP adresy

Spusťte následující rutiny prostředí PowerShell:

```PowerShell
# Create SAP Cluster Group
$SAPSID = "PR1"
$SAPClusterGroupName = "SAP $SAPSID"
$SAPIPClusterResourceName = "SAP $SAPSID IP"
$SAPASCSNetworkName = "pr1-ascs"
$SAPASCSIPAddress = "10.0.6.7"
$SAPASCSSubnetMask = "255.255.255.0"

# Create SAP ASCS instance Virtual IP cluster resource
Add-ClusterGroup -Name $SAPClusterGroupName -Verbose

#Create SAP ASCS Virtual IP Address
$SAPIPClusterResource = Add-ClusterResource -Name $SAPIPClusterResourceName -ResourceType "IP Address" -Group $SAPClusterGroupName -Verbose

# Set static IP Address
$param1 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,Address,$SAPASCSIPAddress
$param2 = New-Object Microsoft.FailoverClusters.PowerShell.ClusterParameter $SAPIPClusterResource,SubnetMask,$SAPASCSSubnetMask
$params = $param1,$param2
$params | Set-ClusterParameter

# Create corresponding network name
$SAPNetworkNameClusterResourceName = $SAPASCSNetworkName
Add-ClusterResource -Name $SAPNetworkNameClusterResourceName -ResourceType "Network Name" -Group $SAPClusterGroupName -Verbose

# Set Network DNS Name
$SAPNetworkNameClusterResource = Get-ClusterResource $SAPNetworkNameClusterResourceName
$SAPNetworkNameClusterResource | Set-ClusterParameter -Name Name -Value $SAPASCSNetworkName

#Check the updated values
$SAPNetworkNameClusterResource | Get-ClusterParameter

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPNetworkNameClusterResourceName -Dependency "[$SAPIPClusterResourceName]" -Verbose

#Start SAP <SID> Cluster Group
Start-ClusterGroup -Name $SAPClusterGroupName -Verbose
```

## <a name="register-sap-start-service-on-both-nodes"></a>Zaregistrovat SAP START Service v obou uzlech

Budete muset znovu zaregistrovat službu sapstart SAP (A) SCS pinta nový profil a cesta k profilu.

Je třeba spustit svůj na uzlech clusteru SCS oba (A).

Spuštění formuláře zvýšenými oprávněními příkaz výzvy následující příkaz:

```
C:\usr\sap\PR1\ASCS00\exe\sapstartsrv.exe -r -p \\sapglobal\sapmnt\PR1\SYS\profile\PR1_ASCS00_pr1-ascs -s PR1 -n 00 -U SAPCLUSTER\SAPServicePR1 -P mypasswd12 -e SAPCLUSTER\pr1adm
```

![Obrázek 2: Opětovná instalace služby SAP][sap-ha-guide-figure-8013]

_**Obrázek 2:** přeinstalovat SAP služby_

Zajišťuje, že parametry jsou správné a zvolte **ruční** jako typ spuštění.

## <a name="stop-ascs-service"></a>Zastavení (A) SCS služby

Zastavit službu SCS SAP (A) **SAP&lt;SID&gt;_ &lt;InstanceNumber&gt;**  na obou (A) SCS uzly clusteru.

## <a name="create-new-sap-service-and-sap-instance-resources"></a>Vytvořit novou službu SAP a SAP Instance prostředky

Teď musíte dokončit vytváření prostředků SAP SAP&lt;SID&gt; skupina clusteru, například je nutné vytvořit prostředky:

* **SAP &lt;SID&gt; &lt;InstanceNumber&gt; služby** a
* **SAP &lt;SID&gt; &lt;InstanceNumber&gt; Instance**

Spuštěním následující rutiny prostředí PowerShell:

```PowerShell
$SAPSID = "PR1"
$SAPInstanceNumber = "00"
$SAPNetworkNameClusterResourceName = "pr1-ascs"

$SAPServiceName = "SAP$SAPSID"+ "_" + $SAPInstanceNumber

$SAPClusterGroupName = "SAP $SAPSID"
$SAPServiceClusterResourceName = "SAP $SAPSID $SAPInstanceNumber Service"

$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPServiceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Service" -SeparateMonitor -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name ServiceName -Value $SAPServiceName

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPNetworkNameClusterResourceName]" -Verbose

$SAPInstanceClusterResourceName = "SAP $SAPSID $SAPInstanceNumber Instance"

# Create SAP Instance cluster resource
$SAPASCSServiceClusterResource = Add-ClusterResource -Name $SAPInstanceClusterResourceName -Group $SAPClusterGroupName -ResourceType "SAP Resource" -SeparateMonitor -Verbose

#Set SAP Instance cluster resource parameters
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystemName -Value $SAPSID -Verbose
$SAPASCSServiceClusterResource  | Set-ClusterParameter  -Name SAPSystem -Value $SAPInstanceNumber -Verbose

#Set resource dependencies
Set-ClusterResourceDependency -Resource $SAPASCSServiceClusterResource  -Dependency "[$SAPServiceClusterResourceName]" -Verbose
```

## <a name="add-a-probe-port"></a>Přidat Port testu

V tomto kroku nakonfigurujete prostředek clusteru SAP port testu SAP. SID IP pomocí prostředí PowerShell. Spustit tuto konfiguraci v jednom z uzlů clusteru SAP ASC nebo SCS, jak je popsáno [sem][sap-high-availability-installation-wsfc-shared-disk-add-probe-port].

## <a name="install-ers-instance-on-both-cluster-nodes"></a>Nainstalovat instanci YBRAT obou uzlů clusteru

V dalším kroku, je třeba nainstalovat instanci YBRAT (zařadit replikace Server) na obou uzlech (A) SCS clusteru.
Možnost instalace najdete v nabídce SWPM:

&lt;Produkt&gt; -> &lt;databázového systému&gt; -> instalace -> Další SAP systém -> instance **Instance serveru replikace zařazování**

## <a name="install-dbms-instance-and-sap-application-servers"></a>Instalace databázového systému Instance a aplikační servery SAP

Dokončení instalace systému SAP nainstalováním:
* Instance databázového systému
* Primární server aplikace SAP
* Další aplikačního serveru SAP

## <a name="next-steps"></a>Další kroky

* [Instalace instance SCS (A) v clusteru s podporou převzetí služeb při selhání bez sdílené disky – oficiální SAP pokyny pro sdílenou složku HA][sap-official-ha-file-share-document]:

* [Prostory úložiště – přímé v systému Windows Server 2016][s2d-in-win-2016]

* [Škálovaný souborový Server pro přehled dat aplikací][sofs-overview]

* [Co je nového v úložiště v systému Windows Server 2016][new-in-win-2016-storage]
