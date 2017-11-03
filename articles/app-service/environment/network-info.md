---
title: "Aspekty sítě pomocí služby Azure App Service environment"
description: "Vysvětluje App Service Environment síťový provoz a jak nastavit skupiny Nsg a udr s vaší App Service Environment"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ccompy
ms.openlocfilehash: 121dd1a90e9bde66f1c3b752412a657a67295084
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="networking-considerations-for-an-app-service-environment"></a>Aspekty sítě služby App Service environment #

## <a name="overview"></a>Přehled ##

 Azure [App Service Environment] [ Intro] nasazení služby Azure App Service na podsíť ve virtuální síti Azure (VNet). Existují dva typy nasazení pro služby App Service environment (App Service Environment):

- **Externí App Service Environment**: zpřístupní aplikace app Service Environment hostované na IP adresu přístupné z Internetu. Další informace najdete v tématu [vytvořit externí App Service Environment][MakeExternalASE].
- **App Service Environment ILB**: zpřístupní aplikace app Service Environment hostované na IP adresu ve virtuální síti. Vnitřní koncový bod je k pro vyrovnávání interní zatížení (ILB), proto volal ILB App Service Environment. Další informace najdete v tématu [vytvoření a použití App Service Environment ILB][MakeILBASE].

Existují dvě verze služby App Service Environment: ASEv1 a ASEv2. Informace o ASEv1 najdete v tématu [Úvod do služby App Service Environment v1][ASEv1Intro]. ASEv1 se dá nasadit v klasický nebo virtuální sítě Resource Manageru. ASEv2 lze nasadit pouze do virtuální sítě Resource Manageru.

Všechna volání ze App Service Environment, které připojuje k Internetu ponechte virtuální sítě pomocí virtuální IP adresu přiřazené App Service Environment. Veřejné IP adresy z této VIP je pak zdrojové IP adresy pro všechna volání z App Service Environment, které připojuje k Internetu. Pokud aplikace, které vaše App Service Environment provádět volání k prostředkům ve vaší virtuální síti nebo přes síť VPN, je jedním z IP adresy v podsíti používané vaší App Service Environment zdrojové IP adresy. Protože App Service Environment není v rámci virtuální sítě, ho také přístup k prostředkům v rámci virtuální sítě bez jakékoli dodatečné konfigurace. Pokud virtuální sítě připojen k síti na pracovišti, aplikací ve vaší App Service Environment také mít přístup k prostředkům existuje. Nemusíte konfigurovat App Service Environment nebo aplikace pro všechny další.

![Externí App Service Environment][1] 

Pokud máte externí App Service Environment, veřejné VIP je také koncového bodu, který aplikace app Service Environment řešení pro:

* HTTP/S. 
* FTP/S. 
* Nasazení webu.
* Vzdálené ladění.

![ILB APP SERVICE ENVIRONMENT][2]

Pokud máte App Service Environment ILB, IP adresa ILB je koncový bod pro protokol HTTP/S, FTP/S, nasazení webu a vzdálené ladění.

Normální aplikace přístupové porty jsou:

| Použití | Z | Akce |
|----------|---------|-------------|
|  PROTOKOL HTTP NEBO HTTPS  | Konfigurovatelná uživatelem |  80, 443 |
|  FTP/FTPS    | Konfigurovatelná uživatelem |  21, 990, 10001-10020 |
|  Visual Studio vzdálené ladění  |  Konfigurovatelná uživatelem |  4016, 4018, 4020, 4022 |

