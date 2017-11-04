---
title: "Služba správy pro službu Azure Search na portálu Azure"
description: "Spravujte Azure Search, hostované cloudové vyhledávací službě v Microsoft Azure, pomocí portálu Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: c87d1fdd-b3b8-4702-a753-6d7e29dbe0a2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/18/2017
ms.author: heidist
ms.openlocfilehash: c293de5b43103c8cbec01f61a26b8b28ac7e9116
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="service-administration-for-azure-search-in-the-azure-portal"></a>Služba správy pro službu Azure Search na portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Služba Azure Search je plně spravovaná, cloudové vyhledávací služba používá pro vytváření bohatě hledání do vlastních aplikací. Tento článek se zabývá *služby správy* úlohy, které můžete provádět v [portál Azure](https://portal.azure.com) pro vyhledávací službu, která jste již zřízeno. *Služba správy* je lightweight návrh omezené na následující úlohy:

* Spravovat a zabezpečit přístup k *klíče api Key* používá pro čtení nebo zápisu přístup k službě.
* Změnou přidělení replik a oddíly upravte kapacitu služby.
* Sledování využití prostředků relativně k maximální limit vaše vrstvy služby.

**Není v oboru** 

*Správa obsahu* (nebo index management) odkazuje na operací, jako je analýza provozu vyhledávání pochopit dotaz svazku, zjistit, jaké podmínky pro a jak jsou výsledky hledání úspěšné v vedení zákazníkům na konkrétní dokumenty v hledání osob indexu. Nápovědu v této oblasti najdete v tématu [Analýza provozu vyhledávání pro službu Azure Search](search-traffic-analytics.md).

*Dotazování výkonu* je také nad rámec tohoto článku. Další informace najdete v tématu [monitorování metriky využití a dotaz](search-monitor-usage.md) a [výkonu a optimalizace](search-performance-optimization.md).

*Upgrade* není správce úloh. Protože prostředky se přidělují při zřízení služby, při přechodu k jiné vrstvě potřeba novou službu. Podrobnosti najdete v tématu [vytvoření služby Azure Search](search-create-service-portal.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Práva správce
Správce předplatného Azure nebo spolusprávce lze provést zřizování nebo vyřazení z provozu samotné služby.

V rámci služby kdo má přístup k adresu URL služby a rozhraní api-key správce má přístup pro čtení a zápis ke službě. Přístup pro čtení a zápis umožňuje přidávat, odstraňovat nebo změnu objektů serveru, včetně klíče api Key, indexy, indexery, zdrojů dat, plány a přiřazení rolí, jak jsou implementované pomocí [role RBAC definované](#rbac).

Všechny interakce uživatele s Azure Search spadá do jedné z těchto režimů: přístup pro čtení a zápis ke službě (oprávnění správce), nebo jen pro čtení přístup ke službě (dotazu oprávnění). Další informace najdete v tématu [spravovat klíče api Key](#manage-keys).

<a id="sys-info"></a>

## <a name="set-rbac-roles-for-administrative-access"></a>Nastavte role RBAC pro přístup pro správu
Azure poskytuje [modelu globální autorizace na základě rolí](../active-directory/role-based-access-control-configure.md) pro všechny služby spravovat prostřednictvím portálu nebo Resource Manager rozhraní API. Role vlastník, Přispěvatel a čtečky určit úroveň služby správy pro uživatele, skupiny a objekty zabezpečení přiřazené ke každé roli služby Active Directory. 

Pro službu Azure Search určit oprávnění RBAC následující úlohy správy:

| Role | Úkol |
| --- | --- |
| Vlastník |Vytvořit nebo odstranit službu nebo libovolného objektu na služby, včetně klíče api Key, indexy, indexery, indexer zdroje dat a indexeru plány.<p>Zobrazit stav služby, včetně počtu a velikosti úložiště.<p>Přidání nebo odstranění členství role (pouze vlastníka můžete spravovat členství role).<p>Správci předplatného a vlastníky služby mají automatické členství v roli vlastníky. |
| Přispěvatel |Stejnou úroveň přístupu jako vlastník, minus RBAC správu rolí. Například Přispěvatel můžete zobrazit a znovu vygenerovat `api-key`, ale nelze upravit členství v rolích. |
| Čtenář |Zobrazení stavu a dotaz klíče služby. Členové této role nelze změnit konfiguraci služby, ani můžete zobrazit klíče správce. |

Role neudělujte přístupová práva ke koncovému bodu služby. Hledání operací služby, jako je například Správa indexu, naplňování indexu a dotazy na data vyhledávání, jsou řízena pomocí klíče api Key, není role. Další informace najdete v tématu "Autorizace pro správu a operace dat" v [co je řízení přístupu na základě Role](../active-directory/role-based-access-control-what-is.md).

<a id="secure-keys"></a>
## <a name="logging-and-system-information"></a>Informace o protokolování a systému
Služba Azure Search nevystavuje soubory protokolu pro jednotlivé služby buď prostřednictvím portálu nebo programovací rozhraní. Na úroveň Basic a vyšší, Microsoft monitoruje všechny služby Azure Search 99,9 % dostupnosti za smlouvy o úrovni služeb (SLA). Pokud služba je pomalé nebo propustnost žádostí nedosahuje prahové hodnoty SLA, zkontrolujte soubory protokolu, která je jim dostupná týmy podpory a vyřešte problém.

Z hlediska obecné informace o službě můžete získat informace o následujícím způsobem:

* Na portálu na řídicím panelu služby, prostřednictvím oznámení, vlastnosti a stavové zprávy.
* Pomocí [prostředí PowerShell](search-manage-powershell.md) nebo [REST API pro správu](https://docs.microsoft.com/rest/api/searchmanagement/) k [získat vlastnosti služby](https://docs.microsoft.com/rest/api/searchmanagement/services), nebo stav na využití prostředků indexu.
* Prostřednictvím [Analýza provozu vyhledávání](search-traffic-analytics.md), jak je uvedeno dříve.

<a id="manage-keys"></a>

## <a name="manage-api-keys"></a>Spravovat klíče api Key
Všechny žádosti o službu vyhledávání potřebovat rozhraní api klíč, který byl vygenerován speciálně pro vaši službu. Tento klíč rozhraní api se jedná o jedinou mechanismus pro ověřování přístupu do vašeho koncového bodu služby vyhledávání. 

Klíč rozhraní api je řetězec tvořený náhodně generované číslic a písmen. Prostřednictvím [oprávnění RBAC](#rbac), můžete odstranit nebo čtení klíčů, ale klíč nelze nahradit heslem definovaný uživatelem. 

Dva typy klíčů se používají k přístupu ke službě vyhledávání:

* Správce (platné pro všechny operace čtení a zápis Service)
* Dotaz (platné pro operace jen pro čtení, například dotazy pro index)

Rozhraní api-key správce je vytvořen po zřízení služby. Existují dva klíče správce, určený jako *primární* a *sekundární* k jejich přímo, ale ve skutečnosti jsou zaměnitelné. Každá služba má dva klíče správce, takže můžete se vrátit jednu bez ztráty přístupu k službě. Můžete obnovit buď klíč správce, ale nemůžete přidat počet klíčů celkový správce. Je delší než dva klíče správce jednu službu vyhledávání.

Klíče dotazu jsou navrženy pro klientské aplikace, které volají přímo vyhledávání. Můžete vytvořit až 50 klíče dotazu. V kódu aplikace zadejte adresu URL vyhledávání a dotazu api-key povolit přístup jen pro čtení ke službě. Kód aplikace také určuje index používá vaše aplikace. Společně koncový bod, klíč rozhraní api pro přístup jen pro čtení a cílový index definovat úroveň oboru a přístup připojení z klientské aplikace.

Chcete-li získat nebo znovu vygenerovat klíče api Key, otevřete řídicí panel služby. Klikněte na tlačítko **klíče** otevřete vysouvací stránce správy klíčů. Příkazy pro opětovné generování nebo vytváření klíčů jsou v horní části stránky. Ve výchozím nastavení se vytvoří pouze klíče správce. Klíče dotazu api Key musí být vytvořeny ručně.

 ![][9]

<a id="rbac"></a>

## <a name="secure-api-keys"></a>Zabezpečovací klíče rozhraní api
Zabezpečení klíčů je zajištěno omezení přístupu prostřednictvím portálu nebo rozhraní správce prostředků (prostředí PowerShell nebo rozhraní příkazového řádku). Jak jsme uvedli, správci předplatného můžete zobrazit a obnovit všechny klíče rozhraní api. Z důvodu zkontrolujte přiřazení rolí pochopit, kdo má přístup ke klíčům správce.

1. V řídicím panelu služby klikněte na ikonu přístup otevřete vysouvací okno uživatelé.
   ![][7]
2. V seznamu uživatelé Zkontrolujte stávající přiřazení rolí. Podle očekávání, správci předplatného už máte plný přístup ke službě přes roli vlastníka.
3. Chcete-li zobrazit další podrobnosti, klikněte na tlačítko **správci předplatného** a potom rozbalte seznam přiřazení role a zjistit, kdo má společné administrativní oprávnění na vaši službu vyhledávání.

Jiný způsob, jak zobrazit přístupová oprávnění je kliknout na **role** v okně uživatelé. Díky tomu zobrazí dostupných rolí a počtu uživatelů nebo skupin, které jsou přiřazené k jednotlivým rolím.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Sledování využití prostředků
Řídicí panel sledování prostředků je omezené na informace zobrazené na řídicím panelu služby a několik metriky, které můžete získat pomocí dotazu na službu. Na řídicím panelu služby, v části využití můžete rychle určit, jestli jsou oddílu prostředků úrovně pro vaše aplikace.

Pomocí rozhraní API služby Search, můžete získat počet dokumentů a indexy. Existují pevných limitů přidružené tyto počty podle cenové úrovně. Další informace najdete v tématu [omezení služby Search](search-limits-quotas-capacity.md). 

* [Získat statistiku indexu](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Počet dokumentů](https://docs.microsoft.com/rest/api/searchservice/count-documents)

> [!NOTE]
> Ukládání do mezipaměti chování můžete dočasně overstate omezení. Například pokud používáte sdílené služby, můžete se setkat dokumentu počet přes pevný limit 10 000 dokumentů. Přehánění je dočasný a bude zjištěn na další kontroly vynucení omezení. 
> 
> 

## <a name="disaster-recovery-and-service-outages"></a>Po havárii výpadkům obnovení a služby

I když jsme můžete vyřazení dat, Azure Search neposkytuje rychlých převzetí služeb při selhání služby, pokud dojde k výpadku na úrovni center clusteru nebo data. Pokud cluster selže v datovém centru, provozní tým rozpozná a pracovní obnovit služby. Během obnovení služby bude docházet výpadku. Můžete požádat kompenzace kompenzovat nedostupnost služby za [smlouvy o úrovni služeb (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Pokud byly služby nepřetržitě se vyžaduje v případě závažné selhání mimo kontrolu společnosti Microsoft, může [zřídit další službu](search-create-service-portal.md) v jiné oblasti a implementace jsou strategie geografická replikace zajistit indexy plně redundantní ve všech službách.

Zákazníci, kteří používají [indexery](search-indexer-overview.md) naplnit a aktualizujte indexy dokáže zpracovat zotavení po havárii prostřednictvím specifické pro geograficky indexery využívat stejný zdroj dat. Dvě služby v různých oblastech, každé spuštění indexeru, mohou indexu z jednoho zdroje dat k dosažení geografická redundance. Pokud indexování ze zdrojů dat, které jsou geograficky redundantní, mějte na paměti, že Azure Search indexery může provádět pouze v přírůstkové indexování z primární repliky. V případě převzetí služeb při selhání nezapomeňte znovu přejděte na příkaz indexeru nové primární replice. 

Pokud nepoužijete indexery, byste použili kódu aplikace za účelem nabízené objektů a dat služby vyhledat jiný paralelně. Další informace najdete v tématu [výkonu a optimalizace ve službě Azure Search](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Zálohování a obnovení

Protože Azure Search není řešení primární datové úložiště, není poskytována formální mechanismus pro samoobslužné služby zálohování a obnovení. Kód aplikace používá k vytváření a naplňování indexu, je fakticky možnost obnovení, pokud omylem odstraníte indexu. 

Znovu sestavit index, byste odstranit (za předpokladu, že existuje), znovu vytvořte index v rámci služby a znovu načtěte načtením dat z primární data store. Alternativně můžete oslovení k [zákaznickou podporu]() pro indexy při vyřazení, pokud je regionální výpadku.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Vertikálně navýšit nebo snížit kapacitu
Všechny služby vyhledávání začíná minimálně jednu repliku a jeden oddíl. Pokud jste se přihlásili k [vrstvy, která poskytuje vyhrazených prostředcích](search-limits-quotas-capacity.md), klikněte **ŠKÁLOVÁNÍ** dlaždici na řídicím panelu služby upravit využití prostředků.

Když přidáte kapacity prostřednictvím buď prostředků, služba používá je automaticky. Je potřeba žádná další akce z vaší strany, ale je k mírnému zpoždění před je realizován dopad nový prostředek. Může trvat 15 minut nebo déle zřízení dalších prostředků.

 ![][10]

### <a name="add-replicas"></a>Přidat repliky
Zvýšení dotazů za sekundu (QPS) nebo dosažení vysoké dostupnosti se provádí přidáním repliky. Každou repliku má jednu kopii indexu, takže přidání jeden další repliky přeloží na jeden další index, které jsou k dispozici pro zpracování požadavků na dotazy služby. Minimálně 3 repliky jsou požadovány pro zajištění vysoké dostupnosti (viz [plánování kapacity](search-capacity-planning.md) podrobnosti).

Vyhledávací službu s další repliky můžete načíst vyrovnávat požadavky na dotaz přes větší počet indexů. S ohledem úrovni svazku dotazu, propustnost dotazu bude rychlejší po další kopie k dispozici pro vyřízení žádosti o indexu. Pokud se setkáváte s latence dotazu, můžete očekávat kladný dopad na výkon po další repliky online.

I když propustnost dotazu se zvyšuje při přidávání repliky, nemá přesněji dvakrát nebo Trojitá jako přidání repliky do služby. Všechny aplikace vyhledávání podléhají vnější faktory, které můžete dotýkat výkonu dotazů. Složitých dotazů a latence sítě jsou dva faktory, které přispívají k rozdíly v dobu odezvy na dotazy.

### <a name="add-partitions"></a>Přidat oddíly
Většina aplikací service má integrovanou potřebu další repliky spíše než oddíly. Pro případy, kdy je potřeba počet vyšší dokumentu můžete přidat oddíly, pokud jste zaregistrovali do služby na úrovni Standard. Základní úroveň nejsou k dispozici pro další oddíly.

Přidá na plán úrovně Standard, oddíly v násobcích 12 (konkrétně 1, 2, 3, 4, 6 a 12). Toto je artefakt horizontálního dělení. Index se vytvoří ve 12 horizontálních oddílů, které může všechny uložené na 1 oddílu nebo rovnoměrně rozdělené do 2, 3, 4, 6 a 12 oddíly (jeden horizontálního oddílu na oddíl).

### <a name="remove-replicas"></a>Odstranit repliky
Po období vysoké dotazu svazků můžete snížit replik po zatížením dotazy vyhledávání mít normalized (například po svátek prodeje jsou přes).

Chcete-li to provést, posuňte jezdec repliky zpět na nižší číslo. Neexistují žádné další kroky potřebné z vaší strany. Snížení počtu repliky ztratí virtuální počítače v datovém centru. Vaše operace přijímání dotazu a data se teď spustí na méně virtuálních počítačích než před. Minimální omezení je jedna replika.

### <a name="remove-partitions"></a>Odebrat oddíly
Na rozdíl od odebírání repliky, který vyžaduje žádné další úsilí na druhé straně, může mít některé pracovní postup, pokud používáte další úložiště, než se může snížit. Například pokud řešení používá tři oddíly, downsizing na jeden nebo dva oddíly vygenerují chybu pokud nového prostoru úložiště je menší než požadovaný. Podle očekávání, jsou vaše volby se odstranit indexy nebo dokumenty v indexu přidružené k uvolnění místa, nebo zachovat aktuální konfiguraci.

Neexistuje žádná metoda zjišťování, která vám ukáže, které horizontálních oddílů indexu jsou uložené na konkrétní oddíly. Každý oddíl poskytuje přibližně 25 GB úložiště, takže budete muset snížit úložiště na velikost, která může být obslouženo počet oddílů, které máte. Pokud chcete vrátit na jeden oddíl, všechny 12 horizontálních oddílů muset přizpůsobit.

Pomohou při plánování budoucí, můžete chtít zkontrolujte úložiště (pomocí [získat statistiku Index](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) Chcete-li zobrazit, kolik ve skutečnosti používá. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Osvědčené postupy na škálování a nasazení
Toto video 30 minut zkontroluje osvědčené postupy pro pokročilé scénáře nasazení, včetně úloh zeměpisné polohy. Můžete také zjistit [výkonu a optimalizace ve službě Azure Search](search-performance-optimization.md) pro stránky nápovědy, které se týkají stejné body.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Další kroky
Jakmile porozumíte Principy správy service, zvažte použití [prostředí PowerShell](search-manage-powershell.md) k automatizaci úloh.

Také doporučujeme přečtení [výkonu a optimalizace článku](search-performance-optimization.md).

Další doporučení je videu si poznamenali v předchozím oddílu. Poskytuje hlubší pokrytí postupy uvedené v této části.

<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



