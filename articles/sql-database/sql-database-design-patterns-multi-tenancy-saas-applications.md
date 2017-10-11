---
title: "Vzory pro víceklientské aplikace SaaS a Azure SQL Database návrhu | Microsoft Docs"
description: "Tento článek popisuje požadavky a běžných vzorů architektura dat databáze více klientů, které aplikace běžící v cloudovém prostředí je potřeba zvážit a různé kompromisy přidružené tyto vzory. Také vysvětluje, jak Azure SQL Database, s jeho elastické fondy a elastické nástroje vám budou snadněji řešit tyto požadavky způsobem ucelená."
keywords: 
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-design
ms.date: 02/01/2017
ms.author: srinia
ms.openlocfilehash: 0f6ba62a01f3211ccaae6b6c48f72e0de54aad78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="design-patterns-for-multi-tenant-saas-applications-and-azure-sql-database"></a>Vzory pro víceklientské aplikace SaaS a Azure SQL Database návrhu
V tomto článku informace o najdete požadavky a běžně data architektura vzorce víceklientské software jako služba (SaaS) databázové aplikace, které běží v prostředí cloudu. Vysvětluje také faktory, které je potřeba zvážit a kompromis vzory návrhu jiný. Elastické fondy a elastické nástroje ve službě Azure SQL Database vám může pomoct podle konkrétních požadavků bez kompromisů jiné cíle.

Vývojář podá někdy volby, které pracují s jejich zájmů dlouhodobé nejlepší při návrhu klientů modely pro datové vrstvy víceklientské aplikací. Na začátku minimálně vývojář může vnímat usnadnění vývoje a nižší náklady na poskytovatele cloudové služby jako důležitější než izolaci klientů nebo škálovatelnost aplikace. Tato volba může vést k obavy spokojenost zákazníků a nákladná oprava kurzu později.

Víceklientské aplikace je aplikace hostované v cloudovém prostředí a který poskytuje stejnou sadu služeb stovkami nebo tisíci klienty, kteří sdílet nebo vidět uživatele toho druhého data. Příkladem je aplikace SaaS, která poskytuje služby klientům v prostředí hostovaných v cloudu.

## <a name="multi-tenant-applications"></a>Víceklientským aplikacím
V víceklientským aplikacím datům a zatížení můžete snadno rozdělit na oddíly. Vám může oddílu data a úlohy, například podél hranice klienta, protože většina požadavků se provádějí v hranicích klienta. Tuto vlastnost je vlastní data a úlohy a jeho upřednostňuje vzory aplikace, které jsou popsané v tomto článku.

Vývojáři použít tento typ aplikace napříč celou spektrum cloudové aplikace, včetně:

* Databázové aplikace partnera, které jsou právě přešla do cloudu jako aplikace SaaS
* Vytvářet pro cloud od základů aplikací SaaS.
* Přímé, zákazník směřujících aplikací
* Zaměstnanecké podnikové aplikace

Aplikace SaaS, které jsou určeny pro cloud a ty s kořenových certifikačních autorit obvykle partner databázové aplikace jsou víceklientským aplikacím. Tyto aplikace SaaS poskytovat svým klientům aplikace speciální software jako služba. Klienty můžete přístup ke službě aplikace a mít úplná vlastnictví přidružená data uložená v rámci aplikace. Ale abyste mohli využívat výhod SaaS, musí klienti předáním některé kontrolu nad svá vlastní data. Poskytovatele služeb SaaS, aby bylo možné zachovat bezpečné a izolované od ostatních klientů dat důvěřují. Příkladem tento druh víceklientské aplikace SaaS jsou MYOB, SnelStart a Salesforce.com. Každá z těchto aplikací můžou být dělené podél hranic klienta a podpora vzorů, které v tomto článku probereme návrhu aplikace.

Aplikace, které poskytují přímé služby zákazníkům nebo zaměstnancům v rámci organizace (často označované jako uživatele, nikoli klientů) jsou jiné kategorii na spektra víceklientské aplikace. Zákazníci objednat předplatné služby a data, která poskytovatele služeb, shromažďuje a ukládá nevlastní. Poskytovatelé služeb mají méně přísné požadavky uchovávat data zákazníků izolovaná od sebe navzájem nad rámec jakákoli ochranu osobních údajů. Tento druh aplikaci zaměřené zákazníka víceklientské příklady poskytovatelů obsahu média jako Netflix, Spotify a Xbox LIVE. Další příklady snadno rozdělený aplikací jsou zákazníkem, internetových aplikací nebo aplikace Internet věcí (IoT) v které každého zákazníka či zařízení může sloužit jako oddílu. Hranice oddílů můžete oddělit uživatelů a zařízení.

