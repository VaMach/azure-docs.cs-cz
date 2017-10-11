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
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: fef9e7b280387934cb093f51b4c8aa134a3b87e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Hybridní připojení služby Azure App Service #

## <a name="overview"></a>Přehled ##

Hybridní připojení je součástí služby v Azure jak funkce ve službě Azure App Service.  Jako služba má použití a možnosti nad rámec těch, které jsou využity v Azure App Service.  Další informace o hybridních připojení a jejich využití mimo Azure App Service, můžete spustit zde [Azure předávání hybridní připojení][HCService]

V rámci Azure App Service hybridní připojení slouží pro přístup k prostředkům aplikací v jiných sítích. Poskytuje přístup z vaší aplikace pro koncový bod aplikace.  Alternativní schopnost přístup k aplikaci není povolen.  Používá v App Service, každý hybridní připojení korelaci na jedné kombinaci hostitele a portu TCP.  To znamená, že koncový bod hybridní připojení může být v jakémkoliv operačním systému a všechny aplikace, pokud že jste nedosáhli naslouchání portu TCP. Hybridní připojení neznáte nebo vědět, co je aplikační protokol nebo co se připojujete.  Jednoduše poskytuje přístup k síti.  


## <a name="how-it-works"></a>Jak to funguje ##
Funkce hybridního připojení se skládá z dvě odchozí volání předávání přes Service Bus.  Existuje připojení z knihovny na hostiteli, kde vaše aplikace běží ve službě app service a nejsou k dispozici z Manager(HCM) připojení hybridní připojení k předávání přes Service Bus.  HCM je předávací služba, která můžete nasadit v rámci sítě hostování 

Přes dvě připojení připojené k má vaše aplikace na druhé straně HCM tunel ke kombinaci pevné hostitele a portu TCP.  Připojení používá TLS 1.2 pro zabezpečení a klíče SAS pro ověřování nebo autorizace.    

![][1]

Pokud vaše aplikace provede požadavek DNS, který odpovídá koncový bod konfigurovat hybridní připojení, bude odchozí přenosy TCP přesměrován dolů hybridní připojení.  

> [!NOTE]
> To znamená, že byste měli zkusit vždy použijte název DNS pro hybridní připojení.  Některé klientský software neprovádí vyhledávání DNS, pokud koncový bod místo toho používá IP adresu.
>
>

Existují dva typy hybridní připojení, nové hybridní připojení, které nabízí jako službu v Azure předávání a starší BizTalk hybridní připojení.  Starší BizTalk hybridní připojení jsou označovány jako Classic hybridní připojení na portálu.  Další informace dále v tomto dokumentu o nich není k dispozici.

### <a name="app-service-hybrid-connection-benefits"></a>Služby App Service hybridní připojení výhody ###

Existuje několik výhod včetně schopnosti hybridní připojení

- Aplikací mít bezpečný přístup na místní systémy a služby bezpečně
- Tato funkce nevyžaduje internet dostupný koncový bod
- je rychlý a snadný nastavit  
- každé hybridní připojení odpovídá kombinaci jednoho hostitele a portu, který je vynikající zabezpečení aspekt
- za normálních okolností nevyžaduje brány firewall díry přes standardní webu porty jsou všechny odchozí připojení
- protože tato funkce je úroveň sítě, který také znamená, že je lhostejné jazyk používaný aplikace a z technologie používané v koncovém bodě
- může sloužit k poskytování přístupu ve více sítí z jedné aplikace 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Kroky, které nelze provést s hybridní připojení ###

Existuje několik možností, nemůžete dělat s hybridní připojení a patří mezi ně:

- připojení na jednotku
- pomocí protokolu UDP
- přístup k protokolu TCP na základě služby, které používají dynamické porty třeba pasivní režim FTP nebo rozšířený pasivní režim
- Podporu protokolu LDAP, jako je někdy vyžaduje UDP
- Podpora služby Active Directory

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Přidávání a vytváření hybridní připojení v aplikaci ##

Hybridní připojení lze vytvořit pomocí portálu aplikace nebo na portálu služby hybridní připojení.  Důrazně doporučujeme vytvořit hybridní připojení, které chcete použít s vaší aplikací pomocí aplikace portálu.  Chcete-li vytvořit hybridní připojení. přejděte na [portál Azure] [ portal] a přejděte do uživatelského rozhraní pro vaši aplikaci.  Vyberte **sítě > Konfigurovat koncové body hybridního připojení**.  Zde vidíte hybridní připojení, které jsou nakonfigurované s vaší aplikací  

