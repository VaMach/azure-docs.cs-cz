---
title: "Vysvětlení architektury Azure Active Directory | Microsoft Docs"
description: "Vysvětluje, co je to klient služby Azure AD a jak spravovat službu Azure pomocí služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2017
ms.author: markvi
ms.openlocfilehash: 3030336f5efca5029e0e790372495df11cdc8aeb
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="understand-azure-active-directory-architecture"></a>Vysvětlení architektury Azure Active Directory
Azure Active Directory (Azure AD) umožňuje zabezpečeně spravovat přístup k prostředkům a službám Azure pro vaše uživatele. Součástí Azure AD je kompletní sada funkcí pro správu identit. Informace o funkcích služby Azure AD najdete v tématu [Co je Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis)

Pomocí Azure AD můžete vytvářet a spravovat uživatele a skupiny a používat sady oprávnění k povolení nebo zamítnutí přístupu k podnikovým prostředkům. Informace o správě identit najdete v tématu věnovaném [základům správy identit Azure](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity).

## <a name="azure-ad-architecture"></a>Architektura Azure AD
Geograficky distribuovaná architektura služby Azure AD kombinuje rozsáhlé monitorování, automatizované přesměrování, převzetí služeb při selhání a možnosti obnovení, které nám umožňují poskytovat našim zákazníkům výkon a dostupnost na podnikové úrovni.

Tento článek se zabývá následujícími prvky návrhu:
 *  Návrh architektury služeb
 *  Škálovatelnost 
 *  Nepřetržitá dostupnost
 *  Datová centra

### <a name="service-architecture-design"></a>Návrh architektury služeb
Nejběžnější způsob, jak vytvořit škálovatelný vysoce dostupný systém bohatý na data, je prostřednictvím nezávislých stavebních bloků nebo jednotek škálování pro datovou vrstvu služby Azure AD. Pro jednotky škálování se používá označení *oddíly*. 

Datová vrstva obsahuje několik front-endových služeb, které poskytují funkce pro čtení a zápis. Následující diagram ukazuje, jak jsou součásti adresáře oddílu s jedním adresářem distribuované v rámci geograficky distribuovaných datových center. 

  ![Oddíly s jedním adresářem](./media/active-directory-architecture/active-directory-architecture.png)

Komponenty architektury služby Azure AD zahrnují primární repliku a sekundární repliky.

**Primární replika**

*Primární replika* obdrží všechny *zápisy* pro oddíl, do kterého patří. Všechny operace zápisu se okamžitě replikují do sekundární repliky v jiném datovém centru. Teprve potom se volající informuje o úspěchu, aby se zajistila geograficky redundantní odolnost zápisů.

**Sekundární repliky**

Všechna *čtení* adresáře se obsluhují ze *sekundárních replik* v datových centrech, které jsou fyzicky umístěné v různých zeměpisných oblastech. Protože data se replikují asynchronně, existuje mnoho sekundárních replik. Čtení adresáře, jako jsou žádosti o ověření, se obsluhují z datových center, která jsou blízko našich zákazníků. Sekundární repliky zodpovídají za škálovatelnost čtení.

### <a name="scalability"></a>Škálovatelnost

Škálovatelnost je schopnost služby rozšířit se a plnit rostoucí požadavky na výkon. Škálovatelnosti zápisu se dosahuje dělením dat. Škálovatelnost čtení se zajišťuje replikací dat z jednoho oddílu do několika sekundárních replik distribuovaných po celém světě.

Požadavky z aplikací adresáře aplikace jsou obecně směrované do datového centra, ke kterému jsou fyzicky nejblíž. Zápisy se transparentně přesměrují na primární repliku, aby se zajistila konzistence čtení a zápisu. Sekundární repliky výrazně rozšiřují rozsah oddílů, protože adresáře obvykle většinu doby obsluhují čtení.

Aplikace adresáře se připojují k nejbližším datovým centrům. To zvyšuje výkon a proto je možné horizontální navýšení kapacity. Vzhledem k tomu, že oddíl adresáře může mít mnoho sekundárních replik, sekundární repliky mohou být umístěné blíž ke klientům adresáře. Jenom interní komponenty služby adresáře, které jsou náročné na zápis, přímo cílí na aktivní primární repliku.

### <a name="continuous-availability"></a>Nepřetržitá dostupnost

Dostupnost (nebo doba provozuschopnosti) definuje schopnost systému pracovat bez přerušení. Klíčem k vysoké dostupnosti služby Azure AD je, že naše služby mohou rychle přesouvat provoz napříč několika geograficky distribuovanými datovými centry. Každé datové centrum je nezávislé, což umožňuje, aby režimy selhání spolu vzájemně nesouvisely.

Návrh oddílů Azure AD je ve srovnání s návrhem podnikové služby AD zjednodušený a to je velmi důležité pro vertikální navyšování kapacity systému. Využili jsme návrh s jedinou předlohou, který zahrnuje pečlivě orchestrovaný a deterministický proces převzetí služeb při selhání primární repliky.

**Odolnost proti chybám**

Systém je dostupnější, pokud je odolný vůči selháním hardwaru, sítě a softwaru. Pro každý oddíl v adresáři existuje hlavní replika s vysokou dostupností: primární replika. V této replice se provádějí jenom zápisy do oddílu. Tato replika se průběžně a pečlivě monitoruje a pokud se zjistí selhání, zápisy je možné okamžitě přesunout do jiné repliky (která se stane novou primární replikou). Během převzetí služeb při selhání může dojít ke ztrátě dostupnosti zápisu, obvykle na 1 až 2 minuty. Dostupnost čtení to během této doby neovlivní.