To platí, pokud jste na externí App Service Environment nebo ILB App Service Environment. Pokud jste na externí App Service Environment, stiskněte tlačítko tyto porty na veřejných virtuálních IP adres. Pokud jste v App Service Environment ILB, stiskněte tlačítko tyto porty na ILB. Pokud se uzamknout port 443, může být dopad na některé funkce umístěné na portálu. Další informace najdete v tématu [portál závislosti](#portaldep).

## <a name="ase-dependencies"></a>Závislosti App Service Environment ##

App Service Environment příchozí přístup závislostí je:

| Použití | Z | Akce |
|-----|------|----|
| Správa | Adresy pro správu služby aplikace | Podsíť App Service Environment: 454, 455 |
|  Interní komunikaci App Service Environment | Podsíť App Service Environment: všechny porty | Podsíť App Service Environment: všechny porty
|  Povolit pro vyrovnávání zatížení Azure příchozí | Nástroje pro vyrovnávání zatížení Azure | Podsíť App Service Environment: všechny porty
|  Aplikace přiřazené IP adresy | Aplikace, které jsou přiřazeny adresy | Podsíť App Service Environment: všechny porty

Příchozí provoz poskytuje příkazy a ovládání App Service Environment kromě sledování systému. Zdroj IP adres pro tyto přenosy dat jsou uvedené v [adresy App Service Environment správu] [ ASEManagement] dokumentu. Konfigurace zabezpečení sítě je potřeba povolit přístup ze všech IP adres na portech 454 a 455.

V rámci podsítě App Service Environment nejsou mnoho porty používané pro interní součást komunikace a můžete změnit.  To vyžaduje všechny porty v podsíti App Service Environment, aby mohl být dostupný z podsítě App Service Environment. 

Pro komunikaci mezi nástroje pro vyrovnávání zatížení Azure a podsíť App Service Environment jsou minimální porty, které musí být otevřený 454 a 455 16001. 16001 port se používá pro zachování připojení provoz mezi nástroje pro vyrovnávání zatížení a App Service Environment. Pokud používáte App Service Environment ILB pak zamknete provoz dolů právě 454, 455, 16001 porty.  Pokud používáte externí App Service Environment budete muset vzít v úvahu přístupové porty normální aplikace.  Pokud používáte aplikaci přiřazenou adresy budete muset otevřít na všechny porty.  Jakmile bude přiřazena adresa pro konkrétní aplikaci, nástroj pro vyrovnávání zatížení bude používat porty, které nejsou známé z předem odeslat HTTP a HTTPS provozy App Service Environment.

Pokud používáte aplikace přiřazené IP adresy, budete muset povolit přenosy z přiřazené vašich aplikací App Service Environment podsítě IP adres.

Pro odchozí přístup App Service Environment závisí na několika externími systémy. Tyto závislosti systému jsou definovány s názvy DNS a nejsou mapovány na sadu pevné IP adresy. Proto App Service Environment vyžaduje přístup z podsítě App Service Environment všechny externí IP adres napříč různými porty. App Service Environment obsahuje následující odchozí závislosti:

| Použití | Z | Akce |
|-----|------|----|
| Azure Storage | Podsíť App Service Environment | Table.Core.Windows.NET, blob.core.windows.net, queue.core.windows.net, file.core.windows.net: 80, 443, 445 (445 je potřeba jenom pro ASEv1.) |
| Azure SQL Database | Podsíť App Service Environment | Database.Windows.NET: 1433, 11000 11999, 14000 14999 (Další informace najdete v tématu [využití portu SQL Database verze 12](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).)|
| Správa Azure | Podsíť App Service Environment | Management.Core.Windows.NET, management.azure.com: 443 
| Ověření certifikátu SSL |  Podsíť App Service Environment            |  OCSP.msocsp.com, mscrl.microsoft.com, crl.microsoft.com: 443
| Azure Active Directory        | Podsíť App Service Environment            |  Internet: 443
| Správa služby App Service        | Podsíť App Service Environment            |  Internet: 443
| Azure DNS                     | Podsíť App Service Environment            |  Internet: 53
| Interní komunikaci App Service Environment    | Podsíť App Service Environment: všechny porty |  Podsíť App Service Environment: všechny porty

Pokud App Service Environment ztratí přístup k tyto závislosti, přestane fungovat. Pokud k tomu dojde dost dlouho, je pozastaven App Service Environment.

### <a name="customer-dns"></a>Zákazník DNS ###

Pokud jsou virtuální síť nakonfigurované zákazník definovaný server DNS, úlohy klientů ji použít. App Service Environment se stále musí komunikovat s Azure DNS pro účely správy. 

Pokud jsou virtuální síť nakonfigurované zákazník DNS na druhé straně sítě VPN, musí být dostupný z podsítě, která obsahuje App Service Environment serverem DNS.

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portál závislosti ##

Kromě funkční závislosti App Service Environment existuje několik další položky týkající se práce s portálem. Některé z možností na portálu Azure závisí na přímý přístup k _SCM lokality_. Pro každou aplikaci ve službě Azure App Service existují dvě adresy URL. První adresa URL je přístup k vaší aplikaci. Druhý adresa URL je pro přístup k webu SCM, což se označuje taky jako _Kudu konzoly_. Funkce, které používají SCM lokality:

-   Webové úlohy
-   Funkce
-   Protokol streamování
-   Kudu
-   Rozšíření
-   Průzkumník procesů
-   Konzola

Pokud používáte App Service Environment ILB, lokalita SCM není dostupné z oblasti mimo virtuální sítě internet. Pokud vaše aplikace je hostitelem App Service Environment ILB, některé funkce nebudou fungovat z portálu.  

Mnoho z těchto možností, které závisí na webu SCM jsou k dispozici přímo v konzole Kudu. Můžete se připojit k němu přímo, a nikoli pomocí portálu. Pokud je vaše aplikace hostovaná v App Service Environment ILB, používáte pro přihlášení vaše přihlašovací údaje pro publikování. Adresa URL pro přístup k webu SCM aplikace hostované v App Service Environment ILB má následující formát: 

```
<appname>.scm.<domain name the ILB ASE was created with> 
```

Pokud vaše App Service Environment ILB je název domény *contoso.net* a je název vaší aplikace *testapp*, aplikace je k dispozici na adrese *testapp.contoso.net*. SCM webu, která jde s ním je k dispozici na adrese *testapp.scm.contoso.net*.

## <a name="functions-and-web-jobs"></a>Funkce a webové úlohy ##

Funkce a webové úlohy závisí na webu SCM, ale jsou podporovány pro použití na portálu, i v případě, že vaše aplikace nacházely ve ILB App Service Environment, dokud bude prohlížeč dosáhnout SCM lokality.  Pokud používáte certifikát podepsaný svým držitelem s vaší App Service Environment ILB, musíte povolit prohlížeč důvěřovat certifikátu.  Pro aplikaci Internet Explorer a Edge, tzn. certifikát musí být v úložišti počítače vztah důvěryhodnosti.  Pokud používáte Chrome, pak to znamená, že jste dříve přijali certifikát v prohlížeči zasažení pravděpodobně z důvodu webu scm přímo.  Nejlepším řešením je použití komerční certifikát, který je v prohlížeči řetěz certifikátů.  

## <a name="ase-ip-addresses"></a>App Service Environment IP adresy ##

App Service Environment má několik IP adres znát. Jsou:

- **Veřejná IP adresa příchozí**: použít pro provoz aplikace v externím App Service Environment a přenosy správy v App Service Environment ILB i externí App Service Environment.
- **Odchozí veřejnou IP adresu**: používá jako IP adresa "od" pro odchozí připojení z App Service Environment která opouští virtuální sítě, které nejsou směrovány dolů sítě VPN.
- **ILB IP adresu**: Pokud používáte ILB App Service Environment.
- **Přiřazené aplikace založená na IP adresy**: jedinou možnou s externí App Service Environment a je nakonfigurovaný na základě IP protokol SSL.

Tyto IP adresy jsou snadno viditelná v ASEv2 na portálu Azure z rozhraní App Service Environment. Pokud máte App Service Environment ILB, IP adresu pro ILB je uvedený.

![IP adresy][3]

### <a name="app-assigned-ip-addresses"></a>Aplikace přiřazené IP adresy ###

S externí App Service Environment můžete přiřadit IP adresy pro jednotlivé aplikace. Nejde udělat s ILB App Service Environment. Další informace o tom, jak nakonfigurovat aplikaci, aby měla vlastní IP adresu, najdete v části [vazby stávající vlastní certifikát SSL pro službu Azure web apps](../app-service-web-tutorial-custom-ssl.md).

Pokud aplikace má vlastní založená na IP adresu, App Service Environment si vyhrazuje dva porty pro mapování na tuto IP adresu. Je jeden port pro přenos HTTP a další port, který je pro protokol HTTPS. Tyto porty jsou uvedeny v uživatelském rozhraní App Service Environment v části IP adresy. Přenosy musí být schopný dosáhnout tyto porty z virtuální IP adresu nebo aplikace jsou nedostupné. Tento požadavek je důležité si pamatovat při konfiguraci skupin zabezpečení sítě (Nsg).

## <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě) ##

[Skupin zabezpečení sítě] [ NSGs] umožňují řídit přístup k síti v rámci virtuální sítě. Pokud používáte portál, je na nejnižší prioritu tak, aby odepřel všechno, co pravidlo implicitní odepřít. Můžete vytvořit jsou vaše povolí pravidla.

V App Service Environment nemáte přístup k virtuálním počítačům použity k hostování App Service Environment, sám sebe. Jsou v předplatném spravovaný společností Microsoft. Pokud chcete omezit přístup k aplikacím na App Service Environment, nastavte skupiny Nsg na podsítě App Service Environment. Při tom věnujte pozornost pečlivě závislosti App Service Environment. Pokud zablokujete všechny závislosti, App Service Environment přestane fungovat.

Skupiny Nsg se dá nakonfigurovat pomocí portálu Azure nebo pomocí prostředí PowerShell. Zde uvedené informace zobrazí na portálu Azure. Vytvořit a spravovat skupiny Nsg na portálu jako prostředek nejvyšší úrovně v rámci **sítě**.

Pokud jsou požadavky na příchozí a odchozí vzít v úvahu, by mělo vypadat jako skupiny Nsg uvedeno v tomto příkladu skupin Nsg. Rozsah adres virtuální sítě je _192.168.250.0/16_, a podsíť, který je App Service Environment ve _192.168.251.128/25_.

První dva příchozí požadavky pro App Service Environment funkce se zobrazí v horní části seznamu v tomto příkladu. Jejich umožňuje správu App Service Environment a App Service Environment pro komunikaci se sám sebe. Jiné položky jsou všechny konfigurovatelné klienta a můžou řídit přístup k síti pro aplikace spouštěné v App Service Environment. 

![Příchozí pravidla zabezpečení][4]

Výchozí pravidlo umožňuje IP adresy ve virtuální síti, aby komunikoval s App Service Environment podsítě. Jiné výchozí pravidlo umožňuje Vyrovnávání zatížení, také známé jako veřejné VIP, ke komunikaci s App Service Environment. Pokud chcete zobrazit výchozí pravidla, vyberte **výchozí pravidla** vedle **přidat** ikonu. Když vložíte odepření všechno ostatní pravidla po pravidla NSG uvedené, abyste zabránili provoz mezi virtuální IP adresu a App Service Environment. Pokud chcete zabránit provoz přicházející ze uvnitř virtuální sítě, přidáte vlastní pravidlo povolující příchozí. Použít zdroj rovna AzureLoadBalancer s cílovým serverem **žádné** a rozsah portů  **\*** . Protože pravidla NSG se použije k podsíti App Service Environment, nemusíte být v cílovém specifické.

Pokud jste přiřadili IP adresu do vaší aplikace, zajistěte, aby že nechat otevřené porty. Pokud chcete zobrazit porty, vyberte **App Service Environment** > **IP adresy**.  

Všechny položky, které jsou uvedené v následující odchozí pravidla, je potřeba, s výjimkou poslední položky. Umožňují přístup k síti do app Service Environment závislosti, které jste si poznamenali dříve v tomto článku. Pokud některý z nich zablokujete, vaše App Service Environment přestane fungovat. Poslední položky v seznamu umožňuje vaší App Service Environment ke komunikaci s další prostředky ve virtuální síti.

![Odchozí pravidla zabezpečení][5]

Po skupin Nsg jsou definovány, přiřadíte k podsíti, který vaše App Service Environment je na. Pokud si nepamatujete App Service Environment virtuální síť nebo podsíť, zobrazí se z portálu pro správu App Service Environment. Chcete-li přiřadit NSG pro podsíť, přejděte k podsíti uživatelského rozhraní a vyberte NSG.

## <a name="routes"></a>Trasy ##

Trasy stát nejčastěji problematické, pokud konfigurujete virtuální síť s Azure ExpressRoute. Existují tři typy tras ve virtuální síti:

-   Systémové trasy
-   Trasy protokolu BGP
-   Trasy definované uživatelem (udr)

Trasy protokolu BGP přepsat systémové trasy. Udr přepsat trasy protokolu BGP. Další informace o trasách v Azure virtuální sítě najdete v tématu [trasy definované uživatelem přehled][UDRs].

Azure SQL database, která App Service Environment se používá ke správě systému má bránu firewall. To vyžaduje komunikaci z veřejné VIP App Service Environment. Připojení k databázi SQL z App Service Environment bude odepřen, pokud jsou odesílány mimo provoz připojení ExpressRoute a na jinou IP adresu.

Pokud jsou odpovědi na příchozí požadavky na správu odeslat dolů ExpressRoute, se liší od původního cíle na zpáteční adresu. Tato neshoda dělí komunikaci TCP.

Pro vaše App Service Environment, kterou práci, zatímco jsou vaše virtuální síť nakonfigurované ExpressRoute je nejjednodušší cesta:

-   Konfigurace ExpressRoute a nabídnout _0.0.0.0/0_. Ve výchozím nastavení, je vynutit tunely všechny odchozí přenosy na místě.
-   Vytvořte UDR. Platí pro podsíť, která obsahuje App Service Environment se předponu adresy z _0.0.0.0/0_ a dalšího směrování typ _Internet_.

Pokud provedete tyto dvě změny, určené internetové komunikaci z podsítě App Service Environment není vynutit dolů ExpressRoute a App Service Environment funguje. 

> [!IMPORTANT]
> Trasy definované v UDR musí být dost konkrétní, aby mají přednost před všechny trasy inzerované konfigurace ExpressRoute. V předchozím příkladu používá 0.0.0.0/0 široký rozsah adres. Se dá potenciálně omylem přepsat inzerování tras, které používají podrobnější rozsahy adres.
>
> Konfigurace ExpressRoute, které mezi Inzerovat trasy z cesty partnerského vztahu veřejný partnerský vztah privátní cestu ASEs nepodporuje. Konfigurace ExpressRoute s veřejné partnerské vztahy nakonfigurované dostávat inzerování trasy od Microsoftu. Tato oznámení obsahují velké sady rozsahů adres Microsoft Azure IP. Pokud rozsahy adres ohlášené mezi v cestě k vytvoření soukromého partnerského vztahu, jsou všechny odchozí síťových paketů z podsítě App Service Environment force tunelovým propojením zákazníka místní síťové infrastruktuře. ASEs aktuálně nepodporuje tento tok sítě. Jedno řešení tohoto problému se trasy mezi – reklamu z cesty partnerského vztahu veřejné do cesty partnerského vztahu privátní ukončit.

Pokud chcete vytvořit UDR, postupujte takto:

1. Přejděte na portálu Azure. Vyberte **sítě** > **směrovacích tabulek**.

2. Vytvořte novou tabulku směrování ve stejné oblasti jako virtuální síť.

3. V rámci tabulky tras uživatelského rozhraní, vyberte **trasy** > **přidat**.

4. Nastavte **typ dalšího směrování** k **Internet** a **předpona adresy** k **0.0.0.0/0**. Vyberte **Uložit**.

    Zobrazí se přibližně takto:

    ![Funkční trasy][6]

5. Když vytvoříte nový směrovací tabulka, přejděte na podsíť, která obsahuje vaše App Service Environment. Vyberte směrovací tabulku ze seznamu na portálu. Po uložení změn, měli byste vidět pak skupiny Nsg a trasy zaznamenán s podsíť.

    ![Skupiny Nsg a trasy][7]

### <a name="deploy-into-existing-azure-virtual-networks-that-are-integrated-with-expressroute"></a>Nasazení do virtuální sítě Azure, které jsou integrované s ExpressRoute ###

K nasazení vaší App Service Environment do virtuální sítě, která je integrovaná s ExpressRoute, předem nakonfigurujte podsítě, kam chcete App Service Environment nasazení. Potom ji nasadit pomocí šablony Resource Manageru. Vytvořit App Service Environment ve virtuální síti, již má nakonfigurované ExpressRoute:

- Vytvoření podsítě pro hostování App Service Environment.

    > [!NOTE]
    > V podsíti, ale App Service Environment může být nic jiného. Je třeba zvolit adresní prostor, který umožňuje růstem do budoucna. Nelze změnit, tato nastavení později. Doporučujeme velikost `/25` adresy 128.

- Vytvoření udr (například směrovací tabulky), jak je popsáno výše a nastavte ho na podsíť.
- Vytvořit App Service Environment pomocí šablony Resource Manageru, jak je popsáno v [vytvořit App Service Environment pomocí šablony Resource Manageru][MakeASEfromTemplate].

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ASEManagement]: ./management-addresses.md
