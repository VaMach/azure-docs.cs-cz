---
title: "SAP NetWeaver HA instalace v clusteru převzetí služeb při selhání se systémem Windows a sdílený Disk pro instanci SCS SAP (A) v Azure | Microsoft Docs"
description: "SAP NetWeaver HA instalace v systému Windows převzetí služeb při selhání clusteru a sdíleného disku SCS Instance SAP (A)"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: goraco
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 6209bcb3-5b20-4845-aa10-1475c576659f
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 938ab6be6b2ba9e1403919cb62f68c65f114c067
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="sap-netweaver-ha-installation-on-windows-failover-cluster-and-shared-disk-for-sap-ascs-instance-on-azure"></a>SAP NetWeaver HA instalace v systému Windows převzetí služeb při selhání clusteru a sdíleného disku SCS Instance SAP (A) v Azure

[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

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
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md
[sap-hana-ha]:sap-hana-high-availability.md
[sap-suse-ascs-ha]:high-availability-guide-suse.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md



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

[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/resource-group-overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md

Tento dokument je popisující, jak nainstalovat a nakonfigurovat vysokou dostupné systémové SAP v Azure, s **Windows Failover Cluster (WSFC)** a pomocí **sdíleného disku clusteru** pro clustering SCS SAP (A) instance.

## <a name="prerequisites"></a>Požadavky

Ujistěte se, že jste si tyto dokumenty, než začnete s instalací:

* [Průvodce architekturou – Clustering SCS Instance SAP (A) v clusteru převzetí služeb při selhání systému Windows pomocí clusteru sdílený Disk][sap-high-availability-guide-wsfc-shared-disk]

* [Azure Příprava infrastruktury pro SAP HA používat pro SAP (A) SCS Instance clusteru převzetí služeb při selhání se systémem Windows a sdíleného disku][sap-high-availability-infrastructure-wsfc-shared-disk]

Instalace databázového systému jsme popisují není nastavení lišit v závislosti na tom, které můžete použít systém databázového systému. Ale předpokládáme, že jsou aspekty vysoké dostupnosti s databázového systému řešit pomocí funkce, které různých výrobců databázového systému podpora pro Azure. Například Always On nebo zrcadlení databáze systému SQL Server a Oracle Data Guard pro databáze Oracle. Ve scénáři, které používáme v tomto článku jsme nebyla přidat další ochranu do databázového systému.

Když různé služby databázového systému interakci s tímto typem Clusterované konfigurace SAP ASC nebo SCS v Azure neexistují žádné zvláštní požadavky.

> [!NOTE]
> Postupy instalace systémů SAP NetWeaver ABAP, Java systémy a systémy ABAP + Java jsou téměř shodné. Nejdůležitější rozdíl je, že systému SAP ABAP má jednu instanci ASC. V systému SAP Java má jednu instanci SCS. V systému SAP ABAP + Java obsahuje jednu instanci ASC a jednu instanci SCS spuštěné ve stejné skupině clusteru převzetí služeb při selhání Microsoft. Případné rozdíly instalace pro každou SAP NetWeaver instalace zásobníku se výslovně uveden. Můžete předpokládat, že všechny ostatní části jsou stejné.  
>
>

## <a name="31c6bd4f-51df-4057-9fdf-3fcbc619c170"></a>Instalace s vysokou dostupností ASC nebo SCS Instance SAP

> [!IMPORTANT]
> Nezapomeňte toto stránkovacím souboru na svazcích DataKeeper zrcadlení. DataKeeper nepodporuje zrcadlové svazky. Můžete ponechat stránkovacím souboru na dočasné jednotce D Azure virtuálního počítače, který je výchozí. Pokud není již existuje, přesuňte stránkovací soubor Windows na jednotku D virtuálního počítače Azure.
>
>

Instalace s vysokou dostupností ASC nebo SCS instance SAP zahrnuje tyto úlohy:

* Vytváření název virtuálního hostitele pro SAP ASC nebo SCS skupinu prostředků clusteru
* Instalace prvního uzlu clusteru SAP
* Úprava profilu SAP ASC nebo SCS instance
* Přidání port testu
* Otevřete port testu brány firewall systému Windows

### <a name="a97ad604-9094-44fe-a364-f89cb39bf097"></a>Vytvořte název virtuálního hostitele pro instanci ASC nebo SCS Clusterové SAP

1.  Ve Správci DNS systému Windows vytvořte záznam DNS pro název virtuálního hostitele ASC nebo SCS instance.

  > [!IMPORTANT]
  > IP adresa, která přiřadíte název virtuálního hostitele ASC nebo SCS instance musí být stejná jako adresa IP, který jste přiřadili k vyrovnávání zatížení Azure (**<*SID*> - lb - ASC **).  
  >
  >

  IP adresa virtuální název hostitele SAP ASC nebo SCS (**pr1. ASC sap**) je stejný jako IP adresu služby Vyrovnávání zatížení Azure (**pr1-lb Asc**).

  ![Obrázek 1: Definování položky DNS pro virtuální název clusteru SAP ASC nebo SCS a adresa TCP/IP][sap-ha-guide-figure-3046]

  _**Obrázek 1:** definovat položku DNS pro virtuální název clusteru SAP ASC nebo SCS a adresa TCP/IP_

2.  Chcete-li definovat přiřazené název virtuálního hostitele IP adresy, vyberte **Správce DNS** > **domény**.

  ![Obrázek 2: Nový virtuální název a adresu TCP/IP pro konfiguraci clusteru SAP ASC nebo SCS][sap-ha-guide-figure-3047]

  _**Obrázek 2:** nový virtuální název a TCP/IP adres pro konfiguraci clusteru SAP ASC nebo SCS_

### <a name="eb5af918-b42f-4803-bb50-eff41f84b0b0"></a>Instalace prvního uzlu clusteru SAP

1.  Provést první možnost uzlu clusteru na uzlu clusteru A. Například **pr1-ASC-0** hostitele.
2.  Ponechat výchozí porty pro vyrovnávání zatížení Azure interní, vyberte:

  * **Systém ABAP**: **Asc** instance číslo **00**
  * **Java systému**: **SCS** instance číslo **01**
  * **ABAP + Java systému**: **Asc** instance číslo **00** a **SCS** instance číslo **01**

  Pro Java SCS použít instanci čísla než 00 pro instanci ABAP ASC a 01 instance, budete muset změnit pravidla, Vyrovnávání zatížení výchozí nástroje pro vyrovnávání zatížení Azure interní první popsaných v tématu [změnit Vyrovnávání zatížení ASC nebo SCS výchozí pravidla pro Azure Nástroj pro vyrovnávání zatížení interní] [sap-ha Průvodce-8.9].

Další několik úloh nejsou popsané v dokumentaci standardní instalace SAP.

> [!NOTE]
> Dokumentaci k instalaci SAP popisuje postup instalace prvního uzlu clusteru ASC nebo SCS.
>
>

### <a name="e4caaab2-e90f-4f2c-bc84-2cd2e12a9556"></a>Upravit profil SAP ASC nebo SCS instance

Je nutné přidat nový parametr profilu. Parametr profil zabrání připojení mezi SAP pracovní procesy a serverem zařazování zavření po nečinnosti příliš dlouho. Jsme uvedených scénář v [přidat položky registru na obou uzlů clusteru instance SAP ASC nebo SCS] [sap-ha Průvodce-8.11]. V této části zavedli jsme taky dvě změny některé základní parametry připojení TCP/IP. V druhém kroku, je nutné nastavit zařadit server k odeslání `keep_alive` signál, aby připojení není dosáhl nástroje pro vyrovnávání zatížení Azure vnitřní limit nečinnosti.

Úprava profilu SAP instance ASC nebo SCS:

1.  Tento parametr profil přidáte do profilu instance SAP ASC nebo SCS:

  ```
  enque/encni/set_so_keepalive = true
  ```
  V našem příkladu je cesta:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_ASCS00_pr1-ascs-sap`

  Například pro SAP SCS instance profilu a odpovídajících cesta:

  `<ShareDisk>:\usr\sap\PR1\SYS\profile\PR1_SCS01_pr1-ascs-sap`

2.  Aby se změny projevily, restartujte instanci /SCS ASC SAP.

### <a name="10822f4f-32e7-4871-b63a-9b86c76ce761"></a>Přidat Port testu

Pomocí nástroje pro vyrovnávání zatížení interní test funkce usnadňují konfiguraci celý cluster pracovat s Vyrovnávání zatížení Azure. Nástroje pro vyrovnávání zatížení Azure interní obvykle distribuuje příchozí zatížení rovnoměrně mezi zúčastněných virtuálních počítačů. Ale tato funkce nebude pracovat v některé konfigurace clusteru vzhledem k tomu, že pouze jedna instance je aktivní. Druhá instance je pasivní a nemůže přijímat úlohy. Funkce kontroly pomáhá při nástroje pro vyrovnávání zatížení Azure interní přiřadí pracovní jenom na aktivní instance. Pomocí funkce testu nástroje pro vyrovnávání zatížení pro vnitřní může zjistit, které instancí jsou aktivní a pak cíle pouze instance se zatížením.

Chcete-li přidat port testu:

1.  Zkontrolujte aktuální **ProbePort** nastavení spuštěním následujícího příkazu Powershellu:

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter
  ```

   Spouštění ho do jedné z virtuálních počítačů v konfiguraci clusteru.

2.  Definujte port testu. Výchozí číslo portu testu je **0**. V našem příkladu používáme port testu **62000**.

  ![Obrázek 3: Port testu konfigurace clusteru je 0 ve výchozím nastavení][sap-ha-guide-figure-3048]

  _**Obrázek 3:** výchozí port test konfigurace clusteru je 0._

  Číslo portu je definována v šablonách SAP Azure Resource Manager. Můžete přiřadit číslo portu v prostředí PowerShell.

  Chcete-li nastavit novou hodnotu ProbePort  **SAP <*SID*> IP ** prostředek clusteru, spusťte následující skript prostředí PowerShell pro aktualizaci proměnné prostředí PowerShell pro vaše prostředí:

  ```PowerShell
  $SAPSID = "PR1"      # SAP <SID>
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  Clear-Host
  $SAPClusterRoleName = "SAP $SAPSID"
  $SAPIPresourceName = "SAP $SAPSID IP"
  $SAPIPResourceClusterParameters =  Get-ClusterResource $SAPIPresourceName | Get-ClusterParameter
  $IPAddress = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Address" }).Value
  $NetworkName = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "Network" }).Value
  $SubnetMask = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "SubnetMask" }).Value
  $OverrideAddressMatch = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "OverrideAddressMatch" }).Value
  $EnableDhcp = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "EnableDhcp" }).Value
  $OldProbePort = ($SAPIPResourceClusterParameters | Where-Object {$_.Name -eq "ProbePort" }).Value

  $var = Get-ClusterResource | Where-Object {  $_.name -eq $SAPIPresourceName  }

  Write-Host "Current configuration parameters for SAP IP cluster resource '$SAPIPresourceName' are:" -ForegroundColor Cyan
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter

  Write-Host
  Write-Host "Current probe port property of the SAP cluster resource '$SAPIPresourceName' is '$OldProbePort'." -ForegroundColor Cyan
  Write-Host
  Write-Host "Setting the new probe port property of the SAP cluster resource '$SAPIPresourceName' to '$ProbePort' ..." -ForegroundColor Cyan
  Write-Host

  $var | Set-ClusterParameter -Multiple @{"Address"=$IPAddress;"ProbePort"=$ProbePort;"Subnetmask"=$SubnetMask;"Network"=$NetworkName;"OverrideAddressMatch"=$OverrideAddressMatch;"EnableDhcp"=$EnableDhcp}

  Write-Host

  $ActivateChanges = Read-Host "Do you want to take restart SAP cluster role '$SAPClusterRoleName', to activate the changes (yes/no)?"

  if($ActivateChanges -eq "yes"){
  Write-Host
  Write-Host "Activating changes..." -ForegroundColor Cyan

  Write-Host
  write-host "Taking SAP cluster IP resource '$SAPIPresourceName' offline ..." -ForegroundColor Cyan
  Stop-ClusterResource -Name $SAPIPresourceName
  sleep 5

  Write-Host "Starting SAP cluster role '$SAPClusterRoleName' ..." -ForegroundColor Cyan
  Start-ClusterGroup -Name $SAPClusterRoleName

  Write-Host "New ProbePort parameter is active." -ForegroundColor Green
  Write-Host

  Write-Host "New configuration parameters for SAP IP cluster resource '$SAPIPresourceName':" -ForegroundColor Cyan
  Write-Host
  Get-ClusterResource -Name $SAPIPresourceName | Get-ClusterParameter
  }else
  {
  Write-Host "Changes are not activated."
  }
  ```

  Po přepnutí  **SAP <*SID*> ** clusteru role online, ověřte, že **ProbePort** nastavena na novou hodnotu.

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPNetworkIPClusterName = "SAP $SAPSID IP"
  Get-ClusterResource $SAPNetworkIPClusterName | Get-ClusterParameter

  ```
  Po spuštění skriptu budete vyzváni, restartujte skupina clusteru SAP k aktivaci změny.

  ![Obrázek 4: Probe port clusteru po nastavení nová hodnota][sap-ha-guide-figure-3049]

  _**Obrázek 4:** testu port clusteru po nastavení nová hodnota_

### <a name="4498c707-86c0-4cde-9c69-058a7ab8c3ac"></a>Otevřete Port testu brány Firewall systému Windows

Budete muset otevřít port testu brány firewall systému Windows na obou uzlů clusteru. Pomocí následujícího skriptu otevřít port testu brány firewall systému Windows. Aktualizujte proměnné prostředí PowerShell pro vaše prostředí.

  ```PowerShell
  $ProbePort = 62000   # ProbePort of the Azure Internal Load Balancer

  New-NetFirewallRule -Name AzureProbePort -DisplayName "Rule for Azure Probe Port" -Direction Inbound -Action Allow -Protocol TCP -LocalPort $ProbePort
  ```

**ProbePort** je nastaven na **62000**. Teď můžete přístup ke sdílené složce  **\\\ascsha-clsap\sapmnt** z jiných hostitelů, například jako z **ascsha dbas**.

## <a name="85d78414-b21d-4097-92b6-34d8bcb724b7"></a>Instalovat instanci databáze

Chcete-li nainstalovat instanci databáze, postupujte podle procesu popsaného v dokumentaci k instalaci SAP.

## <a name="8a276e16-f507-4071-b829-cdc0a4d36748"></a>Instalaci druhého uzlu clusteru

K instalaci druhé clusteru, postupujte podle kroků v Průvodci instalací SAP.

## <a name="094bc895-31d4-4471-91cc-1513b64e406a"></a>Změnit typ spuštění služby instance SAP YBRAT Windows

Změnit typ spuštění služby Windows YBRAT SAP **automaticky (zpožděné spuštění)** na obou uzlů clusteru.

![Obrázek 5: Změňte typ služby pro instance SAP YBRAT na automatické zpožděné][sap-ha-guide-figure-3050]

_**Obrázek 5:** změnit typ služby pro instance SAP YBRAT pro odložené automatické_

## <a name="2477e58f-c5a7-4a5d-9ae3-7b91022cafb5"></a>Instalace serveru primární aplikace SAP

Nainstalovat instanci serveru primární aplikace (Pa) <*SID*> - di - 0 na virtuálním počítači, který jste určili pro hostování Pa ADRESAMI. Neexistují žádné závislosti na Azure nebo DataKeeper specifická nastavení.

## <a name="0ba4a6c1-cc37-4bcf-a8dc-025de4263772"></a>Instalace serveru SAP další aplikace

Nainstalujte SAP serveru pro další aplikace (AAS) na všechny virtuální počítače, které jste označili k hostování instance aplikačního serveru SAP. Například na <*SID*> - di - 1 pro <*SID*> - di -&lt;n&gt;.

> [!NOTE]
> To dokončí instalaci systému SAP NetWeaver vysokou dostupnost. Pokračujte dále testování převzetí služeb při selhání.
>


## <a name="18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9"></a>Testovací převzetí služeb při selhání SAP ASC nebo SCS Instance a SIOS replikace
Je snadné pro testování a monitorování SAP ASC nebo SCS instance převzetí služeb při selhání a replikaci disku SIOS pomocí nástroje Správce clusteru převzetí služeb při selhání a s DataKeeper správy a konfigurace.

### <a name="65fdef0f-9f94-41f9-b314-ea45bbfea445"></a>V uzlu clusteru A je spuštěna Instance ASC nebo SCS SAP

**SAP PR1** skupina clusteru běží na uzlu clusteru A. Například na **pr1-ASC-0**. Přiřadit sdílené diskové jednotce S, která je součástí systému **SAP PR1** skupina clusteru a který instance ASC nebo SCS používá A. uzlu v clusteru

![Obrázek 6: Správce clusteru převzetí služeb při selhání: Skupina clusteru SAP < SID > běží na uzlu clusteru A][sap-ha-guide-figure-5000]

_**Obrázek 6:** Správce clusteru převzetí služeb při selhání: SAP <*SID*> Skupina clusteru běží na uzlu clusteru A_

Nástroje pro správu s DataKeeper a konfiguraci uvidíte, že dat sdíleného disku je synchronně replikovat ze zdrojového svazku jednotky S na uzlu clusteru A jednotky cílový svazek S na uzlu clusteru B. Například se replikují z **pr1-ASC-0 [10.0.0.40]** k **pr1-ASC-1 [10.0.0.41]**.

![Obrázek 7: V DataKeeper s replikovat místní svazek z uzlu clusteru A k uzlu clusteru B][sap-ha-guide-figure-5001]

_**Obrázek 7:** v DataKeeper s replikovat místní svazek z uzlu clusteru A k uzlu clusteru B_

### <a name="5e959fa9-8fcd-49e5-a12c-37f6ba07b916"></a>Převzetí služeb při selhání z uzlu A k uzlu B

1.  Vyberte jednu z těchto možností k zahájení převzetí služeb při selhání SAP <*SID*> Skupina clusteru z uzlu clusteru A k uzlu clusteru B:
  - Pomocí Správce clusteru převzetí služeb při selhání  
  - Pomocí prostředí PowerShell pro Cluster převzetí služeb při selhání

  ```PowerShell
  $SAPSID = "PR1"     # SAP <SID>

  $SAPClusterGroup = "SAP $SAPSID"
  Move-ClusterGroup -Name $SAPClusterGroup

  ```
2.  Restartovat A uzlu clusteru v rámci hostovaného operačního systému Windows (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).  
3.  Restartovat uzel A cluster z portálu Azure (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).  
4.  Restartujte počítač A uzlu clusteru pomocí prostředí Azure PowerShell (tím se vyvolá automatické převzetí služeb při selhání z SAP <*SID*> Skupina clusteru z uzlu A k uzlu B).

  Po převzetí služeb při selhání, SAP <*SID*> Skupina clusteru běží na uzlu clusteru B. Například je spuštěn na **pr1-ASC-1**.

  ![Obrázek 8: Ve Správci clusteru převzetí služeb při selhání skupiny clusteru SAP < SID > běží na uzlu clusteru B][sap-ha-guide-figure-5002]

  _**Obrázek 8**: ve Správci clusteru převzetí služeb při selhání, SAP <*SID*> Skupina clusteru běží na uzlu clusteru B_

  Sdílený disk je teď připojené v clusteru uzlu B. s DataKeeper se replikuje data ze zdrojového svazku jednotky S na uzlu clusteru B na cílový svazek jednotky S na uzlu clusteru A. Například je replikace z **pr1-ASC-1 [10.0.0.41]** k **pr1-ASC-0 [10.0.0.40]**.

  ![Obrázek 9: SIOS DataKeeper z uzlu clusteru B A uzlu v clusteru se replikuje místního svazku][sap-ha-guide-figure-5003]

  _**Obrázek 9:** s DataKeeper replikuje místní svazek z uzlu clusteru B A uzlu v clusteru_