![][2]

Chcete-li přidat nové hybridní připojení, klikněte na tlačítko Přidat hybridní připojení.  Uživatelské rozhraní, které se otevře seznamy hybridní připojení, který jste už vytvořili.  Chcete-li přidat jeden nebo více těchto do vaší aplikace, klikněte na ty, které chcete a stiskněte tlačítko **přidat vybrané hybridní připojení**.  

![][3]

Pokud chcete vytvořit nové hybridní připojení, klikněte na tlačítko **vytvořit nové hybridní připojení**.  Tady zadáte: 

- název koncového bodu
- hostitele koncového bodu
- koncový port
- obor názvů sběrnice, který chcete použít

![][4]

Každý hybridní připojení je vázaný na obor názvů sběrnice a každý obor názvů sběrnice je v oblasti Azure.  Je důležité se pokusit použít ve stejné oblasti jako vaše aplikace. aby se zabránilo latence sítě vyvolané obor názvů sběrnice.

Pokud chcete odebrat hybridní připojení z vaší aplikace, klikněte na něj pravým tlačítkem a vyberte **odpojení**.  

Jakmile hybridní připojení se přidá do webové aplikace, uvidíte na něm podrobnosti jednoduše kliknutím na něm.  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a>Hybridní připojení a plány služby App Service ##

Pouze hybridní připojení, které nyní můžete provést jsou nové hybridní připojení.  Jsou k dispozici v Basic, Standard, Premium a izolovaná ceny SKU.  Existují omezení svázané s cenový plán.  

| Ceny plán | Počet hybridní připojení použitelné v plánu |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Vzhledem k tomu, že existují omezení plán služby App Service je také uživatelského rozhraní v plán služby App Service, ukazuje, kolik hybridní připojení se používají a které aplikace.  

![][6]

Stejně jako s zobrazení aplikace se zobrazí podrobnosti na hybridní připojení kliknutím na.  Ve vlastnostech zobrazeny zde můžete zobrazit všechny informace, které jste měli v zobrazení aplikace ale můžete také zjistit, kolik aplikací v stejný plán služby App Service používáte hybridní připojení.

![][7]

Zatímco je omezený počet koncové body hybridního připojení, které mohou být používány plánu služby App Service, každý hybridní připojení používají lze napříč libovolný počet aplikací v tomto plánu služby App Service.  To je říkají, že pokud došlo k 1 hybridní připojení, který lze použít v 5 samostatných aplikací my plán služby App Service, která je stále právě 1 hybridní připojení.