Ne všechny aplikace oddílu snadno podél jedné vlastnosti, například klienta, zákazníka, uživatele nebo zařízení. Komplexní enterprise prostředek plánování (ERP) aplikace, například má produktů, objednávek a zákazníků. Má obvykle komplexní schéma s tisíci vysoce vzájemně propojené tabulky.

Žádná strategie jednoho oddílu můžete použít pro všechny tabulky a fungovat na všech úloh aplikace. Tento článek se zaměřuje na více klientů aplikace, které mají snadno rozdělený data a úlohy.

## <a name="multi-tenant-application-design-trade-offs"></a>Kompromis víceklientské aplikace návrhu
Návrhový vzor, který obvykle vybírá vývojář víceklientské aplikace je založena na zvážení následující faktory:

* **Izolaci klientů**. Vývojář musí zajistit, že žádný klient má nežádoucí přístup k datům ostatních klientů. Požadavek na izolaci rozšiřuje na další vlastnosti, jako je například poskytování ochrany z aktivní okolí, bude možné obnovit data klienta a implementace vlastního nastavení konkrétního klienta.
* **Cloudové náklady prostředků**. Aplikace SaaS musí být cenově konkurenceschopným. Vývojář aplikace víceklientské zvolit optimalizovat pro nižší náklady na použití cloudové prostředky, jako je například úložiště a výpočetní náklady.
* **Snadné DevOps**. Vývojář aplikace více klientů musí obsahovat ochrany izolace, udržovat a sledovat stav své aplikace a schématu databáze a vyřešit problémy klienta. Složitostí při vývoji aplikací a operace se překládá přímo na vyšší náklady a vyzve s spokojenost klienta.
* **Škálovatelnost**. Schopnost postupně přidávat další klienty a kapacity pro klienty, kteří vyžadují je nutné pro úspěšné operace SaaS.

Každý z těchto faktorů má kompromis porovnání do jiného. Nabídka nemusí nabízet nejpohodlnější vývojového prostředí cloudu nejnižší náklady. Je důležité pro vývojáře k rozhodování informováni o těchto možnostech a jejich kompromis během procesu návrhu aplikace.

Vzor oblíbených vývoj je pack několik klientů do jednoho nebo několika databází. Výhody tohoto přístupu jsou nižší náklady, protože platíte za několik databází a relativní jednoduchost pracovat s omezený počet databází. Ale v čase, vývojář víceklientské aplikace SaaS bude Uvědomte si, že tato volba má významné downsides v izolaci mezi klienty a škálovatelnost. Pokud se stane důležité izolaci klientů, další úsilí se vyžaduje k ochraně dat klienta ve sdíleném úložišti před neoprávněným přístupem nebo aktivní okolí. Tato další úsilí může výrazně zvýšit úsilí vývoj a náklady na údržbu izolace. Podobně pokud přidávání klientů je potřeba, tento vzor návrhu obvykle vyžaduje znalosti znovu distribuovat klienta data mezi databázemi správně škálování datové vrstvy aplikace.  

Často izolaci klientů je základní požadavek ve víceklientské aplikace SaaS, které nahrazovat podnikům a organizacím. Vývojář může zvážit podle dosahovaný výhody v jednoduchost a náklady na přes izolaci mezi klienty a škálovatelnost. Tato změna potvrdit složité a nákladné jako službu zvětšování a požadavky na izolaci klientů přestat více důležité a spravované na aplikační vrstvu. V aplikacích více klientů, které poskytují služby přímé, určených pro zákazníky, ale izolaci klientů může být s nižší prioritou než optimalizace nákladů cloudu prostředku.

## <a name="multi-tenant-data-models"></a>Víceklientské datové modely
Běžné postupy pro návrhový pro umístění dat klienta podle tři odlišné modely na obrázku 1.

![datové modely víceklientské aplikace](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)

Obrázek 1: Běžné postupy pro návrhový pro více klientů datové modely

