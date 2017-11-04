---
title: "Aplikační služba Azure hybridních připojení | Microsoft Docs"
description: "Jak vytvořit a používat hybridní připojení pro přístup k prostředkům v sítích různorodých"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: ccompy
ms.openlocfilehash: f0b148cb9c304c54b47be9601740e7634462d59b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Hybridní připojení služby Azure App Service #

## <a name="overview"></a>Přehled ##

Hybridní připojení je součástí služby v Azure jak funkce ve službě Azure App Service.  Jako služba se používá a možnosti nad rámec těch, které jsou využity v Azure App Service.  Další informace o hybridních připojení a jejich využití mimo Azure App Service, můžete spustit zde [Azure předávání hybridní připojení][HCService]

V rámci Azure App Service hybridní připojení slouží pro přístup k prostředkům aplikací v jiných sítích. Poskytuje přístup z vaší aplikace pro koncový bod aplikace.  Alternativní schopnost přístup k aplikaci není povolen.  Jako použít ve službě App Service, každý hybridní připojení korelaci na jedné kombinaci hostitele a portu TCP.  To znamená, že koncový bod hybridní připojení může být v jakémkoliv operačním systému a všechny aplikace, zadali jste nedosáhli naslouchání portu TCP. Hybridní připojení neznáte nebo vědět, co je aplikační protokol nebo co se připojujete.  Jednoduše zadáváte přístup k síti.  


## <a name="how-it-works"></a>Jak to funguje ##
Funkce hybridní připojení se skládá z dvě odchozí volání předávání přes Service Bus.  Existuje připojení z knihovny na hostiteli, kde vaše aplikace běží ve službě App Service a připojení ze Správce hybridního připojení (HCM) k předávání přes Service Bus.  HCM je předávací služba, která můžete nasadit v rámci sítě k prostředku, který se pokoušíte získat přístup k hostování. 

Prostřednictvím dvou připojené k připojení má vaše aplikace na druhé straně HCM tunel ke kombinaci pevné hostitele a portu TCP.  Připojení používá TLS 1.2 pro zabezpečení a klíče SAS pro ověřování nebo autorizace.    

![Hybridní připojení nejvyšších úrovní toku][1]

Pokud vaše aplikace provede požadavek DNS, který odpovídá koncový bod nakonfigurovaný hybridní připojení, bude přesměrován odchozí přenosy TCP prostřednictvím daného hybridního připojení.  

> [!NOTE]
> To znamená, že byste měli zkusit vždy použijte název DNS pro hybridní připojení.  Některé klientský software neprovádí vyhledávání DNS, pokud koncový bod místo toho používá IP adresu.
>
>

Existují dva typy hybridní připojení: nové hybridní připojení, které jsou nabízeny jako služba v rámci Azure předávání a starší BizTalk hybridní připojení.  Starší BizTalk hybridní připojení jsou označovány jako Classic hybridní připojení na portálu.  Další informace dále v tomto dokumentu o nich není k dispozici.

### <a name="app-service-hybrid-connection-benefits"></a>Výhody služby hybridní připojení aplikace ###

Existuje několik výhod do funkce hybridních připojení, včetně:

- Aplikací mít bezpečný přístup k místním systémům a služby bezpečně.
- Tato funkce nevyžaduje, aby koncový bod přístupné z Internetu.
- Je rychlý a snadný nastavit. 
- Každé hybridní připojení odpovídá kombinaci jednoho hostitele a portu, který je vynikající zabezpečení aspekt.
- Za normálních okolností nevyžaduje brány firewall díry přes standardní webu porty jsou všechny odchozí připojení.
- Protože tato funkce je úroveň sítě, je lhostejné jazyk používaný aplikace a z technologie používané v koncovém bodě.
- Slouží k poskytování přístupu ve více sítí z jedné aplikace. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Kroky, které nelze provést s hybridní připojení ###

Existuje několik věcí, které nemůžete dělat s hybridní připojení, včetně:

- připojení na jednotku
- pomocí protokolu UDP
- Přístup k protokolu TCP na základě služby, které používají dynamické porty, jako je například pasivní režim FTP nebo rozšířený pasivní režim
- Podporu protokolu LDAP, jako je někdy vyžaduje UDP
- Podpora služby Active Directory

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Přidávání a vytváření hybridní připojení v aplikaci ##

Prostřednictvím aplikaci aplikační služby na portálu Azure nebo z předávání přes Azure na portálu Azure můžete vytvořit hybridní připojení.  Důrazně doporučujeme vytvořit hybridní připojení prostřednictvím aplikace služby App Service, který chcete používat s hybridní připojení.  Chcete-li vytvořit hybridní připojení, přejděte na [portál Azure] [ portal] a vyberte svou aplikaci.  Vyberte **sítě > nakonfigurovat koncové body hybridního připojení**.  Zde vidíte hybridní připojení, které jsou nakonfigurované pro vaši aplikaci.  

![Seznam hybridní připojení][2]

