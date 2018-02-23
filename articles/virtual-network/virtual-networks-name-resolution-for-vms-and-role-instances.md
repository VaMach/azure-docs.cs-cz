---
title: "Překlad názvů pro virtuální počítače a instance rolí"
description: "Název řešení scénáře Azure IaaS, hybridní řešení, která mezi jiné cloudové služby, služby Active Directory a pomocí serveru DNS."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: jdial
ms.openlocfilehash: 5ea3e4ad68fd37ccaa6e081febe4827bdb2e196d
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Překlad názvů pro virtuální počítače a instance rolí

V závislosti na tom, jak používat Azure k hostování IaaS a PaaS, hybridní řešení musíte povolit virtuální počítače a instance rolí, které vytvoříte pro komunikaci mezi sebou. I když tato komunikace lze provést pomocí IP adresy, je mnohem jednodušší použít názvy, které můžete snadno zapamatovaných a se nezmění. 

Pokud potřebujete překládat názvy domén na interní IP adresy instance rolí a virtuální počítače hostované v Azure, mohou používat jednu ze dvou metod:

* [Rozlišení názvů Azure](#azure-provided-name-resolution)
* [Překlad názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server) (které může předávat dotazy na servery DNS poskytnutých platformou Azure) 

Typ překladu adres, které můžete použít závisí na tom, jak vaše virtuální počítače a instance rolí musí vzájemně komunikovat. Následující tabulka uvádí scénáře a odpovídající název řešení řešení:

| **Scénář** | Řešení | **Přípona** |
| --- | --- | --- |
| Název rozlišení mezi instancí role nebo umístěný ve stejné cloudové služby nebo virtuální sítě virtuálních počítačů. |[Rozlišení názvů Azure](#azure-provided-name-resolution) |název hostitele nebo plně kvalifikovaný název domény |
| Překlad názvů ze Azure App Service (webová aplikace, funkce nebo robota) pomocí integrace virtuální sítě do instancí role nebo virtuální počítače umístěné ve stejné virtuální síti. |Spravované zákazníkem servery DNS předávání dotazů mezi virtuálními sítěmi pro řešení Azure (DNS proxy). V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server). |Pouze plně kvalifikovaný název domény |
| Překlad mezi instancí role nebo virtuální počítače umístěné v různých virtuálních sítích. |Spravované zákazníkem servery DNS předávání dotazů mezi virtuálními sítěmi pro řešení Azure (DNS proxy). V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server). |Pouze plně kvalifikovaný název domény |
| Překlad z webové aplikace služby App Service pro virtuální počítače umístěné ve stejné virtuální síti. |Spravované zákazníkem servery DNS předávání dotazů mezi virtuálními sítěmi pro řešení Azure (DNS proxy). V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server). |Pouze plně kvalifikovaný název domény |
| Překlad z webové aplikace služby App Service pro virtuální počítače nachází v jiné virtuální síti. |Spravované zákazníkem servery DNS předávání dotazů mezi virtuálními sítěmi pro řešení Azure (DNS proxy). V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server-for-web-apps). |Pouze plně kvalifikovaný název domény |
| Rozlišení názvů místní počítač a služby z instancí role nebo virtuálních počítačů v Azure. |Spravované zákazníkem servery DNS (místní řadič domény, řadiče místní domény jen pro čtení nebo sekundární DNS synchronizovat, například pomocí přenosy zón,). V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server). |Pouze plně kvalifikovaný název domény |
| Řešení Azure názvy hostitelů z místního počítače. |Předávaly dotazy na zákazníkem spravovaný proxy server DNS v odpovídající virtuální síť proxy server předává dotazy k Azure pro překlad. V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server). |Pouze plně kvalifikovaný název domény |
| Reverse DNS pro interní IP adresy. |[Překlad názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server). |Neuvedeno |
| Překlad mezi virtuální počítače nebo instance rolí, které jsou umístěné v různých cloudové služby, není ve virtuální síti. |Není k dispozici. Připojení mezi virtuální počítače a instance rolí v různých cloudové služby není podporované mimo virtuální síť. |Neuvedeno|

## <a name="azure-provided-name-resolution"></a>Rozlišení názvů Azure