* **Databáze za klienta**. Každý klient má svou vlastní databázi. Všechna data konkrétního klienta je omezen na databázi klienta a jsou izolovány od ostatních klientů a jejich data.
* **Sdílené horizontálně dělené databáze**. Několik klientů sdílet mezi více databází. Odlišnou sadu klientů je přiřazený k každou databázi pomocí strategie dělení například hash, rozsah nebo seznamu dělení. Tato strategie distribuční dat často se označuje jako horizontálního dělení.
* **Sdílené databáze jedním**. Jedinou, někdy velký, databázi obsahuje data pro všechny klienty, které jsou jednoznačně rozlišit ve sloupci ID klienta.

> [!NOTE]
> Vývojář aplikací zvolit umístit jiných klientů v jiné databázi schémat, a pak použijte název schématu k rozlišení různých klientů. Vzhledem k tomu obvykle vyžaduje použití dynamické SQL a nemůže být efektivní při ukládání do mezipaměti plánu nedoporučujeme tento přístup. Ve zbývající části tohoto článku zaměříme na sdílené tabulky přístupu pro tuto kategorii víceklientské aplikace.
> 
> 

## <a name="popular-multi-tenant-data-models"></a>Oblíbené víceklientské datové modely
Je důležité vyhodnotit různé typy víceklientské datové modely z hlediska aplikace návrhu kompromisy, které jsme jste určili. Tyto faktory pomoci charakterizovat tři nejběžnější modely víceklientské dat popsané výše a jejich využití databáze, jak je znázorněno na obrázku 2.

* **Izolace.** Úroveň izolace mezi klienty lze míru izolace kolik klientů lze dosáhnout datový model.
* **Cloudové náklady prostředků**. Množství sdílení prostředků mezi klienty můžete optimalizovat náklady prostředků cloudu. Prostředek může být definován jako náklady na výpočetních operací a úložiště.
* **Náklady na DevOps**. Snadný vývoj aplikací, nasazení a správy snižuje celkové náklady na operaci SaaS.  

Obrázek 2 ukazuje osy Y úroveň izolace klienta. Ukazuje osy X úroveň sdílení prostředků. Šedá, diagonálních Šipka uprostřed označuje směr nákladů DevOps, které sledují zvýšení nebo snížení.

![Vzory návrhu oblíbených víceklientské aplikace](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png)

Obrázek 2: Oblíbených víceklientské datové modely

Pravém dolním kvadrantu obrázek 2 ukazuje application vzor, který používá potenciálně velké, sdílené jedné databáze a sdílené tabulky (nebo samostatné schéma) přístup. Je vhodné pro sdílení, protože všichni klienti používají stejné databáze prostředků (procesoru, paměti, vstupní a výstupní) v jediné databázi prostředků. Ale izolaci klientů je omezená. Možná budete muset provést další kroky k ochraně klientů od sebe navzájem na aplikační vrstvu. Tyto další kroky může výrazně zvýšit DevOps náklady na vývoj a správu aplikace. Škálovatelnost je omezena škále hardwaru, který je hostitelem databáze.

Dolním kvadrantu obrázek 2 ukazuje několik klientů horizontálně dělené napříč více databází (obvykle jiný hardware jednotek škálování). Každou databázi hostuje podmnožině klientů, které starosti škálovatelnost jiných vzorů. Pokud další kapacity se vyžaduje pro více klientů, můžete snadno umístit klienty na nové databáze přidělené jednotky škálování nový hardware. Ale tím se snižuje množství sdílení prostředků. Pouze klienti umístěny na stejném jednotek škálování sdílet zdroje. Tento přístup poskytuje málo zlepšování pro izolaci klientů, protože velký počet klientů jsou stále společně umístěná, bez automaticky chráněn z druhé strany akcí. Složitost aplikace zůstává vysoké.

Levém horním kvadrantu na obrázku 2 je třetí přístup. Každý klient data umístí do svou vlastní databázi. Tento přístup má vlastnosti dobrý izolaci klientů, ale omezuje sdílení prostředků při každé databáze má svou vlastní vyhrazených prostředcích. Tento přístup je vhodný, pokud všichni klienti předvídatelný úlohy. Pokud klientské úlohy se méně předvídatelný, nejde optimalizovat zprostředkovatele sdílení prostředků. Nepředvídatelnost je běžné pro SaaS aplikace. Zprostředkovatel musí být buď přečerpání zřídit, splňovat požadavky na nebo nižší prostředky. Náklady na vyšší nebo nižší spokojenost klienta buď výsledky akce. Vyšší stupeň sdílení prostředků mezi klienty stane žádoucí, aby více nákladově efektivní řešení. Zvýšením počtu databází se taky zvýší DevOps náklady na nasazení a údržbu aplikace. Bez ohledu na tyto otázky tato metoda poskytuje nejlepší a nejjednodušší izolaci pro klienty.

