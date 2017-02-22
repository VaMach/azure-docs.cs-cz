---
title: Typy IP adres v Azure | Dokumentace Microsoftu
description: "Další informace o veřejných a privátních IP adresách v Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 3de0b167d0ad32de17093caf7e66a6d08f5c1c61
ms.openlocfilehash: 762b048056752abd24328433ceb57de492dbf884


---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>Typy IP adres a metody přidělování v Azure
Přiřazením IP adres k prostředkům Azure umožníte komunikaci s ostatními prostředky Azure, místní sítí a internetem. Existují dva typy IP adres, které můžete v Azure využít:

* **Veřejné IP adresy**: Slouží ke komunikaci s internetem, včetně veřejně přístupných služeb Azure.
* **Privátní IP adresy**: Slouží ke komunikaci v rámci virtuální sítě Azure (VNet) a místní sítě, pokud použijete VPN Gateway nebo okruh ExpressRoute pro rozšíření sítě do Azure.

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).  Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](virtual-network-ip-addresses-overview-classic.md).
> 

Pokud už klasický model nasazení znáte, prohlédněte si [rozdíly v IP adresování mezi klasickým nasazením a nasazením Resource Manageru](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Veřejné IP adresy
Veřejné IP adresy umožňují prostředkům Azure komunikovat s internetem a veřejnými službami Azure, jako jsou například [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL Databases](../sql-database/sql-database-technical-overview.md) a [Azure Storage](../storage/storage-introduction.md).

[Veřejná IP](resource-groups-networking.md#public-ip-address) adresa v Azure Resource Manageru je prostředek, který má svoje vlastní vlastnosti. Veřejnou IP adresu můžete přiřadit libovolnému z následujících prostředků:

* Virtuální počítače
* Internetové nástroje pro vyrovnávání zatížení
* VPN Gateway
* Application Gateway

### <a name="allocation-method"></a>Metoda přidělování
Existují dvě metody přidělení IP adresy *veřejnému* IP prostředku – *dynamická* a *statická*. Výchozí metoda přidělení je *dynamická*, při které IP adresa **není** přidělená v okamžiku svého vytvoření. Místo toho se veřejná IP adresa přidělí, když spustíte (nebo vytvoříte) přidružený prostředek (jako je virtuální počítač nebo nástroj pro vyrovnávání zatížení). Když tento prostředek zastavíte (nebo odstraníte), IP adresa se uvolní. To způsobuje, že se IP adresa při zastavení a spuštění prostředku změní.

Pokud chcete zajistit, aby IP adresa přidruženého prostředku zůstala stejná, můžete explicitně nastavit *statickou* metodu přidělování. V tomto případě se IP adresa přiřadí okamžitě. Uvolní se, jenom když prostředek odstraníte nebo změníte jeho metodu přidělování na *dynamickou*.

> [!NOTE]
> Ani při nastavení *statické* metody přidělování není možné určit vlastní IP adresu přiřazenou *veřejnému IP prostředku*. Tato adresa se vybírá z fondu dostupných IP adres v umístění Azure, ve kterém je prostředek vytvořený.
>

Statické veřejné IP adresy se obvykle používají v následujících scénářích:

* Koncoví uživatelé potřebují aktualizovat pravidla brány firewall pro komunikaci s prostředky Azure.
* Překlad názvů DNS, kde by změna IP adresy vyžadovala aktualizace záznamů A.
* Vaše prostředky Azure komunikují s ostatními aplikacemi nebo službami, které využívají model zabezpečení založený na IP adresách.
* Využíváte certifikáty SSL propojené k IP adrese.

> [!NOTE]
> Seznam rozsahů IP adres, ze kterých se veřejné IP adresy (dynamické/statické) přidělují prostředkům Azure, je zveřejněný na webu [Rozsahy IP adres datacenter Azure](https://www.microsoft.com/download/details.aspx?id=41653).
>

### <a name="dns-hostname-resolution"></a>Překlad názvů hostitelů DNS
Můžete zadat popisek názvu domény DNS pro veřejný IP prostředek. Na serverech DNS spravovaných Azure se vytvoří mapování *popisek_názvu_domény*.*umístění*.cloudapp.azure.com na veřejnou IP adresu. Pokud například vytvoříte veřejný IP prostředek, který jako *popisek_názvu_domény* má **contoso** a jako *umístění* v Azure používá **Západní USA**, plně kvalifikovaný název domény (FQDN) **contoso.westus.cloudapp.azure.com** se přeloží na veřejnou IP adresu tohoto prostředku. Tento plně kvalifikovaný název domény můžete použít k vytvoření vlastního záznamu CNAME domény odkazujícího na veřejnou IP adresu v Azure.

> [!IMPORTANT]
> Každý vytvořený popisek názvu domény musí být v rámci příslušného umístění Azure jedinečný.  
>

### <a name="virtual-machines"></a>Virtuální počítače
Veřejnou IP adresu můžete k virtuálnímu počítači s [Windows](../virtual-machines/virtual-machines-windows-about.md) nebo [Linuxem](../virtual-machines/virtual-machines-linux-about.md) přidružit tak, že ji přiřadíte jeho **síťovému rozhraní**. V případě virtuálních počítačů s více síťovými rozhraními můžete přiřazovat jenom k *primárnímu* síťovému rozhraní. Virtuálnímu počítači můžete přiřadit dynamickou nebo statickou veřejnou IP adresu.

### <a name="internet-facing-load-balancers"></a>Internetové nástroje pro vyrovnávání zatížení
Veřejnou IP adresu můžete přiřadit službě [Azure Load Balancer](../load-balancer/load-balancer-overview.md) tak, že ji přiřadíte konfiguraci **front-endu** tohoto nástroje pro vyrovnávání zatížení. Tato veřejná IP adresa slouží jako virtuální IP adresa (VIP) s vyrovnáváním zatížení. Front-endu nástroje pro vyrovnávání zatížení můžete přiřadit dynamickou nebo statickou veřejnou IP adresu. Front-endu nástroje pro vyrovnávání zatížení můžete také přiřadit několik veřejných IP adres, což umožňuje použití scénářů s [několika VIP](../load-balancer/load-balancer-multivip.md), jako je víceklientské prostředí s weby využívajícími SSL.

### <a name="vpn-gateways"></a>VPN Gateway
[Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) se používá k připojení virtuální sítě Azure (VNet) k ostatním virtuálním sítím Azure nebo k místní síti. Abyste povolili komunikaci se vzdálenou sítí, musíte přiřadit veřejnou IP adresu její **konfiguraci protokolu IP**. V současné době je možné službě VPN Gateway přiřadit jenom *dynamickou* veřejnou IP adresu.

### <a name="application-gateways"></a>Application Gateway
Veřejnou IP adresu můžete přiřadit službě [Azure Application Gateway](../application-gateway/application-gateway-introduction.md) tak, že ji přiřadíte konfiguraci **front-endu** této brány. Tato veřejná IP adresa slouží jako virtuální IP adresa (VIP) s vyrovnáváním zatížení. V současné době je možné službě Application Gateway přiřadit jenom *dynamickou* veřejnou IP adresu.

### <a name="at-a-glance"></a>Přehledně
Následující tabulka ukazuje konkrétní vlastnost, jejímž prostřednictvím je možné veřejnou IP adresu přiřadit prostředku nejvyšší úrovně, a metody přidělení (dynamické nebo statické), které je možné použít.

| Prostředek nejvyšší úrovně | Přidružení IP adresy | Dynamická | Statická |
| --- | --- | --- | --- |
| Virtuální počítač |Síťové rozhraní |Ano |Ano |
| Nástroj pro vyrovnávání zatížení |Konfigurace front-endu |Ano |Ano |
| VPN Gateway |Konfigurace protokolu IP brány |Ano |Ne |
| Application Gateway |Konfigurace front-endu |Ano |Ne |

## <a name="private-ip-addresses"></a>Privátní IP adresy
Privátní IP adresy umožňují prostředkům Azure komunikovat s ostatními prostředky ve [virtuální](virtual-networks-overview.md) nebo místní síti prostřednictvím brány sítě VPN nebo okruhu ExpressRoute, a to bez použití IP adresy dostupné na internetu.

V modelu nasazení Azure Resource Manager se IP adresa přidruží k následujícím typům prostředků Azure:

* Virtuální počítače
* Interní nástroje pro vyrovnávání zatížení
* Application Gateway

### <a name="allocation-method"></a>Metoda přidělování
Privátní IP adresa se přiděluje z rozsahu adres v podsíti, ke které je prostředek připojen. Rozsah adres samotné podsítě je součástí rozsahu adres virtuální sítě.

Existují dvě metody přidělení privátní IP adresy: *dynamická* a *statická*. Výchozí metodou je *dynamické* přidělení, kdy se IP adresa automaticky přiděluje z podsítě prostředku (pomocí protokolu DHCP). Tuto IP adresa se může při zastavení a spuštění prostředku změnit.

Pokud chcete zajistit, aby IP adresa zůstávala stejná, můžete nastavit *statickou* metodu přidělení. V tomto případě musíte také poskytnout platnou IP adresu, která je součástí podsítě prostředku.

Statické privátní IP adresy se obvykle používají pro:

* Virtuální počítače, které slouží jako řadiče domény nebo servery DNS.
* Prostředky, které vyžadují pravidla brány firewall s využitím IP adres.
* Prostředky, ke kterým se přistupuje z jiných aplikací nebo prostředků prostřednictvím IP adresy.

### <a name="virtual-machines"></a>Virtuální počítače
Privátní IP adresa se přiřazuje **síťovému rozhraní** virtuálního počítače s [Windows](../virtual-machines/virtual-machines-windows-about.md) nebo [Linuxem](../virtual-machines/virtual-machines-linux-about.md). V případě virtuálního počítače s několika síťovými rozhraními se privátní IP adresa přiřadí každému z těchto rozhraní. Pro síťové rozhraní můžete určit dynamickou nebo statickou metodu přidělení.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Překlad názvů hostitelů interních služeb DNS (pro virtuální počítače)
Všechny virtuální počítače Azure jsou ve výchozím nastavení nakonfigurované se [servery DNS spravovanými Azure](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) (pokud explicitně nenakonfigurujete vlastní servery DNS). Tyto servery DNS poskytují interní překlad IP adres pro virtuální počítače umístěné ve stejné virtuální síti.

Když vytvoříte virtuální počítač, do serverů DNS spravovaných Azure se přidá mapování názvu hostitele na jeho IP adresu. V případě virtuálního počítače s několika síťovými rozhraními se název hostitele mapuje na privátní IP adresu primárního síťového rozhraní.

Virtuální počítače nakonfigurované servery DNS spravovanými Azure mohou překládat názvy hostitelů všech virtuálních počítačů v rámci virtuální sítě na jejich privátní IP adresy.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interní nástroje pro vyrovnávání a Application Gateway
Privátní IP adresu můžete přiřadit konfiguraci **front-endu** nástroje [Azure Internal Load Balancer](../load-balancer/load-balancer-internal-overview.md) (ILB) nebo služby [Azure Application Gateway](../application-gateway/application-gateway-introduction.md). Tato privátní IP adresa slouží jako interní koncový bod, který je přístupný pouze pro prostředky v příslušné virtuální síti (VNet) a ve vzdálených sítích připojených k virtuální síti. Konfiguraci front-endu můžete přiřadit dynamickou nebo statickou privátní IP adresu.

### <a name="at-a-glance"></a>Přehledně
Následující tabulka ukazuje konkrétní vlastnost, jejímž prostřednictvím je možné privátní IP adresu přiřadit prostředku nejvyšší úrovně, a metody přidělení (dynamické nebo statické), které je možné použít.

| Prostředek nejvyšší úrovně | Přidružení IP adresy | Dynamická | Statická |
| --- | --- | --- | --- |
| Virtuální počítač |Síťové rozhraní |Ano |Ano |
| Nástroj pro vyrovnávání zatížení |Konfigurace front-endu |Ano |Ano |
| Application Gateway |Konfigurace front-endu |Ano |Ano |

## <a name="limits"></a>Omezení
Omezení IP adresování jsou uvedená v kompletní sadě [omezení sítě](../azure-subscription-service-limits.md#networking-limits) v Azure. Tato omezení platí pro jednotlivé oblasti a jednotlivá předplatná. Pokud chcete v závislosti na svých obchodních potřebách zvýšit výchozí omezení na povolené maximum, [kontaktujte podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="pricing"></a>Ceny
Za veřejné IP adresy se může účtovat nominální poplatek. Další informace o cenách IP adres v Azure najdete na stránce [Ceny IP adres](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="next-steps"></a>Další kroky
* [Nasazení virtuálního počítače se statickou veřejnou IP adresou pomocí webu Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
* [Nasazení virtuálního počítače se statickou veřejnou IP adresou pomocí šablony](virtual-network-deploy-static-pip-arm-template.md)
* [Nasazení virtuálního počítače se statickou privátní IP adresou pomocí webu Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)



<!--HONumber=Jan17_HO5-->


