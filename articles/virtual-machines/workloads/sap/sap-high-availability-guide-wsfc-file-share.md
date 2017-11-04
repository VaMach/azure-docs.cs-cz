---
title: "Clustering SAP (A) SCS instanci v clusteru převzetí služeb při selhání systému Windows používá sdílenou složku v Azure | Microsoft Docs"
description: "Clustering SCS Instance SAP (A) v clusteru převzetí služeb při selhání systému Windows používá sdílenou složku"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 94d725cfb072091e57c96d3b2aca7b2e73657eef
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2492395]:https://launchpad.support.sap.com/#/notes/2492395

[kb4025334]:https://support.microsoft.com/help/4025334/windows-10-update-kb4025334

[dv2-series]:https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dv2-series
[ds-series]:https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#ds-series

[sap-installation-guides]:http://service.sap.com/instguides

[azure-subscription-service-limits]:../../../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../../../azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md

[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-guide-wsfc-shared-disk]:sap-high-availability-guide-wsfc-shared-disk.md
[sap-high-availability-guide-wsfc-file-share]:sap-high-availability-guide-wsfc-file-share.md
[sap-ascs-high-availability-multi-sid-wsfc]:sap-ascs-high-availability-multi-sid-wsfc.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-infrastructure-wsfc-file-share]:sap-high-availability-infrastructure-wsfc-file-share.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-volumes-s2d-choosing-filesystem]:https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes#choosing-the-filesystem
[choosing-the-size-of-volumes-s2d]:https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes#choosing-the-size-of-volumes
[deploy-sofs-s2d-in-azure]:https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment
[s2d-in-win-2016]:https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview
[deep-dive-volumes-in-s2d]:https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (SAP multi-SID high-availability configuration)

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

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


# <a name="clustering-sap-ascs-instance-on-windows-failover-cluster-using-file-share-on-azure"></a>Clustering SCS Instance SAP (A) v clusteru převzetí služeb při selhání systému Windows používá sdílenou složku v Azure

> ![Windows][Logo_Windows] Windows
>

Windows Server Failover Clustering je základem SAP ASC nebo SCS instalace vysokou dostupnost a databázového systému v systému Windows.

Cluster s podporou převzetí služeb při selhání je skupina 1 + n nezávislých serverů (uzlů) které vzájemně spolupracují a zvyšují tak dostupnost aplikací a služeb. Pokud dojde k selhání uzlu, Windows Server Failover Clustering vypočítá počet chyb, ke kterým dochází při zachování pořádku cluster, který poskytuje aplikacím a službám. Můžete z různých kvora režimy k dosažení clustering převzetí služeb při selhání.

## <a name="prerequisite"></a>Požadavek
Ujistěte se, že jste si tyto dokumenty, než začnete s tímto dokumentem:

* [Scénáře pro SAP NetWeaver a architektura vysoké dostupnosti virtuálních počítačů Azure][sap-high-availability-architecture-scenarios]

> [!IMPORTANT]
>Clustering instancí SCS SAP (A) s sdílené složky je podporována pro **SAP NetWeaver 7.40 (a vyšší)** produkty, s **SAP jádra 7.49 (a vyšší)**.
>


## <a name="windows-server-failover-clustering-in-azure"></a>Windows Server Failover Clustering v Azure

Porovnání s úplné nasazení systému nebo privátního cloudu, virtuálních počítačů Azure vyžaduje další kroky konfigurace, Windows Server Failover Clustering. Když vytvoříte cluster, budete muset nastavit několik IP adresy a názvy virtuálních hostitelů pro SAP ASC nebo SCS instanci.

### <a name="name-resolution-in-azure-and-cluster-virtual-host-name"></a>Rozpoznání názvu v Azure a virtuální název hostitele clusteru

Azure Cloudová platforma nenabízí možnost konfigurovat virtuální IP adresy, například plovoucí IP adresy. Je třeba nastavit virtuální IP adresy k dosažení prostředek clusteru v cloudu alternativní řešení. Azure má **nástroj pro vyrovnávání zatížení pro vnitřní** ve službě Vyrovnávání zatížení Azure. S nástrojem pro vyrovnávání zatížení pro vnitřní klienti moci clusteru připojit přes virtuální IP adresu clusteru. Je třeba nasadit nástroj pro vyrovnávání zatížení interní ve skupině prostředků, který obsahuje uzly clusteru. Potom nakonfigurujte všechny nezbytné port předávání pravidla s sondy porty nástroje pro vyrovnávání zatížení interní. Můžete se klienti připojují prostřednictvím název virtuálního hostitele. DNS server přeloží IP adresu clusteru a port obslužných rutin vyrovnávání interní služby load předávání do aktivního uzlu clusteru.

![Obrázek 1: Windows Server Failover Clustering konfiguraci v Azure bez sdíleného disku][sap-ha-guide-figure-1001]

_**Obrázek 1:** konfiguraci služby Windows Server Failover Clustering v Azure bez sdíleného disku_