Společně s překladu názvů DNS veřejné Azure poskytuje interní překlad adres pro virtuální počítače a instance rolí, které se nacházejí v rámci stejné virtuální síti nebo cloudové služby. Virtuální počítače nebo instance v cloudové službě sdílejí stejnou příponu DNS (takže stačí samotná název hostitele), ale v různých cloudu klasické virtuální sítě služby mají různé přípony DNS, plně kvalifikovaný název domény je nutný k překladu názvů různých cloudových služeb. Ve virtuálních sítích v modelu nasazení Resource Manager přípona DNS je konzistentní napříč virtuální sítě (takže plně kvalifikovaný název domény není nezbytný) a názvy DNS lze přiřadit k síťové karty a virtuální počítače. I když Azure překlad nevyžaduje žádnou konfiguraci, není příslušnou volbu pro všechny scénáře nasazení, jak je vidět v předchozí tabulce.

> [!NOTE]
> Při použití webových a pracovních rolí, můžete taky přejít interní IP adresy instance rolí, které jsou založené na role název a číslo instance pomocí REST API pro správu služby Azure. Další informace najdete v tématu [služby referenční dokumentace rozhraní API REST správy](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features"></a>Funkce

* Snadné použití: Chcete-li používat Azure překlad není nutná žádná konfigurace.
* Služba Azure název řešení je vysoce dostupný, ukládání, je potřeba vytvořit a spravovat clustery serverů DNS.
* Můžete použít ve spojení s vlastní servery DNS přeložit na místě a Azure názvy hostitelů.
* Název řešení je k dispozici mezi instancí role virtuálních počítačů v rámci stejné cloudové služby bez nutnosti plně kvalifikovaný název domény.
* Název řešení je k dispozici mezi virtuálními počítači ve virtuálních sítích, které používají model nasazení Resource Manager, bez nutnosti plně kvalifikovaný název domény. Při překladu názvů v různých cloudové služby, virtuálních sítí v modelu nasazení classic vyžadují plně kvalifikovaný název domény. 
* Názvy hostitelů, které nejlépe popisují vaše nasazení, můžete místo práce s automaticky generovaných názvů.

### <a name="considerations"></a>Požadavky

* Přípona DNS vytvořit Azure nelze upravit.
* Nelze ručně zaregistrovat vlastní záznamy.
* Nejsou podporovány WINS a NetBIOS (nejde zobrazit vaše virtuální počítače v Průzkumníku Windows).
* Názvy hostitelů musí být kompatibilní s DNS. Názvy musí používat pouze 0 – 9, a – z a '-' a nesmí začínat ani končit '-'. Najdete v dokumentu RFC 3696 část 2.
* Pro každý virtuální počítač je omezen provoz dotazu DNS. Omezení by nemělo mít vliv na většinu aplikací. Pokud je dodržena omezení požadavků, ujistěte se, zda je povoleno ukládání do mezipaměti na straně klienta. Další informace najdete v tématu [maximální využití služby Azure překlad](#Getting-the-most-from-Azure-provided-name-resolution).
* Pouze virtuální počítače v první 180 cloudové služby jsou registrované pro každou virtuální síť v modelu nasazení classic. Toto omezení se nevztahuje k virtuálním sítím v modelu nasazení Resource Manager.

## <a name="dns-client-configuration"></a>Konfigurace klienta DNS

### <a name="client-side-caching"></a>Ukládání do mezipaměti na straně klienta

Ne každý dotaz DNS musí být odesílají přes síť. Ukládání do mezipaměti na straně klienta pomáhá snížit latenci a zlepšit odolnost proti sítě blips vyřešte opakující se dotazy DNS z místní mezipaměti. Záznamy DNS obsahovat hodnotu time to live (TTL) což umožňuje do mezipaměti ukládat záznam pro stejně dlouho bez dopadu záznamů aktuálnosti tak ukládání do mezipaměti na straně klienta je vhodná pro většinu situacích.

Výchozí nastavení klienta DNS systému Windows má integrovanou mezipaměť DNS. Některých distribucích systému Linux nezahrnují ukládání do mezipaměti ve výchozím nastavení. Přidání mezipaměť DNS pro každý virtuální počítač Linux (po kontrole, zda není k dispozici místní mezipaměti již), se nedoporučuje.

Existuje několik různých DNS ukládání do mezipaměti balíčky, které jsou k dispozici. Například dnsmasq. Následující kroky seznamu instalace dnsmasq na nejběžnější distribucích:

* **Ubuntu (používá resolvconf)**:
  * Instalovat balíček dnsmasq s `sudo apt-get install dnsmasq`.
* **SUSE (používá netconf)**:
  * Instalovat balíček dnsmasq s `sudo zypper install dnsmasq`.
  * Povolit službu dnsmasq s `systemctl enable dnsmasq.service`. 
  * Spusťte službu dnsmasq s `systemctl start dnsmasq.service`. 
  * Upravit **/etc/sysconfig/network/config** a změňte *NETCONFIG_DNS_FORWARDER = ""* k *dnsmasq*.
  * Aktualizovat resolv.conf s `netconfig update` nastavení mezipaměti jako místní překladač služby DNS.
* **OpenLogic (používá NetworkManager)**:
  * Instalovat balíček dnsmasq s `sudo yum install dnsmasq`.
  * Povolit službu dnsmasq s `systemctl enable dnsmasq.service`.
  * Spusťte službu dnsmasq s `systemctl start dnsmasq.service`.
  * Přidat *předřadit domény názvové servery 127.0.0.1;* k **/etc/dhclient-eth0.conf**.
  * Restartujte síťovou službu s `service network restart` nastavení mezipaměti jako místní překladač služby DNS.

> [!NOTE]
> Balíček 'dnsmasq' je pouze jeden z mnoha mezipaměti DNS, která je k dispozici pro Linux. Před jeho použitím, zkontrolujte jeho vhodnosti pro konkrétních potřeb a žádné jiné mezipaměti nainstalovaného.
> 
> 
    
### <a name="client-side-retries"></a>Opakování na straně klienta

DNS je primárně protokol UDP. Jako protokol UDP nezaručuje doručení zpráv, je logika opakovaných pokusů zpracována v samotný protokol DNS. Každý klient DNS (operačního systému) může vykazovat různé opakování logiku v závislosti na nástroj creator předvoleb:

* Operační systémy Windows znova za jeden druhý a pak znovu po jiné 2, 4 a jiné čtyři sekund. 
* Výchozí opakování instalace Linux po pět sekund. Změna opakovaném na pětkrát v intervalech 1 sekundu, se doporučuje.

Zkontrolujte aktuální nastavení na virtuální počítač s Linuxem pomocí `cat /etc/resolv.conf`. Podívejte se na *možnosti* řádek, například:

```bash
options timeout:1 attempts:5
```

Soubor resolv.conf je obvykle automaticky generovaný a by neměla být upravována. Konkrétní kroky pro přidání *možnosti* řádku se liší podle distro:

* **Ubuntu** (používá resolvconf):
  * Přidání možnosti řádek pro **/etc/resolveconf/resolv.conf.d/head**.
  * Spustit `resolvconf -u` aktualizovat.
* **SUSE** (používá netconf):
  * Přidat *timeout:1 pokusů: 5* k **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parametr v **/etc/sysconfig/network/config**. 
  * Spustit `netconfig update` aktualizovat.
* **OpenLogic** (používá NetworkManager):
  * Přidat *echo "možnosti timeout:1 pokusů: 5"* k **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  * Aktualizace s `service network restart`.

## <a name="name-resolution-using-your-own-dns-server"></a>Překlad názvů pomocí serveru DNS

### <a name="vms-and-role-instances"></a>Virtuální počítače a instance rolí

Existuje několik situací, kde potřeb název řešení může přejít nad rámec funkce poskytované službou Azure, například při použití domén služby Active Directory, nebo pokud vyžadují překlad DNS mezi virtuálními sítěmi. Tak, aby pokrýval scénáře, Azure poskytuje možnost musíte použít vlastní servery DNS.

Servery DNS v rámci virtuální sítě může předat dotazy DNS Azure rekurzivní překladače přeložit názvy hostitelů v rámci této virtuální sítě. Řadič domény (DC), který běží v Azure můžete například odpovídat na dotazy DNS pro jeho domény a předávat všechny další dotazy k Azure. Předávání dotazů umožňuje virtuálních počítačů k místním prostředkům (přes řadič domény) a Azure názvy hostitelů (prostřednictvím předávání). Přístup k Azure rekurzivní překladače se poskytuje prostřednictvím virtuální IP adresa 168.63.129.16.

Předávání DNS také umožňuje překlad DNS propojování virtuálních sítí a povoluje vaše místní počítače přeložit názvy hostitelů Azure. Aby bylo možné přeložit název hostitele Virtuálního počítače, serveru DNS virtuální počítač se musí nacházet ve stejné virtuální síti a nakonfigurovat tak, aby dotazy dopředného název hostitele Azure. Jako přípona DNS se liší v každé virtuální sítě, můžete použít pravidla pro podmíněné předávání aby odesílalo dotazy DNS na správnou virtuální síť pro překlad. Následující obrázek ukazuje dvě virtuální sítě a místní síti provádění propojování virtuálních sítí překlad DNS pomocí této metody. Předávání DNS příklad je k dispozici v [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) a [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Instance role můžete provádět překlad virtuálních počítačů v rámci stejné virtuální síti pomocí plně kvalifikovaný název domény, který používá název virtuálního počítače spolu s příponou DNS "internal.cloudapp.net". V takovém případě překlad je však pouze úspěchu, pokud má název virtuálního počítače, které jsou definované v instanci role [Role schématu (soubor .cscfg)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Instance rolí, které je třeba provádět překlad virtuálních počítačů v jinou virtuální sítí (plně kvalifikovaný název domény pomocí **internal.cloudapp.net** příponu) muset učinit pomocí vlastní servery DNS předávání mezi dvěma virtuálními sítěmi, jak je popsáno v v této části.
>

![Propojování virtuálních sítí DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Při použití Azure překlad IP adres interní příponu DNS (`*.internal.cloudapp.net`) poskytuje pro každý virtuální počítač Azure a DHCP. Tato přípona umožňuje rozlišení názvů hostitelů, jako jsou záznamy název hostitele v *internal.cloudapp.net* zóny. Při použití vlastní název řešení řešení, tato přípona se nedodává k virtuálním počítačům, protože ji naruší jiné DNS architektury (jako je připojený k doméně scénáře). Místo toho Azure poskytuje nefunkční zástupný znak (*reddog.microsoft.com*).

V případě potřeby příponu interní DNS se dá určit pomocí prostředí PowerShell nebo rozhraní API:

* Pro virtuální sítě v modelech nasazení Resource Manager, je k dispozici prostřednictvím přípona [karty síťového rozhraní](virtual-network-network-interface.md) prostředků nebo prostřednictvím [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) rutiny.
* Ve model nasazení classic, je k dispozici prostřednictvím přípona [získat rozhraní API nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) volání nebo prostřednictvím [Get-AzureVM – ladění](/powershell/module/azure/get-azurevm) rutiny.

Pokud předávání dotazů do Azure, není vašim potřebám, potřebujete poskytovat řešení DNS. Musí vaše řešení DNS:

* Zadejte odpovídající název hostitele řešení prostřednictvím [DDNS](virtual-networks-name-resolution-ddns.md), např. Všimněte si, pokud pomocí DDNS budete muset zakázat úklidu záznamů DNS zapůjčení DHCP Azure jsou dlouhé a proces úklidu může odebrat DNS zaznamenává předčasně. 
* Zadejte odpovídající rekurzivní překlad názvů povolit překlad názvů externí domény.
* Být přístupné (TCP a UDP na port 53) z klientů, které slouží a mít přístup k Internetu.
* Nutné zabezpečit před přístupem z Internetu, zmírnit hrozby představované externí agenty.

> [!NOTE]
> Pro nejlepší výkon, při použití virtuálních počítačích Azure jako servery DNS, by mělo být zakázáno IPv6 a [veřejná IP adresa na úrovni Instance](virtual-networks-instance-level-public-ip.md) by se měla přiřadit k jednotlivým serverům DNS virtuálního počítače. Analýza výkonu další a optimalizace, které při použití Windows serveru jako serveru DNS, najdete v části [název řešení výkon rekurzivní Windows DNS Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web Apps
Pokud potřebujete provést překlad z vaší aplikace služby webové aplikace se propojit k virtuální síti, na virtuální počítače ve stejné virtuální síti, pak kromě nastavení vlastního serveru DNS, který má server pro předávání DNS, předá dotazy do Azure (virtuální IP adresy 168.63.129.16) , musíte také provést následující kroky:
* Povolit integraci virtuální sítě pro webové aplikace App Service, pokud neudělali, jak je popsáno v [integraci aplikace s virtuální sítí](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Na portálu Azure pro plán aplikační služby hostování webové aplikace, vyberte **synchronizace sítě** pod **sítě**, **integrace virtuální sítě**, jak je znázorněno v následujícím Obrázek:

    ![Překlad virtuální sítě webové aplikace](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Překladu názvů z webové aplikace App Service propojené k virtuální síti, na virtuální počítače v jinou virtuální síť vyžaduje použití vlastní servery DNS na obě virtuální sítě, následujícím způsobem:
* Nastavení serveru DNS ve virtuální síti cíl na virtuální počítač, který můžete dál dotazy k Azure rekurzivní překladač (virtuální IP adresy 168.63.129.16). Předávání DNS příklad je k dispozici v [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) a [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Nastavení DNS pro předávání ve virtuální síti zdroje na virtuálním počítači. Nakonfigurujte toto předávání DNS pro předávání dotazů na server DNS ve virtuální síti cíl.
* Nakonfigurujte zdrojového serveru DNS virtuální sítě zdroje.
* Povolit integraci virtuální sítě pro webové aplikace App Service propojení virtuální sítě zdroje podle pokynů v [integraci aplikace s virtuální sítí](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Na portálu Azure pro plán aplikační služby hostování webové aplikace, vyberte **synchronizace sítě** pod **sítě**, **integrace virtuální sítě**. 

## <a name="specifying-dns-servers"></a>Určení serverů DNS
Pokud používáte vlastní servery DNS, Azure poskytuje možnost se určením několika DNS serverů pro virtuální síť, nebo v síťové rozhraní (Resource Manager) nebo je Cloudová služba (klasické). Servery DNS zadané pro cloudové služby nebo síťová rozhraní získat přednost přes servery DNS zadané pro virtuální síť.

> [!NOTE]
> Vlastnosti síťového připojení, například server DNS IP adresy, by neměla být upravována přímo v rámci virtuálních počítačů Windows může získat vymazat během služby retušovat při získá nahradit virtuální síťový adaptér. 
> 
> 

Při použití modelu nasazení Resource Manager, servery DNS lze na portálu, rozhraní API nebo šablony: [virtuální síť](https://msdn.microsoft.com/library/azure/mt163661.aspx) a [síťové rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx), nebo v prostředí PowerShell: [virtuální síť ](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork) a [síťové rozhraní](/powershell/module/azurerm.network/new-azurermnetworkinterface).

Při použití modelu nasazení classic, servery DNS pro virtuální sítě lze na portálu nebo [ *konfigurace sítě* soubor](https://msdn.microsoft.com/library/azure/jj157100). Pro cloudové služby, servery DNS nejsou zadány prostřednictvím [ *konfigurace služby* soubor](https://msdn.microsoft.com/library/azure/ee758710) nebo pomocí prostředí PowerShell, [New-AzureVM](/powershell/module/azure/new-azurevm).

> [!NOTE]
> Pokud změníte nastavení DNS pro virtuální sítě nebo virtuální počítač, který je už nasazená, budete muset restartovat jednotlivé ovlivněné virtuální počítače změny se projeví.
> 
> 

## <a name="next-steps"></a>Další postup

Model nasazení Resource Manager:

* [Vytvořit nebo aktualizovat virtuální síť](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Vytvořit nebo aktualizovat karty síťového rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)
* [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)

Model nasazení Classic:

* [Schéma konfigurace služby Azure](https://msdn.microsoft.com/library/azure/ee758710)
* [Schéma konfigurace virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100)
* [Konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md)
