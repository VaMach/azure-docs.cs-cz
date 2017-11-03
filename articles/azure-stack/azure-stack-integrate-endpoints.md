---
title: "Azure zásobníku datacenter integrace – Publikování koncové body"
description: "Zjistěte, jak publikovat koncové body Azure zásobníku ve vašem datovém centru"
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 10/18/2017
ms.author: victorh
keywords: 
ms.openlocfilehash: 0d15252079b62f6a74a1279309fb9b1b3ed5711e
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure zásobníku datacenter integrace – Publikování koncové body

*Platí pro: Azure zásobníku integrované systémy*

Azure zásobníku nastaví různých koncových bodů (VIP - virtuální IP adresy) pro její role infrastruktury. Tyto virtuální IP adresy jsou přiděleny z fondu veřejných IP adres. Každý virtuální IP adresy, je zabezpečen seznam řízení přístupu (ACL) ve vrstvě softwarově definované sítě. Seznamy ACL se také používají ve fyzické přepínače (mandátu a BMC) pro další posílení zabezpečení řešení. Položka DNS se vytvoří pro každý koncový bod v externí zóně DNS, která byla zadaná v době nasazení.


Následující diagram architektury ukazuje vrstvy jinou síť a seznamy řízení přístupu:

![Diagram architektury](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Porty a protokoly (příchozí)

V následující tabulce jsou uvedeny infrastruktury virtuální IP adresy, které jsou vyžadovány pro publikování koncové body Azure zásobníku k externím sítím. V seznamu jsou uvedeny každý koncový bod, požadovaný port a protokol. Koncové body, které jsou požadované pro zprostředkovatele další prostředků, jako je poskytovatelem prostředků SQL a jiné, jsou popsané v dokumentaci k nasazení zprostředkovatele prostředku.

Interní infrastruktury virtuální IP adresy nejsou uvedené, protože není jsou vyžadována pro publikování zásobník Azure.

> [!NOTE]
> Virtuální IP adresy uživatele je dynamická definované sami uživatelé s žádnou kontrolu operátorem zásobník Azure.


|Koncový bod (VIP)|Záznam hostitele DNS|Protocol (Protokol)|Porty|
|---------|---------|---------|---------|
|SLUŽBA AD FS|`Adfs.[Region].[External FQDN]`|HTTPS|443|
|Portál (správce)|`Adminportal.[Region].[External FQDN]`|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Azure Resource Manager (správce)|`Adminmanagement.[Region].[External FQDN]`|HTTPS|443<br>30024|
|Portál (uživatel)|`Portal. [Region].[External FQDN]`|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (uživatel)|`Management.[Region].[External FQDN]`|HTTPS|443<br>30024|
|Graph|`Graph.[Region].[External FQDN]`|HTTPS|443|
|Seznam odvolaných certifikátů|`Crl.[Region].[External FQDN]`|HTTP|80|
|DNS|`*.[Region].[External FQDN]`|TCP A UDP|53|
|Key Vault (uživatel)|`*.vault.[Region].[External FQDN]`|TCP|443|
|Key Vault (správce)|`*.adminvault.[Region].[External FQDN]`|TCP|443|
|Fronty úložiště|`*.queue.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|
|Tabulka úložiště|`*.table.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|
|Objekt Blob úložiště|`*.blob.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|

## <a name="ports-and-urls-outbound"></a>Porty a adresy URL (odchozí)

Azure zásobníku podporuje pouze transparentní proxy servery. V nasazení, kde transparentní proxy odchozí připojení pro tradiční proxy server, musíte povolit následující porty a adresy URL pro odchozí komunikace:


|Účel|ADRESA URL|Protocol (Protokol)|Porty|
|---------|---------|---------|---------|
|Identita|`login.windows.net`<br>`login.microsoftonline.com`<br>`graph.windows.net`|HTTP<br>HTTPS|80<br>443|
|Syndikace Marketplace.|`https://management.azure.com`<br>`https://*.blob.core.windows.net`<br>`https://*.azureedge.net`<br>`https://*.microsoftazurestack.com`|HTTPS|443|
|Opravy a aktualizace|`https://*.azureedge.net`|HTTPS|443|
|Registrace|`https://management.azure.com`|HTTPS|443|
|Využití|`https://*.microsoftazurestack.com`<br>`https://*.trafficmanager.com`|HTTPS|443|

## <a name="firewall-publishing"></a>Publikování brány firewall

Porty uvedené v předchozí části platí pro příchozí komunikaci, při publikování zásobníku služby Azure přes existující bránu firewall.

Doporučujeme, abyste se použít zařízení brány firewall ke zvýšení zabezpečení Azure zásobníku. Je však není explicitní požadavek. I když se třeba k útokům (DDOS) distribuované denial-of-service a prověřování obsahu vám může pomoci brány firewall, budou také k propustnost úzkým místem pro služby Azure storage jako objekty BLOB, tabulek a front.

Podle modelu Identity (Azure AD ani AD FS), může nebo nemusí být pro publikování koncový bod služby AD FS vyžaduje. Pokud se používá režim odpojené nasazení, musíte publikovat koncový bod služby AD FS. (Další informace najdete v tématu Datacenter integraci identity.)

Azure Resource Manager (správce), portál správce a koncové body Key Vault (správce) nevyžadují nutně externí publikování. To závisí na scénáři. Například jako poskytovatele služeb, můžete omezit prostor k útoku a jenom správu zásobník Azure z uvnitř vaší sítě a nikoli z Internetu.

V organizaci, enterprise externí síti může být existující podnikové síti. V takové situaci musíte publikovat těchto koncových bodů provoz zásobník Azure z podnikové sítě.

## <a name="edge-firewall-scenario"></a>Scénář hraniční brány firewall

V nasazení okraj je zásobník Azure nasadit přímo za hraniční směrovač (poskytovatel) s nebo bez brány firewall úrovních před ním.

![Diagram architektury nasazení edge Azure zásobníku](media/azure-stack-integrate-endpoints/Integrate-Endpoints-02.png)

Obvykle jsou veřejné směrovatelné IP adresy zadané pro fondu veřejných virtuálních IP adres v době nasazení v nasazení okraj. Tento scénář umožňuje uživatelům mít úplná samoobslužné řízené cloudové prostředí jako ve veřejné cloud jako je například Azure.

### <a name="using-nat"></a>Použití NAT

I když se nedoporučuje kvůli režijní náklady, můžete použít překlad adres (NAT) pro publikování koncové body. Pro koncový bod publikování, které plně řídí uživatelé, to vyžaduje pravidlo NAT na uživatele VIP, který obsahuje všechny porty, které může uživatel použít.

Je potřeba zamyslet se, že Azure nepodporuje nastavení tunelového připojení sítě VPN pro koncový bod pomocí NAT v případě hybridního cloudu s Azure.

## <a name="enterpriseintranetperimeter-network-firewall-scenario"></a>Scénáře brány firewall Enterprise, intranetu nebo hraniční síti

V nasazení služby enterprise, intranetu nebo hraniční zásobník Azure nasadí nad rámec druhá brána firewall, která je obvykle součástí hraniční síti (označované také jako DMZ).

![Scénáře brány firewall Azure zásobníku](media/azure-stack-integrate-endpoints/Integrate-Endpoints-03.png)

Pokud byly nastaveny veřejné směrovatelné IP adresy pro veřejný fondu virtuálních IP adres Azure zásobníku, tyto adresy logicky patří do hraniční sítě a vyžadovat pravidla pro publikování v primární brány firewall.

### <a name="using-nat"></a>Použití NAT

Pokud neveřejný směrovatelné IP adresy jsou používány pro veřejné fondu virtuálních IP adres Azure zásobníku, NAT se používá v sekundární brány firewall k publikování koncové body Azure zásobníku. V tomto scénáři budete muset nakonfigurovat pravidla pro publikování v bráně firewall primární za okraj a na sekundární brány firewall. Pokud chcete použít NAT, zvažte následující body:

- NAT přidá zatížení při správě pravidel brány firewall, protože uživatelé řídit své vlastní koncové body a vlastní pravidla pro publikování v zásobníku softwarově definované sítě (SDN). Uživatelé musí kontaktovat operátor zásobník Azure získat jejich VIP publikována a k aktualizaci seznamu port.
- Při použití NAT limity činnost koncového uživatele, poskytuje úplné řízení ke operátor přes publikování požadavky.
- Pro hybridní scénáře cloudu s Azure zvažte, že Azure nepodporuje nastavení tunelového připojení sítě VPN pro koncový bod pomocí adres (NAT)


## <a name="next-steps"></a>Další kroky

[Zásobník datacenter integrace se službou Azure - zabezpečení](azure-stack-integrate-security.md)