## <a name="sap-ascs-ha-with-file-share"></a>SAP (A) SCS HA s sdílené složky

SAP vyvinuté nový přístup a alternativa k cluster sdílených disků, do instance SAP (A) SCS clusteru v clusteru převzetí služeb při selhání se systémem Windows.

Tady se používá **sdílenou složku SMB** je možnost nasazovat **SAP globální HOSTITELSKÝCH souborů**.

> [!NOTE]
>Sdílené složky SMB je další možnost pro cluster sdílených disků pro clustering instancí SCS SAP (A).  
>

Co je specifický pro tato architektura je následující:

* **SAP centrální služby (s procesy struktura a zpráv a zařadit vlastní soubor), jsou odděleni od SAP globální hostitelských souborů**
* **SAP centrální služby běžet pod instance SCS SAP (A)**
* Instance SCS SAP (A) je v clusteru a je přístupný pomocí virtuální název hostitele **< (A) SCSVirtualHostName >**
* SAP globální soubory jsou umístěny na sdílenou složku SMB a ke kterým se přistupuje pomocí <SAPGLOBALHost> název hostitele \\ \\ &lt;SAPGLOBALHost&gt;\sapmnt\\&lt;SID&gt;\SYS\...
* Instance SCS SAP (A) je nainstalována na místní disk v obou uzlů clusteru
* **< (A) SCSVirtualHostName >** názvu sítě se liší od  **&lt;SAPGLOBALHost&gt;**

![Obrázek 2: Nové SAP (A) SCS HA architektury s sdílené složky SMB][sap-ha-guide-figure-8004]

_**Obrázek 2:** nové SAP (A) SCS HA architektury s sdílené složky SMB_

Předpoklady pro sdílené složky SMB:

* Protokol SMB 3.0 (nebo vyšší)
* Možnost nastavit Active Directory (AD) seznam řízení přístupu (ACL) pro **skupiny uživatelů AD** a **počítače objekt počítače$**
* Sdílené složky musí být povoleno HA:
    * Disky, na které se používá k ukládání souborů nesmí být jediný bod selhání
    * Ujistěte se, že výpadek serverech nebo virtuálních počítačů není způsobilo výpadek sdílené složky

Nyní **SAP &lt;SID&gt;**  role clusteru není obsahující cluster sdílených disků nebo prostředku obecné sdílené složky clusteru.


![Obrázek 3: < SID > SAP role prostředky clusteru při použití sdílené složky][sap-ha-guide-figure-8005]

_**Obrázek 3:** **SAP &lt;SID&gt;**  role prostředky clusteru při použití sdílené složky_


## <a name="scale-out-file-share-sofs-with-storage-spaces-direct-s2d-on-azure-as-sapmnt-file-share"></a>Škálováním na více systémů (SOFS) sdílené složky s prostory úložiště – přímé (S2D) v Azure jako SAPMNT sdílené složky

Hostování a chránit soubory SAP globální hostitele a nabídku službu sdílené složky souborů s vysokou dostupností SAPMNT můžete SOFS.

![Obrázek 4: SOFS sdílené složky používají k ochraně souborů globální hostitele SAP][sap-ha-guide-figure-8006]

_**Obrázek 4:** SOFS sdílené složky používají k ochraně souborů globální hostitele SAP_

> [!IMPORTANT]
>Sdílené složky serveru SOFS plně podporuje cloudu Microsoft Azure stejně jako v místním prostředí.
>

**SOFS** nabízí vysoce dostupné a škálovatelné vodorovně SAPMNT sdílené složky.

**Prostory úložiště – přímé (S2D)**, se používá jako **sdíleného disku** pro SOFS umožňuje vytváření vysoce dostupné a škálovatelné úložiště serverů pomocí místní úložiště. Sdílené úložiště, které se používá pro SOFS, například pro soubory SAP GLOBALHOST proto není jediný bod selhání (SPOF).

> [!IMPORTANT]
>Pokud máte v plánu zotavení po havárii instalační program, doporučuje se SOFS řešení pro vysoce dostupné sdílené složky v Azure.
>

### <a name="sap-prerequisites-for-sofs-in-azure"></a>SAP požadavky pro SOFS v Azure

Pro SOFS potřebujete:

* Alespoň dva uzly clusteru pro SOFS

* Každý uzel musí mít aspoň dva místní disky

* Z důvodů výkonu, je nutné použít **zrcadlení odolnosti**:
    * **způsob 2** zrcadlení pro SOFS s dvěma uzly clusteru
    * **způsob 3** zrcadlení pro SOFS s uzly clusteru (nejméně tři)


* Je **doporučuje mít 3 (nebo další clusteru) uzly pro SOFS s 3 pro dvoucestné zrcadlení**.
Tato instalace nabízí další škálovatelnost a další odolnost úložiště, než instalační program SOFS s 2 uzly clusteru a 2 pro dvoucestné zrcadlení.

* Je nutné použít **Azure Premium disku**