Není dalších nákladů na hybridní připojení nad rámec se dá se použít v Basic, Standard, Premium nebo izolované SKU.  Podrobnosti o cenách pro hybridní připojení prosím naleznete zde: [ceny služby Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Správce hybridního připojení ##

V pořadí pro hybridní připojení k pracovním musíte přenosového agenta v síti, která hostí váš koncový bod hybridní připojení.  Tento přenosový agent se nazývá hybridní připojení správce (HCM).  Tento nástroj můžete stáhnout z **sítě > nakonfigurovat koncové body hybridního připojení** dostupné z aplikace v rámci uživatelského rozhraní [portál Azure][portal].  

Tento nástroj běží na systému Windows server 2008 R2 a novějších verzích systému Windows.  Nainstalovaný HCM spuštění jako služba.  Tato služba se připojí k předávání přes Azure sběrnice podle nakonfigurované koncové body.  Připojení z HCM jsou odchozí na porty 80 a 443.    

HCM má uživatelské rozhraní k jeho konfiguraci.  Po instalaci HCM můžete zprovoznit rozhraní spuštěním HybridConnectionManagerUi.exe která se nachází v instalačním adresáři správce hybridního připojení.  Je také snadno dosáhla ve Windows 10 zadáním *uživatelského rozhraní správce hybridního připojení* vyhledávacího pole.  

Při spuštění uživatelského rozhraní HCM je první věcí, kterou vidíte tabulku, která obsahuje seznam všech hybridní připojení, které jsou nakonfigurovány k této instanci HCM.  Pokud chcete, aby všechny změny, které potřebujete k ověření pomocí Azure. 

Přidat jeden nebo více hybridní připojení k vaší HCM:

1. Spuštění uživatelského rozhraní HCM
1. Klikněte na tlačítko Konfigurovat další hybridní připojení![][8]

1. Přihlaste se pomocí účtu Azure
1. Zvolit předplatné
1. Klikněte na hybridní připojení, které chcete tuto HCM pro předávání![][9]

1. Kliknutí na Uložit

V tomto okamžiku se zobrazí hybridní připojení, které jste přidali.  Můžete také kliknutím na nakonfigurované hybridní připojení a zobrazit podrobnosti o připojení.

![][10]

Pro vaše HCM být schopné podporovat hybridní připojení, který je nakonfigurován s musí:

- TCP přístup k Azure přes porty 80 a 443
- TCP přístup ke koncovému bodu hybridní připojení
- Umožňuje provést DNS ups podívejte se na hostiteli koncový bod a obor názvů sběrnice azure

HCM podporuje nové hybridní připojení jak starší BizTalk hybridní připojení.

### <a name="redundancy"></a>Redundance ###

Každý HCM může podporovat víc hybridní připojení.  Jakékoli dané hybridní připojení může také podporovat více HCMs.  Výchozí chování je kruhové dotazování provoz mezi nakonfigurované HCMs pro libovolný dané koncový bod.  Pokud chcete vysoké dostupnosti na hybridní připojení z vaší sítě, jednoduše vytvoří instanci více HCMs na samostatných počítačích. 

### <a name="manually-adding-a-hybrid-connection"></a>Ručně přidejte hybridní připojení ###

Pokud chcete někomu mimo vaše předplatné pro hostování HCM instance pro danou hybridní připojení, můžete sdílet s nimi připojovací řetězec brány pro hybridní připojení.  Můžete to vidět ve vlastnostech pro hybridní připojení v [portál Azure][portal]. Chcete-li použít tento řetězec, klikněte na tlačítko **nakonfigurovat ručně** tlačítka na HCM a vložte připojovací řetězec brány.


## <a name="troubleshooting"></a>Řešení potíží ##

Když hybridní připojení nastavená s běžící aplikací a je alespoň jeden HCM, která má toto hybridní připojení nakonfigurovaná a potom se dozvíte stav **připojeno** na portálu.  Pokud není vyslovení **připojeno** pak to znamená, že buď aplikace je mimo provoz nebo vaše HCM se nemůže připojit se k Azure na porty 80 nebo 443.  

Hlavním důvodem, který klienti se nemůže připojit k jejich koncový bod je, protože koncový bod byl zadán pomocí IP adresy místo názvu DNS.  Pokud vaše aplikace nelze dosáhnout požadovaného koncového bodu a použít IP adresu, přejdete k používání název DNS, který je na hostiteli, kde je spuštěna HCM.  Další body ke kontrole jsou správně vyřeší názvu DNS na hostitele, na kterém je spuštěný HCM a zda existuje připojení mezi hostiteli se spuštěným HCM ke koncovému bodu hybridní připojení.  

V App Service, která se může vyvolat z konzoly názvem tcpping je nástroj.  Tento nástroj vám sdělí, pokud máte přístup k koncový bod TCP, ale neobsahuje žádné informace můžete Pokud máte přístup do koncového bodu hybridní připojení.  Pokud se použije v konzole pro koncový bod hybridní připojení, úspěšný příkaz ping pouze zjistíte, že máte hybridní připojení nakonfigurovaná pro vaši aplikaci, která používá tuto kombinaci hostitele a portu.  

## <a name="biztalk-hybrid-connections"></a>Hybridní připojení BizTalk ##

Starší schopností BizTalk hybridní připojení bylo ukončeno vypnout k vytváření další BizTalk hybridní připojení.  Můžete pokračovat pomocí dříve existující připojení BizTalk hybridní aplikace, ale by bylo nutné migrovat na novou službu.  Mezi výhody v rámci nové služby BizTalk verze jsou:

- není třeba žádné další účet BizTalk
- Je protokol TLS 1.2 místo 1.0 jako BizTalk hybridní připojení
- Komunikace je přes porty 80 a 443 pomocí názvu DNS k dosažení Azure místo IP adresy a řadu dalších jiné porty.  

Přidat hybridní připojení BizTalk do vaší aplikace, přejděte na aplikace v rámci [portál Azure] [ portal] a klikněte na tlačítko **sítě > nakonfigurovat koncové body hybridního připojení**.  V tabulce Classic hybridní připojení klikněte na tlačítko **přidat classic hybridní připojení**.  Zde uvidíte seznam BizTalk hybridní připojení.  


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