Tyto faktory ovlivňují také vzor návrhu, který vybere zákazníka:

* **Vlastnictví dat klienta**. Aplikace, ve kterém klienti zachovat vlastnictví svá vlastní data upřednostňuje vzor jedné databáze každého klienta.
* **Škálování**. Aplikace, která cílí stovky tisíc nebo miliony klienty upřednostňuje přístupy například horizontálního dělení sdílení databází. Požadavky na izolaci stále může představovat problémy.
* **Hodnota a obchodní model**. Pokud aplikace výnosy za klienta Pokud malá (méně než dolar), požadavky na izolaci stát méně kritický a sdílenou databázi, má smysl. Pokud za klienta výnos několik Kč, více možné je model databáze za klienta. Může pomoci snížit náklady na vývoj.

Zadána návrhu kompromisy znázorněno na obrázku 2 musí dobrý klienta izolace vlastnosti začlenit s sdílení mezi klienty optimální prostředků ideální víceklientského modelu. Tento model se vejde kategorie popsané v pravém horním kvadrantu obrázku 2.

## <a name="multi-tenancy-support-in-azure-sql-database"></a>Podpora více klientů ve službě Azure SQL Database
Azure SQL Database podporuje všechny vzorky víceklientské aplikace, které jsou uvedené na obrázku 2. S elastické fondy podporuje také application vzor, který kombinuje sdílení dobrý prostředků a výhody izolace databáze za klienta přístupu (viz kvadrantu pravém na obrázku 3). Elastické databáze nástroje a možnosti služby SQL Database pomoct snížit náklady na vývoj a provoz aplikace, která má mnoho databází (znázorněné na obrázku 3 podbarvené oblasti). Tyto nástroje můžete vytvářet a spravovat aplikace, které používají některá z několika databáze vzory.

![Vzorce v databázi Azure SQL](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png)

Obrázek 3: Vzory víceklientské aplikace ve službě Azure SQL Database

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Model databáze na klienta s elastické fondy a nástroje
Elastické fondy v databázi SQL kombinovat s sdílení mezi databází klienta do lepší podporu přístup databáze za klienta prostředků izolaci klientů. Databáze SQL je dat vrstvy řešení SaaS poskytovatelů, kteří sestavení víceklientským aplikacím. Zatížení mezi klienty sdílení prostředků přesouvá z aplikační vrstvu v databázové vrstvě služby. Elastické úlohy, elastické dotazu, elastické transakce a klientské knihovny pro elastické databáze je zjednodušená složitost správy a dotazování na škálování mezi databázemi.

| Požadavky na aplikace | Možnosti databáze SQL |
| --- | --- |
| Izolaci mezi klienty a sdílení prostředků |[Elastické fondy](sql-database-elastic-pool.md): přidělte fond prostředků, databáze SQL a sdílení prostředků napříč různými databázemi. Jednotlivé databáze také můžete vybírat tolik prostředky z fondu tak, aby dokázala pojmout kapacity vyžádání špičky z důvodu změn v klientské úlohy. Elastický fond sám sebe můžete podle potřeby škálovat nahoru nebo dolů. Elastické fondy také poskytují snadné správy a monitorování a řešení potíží s na úrovni fondu. |
| Snadné DevOps mezi databázemi |[Elastické fondy](sql-database-elastic-pool.md): jak si předtím poznamenali. |
| | [Elastické dotazu](sql-database-elastic-query-horizontal-partitioning.md): dotazu mezi databázemi pro vytváření sestav nebo analýza cross klient. |
| | [Elastické úlohy](sql-database-elastic-jobs-overview.md): balíček a spolehlivě nasazovat operací údržby databáze nebo změny schématu databáze do více databází. |
| | [Elastické transakce](sql-database-elastic-transactions-overview.md): proces změny několik databází atomic a izolované způsobem. Pokud aplikace potřebují "všechny nebo nic" záruky přes několik operací databáze, je nutné elastické transakce. |
| | [Klientská knihovna pro elastické databáze](sql-database-elastic-database-client-library.md): Spravovat distribuce, data a mapovat klienty k databázím. |