* Je **doporučená** používat **discích spravovaných pomocí Azure**

* Je **doporučená** do formátu svazků New **odolný systém souborů (ReFS)**
    * [Poznámka SAP 1869038 - SAP podpora pro systém souborů ReFs] [1869038]
    * V tématu [výběr systém souborů] [ planning-volumes-s2d-choosing-filesystem] kapitoly plánování svazky v prostory úložiště – přímé.
    * Zajistěte, aby k instalaci tohoto [MS **KB4025334** kumulativní aktualizace][kb4025334].


* Můžete použít **DS-Series** nebo **DSv2-Series** velikostí virtuálních počítačů Azure

* Pokud chcete mít funkční mimo výkon sítě virtuálních počítačů, který je potřebný pro prostory úložiště – přímé synchronizace disku, měli byste použít typ virtuálního počítače, který má alespoň **"vysoká" šířka pásma sítě,**.
Další podrobnosti najdete v tématu [DSv2-Series] [ dv2-series] a [DS-Series] [ ds-series] specifikace.

* Je **doporučená** opustit a **záložní některé kapacita ve fondu úložiště volné**. Nabízí objem místa Opravit "místní" po selhání jednotky, zvýšení zabezpečení dat a výkonu.

 Další podrobnosti najdete v tématu [výběr velikost svazků][choosing-the-size-of-volumes-s2d]


* Virtuální počítače Azure SOFS musí být nasazený v **vlastní sadu dostupnosti Azure**

* Není nutné ke konfiguraci Azure interní nástroj pro vyrovnávání zatížení pro název síťové sdílené složky serveru SOFS souborů, například <SAPGlobalHostName>, jak se provádí pro < (A) SCSVirtualHostname > instance SCS SAP (A) nebo databázového systému. SOFS je škálování zatížení pro všechny uzly clusteru, takže <SAPGlobalHostName> používá místní IP všech uzlů clusteru.


> [!IMPORTANT]
>SAPMNT sdílené složky, která odkazuje na SAPGLOBALHOST nelze změnit. SAP nepodporuje jako sapmnt jiný název sdílení.
>[SAP Poznámka 2492395 – lze změnit sapmnt název sdílené složky?][2492395]

### <a name="configuring-sap-ascs-instances-and-sofs-in-two-clusters"></a>Konfigurace SAP (A) SCS instancí a SOFS ve dvou clusterech

Máte možnost k nasazení SAP (A) SCS instance do jednoho clusteru, s vlastní SAP <SID> role clusteru. Sdílené složky serveru SOFS je nakonfigurován v jiného clusteru s jinou rolí clusteru.

> [!IMPORTANT]
>V tomto scénáři je instance SAP (A) SCS nakonfigurovaná pro přístup k SAP GLOBALHost pomocí cesty UNC \\ \\ &lt;SAPGLOBALHost&gt;\sapmnt\\&lt;SID&gt;\SYS\...
>

![Obrázek 5: SAP (A) SCS instance a SOFS nasazené v dva clustery][sap-ha-guide-figure-8007]

_**Obrázek 5:** SAP (A) SCS instance a SOFS nasazené v dva clustery_

> [!IMPORTANT]
>V cloudu Azure, každý cluster, který používá pro SAP a SOFS soubor, který sdílené složky musí být nasazený v nastavit své vlastní Azure dostupnosti, aby distribuovány na základní infrastrukturu Azure umístění těchto virtuálních počítačů v clusteru.
>

## <a name="generic-file-share-with-sios-as-cluster-shared-disks"></a>Obecné sdílené složky pro SIOS jako Cluster sdílených disků


> [!IMPORTANT]
>SOFS je vhodné řešení pro vysoce dostupné sdílené složky.
>
>Ale pokud budete chtít nastavit také **zotavení po havárii** pro vaše vysoce dostupné sdílené složky, budete muset použít obecné sdílené složky a SISO technologií pro cluster sdílených disků.
>

Obecná sdílená složka je další možností pro dosažení vysoce dostupné sdílené složky.

Zde jako cluster s podporou sdílený disk můžete řešení SIOS 3. stran.

## <a name="next-steps"></a>Další kroky

* [Azure Příprava infrastruktury pro SAP HA používat pro SAP (A) SCS Instance clusteru převzetí služeb při selhání se systémem Windows a sdílené složky][sap-high-availability-infrastructure-wsfc-file-share]

* [SAP NetWeaver HA instalace v systému Windows převzetí služeb při selhání clusteru a sdílené složky SCS Instance SAP (A)][sap-high-availability-installation-wsfc-shared-disk]

* [Nasadit dvěma uzly prostory úložiště – přímé škálovaný souborový server UPD úložiště v Azure][deploy-sofs-s2d-in-azure]

* [Prostory úložiště – přímé v systému Windows Server 2016][s2d-in-win-2016]

* [Podrobné informace: Přímé svazky v prostorech úložiště][deep-dive-volumes-in-s2d]