Chcete-li přidat nové hybridní připojení, klikněte na tlačítko Přidat hybridní připojení.  Uživatelské rozhraní, které se otevře seznamy hybridní připojení, který jste už vytvořili.  Chcete-li přidat jeden nebo více těchto do vaší aplikace, klikněte na ty, které chcete a stiskněte tlačítko **přidat vybrané hybridní připojení**.  

![Hybridní připojení portálu][3]

Pokud chcete vytvořit nové hybridní připojení, klikněte na tlačítko **vytvořit nové hybridní připojení**.  Tady zadáte: 

- název koncového bodu
- hostitele koncového bodu
- koncový port
- Obor názvů sběrnice, který chcete použít

![Vytvořit hybridní připojení.][4]

Každý hybridní připojení je vázaný na oboru názvů Service Bus a každý obor názvů sběrnice je v oblasti Azure.  Je důležité se pokusit použít ve stejné oblasti jako vaše aplikace. aby se zabránilo latence sítě vyvolané oboru názvů Service Bus.

Pokud chcete odebrat hybridní připojení z vaší aplikace, klikněte na něj pravým tlačítkem a vyberte **odpojení**.  

Jakmile hybridní připojení se přidá do vaší aplikace, uvidíte na něm podrobnosti jednoduše kliknutím na něm. 

![Podrobné informace o hybridních připojení][5]

### <a name="creating-a-hybrid-connection-in-the-azure-relay-portal"></a>Vytvoření hybridní připojení na portálu Azure předávání ###

Kromě portálu zkušeností z v aplikaci je zde také schopnost vytvářet hybridní připojení z portálu Azure předávání. V pořadí pro hybridní připojení, který bude používat Azure App Service, musí splňovat dvě kritéria. Musí být:

* Vyžadovat autorizací klienta
* Mít položka metadat s názvem koncový bod, který obsahuje kombinaci hostitele a portu jako hodnotu

## <a name="hybrid-connections-and-app-service-plans"></a>Hybridní připojení a plány služby App Service ##

Hybridní připojení jsou dostupné jenom v Basic, Standard, Premium a izolovaná ceny SKU.  Existují omezení svázané s cenový plán.  

> [!NOTE] 
> Můžete vytvořit pouze nové hybridní připojení, které jsou založené na předávání přes Azure. Nelze vytvořit nový BizTalk hybridní připojení.
>

| Ceny plán | Počet hybridní připojení použitelné v plánu |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Vzhledem k tomu, že existují omezení plán služby App Service, je také uživatelského rozhraní v plán služby App Service, ukazuje, kolik hybridní připojení se používají a které aplikace.  

![Vlastnosti úrovně plánu službu v aplikaci][6]

Informace se zobrazí kliknutím na na hybridní připojení.  Ve vlastnostech tady uvedené zobrazí se všechny informace, které jste viděli v zobrazení aplikace a můžete také zjistit, kolik aplikací v stejný plán služby App Service používáte hybridní připojení.

Zatímco je omezený počet koncových bodů hybridní připojení, které mohou být používány plánu služby App Service, lze každou hybridní připojení používají napříč libovolný počet aplikací v tomto plánu služby App Service.  Jinými slovy jeden hybridní připojení, který se používá v 5 samostatných aplikací v plán služby App Service se počítá jako 1 hybridní připojení.