## <a name="shared-models"></a>Sdílené modely
Jak je popsáno výše, pro většinu poskytovatelů SaaS, může představovat sdílený model přístup problémy s klienta izolace problémů a složité kroky se vývoj aplikací a údržbu. Ale pro víceklientské aplikace, které poskytují služby přímo k příjemce, nemusí být požadavky na izolaci klienta jako vysokou prioritu jako minimalizovat náklady. Budou pravděpodobně moci pack klientům v jedné nebo více databází v vysokou hustotou ke snížení nákladů. Modely sdílené databáze pomocí jedné databáze nebo více horizontálně dělené databází může způsobit další efektivity prostředků sdílení a celkové náklady. Azure SQL Database zajišťuje, že některé funkce, které pomáhají zákazníkům sestavení izolace pro lepší zabezpečení a správu ve velkém měřítku v datové vrstvě.

| Požadavky na aplikace | Možnosti databáze SQL |
| --- | --- |
| Funkce izolace zabezpečení |[Zabezpečení na úrovni řádků](https://msdn.microsoft.com/library/dn765131.aspx) |
| [Schéma databáze](https://msdn.microsoft.com/library/dd207005.aspx) | |
| Snadné DevOps mezi databázemi |[Elastické dotazu](sql-database-elastic-query-horizontal-partitioning.md) |
| | [Elastické úlohy](sql-database-elastic-jobs-overview.md) |
| | [Elastické transakce](sql-database-elastic-transactions-overview.md) |
| | [Klientská knihovna Elastic Database](sql-database-elastic-database-client-library.md) |
| | [Rozdělení elastické databáze a sloučení](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Souhrn
Požadavky na izolaci klientů jsou důležité pro většinu víceklientské aplikace SaaS. Nejlepší možnost k zajištění izolace leans výraznou směrem k přístup databáze za klienta. Tyto dva přístupy vyžadují investice do vrstvy komplexní aplikace, které vyžadují zkušený vývoj zaměstnanci k zajištění izolace, což významně zvyšuje náklady a rizika. Pokud požadavky na izolaci nejsou v úvahu již v rané fázi vývoje služby, může být modernizace je i dražší v první dva modely. Hlavní nevýhody přidruženého k modelu databáze na klienta se vztahují k vyšší cloudové náklady prostředků z důvodu snížené sdílení a údržbu a správu velkého počtu databází. Vývojáři aplikací SaaS často potýkat s tím zajistily při provádění těchto kompromis.

I když kompromis může být hlavní překážek s většinu poskytovatelů cloudových služeb databáze, databáze SQL Azure eliminuje překážek s jeho elastický fond a možnosti elastické databáze. Vývojáři SaaS můžete kombinovat charakteristiky izolace modelu databáze za klienta a optimalizaci sdílení prostředků a vylepšení možností správy mnoho databází pomocí elastické fondy a související nástroje.

Víceklientské aplikace poskytovatele, kteří mají žádné požadavky na izolaci klientů a kdo může klientům pack databáze v vysokou hustotou zjistit, že sdílená data modelů výsledek v další efektivita při sdílení prostředků a snížit celkové náklady. Funkce zabezpečení, nástroje elastické databáze Azure SQL Database a knihovny horizontálního dělení pomoci poskytovatelů SaaS vytvářet a spravovat více klientů aplikace.

## <a name="next-steps"></a>Další kroky
[Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md) a ukázkovou aplikaci, která demonstruje klientské knihovny.

Vytvoření [vlastní řídicí panel elastický fond pro SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) a ukázkovou aplikaci používající fondů elastické databáze nákladově efektivní, škálovatelné řešení.

Pomocí nástroje Azure SQL Database pro [migrovat existující databáze pro rozšíření Škálováním](sql-database-elastic-convert-to-use-elastic-tools.md).

Vytvoření fondu elastické databáze pomocí portálu Azure, naleznete v části [vytvoření fondu elastické databáze](sql-database-elastic-pool-manage-portal.md).  

Zjistěte, jak [sledování a správě fondu elastické databáze](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>Další zdroje

* [Nasazení a prozkoumejte víceklientské aplikace, která používá Azure SQL Database – Wingtip SaaS](sql-database-saas-tutorial.md)
* [Co je Azure elastickém fondu?](sql-database-elastic-pool.md)
* [Horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md)
* [víceklientské aplikací pomocí nástroje elastické databáze a zabezpečení na úrovni řádků](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [Ověřování v víceklientské aplikace pomocí Azure Active Directory a OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Aplikace Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)


## <a name="questions-and-feature-requests"></a>Otázky a žádosti o funkce

Máte dotazy, pokud nás chcete najít v [SQL Database fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Přidat v žádosti o funkci [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).

