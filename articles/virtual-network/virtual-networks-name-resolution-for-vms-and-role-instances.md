---
title: "Řešení pro virtuální počítače a instance rolí"
description: "Název řešení scénáře pro Azure IaaS, hybridní řešení, která mezi jiné cloudové služby, služby Active Directory a pomocí serveru DNS "
services: virtual-network
documentationcenter: na
author: GarethBradshawMSFT
manager: carmonm
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2016
ms.author: telmos
ms.openlocfilehash: 479cf8cf358d0b242d8ce030d8639b493e4767d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Překlad názvů pro instance rolí a VM
V závislosti na tom, jak používat Azure k hostování IaaS a PaaS, hybridní řešení musíte povolit virtuální počítače a instance rolí, které vytvoříte pro komunikaci mezi sebou. I když tato komunikace lze provést pomocí IP adresy, je mnohem jednodušší použít názvy, které můžete snadno zapamatovaných a se nezmění. 

Pokud potřebujete překládat názvy domén na interní IP adresy instance rolí a virtuální počítače hostované v Azure, mohou používat jednu ze dvou metod:

* [Rozlišení názvů Azure](#azure-provided-name-resolution)
* [Překlad názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server) (které může předávat dotazy na servery DNS poskytnutých platformou Azure) 

Typ překladu adres, které můžete použít závisí na tom, jak vaše virtuální počítače a instance rolí musí vzájemně komunikovat.

**Následující tabulka uvádí scénáře a odpovídající název řešení řešení:**

| **Scénář** | **Řešení** | **Přípona** |
| --- | --- | --- |
| Překlad mezi instancí role nebo umístěný ve stejné cloudové služby nebo virtuální sítě virtuálních počítačů |[Rozlišení názvů Azure](#azure-provided-name-resolution) |název hostitele nebo plně kvalifikovaný název domény |
| Překlad mezi instancí role nebo umístěné v různých virtuálních sítích virtuálních počítačů |Spravované zákazníkem servery DNS předávání dotazů mezi virtuálními sítěmi pro řešení Azure (DNS proxy).  V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server) |Pouze plně kvalifikovaný název domény |
| Překlad názvu počítače a služby v místě z instancí role nebo virtuálních počítačů v Azure |Spravované zákazníkem servery DNS (např. místní řadič domény, řadiče místní domény jen pro čtení nebo sekundární DNS synchronizovat pomocí přenosy zóny).  V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server) |Pouze plně kvalifikovaný název domény |
| Řešení Azure názvy hostitelů z místního počítače |Předávaly dotazy na zákazníkem spravovaný proxy server DNS v odpovídající virtuální síť proxy server předává dotazy k Azure pro překlad. V tématu [překladu IP adresy serveru DNS](#name-resolution-using-your-own-dns-server) |Pouze plně kvalifikovaný název domény |
| Reverse DNS pro interní IP adresy |[Překlad názvů pomocí serveru DNS](#name-resolution-using-your-own-dns-server) |neuvedeno |
| Překlad mezi virtuální počítače nebo instance rolí, které jsou umístěné v různých cloudové služby, není ve virtuální síti |Není k dispozici. Připojení mezi virtuální počítače a instance rolí v různých cloudové služby není podporované mimo virtuální síť. |neuvedeno |

## <a name="azure-provided-name-resolution"></a>Rozlišení názvů Azure
Společně s překladu názvů DNS veřejné Azure poskytuje interní překlad adres pro virtuální počítače a instance rolí, které se nacházejí v rámci stejné virtuální síti nebo cloudové služby.  Virtuální počítače nebo instance v cloudové službě sdílejí stejnou příponu DNS (takže stačí samotná název hostitele), ale v různých cloudu klasické virtuální sítě služby mají různé přípony DNS, plně kvalifikovaný název domény je nutný k překladu názvů různých cloudových služeb.  Ve virtuálních sítích v modelu nasazení Resource Manager přípona DNS je konzistentní napříč virtuální sítě (takže plně kvalifikovaný název domény není nezbytný) a názvy DNS lze přiřadit k síťové karty a virtuální počítače. I když Azure překlad nevyžaduje žádnou konfiguraci, není příslušnou volbu pro všechny scénáře nasazení, jak je vidět na výše uvedené tabulce.

> [!NOTE]
> V případě webových a pracovních rolí můžete také přístup k interní IP adresy instance rolí, které jsou založené na role název a číslo instance pomocí REST API pro správu služby Azure. Další informace najdete v tématu [služby referenční dokumentace rozhraní API REST správy](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features-and-considerations"></a>Funkce a důležité informace
**Funkce:**

* Snadné použití: Chcete-li používat Azure překlad není nutná žádná konfigurace.
* Služba Azure název řešení je vysoce dostupný, ukládání, je potřeba vytvořit a spravovat clustery serverů DNS.
* Můžete použít ve spojení s vlastní servery DNS přeložit na místě a Azure názvy hostitelů.
* Název řešení je k dispozici mezi instancí role virtuálních počítačů v rámci stejné cloudové služby bez nutnosti plně kvalifikovaný název domény.
* Název řešení je k dispozici mezi virtuálními počítači ve virtuálních sítích, které používají model nasazení Resource Manager, bez nutnosti plně kvalifikovaný název domény. Při překladu názvů v různých cloudové služby, virtuálních sítí v modelu nasazení classic vyžadují plně kvalifikovaný název domény. 
* Názvy hostitelů, které nejlépe popisují vaše nasazení, můžete místo práce s automaticky generovaných názvů.

**Aspekty:**

* Přípona DNS vytvořit Azure nelze upravit.
* Nelze ručně zaregistrovat vlastní záznamy.
* Služba WINS a NetBIOS nejsou podporovány. (Virtuální počítače v Průzkumníku Windows nejde zobrazit.)
* Názvy hostitelů musí být kompatibilní s DNS (uživatel musí použít jenom 0 – 9, a – z a '-' a nesmí začínat ani končit '-'. Viz část 3696 RFC 2.)
* Pro každý virtuální počítač je omezen provoz dotazu DNS. To by neměla mít vliv na většinu aplikací.  Pokud je dodržena omezení požadavků, ujistěte se, zda je povoleno ukládání do mezipaměti na straně klienta.  Další podrobnosti najdete v tématu [maximální využití služby Azure překlad](#Getting-the-most-from-Azure-provided-name-resolution).
* Pouze virtuální počítače v první 180 cloudové služby jsou registrované pro každou virtuální síť v modelu nasazení classic. To neplatí k virtuálním sítím v modelu nasazení Resource Manager.

### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Maximální využití služby Azure překlad
**Ukládání do mezipaměti na straně klienta:**

Ne každý dotaz DNS musí být odesílají přes síť.  Ukládání do mezipaměti na straně klienta pomáhá snížit latenci a zlepšit odolnost proti sítě blips vyřešte opakující se dotazy DNS z místní mezipaměti.  Záznamy DNS obsahovat Time To Live (TTL) umožňující mezipaměti ukládat záznam pro stejně dlouho bez dopadu záznamů aktuálnosti tak ukládání do mezipaměti na straně klienta je vhodná pro většinu situacích.

Výchozí nastavení klienta DNS systému Windows má integrovanou mezipaměť DNS.  Některé Linux distribucích nezahrnují ukládání do mezipaměti ve výchozím nastavení, je doporučeno, jednu pro každý virtuální počítač Linux přidat (po kontrole, zda není k dispozici místní mezipaměti již).

Existuje několik různých DNS ukládání do mezipaměti balíčků dostupné, například dnsmasq, tady jsou kroky instalace dnsmasq na nejběžnější distribucích:

* **Ubuntu (používá resolvconf)**:
  * Stačí nainstalujte balíček dnsmasq ("sudo instalace výstižný get dnsmasq").
* **SUSE (používá netconf)**:
  * instalovat balíček dnsmasq ("sudo zypper instalace dnsmasq") 
  * Povolení služby dnsmasq ("Povolit dnsmasq.service systemctl") 
  * Spusťte službu dnsmasq ("systemctl počáteční dnsmasq.service") 
  * Upravit "/ atd/sysconfig/síť/config" a změňte NETCONFIG_DNS_FORWARDER = "" na "dnsmasq"
  * Aktualizujte resolv.conf ("netconfig update") k nastavení mezipaměti jako místní překladač služby DNS
* **OpenLogic (používá NetworkManager)**:
  * instalovat balíček dnsmasq ("sudo yum instalace dnsmasq")
  * Povolení služby dnsmasq ("Povolit dnsmasq.service systemctl")
  * Spusťte službu dnsmasq ("systemctl počáteční dnsmasq.service")
  * Přidání "předřazení domény názvové servery 127.0.0.1;" na "/etc/dhclient-eth0.conf"
  * Restartujte síťovou službu ("sítě restartování služby") nastavit jako překladač DNS místní mezipaměti

> [!NOTE]
> Balíček 'dnsmasq' je pouze jeden z mnoha mezipaměti DNS, která je k dispozici pro Linux.  Před jeho použitím, zkontrolujte jeho vhodnosti pro konkrétních potřeb a žádné jiné mezipaměti je nainstalovaná.
> 
> 

**Opakování na straně klienta:**

DNS je primárně protokol UDP.  Jako protokol UDP nezaručuje doručení zpráv, je logika opakovaných pokusů zpracována v samotný protokol DNS.  Každý klient DNS (operačního systému) může vykazovat různé opakování logiku v závislosti na předvoleb creators:

* Operační systémy Windows opakujte po 1 druhý a pak znovu po jiné 2, 4 a jiné 4 sekundy. 
* Výchozí opakování instalace Linux po 5 sekund.  Doporučujeme toto 5krát v 1 druhý intervalech zkuste změnit.  

Chcete-li zkontrolovat aktuální nastavení na virtuální počítač s Linuxem, 'cat /etc/resolv.conf' a podívejte se na řádku položku Možnosti' například:

    options timeout:1 attempts:5

Soubor resolv.conf je obvykle automaticky generovaný a by neměla být upravována.  Konkrétní kroky pro přidání položku Možnosti' řádek se liší podle distro:

* **Ubuntu** (používá resolvconf):
  * Přidání možnosti řádek pro ' / etc/resolveconf/resolv.conf.d/head. 
  * Spustit "resolvconf -u" aktualizace
* **SUSE** (používá netconf):
  * Přidejte 'timeout:1 pokusů: 5' do NETCONFIG_DNS_RESOLVER_OPTIONS = "" parametr v/atd/sysconfig nebo sítě nebo konfigurace 
  * spustit 'netconfig aktualizace, aktualizace
* **OpenLogic** (používá NetworkManager):
  * Přidejte 'echo "timeout:1 možnosti pokusů: 5" ' do ' nebo etc/NetworkManager/dispatcher.d/11-dhclient' 
  * spustit 'služby sítě restartování' Chcete-li aktualizovat

## <a name="name-resolution-using-your-own-dns-server"></a>Překlad názvů pomocí serveru DNS
Existuje několik situací, kde potřeb název řešení může přejít nad rámec funkce poskytované službou Azure, například při použití domén služby Active Directory, nebo pokud vyžadují překlad DNS mezi virtuálními sítěmi (virtuální sítě).  Tak, aby pokrýval scénáře, Azure poskytuje možnost musíte použít vlastní servery DNS.  

Servery DNS v rámci virtuální sítě může předat dotazy DNS Azure rekurzivní překladače přeložit názvy hostitelů v rámci této virtuální sítě.  Řadiči domény (DC) běží v Azure můžete například odpovídat na dotazy DNS pro jeho domény a předávat všechny další dotazy k Azure.  To umožňuje virtuálních počítačů k místním prostředkům (přes řadič domény) a Azure názvy hostitelů (prostřednictvím předávání).  Přístup k Azure rekurzivní překladače se poskytuje prostřednictvím virtuální IP adresa 168.63.129.16.

Předávání DNS také umožňuje překlad DNS mezi virtuálními a povoluje vaše místní počítače přeložit názvy hostitelů Azure.  Aby bylo možné přeložit název hostitele Virtuálního počítače, serveru DNS virtuální počítač se musí nacházet ve stejné virtuální síti a nakonfigurovat tak, aby dotazy dopředného název hostitele Azure.  Jako přípona DNS se liší v každé virtuální síť, můžete použít pravidla pro podmíněné předávání aby odesílalo dotazy DNS na správnou virtuální sítě pro překlad.  Následující obrázek ukazuje dvě virtuální sítě a k místní síti provádění překlad DNS mimo virtuální síť pomocí této metody.  Předávání DNS příklad je k dispozici v [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) a [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Mezi virtuálními DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Při použití Azure překlad IP adres příponu interní DNS (*. internal.cloudapp.net) je zadán pro každý virtuální počítač pomocí systému DHCP.  To umožňuje rozlišení názvů hostitelů, jako jsou název hostitele záznamy v zóně internal.cloudapp.net.  Při použití vlastní název řešení řešení, příponou IDN není součástí k virtuálním počítačům, protože ji naruší jiné DNS architektury (jako je připojený k doméně scénáře).  Místo toho jsme představují zástupce nefunkční (reddog.microsoft.com).  

V případě potřeby příponu interní DNS se dá určit pomocí prostředí PowerShell nebo rozhraní API:

* Pro virtuální sítě v modelech nasazení Resource Manager, je k dispozici prostřednictvím přípona [karty síťového rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx) prostředků nebo prostřednictvím [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) rutiny.    
* Ve model nasazení classic, je k dispozici prostřednictvím přípona [získat rozhraní API nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) volání nebo prostřednictvím [Get-AzureVM – ladění](https://msdn.microsoft.com/library/azure/dn495236.aspx) rutiny.

Pokud předávání dotazů do Azure, není vašim potřebám, musíte zadat vlastní řešení DNS.  Bude potřeba řešení DNS:

* Zadejte odpovídající název hostitele řešení, například prostřednictvím [DDNS](virtual-networks-name-resolution-ddns.md).  Poznámka: Pokud používáte DDNS budete muset zakázat zapůjčení DHCP Azure a jsou velmi dlouhé a úklidu úklidu záznamů DNS může předčasně odebrat záznamy DNS. 
* Zadejte odpovídající rekurzivní překlad názvů povolit překlad názvů externí domény.
* Být přístupné (TCP a UDP na port 53) z klientů, které slouží a mít přístup k Internetu.
* Nutné zabezpečit před přístupem z Internetu, zmírnit hrozby představované externí agenty.

> [!NOTE]
> Pro nejlepší výkon, při použití virtuálních počítačích Azure jako servery DNS, by mělo být zakázáno IPv6 a [veřejná IP adresa na úrovni Instance](virtual-networks-instance-level-public-ip.md) by se měla přiřadit k jednotlivým serverům DNS virtuálního počítače.  Pokud se rozhodnete používat Windows Server jako DNS server, [v tomto článku](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) poskytuje další výkonu analýzy a optimalizace.
> 
> 

### <a name="specifying-dns-servers"></a>Určení serverů DNS
Pokud používáte vlastní servery DNS, Azure poskytuje možnost se určením několika DNS serverů pro virtuální síť, nebo v síťové rozhraní (Resource Manager) nebo je Cloudová služba (klasické).  Servery DNS zadané pro cloudové služby nebo síťová rozhraní získat přednost přes uvedenými virtuální sítě.

> [!NOTE]
> Vlastnosti síťového připojení, například server DNS IP adresy, by neměla být upravována přímo v rámci virtuálních počítačů Windows může získat vymazat během služby retušovat při získá nahradit virtuální síťový adaptér. 
> 
> 

Při použití modelu nasazení Resource Manager, servery DNS lze na portálu, rozhraní API nebo šablony ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [síťový adaptér](https://msdn.microsoft.com/library/azure/mt163668.aspx)) nebo prostředí PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [ síťový adaptér](https://msdn.microsoft.com/library/mt619370.aspx)).

Při použití modelu nasazení classic, servery DNS pro virtuální sítě lze na portálu nebo [ *konfigurace sítě* soubor](https://msdn.microsoft.com/library/azure/jj157100).  Pro cloudové služby, servery DNS, které jsou určené prostřednictvím [ *konfigurace služby* soubor](https://msdn.microsoft.com/library/azure/ee758710) nebo v prostředí PowerShell ([New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [!NOTE]
> Pokud změníte nastavení DNS pro virtuální sítě nebo virtuální počítač, který je už nasazená, budete muset restartovat jednotlivé ovlivněné virtuální počítače změny se projeví.
> 
> 

## <a name="next-steps"></a>Další kroky
Model nasazení Resource Manager:

* [Vytvořit nebo aktualizovat virtuální síť](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Vytvořit nebo aktualizovat karty síťového rozhraní](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [Nový AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
* [Nové AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

Model nasazení Classic:

* [Schéma konfigurace služby Azure](https://msdn.microsoft.com/library/azure/ee758710)
* [Schéma konfigurace virtuální sítě](https://msdn.microsoft.com/library/azure/jj157100)
* [Konfigurace virtuální sítě pomocí konfiguračního souboru sítě](virtual-networks-using-network-configuration-file.md) 