Operace čtení (jejichž počet mnohonásobně převyšuje počet zápisů) jdou jenom do sekundárních replik. Vzhledem k tomu, že sekundární repliky jsou idempotentní, ztráta libovolné repliky v daném oddílu se dá snadno vykompenzovat přesměrováním čtení do jiné repliky, obvykle ve stejném datovém centru.

**Odolnost dat**

Zápis je před potvrzením bezpečně uložený nejméně do dvou datových center. Nejdřív se zápis potvrdí v primární replice a potom se okamžitě replikuje nejméně do jednoho dalšího datového centra. To zajistí, že při potenciální katastrofické ztrátě datového centra, které je hostitelem primární repliky, nedojde ke ztrátě dat.

Azure AD udržuje [plánovanou dobu obnovení (RTO)](https://en.wikipedia.org/wiki/Recovery_time_objective) pro čtení adresáře a vydávání tokenů na nule a pro zápisy adresáře v řádu minut (přibližně 5 minut). Zajišťujeme také nulový [cíl bodu obnovení (RPO)](https://en.wikipedia.org/wiki/Recovery_point_objective) neztratíme data při převzetí služeb při selhání.

### <a name="data-centers"></a>Datová centra

Repliky Azure AD jsou uložené v datových centrech rozmístěných po celém světě. Další informace najdete v tématu věnovaném [datovým centrům Azure](https://azure.microsoft.com/en-us/overview/datacenters).

Azure AD funguje napříč datovými centry s následujícími charakteristikami:

 * Služby Authentication, Graph a další služby AD se nacházejí za službou Gateway. Gateway spravuje vyrovnávání zatížení těchto služeb. Pokud se pomocí transakčních sond stavu zjistí, že některý server není v pořádku, automaticky provede převzetí služeb při selhání. Na základě těchto sond stavu služba Gateway dynamicky směruje provoz na datová centra, která jsou v pořádku.
 * Pro *čtení* má adresář sekundární repliky a odpovídající front-endové služby v konfiguraci typu aktivní-aktivní, které se provozují v několika datových centrech. V případě selhání celého datového centra se provoz automaticky přesměruje do jiného datového centra.
 *  Pro *zápisy* adresář provede převzetí služeb při selhání primární (hlavní) repliky napříč datovými centry prostřednictvím plánovaných (nový primární se synchronizuje s původního primárního) nebo nouzových postupů převzetí. Odolnosti dat se dosahuje tím, že se každé potvrzení replikuje nejméně do dvou datových center.

**Konzistence dat**

Adresářový model zajišťuje eventuální konzistenci. Jedním z typických problémů distribuovaných systémů s asynchronní replikaci je, že data vrácená z „konkrétní“ repliky nemusí být aktuální. 

Azure AD poskytuje konzistenci čtení a zápisu pro aplikace cílením na sekundární repliku. Směřuje svoje zápisy do primární repliky a synchronně stahuje zápisy zpátky do sekundární repliky.

Aplikace, které zapisují pomocí rozhraní Graph API služby Azure AD, nezachovávají vztahy spřažení k replice adresáře pro zajištění konzistence čtení a zápisu. Služba Azure AD Graph udržuje logickou relaci, která je spřažená se sekundární replikou používanou pro čtení. Tento vztah spřažení zachycuje „token repliky“, který služba Graph ukládá do mezipaměti pomocí distribuované mezipaměti. Tento token se potom využívá pro následné operace ve stejné logické relaci. 

 >[!NOTE]
 >Zápisy se okamžitě replikují do sekundární repliky, pro kterou byla provedena čtení logické relace.
 >

**Ochrana záloh**

Adresář místo trvalého odstranění implementuje obnovitelné odstranění. Uživatelům a tenantům to umožňuje snadné obnovení v případě náhodných odstranění ze strany zákazníka. Pokud správce tenanta neúmyslně odstraní uživatele, může tuto operaci snadno vrátit zpět a odstraněné uživatele obnovit. 

Azure AD implementuje denní zálohy všech dat, a proto může autoritativně obnovit data v případě jakýchkoli logických odstranění nebo poškození. Naše datová vrstva využívá kódy pro opravu chyb. Může tak kontrolovat a automaticky opravovat určité typy diskových chyb.

**Metriky a monitorování**

Spouštění služby s vysokou dostupností vyžaduje špičkové metriky a možnosti monitorování. Azure AD průběžně analyzuje a reportuje metriky stavu klíčových služeb a kritéria úspěchu pro každou ze svých služeb. Neustále vyvíjíme a ladíme metriky, monitorování a generování výstrah pro jednotlivé scénáře, v rámci jednotlivých služeb Azure AD a napříč všemi službami.

Pokud některá ze služeb Azure AD nefunguje podle očekávání, okamžitě přijmeme opatření, abychom co nejdříve obnovili funkčnost. Nejdůležitější metrika Azure AD sleduje, jak rychle jsme schopni detekovat potíže zákazníka nebo živého webu a zmírnit je. Intenzivně investujeme do monitorování a výstrah, abychom minimalizovali dobu detekce (cílová hodnota TTD: < 5 minut) a provozní připravenosti s cílem minimalizovat dobu zmírnění problému (cílová hodnota TTM: < 30 minut).

**Bezpečný provoz**

Zavádíme provozní kontrolní mechanismy, jako je vícefaktorové ověřování (MFA) pro všechny operace a také auditování všech operací. Navíc průběžně využíváme systém elevací za běhu k udělení nezbytného dočasného přístupu pro všechny provozní úlohy na vyžádání. Další informace najdete v tématu [Důvěryhodný cloud](https://azure.microsoft.com/en-us/support/trust-center).

## <a name="next-steps"></a>Další kroky
[Příručka pro vývojáře pro službu Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