Není dalších nákladů na použití hybridní připojení.  Podrobnosti o cenách hybridní připojení, přejděte sem: [ceny služby Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Správce hybridního připojení ##

Pořadí pro hybridní připojení fungovat je nutné přenosového agenta v síti, která hostí váš koncový bod hybridní připojení.  Tento přenosový agent se nazývá hybridní připojení správce (HCM).  Tento nástroj můžete stáhnout z **sítě > nakonfigurovat koncové body hybridního připojení** dostupná z vaší aplikace v uživatelském rozhraní [portál Azure][portal].  

Tento nástroj běží na systému Windows server 2012 a novějších verzích systému Windows.  Nainstalovaný HCM spuštění jako služba.  Tato služba se připojuje k Azure Service Bus Relay podle nakonfigurované koncové body.  Připojení z HCM jsou odchozí do Azure přes port 443.    

HCM má uživatelské rozhraní k jeho konfiguraci.  Po instalaci HCM můžete zprovoznit rozhraní spuštěním HybridConnectionManagerUi.exe která se nachází v instalačním adresáři správce hybridního připojení.  Je také snadno dosáhla ve Windows 10 zadáním *uživatelského rozhraní správce hybridního připojení* vyhledávacího pole.  

![Hybridní připojení portálu][7]

Při spuštění uživatelského rozhraní HCM je první věcí, kterou vidíte tabulku, která obsahuje seznam všech hybridní připojení, které jsou nakonfigurované k této instanci HCM.  Pokud chcete něco změnit, musíte se k ověření pomocí Azure. 

Přidat jeden nebo více hybridní připojení k vaší HCM:

1. Spuštění uživatelského rozhraní HCM
1. Klikněte na tlačítko Konfigurovat další hybridní připojení ![přidat HC HCM][8]

1. Přihlaste se pomocí účtu Azure
1. Zvolit předplatné
1. Klikněte na hybridní připojení chcete HCM Relay ![vyberte HC][9]

1. Kliknutí na Uložit

V tomto okamžiku se zobrazí hybridní připojení, které jste přidali.  Můžete také kliknutím na nakonfigurované hybridní připojení a zobrazit podrobnosti o it.

![Podrobnosti o HC][10]

Pro vaše HCM být schopné podporovat hybridní připojení, je nakonfigurován s musí:

- TCP přístup k Azure přes porty 80 a 443
- TCP přístup ke koncovému bodu hybridní připojení.
- Umožňuje provést look-ups DNS v hostiteli koncový bod a oboru názvů Azure Service Bus

HCM podporuje jak nové hybridní připojení, tak i starší BizTalk hybridní připojení.

> [!NOTE]
> Předávání přes Azure spoléhá na webové sokety připojení. Tato možnost je pouze k dispozici v systému Windows Server 2012 nebo novější. Z tohoto důvodu správce hybridního připojení není podporováno na ničem starších než Windows Server 2012.
>

### <a name="redundancy"></a>Redundance ###

Každý HCM může podporovat víc hybridní připojení.  Jakékoli dané hybridní připojení může také podporovat více HCMs.  Výchozí chování je kruhové dotazování provoz mezi nakonfigurované HCMs pro libovolný dané koncový bod.  Pokud chcete vysoké dostupnosti na hybridní připojení z vaší sítě, jednoduše vytvoří instanci více HCMs na samostatných počítačích. 

### <a name="manually-adding-a-hybrid-connection"></a>Ručně přidejte hybridní připojení ###

Pokud chcete někomu mimo vaše předplatné pro hostování HCM instance pro danou hybridní připojení, můžete sdílet připojovací řetězec brány pro hybridní připojení s nimi.  Můžete to vidět ve vlastnostech pro hybridní připojení v [portál Azure][portal]. Chcete-li použít tento řetězec, klikněte na tlačítko **zadejte ručně** tlačítka na HCM a vložte připojovací řetězec brány.


## <a name="troubleshooting"></a>Řešení potíží ##

Stav připojení pro hybridní připojení znamená, že minimálně jeden HCM je nakonfigurován pomocí této hybridní nedostupné a je schopný dosáhnout Azure.  Pokud nebyl zobrazen stav pro hybridní připojení **připojeno**, pak hybridní připojení není nakonfigurováno na všechny HCM, který má přístup k Azure.

Hlavním důvodem, který klienti se nemůže připojit k jejich koncový bod je, protože koncový bod byl zadán pomocí IP adresy místo názvu DNS.  Pokud vaše aplikace nelze dosáhnout požadovaného koncového bodu a použít IP adresu, přejdete k používání název DNS, který je na hostiteli, kde je spuštěna HCM.  Další body ke kontrole jsou správně vyřeší názvu DNS na hostitele, na kterém je spuštěný HCM a zda existuje připojení mezi hostiteli se spuštěným HCM ke koncovému bodu hybridní připojení.  

Ve službě App Service, která se může vyvolat z konzoly Rozšířené nástroje (Kudu) s názvem tcpping není nástroj.  Tento nástroj vám sdělí, pokud máte přístup k koncový bod TCP, ale jeho nezjistíte Pokud máte přístup ke koncovému bodu hybridní připojení.  Pokud se použije v konzole pro koncový bod hybridní připojení, úspěšný příkaz ping pouze zjistíte, že máte hybridní připojení nakonfigurovaná pro vaši aplikaci, která používá tuto kombinaci hostitele a portu.  

## <a name="biztalk-hybrid-connections"></a>Hybridní připojení BizTalk ##

Starší schopností BizTalk hybridní připojení bylo ukončeno vypnout na nová BizTalk hybridní připojení.  Můžete pokračovat v používání vaší stávající hybridní připojení BizTalk s vaší aplikací, ale měli migrovat na nové hybridní připojení, který používat předávání přes Azure.  Mezi výhody v rámci nové služby BizTalk verze jsou:

- Vyžaduje se žádné další účet BizTalk.
- Protokol TLS je 1.2 místo 1.0 jako BizTalk hybridní připojení.
- Komunikace je přes porty 80 a 443 pomocí názvu DNS k dosažení Azure místo IP adresy a řadu dalších jiné porty.  

Chcete-li přidat stávající hybridní připojení BizTalk do vaší aplikace, přejděte na aplikace v rámci [portál Azure] [ portal] a klikněte na tlačítko **sítě > nakonfigurovat koncové body hybridního připojení**.  V tabulce Classic hybridní připojení, klikněte na tlačítko **přidat Classic hybridní připojení**.  Z tohoto místa zobrazí se seznam BizTalk hybridní připojení.  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/
