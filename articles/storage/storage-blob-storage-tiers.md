---
title: "Studené úložiště Azure pro objekty blob | Dokumentace Microsoftu"
description: "Úrovně úložiště služby Azure Blob Storage umožňují vybrat si cenově výhodné úložiště pro data objektů podle toho, jak často k nim chcete přistupovat. Úroveň studeného úložiště Azure je optimalizovaná pro data, která se využívají méně často."
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2017
ms.author: mihauss
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: 618d31e991d9405ece6533727d700e316ae85bec
ms.lasthandoff: 04/06/2017


---
# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob Storage: Horká a studená úroveň úložiště
## <a name="overview"></a>Přehled
Azure Storage nabízí dvě úrovně úložiště pro ukládání objektů Blob. Data, abyste mohli data ukládat co nejhospodárněji – to znamená podle toho, jak je používáte. **Úroveň horkého úložiště** Azure je optimalizovaná pro ukládání dat, která se využívají často. **Úroveň studeného úložiště** Azure je optimalizovaná pro ukládání dat, která se nevyužívají často. U dat ve studeném úložišti se toleruje horší dostupnost, ale přesto se u nich vyžaduje vysoká odolnost a podobná přístupová a přenosová rychlost jako u horkých dat. U studených dat jsou poplatky za uložení výrazně levnější, ovšem za cenu mírně horší dostupnosti a vyšších nákladů na přístup.

Data uložená v cloudu dnes narůstají exponenciální rychlostí. Pokud chcete náklady na rozšiřující se úložiště udržet pod kontrolou, je vhodné uspořádat data podle vlastností, jako je četnost přístupu a plánovaná doba uchování. Data uložená v cloudu se liší tím, jak se generují, zpracovávají a jak se k nim přistupuje po celou dobu jejich životnosti. Některá data se během svojí existence využívají nebo mění často. Některá data se používají často v rané fázi svého životního cyklu, ale s tím jak stárnou, přístup k nim výrazně klesá. Některá data se po uložení v cloudu využívají zřídka, pokud vůbec někdy.

Pro každý z těchto scénářů využívání je vhodná jiná úroveň úložiště, která je optimalizovaná pro určitý vzor přístupu. Se zavedením horké a studené úrovně teď služba Azure Blob Storage vychází vstříc potřebě různých úrovní úložiště s odlišnými cenovými modely.

## <a name="blob-storage-accounts"></a>Účty úložiště Blob
**Účty úložiště Blob** jsou účty specializovaného úložiště pro ukládání nestrukturovaných dat jako blobů (objektů) v úložišti Azure Storage. S účty úložiště Blob si teď můžete vybrat mezi horkou a studenou úrovní – málo používaná data ukládejte do studeného úložiště s nižší cenou za uložení, často používaná data ukládejte do horkého úložiště s nižší cenou za přístup. Účty úložiště Blob jsou podobné účtům úložiště pro obecné účely a mají stejnou vysokou odolnost, dostupnost, škálovatelnost a výkonnost, a navíc mají 100% konzistentnost rozhraní API pro objekty blob bloku a doplňovací objekty blob.

> [!NOTE]
> Účty úložiště Blob podporují pouze objekty blob bloku a doplňovací objekty blob, nepodporují objekty blob stránky.
> 
> 

Účty služby Blob Storage zpřístupňují atribut **Úroveň přístupu**, který umožňuje označit vrstvu úložiště jako **horkou** nebo **studenou** podle toho, jaká data se do účtu ukládají. Pokud začnete k datům přistupovat jinak často, můžete mezi úrovněmi úložiště kdykoliv přepnout.

> [!NOTE]
> Se změnou úrovně úložiště můžou být spojeny další poplatky. Další informace najdete v části [Ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing).
> 
> 

Na úrovni horkého úložiště můžete mít třeba tyhle typy dat:

* Data, která se aktivně používají nebo se k nim očekává častý aktivní přístup (čtení a zápis).
* Data, která jsou určená ke zpracování a eventuální migraci na úroveň studeného úložiště.

Studená úroveň úložiště se hodí například pro tahle data:

* Záloha, archivace a datové sady pro zotavení po havárii.
* Starší obsah a média, které se již nezobrazují často, ale které by však měly být na vyžádání okamžitě dostupné.
* Velké datové sady, které je potřeba levně uložit, zatímco se shromažďují další data pro budoucí zpracování. (*např.* dlouhodobé uložení vědeckých dat, nezpracovaná telemetrická data z výrobního závodu)
* Původní (hrubá, nezpracovaná) data, která je potřeba zachovat i po jejich zpracování do konečné, použitelné podoby. (*např.* původní multimediální záznamy po překódování do jiných formátů)
* Data pro soulad a archivaci, které je potřeba uchovat po dlouhou dobu a téměř se k nim nepřistupuje. (*např.* záznamy z bezpečnostních kamer, staré rentgenové snímky / snímky magnetické rezonance pro zdravotnická zařízení, zvukové záznamy a přepisy zákaznických hovorů pro finanční služby)

Další informace o účtech úložiště najdete v tématu [Účty Azure Storage](storage-create-storage-account.md).

Pro aplikace, které potřebují jen úložiště objektů blob bloku a doplňovacích objektů blob doporučujeme použít účty úložiště Blob a využít tak výhody specializovaného cenového modelu úložiště děleného na úrovně. Uvědomujeme si, že za určitých okolností to nemusí být možné, protože někdy stačí použít účty úložiště pro obecné účely, třeba v těchto situacích:

* Potřebujete používat tabulky, fronty nebo soubory a chcete mít svoje objekty blob uložené ve stejném účtu úložiště. Všimněte si, že kromě stejných sdílených klíčů nemá jejich uložení ve stejném účtu žádnou jinou technickou výhodu.
* Stejně budete muset použít model nasazení Classic. Účty úložiště Blob jsou dostupné jen přes model nasazení Azure Resource Manager.
* Potřebujete použít objekty blob stránky. Účty úložiště Blob nepodporují objekty blob stránky. Pokud nemáte nějaký zvláštní nebo konkrétní důvod používat objekty blob stránky, obvykle doporučujeme používat objekty blob bloku.
* Používáte verzi rozhraní [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx), která je starší než 14.2.2014, nebo klientskou knihovnu verze nižší než 4.x a nemůžete svoji aplikaci upgradovat.

> [!NOTE]
> Účty Blob Storage jsou aktuálně podporované ve všech oblastech Azure.
> 
> 

## <a name="comparison-between-the-storage-tiers"></a>Srovnání úrovní úložiště
V této tabulce najdete srovnání obou úrovní úložiště:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Úroveň horkého úložiště</center></strong></td>
    <td><strong><center>Úroveň studeného úložiště</center></strong></td
</tr>
<tr>
    <td><strong><center>Dostupnost</center></strong></td>
    <td><center>99,9 %</center></td>
    <td><center>99 %</center></td>
</tr>
<tr>
    <td><strong><center>Dostupnost<br>(přístupy pro čtení RA-GRS)</center></strong></td>
    <td><center>99,99 %</center></td>
    <td><center>99,9 %</center></td>
</tr>
<tr>
    <td><strong><center>Poplatky za používání</center></strong></td>
    <td><center>Vyšší poplatky za uložení<br>Nižší poplatky za přístup a transakci</center></td>
    <td><center>Nižší poplatky za uložení<br>Vyšší poplatky za přístup a transakci</center></td>
</tr>
<tr>
    <td><strong><center>Minimální velikost objektu<center></strong></td>
    <td colspan="2"><center>–</center></td>
</tr>
<tr>
    <td><strong><center>Minimální doba uložení<center></strong></td>
    <td colspan="2"><center>–</center></td>
</tr>
<tr>
    <td><strong><center>Latence<br>(Čas do prvního bajtu)<center></strong></td>
    <td colspan="2"><center>milisekundy</center></td>
</tr>
<tr>
    <td><strong><center>Škálovatelnost a cíle výkonnosti<center></strong></td>
    <td colspan="2"><center>Stejné jako u účtů úložiště pro obecné účely</center></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Účty úložiště Blob podporují stejnou škálovatelnost a cíle výkonnosti jako účty úložiště pro obecné účely. Další informace najdete v tématu [Škálovatelnost a cíle výkonnosti Azure Storage Scalability](storage-scalability-targets.md).
> 
> 

## <a name="pricing-and-billing"></a>Ceny a fakturace
Účty úložiště Blob vychází z nových cenových modelů pro úrovně úložiště. Při použití účtu úložiště Blob je potřeba vzít v úvahu tyto fakturační podmínky:

* **Cena za uložení**: Vedle uloženého množství dat se cena za uložení odvíjí také od úrovně úložiště. Na úrovni studeného úložiště je sazba za GB nižší než na horké úrovni.
* **Cena za přístup**: Přístup k datům ve studeném úložišti je zpoplatněný podle sazby za GB přečtených a zapsaných dat.
* **Cena za transakci**: Na obě úrovně se vztahuje poplatek za jednotlivé transakce. Cena za transakci je však na úrovni studeného úložiště vyšší než na úrovni horkého úložiště.
* **Cena za přenosy dat geografické replikace**: Ta se vztahuje jen na účty s nastavenou geografickou replikací, jako třeba GRS a RA-GRS. Přenos dat geografické replikace je zpoplatněný podle sazby za GB.
* **Cena za odchozí přenosy dat**: Odchozí přenosy dat (dat přenesených směrem z oblasti Azure) jsou zpoplatněné podle využití šířky pásma sazbou za GB, stejně jako je tomu u účtů úložiště pro obecné účely.
* **Změna úrovně úložiště**: Změna úrovně ze studené na horkou je za každý přechod zpoplatněna částkou ve výši, která odpovídá přečtení všech dat v aktuálním účtu úložiště. Změna úrovně úložiště z horké na studenou je naopak bezplatná.

> [!NOTE]
> Aby si zákazníci mohli nové úrovně úložiště vyzkoušet a ověřit, jak služba po spuštění funguje, je i změna úrovně úložiště ze studené na horkou až do 30. června 2016 bezplatná. Od 1. července 2016 budou všechny přechody ze studeného úložiště na horké zpoplatněné. Další informace o cenovém modelu pro účty úložiště Blob najdete na stránce [Azure Storage – Ceny](https://azure.microsoft.com/pricing/details/storage/). Další informace o poplatcích za odchozí přenosy dat najdete na stránce [Informace o cenách přenosů dat](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

## <a name="quick-start"></a>Rychlý start
V tomto oddílu si předvedeme následující scénáře použití rozhraní Azure Portal:

* Vytvoření účtu úložiště Blob.
* Správa účtu úložiště Blob.

### <a name="using-the-azure-portal"></a>Použití webu Azure Portal
#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Vytvoření účtu úložiště Blob přes web Azure Portal
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra vyberte **Nový** > **Data a úložiště** > **Účet úložiště**.
3. Zadejte název účtu úložiště.
   
    Název musí být globálně jedinečný, protože je součástí adresy URL pro přístup k objektům v účtu úložiště.  
4. Jako model nasazení vyberte **Resource Manager**.
   
    Úrovně úložiště lze použít jen s účty úložiště Resource Manageru, proto u nových prostředků doporučujeme tento model nasazení. Další informace najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).  
5. V rozevíracím seznamu Druh účtu vyberte **Blob Storage**.
   
    Tady vybíráte typ účtu úložiště. Úrovně úložiště nejsou dostupné v úložišti pro obecné účely. Využít je můžete jen s účtem typu Blob Storage.     
   
    Všimněte si, že když tuto možnost vyberete, úroveň výkonu se nastaví na Standardní. V úrovni výkonu Premium nejsou úrovně úložiště dostupné.
6. Vyberte možnost replikace pro účet úložiště: **LRS**, **GRS** nebo **RA-GRS**. Výchozí hodnota je **RA-GRS**.
   
    LRS = místně redundantní úložiště; GRS = geograficky redundantní úložiště (2 oblasti); RA GRS je geograficky redundantní úložiště s oprávněním ke čtení (2 oblasti s oprávněním ke čtení v té druhé).
   
    Další informace o možnostech replikace v Azure Storage najdete v článku [Replikace Azure Storage](storage-redundancy.md).
7. Vyberte si úroveň úložiště, která vám vyhovuje: **Úroveň přístupu** nastavte na **Studená** nebo **Horká**. Výchozí hodnota je **Hot**.
8. Vyberte předplatné, ve kterém chcete vytvořit nový účet úložiště.
9. Zadejte novou skupinu prostředků nebo vyberte existující skupinu prostředků. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
10. K účtu úložiště přiřaďte oblast.
11. Vytvořte účet úložiště kliknutím na **Vytvořit**.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Změna úrovně úložiště u účtu úložiště Blob přes web Azure Portal
1. Přihlaste se na web [Azure Portal ](https://portal.azure.com).
2. Klikněte na možnost Všechny prostředky a kliknutím na účet úložiště do účtu přejděte.
3. V okně Nastavení klikněte na **Konfigurace**, odkud můžete zobrazit nebo změnit konfiguraci účtu.
4. Vyberte si úroveň úložiště, která vám vyhovuje: **Úroveň přístupu** nastavte na **Studená** nebo **Horká**.
5. V horní části okna klikněte na možnost Uložit.

> [!NOTE]
> Se změnou úrovně úložiště můžou být spojeny další poplatky. Další informace najdete v části [Ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing).
> 
> 

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Vyhodnocení služby a migrace na účty úložiště Blob
V tomto oddílu chceme uživatelům pomoci s pohodlným přechodem k účtům úložiště Blob. Jako uživatel stojíte před jednou z těchto dvou možností:

* Máte účet úložiště pro obecné účely a chcete vyhodnotit přechod k účtu úložiště Blob s vhodnou úrovní úložiště.
* Rozhodli jste se používat účet úložiště Blob, nebo už ho dokonce máte, a chcete vyhodnotit, jestli si vybrat horkou nebo studenou úroveň.

V obou případech je nejdřív na místě odhadnout cenu ukládání a přístupu k datům uloženým na účtu úložiště Blob a srovnat ji s aktuálními náklady.

### <a name="evaluating-blob-storage-account-tiers"></a>Vyhodnocení úrovní účtu úložiště Blob
Abyste stanovili přibližnou cenu za ukládání a přístup k datům uloženým na účtu úložiště Blob, vyhodnoťte, jak v současné době k datům přistupujete, nebo odhadněte, jak k nim přistupovat budete. Celkově vzato budete chtít znát:

* Spotřebu úložiště – Kolik dat ukládáte a jak se toto množství měsíc od měsíce mění?
* Vzorec přistupování k úložišti – Kolik dat se na účtu čte a zapisuje (včetně nových dat)? Ke kolika transakcím dochází při přístupu k datům a o jaké transakce se jedná?

#### <a name="monitoring-existing-storage-accounts"></a>Monitorování existujících účtů úložiště
K monitorování existujících účtů úložiště a sesbírání dat můžete využít službu Azure Storage Analytics, která aktivitu zaprotokoluje a na účtu úložiště naměří potřebné údaje.
Služba Storage Analytics může naměřená data, včetně souhrnné statistiky transakcí a dat o kapacitě požadavků na službu Blob Storage, ukládat jak pro účty úložiště pro obecné účely, tak pro účty úložiště Blob.
Tato data se ukládají do známých tabulek na tom samém účtu úložiště.

Další podrobnosti najdete na stránce [o metrikách Storage Analytics](https://msdn.microsoft.com/library/azure/hh343258.aspx) a [o tabulkovém schématu metrik Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Účet úložiště Blob zpřístupňuje koncový bod tabulkové služby jenom pro účely ukládání a zpřístupnění dat naměřených tomuto účtu.
> 
> 

Pokud chcete monitorovat spotřebu úložiště Blob, je potřeba povolit kapacitní metriky.
Když tuto funkci zapnete, data o kapacitě služby Blob pro daný účet úložiště se budou denně zaznamenávat jako zápisy do tabulky *$MetricsCapacityBlob* v rámci stejného účtu úložiště.

Aby bylo možné monitorovat vzorec přistupování k datům služby Blob Storage, je potřeba povolit hodinovou metriku transakcí na úrovni API.
Když tuto funkci zapnete, data o transakcích API se budou každou hodinu shromažďovat a zaznamenávat jako zápisy do tabulky *$MetricsHourPrimaryTransactionsBlob* v rámci stejného účtu úložiště. V případě účtů úložiště RA-GRS zaznamenává tabulka *$MetricsHourSecondaryTransactionsBlob* transakce do sekundárního koncového bodu.

> [!NOTE]
> Pokud máte účet úložiště pro obecné účely, ve kterém jsou uložené objekty blob stránek a disky virtuálních počítačů vedle dat doplňovacích objektů blob, odhad tímto postupem provést nepůjde. A to proto, že nebude možné rozeznat, která naměřená kapacita a transakce patří jen k blob blokům či jen k doplňovacím objektům blob, které lze migrovat na účet úložiště Blob.
> 
> 

Pokud chcete dobře odhadnout spotřebu dat a přístup k nim, doporučujeme pro měření dat vybrat takovou dobu uchování, která dobře vystihuje pravidelné používání, a potom údaje extrapolovat.
Můžete například měřená data uchovávat po sedm dní, sesbírat jednou za týden a analyzovat je na konci měsíce.
Nebo změřte a nasbírejte data za posledních 30 dní a na konci 30denního období je analyzujte.

Podrobnosti o povolení, shromažďování a zobrazování dat metrik naleznete v tématu [Povolení metrik Azure Storage a prohlížení dat metrik](storage-enable-and-view-metrics.md).

> [!NOTE]
> Uložení, zobrazování a stahování analyzovaných data je účtováno stejně jako běžná uživatelská data.
> 
> 

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Odhadnutí nákladů s pomocí naměřených údajů o využití
##### <a name="storage-costs"></a>Cena za uložení
Poslední položka v tabulce kapacitní metriky *$MetricsCapacityBlob* s klíčem řádku *data* zobrazuje kapacitu úložiště spotřebovanou uživatelskými daty.
Poslední položka v tabulce kapacitní metriky *$MetricsCapacityBlob* s klíčem řádku *analytics* zobrazuje kapacitu úložiště spotřebovanou protokoly analýzy.

Celková kapacita spotřebovaná uživatelskými daty a protokoly analýzy (pokud jsou povoleny) se potom dají použít k odhadu nákladů za uložení dat v účtu úložiště.
Stejnou metodu lze použít taky pro odhad nákladů za uložení blob bloků a doplňujících objektů blob v účtech úložiště pro obecné účely.

##### <a name="transaction-costs"></a>Cena za transakce
Součet *TotalBillableRequests* všech položek rozhraní API v tabulce metrik transakcí udává celkový počet transakcí daného rozhraní API. *Například* celkový počet transakcí *GetBlob* v daném časovém období se dá vypočítat jako celkový součet fakturovatelných požadavků všech položek s klíčem řádku *user;GetBlob*.

Pokud chcete odhadnout náklady za transakce v účtu úložiště Blob, je potřeba rozdělit transakce do tří skupin, protože se cenově liší.

* Transakce zápisu jako *PutBlob*, *PutBlock*, *PutBlockList*, *AppendBlock*, *ListBlobs*, *ListContainers*, *CreateContainer*, *SnapshotBlob* a *CopyBlob*.
* Transakce odstranění jako *DeleteBlob* a *DeleteContainer*.
* Veškeré ostatní transakce.

Pokud chcete odhadnout náklady za transakce v účtu úložiště pro obecné účely, je potřeba započítat všechny transakce bez ohledu na operaci či rozhraní API.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Přístup k datům a cena za přenos dat – geografická replikace
Analýza úložiště sice k účtu úložiště nevypíše množství přečtených a zapsaných dat, toto množství lze ale zhruba odhadnout z tabulky metriky transakcí.
Součet *TotalBillableRequests* všech položek rozhraní API v tabulce metrik transakcí udává celkové množství příchozích dat k tomuto rozhraní API v bajtech.
Podobně součet *TotalEgress* udává celkové množství odchozích dat v bajtech.

Pokud chcete odhadnout náklady za přístup k datům v účtu úložiště Blob, je potřeba transakce rozdělit do dvou skupin.

* Množství dat načtených z účtu úložiště lze odhadnout ze součtu *TotalEgress* především u operací *GetBlob* a *CopyBlob*.
* Množství dat zapsaných do účtu úložiště lze odhadnout ze součtu *TotalIngress* především u operací *PutBlob*, *PutBlock*, *CopyBlob* a *AppendBlock*.

Také cena za přenos geograficky replikovaných dat účtů úložiště Blob se v případě typu GRS nebo RA-GRS dá vypočítat pomocí toho, že odhadnete množství zapsaných dat.

> [!NOTE]
> Podrobnější příklad výpočtu ceny za využívání úrovně horkého či studeného úložiště najdete v odpovědi na často kladenou otázku *„Co je horká a studená úroveň a jak určit, která z nich se má použít?“* na stránce [Ceny za Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
> 
> 

### <a name="migrating-existing-data"></a>Migrace existujících dat
Účet úložiště Blob se specializuje pouze na ukládání objektů blob bloku a doplňovacích objektů blob. Existující účty úložiště pro obecné účely, ve kterých můžete ukládat tabulky, fronty, soubory a disky i objekty blob, se nedají převést na účty úložiště Blob. Pokud chcete využívat úrovně úložiště, budete muset vytvořit nové účty úložiště Blob a svoje existující data migrovat do těchto nových účtů.

Následující metody můžete použít pro migraci existujících dat do účtů úložiště Blob z místních úložných zařízení, z cloudového úložiště jiných poskytovatelů nebo z existujícího účtu úložiště Azure pro obecné účely:

#### <a name="azcopy"></a>AzCopy
AzCopy je nástroj Windows, který se spouští z příkazového řádku a který je určený pro vysoce výkonné kopírování dat do Azure Storage a z Azure Storage. Pomocí nástroje AzCopy můžete kopírovat data do svého účtu úložiště Blob z existujících účtů úložiště pro obecné účely nebo ukládat data z lokálních úložných zařízení do svého účtu úložiště Blob.

Další informace najdete v tématu [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Knihovna pro přesun dat
Knihovna pro přesun dat v Azure Storage pro .NET je založená na platformě pro přesun základních dat, kterou využívá nástroj AzCopy. Knihovna je určená pro vysoce výkonné, spolehlivé a snadné operace přenosu dat, podobně jako AzCopy. Díky tomu můžete naplno využívat výhody funkcí AzCopy ve svojí aplikaci nativně, bez nutnosti spouštět a sledovat externí instance nástroje AzCopy.

Další informace najdete v tématu [Knihovna pro přesun dat v Azure Storage pro .Net](https://github.com/Azure/azure-storage-net-data-movement).

#### <a name="rest-api-or-client-library"></a>Rozhraní REST API nebo knihovna klienta
Pomocí některé z knihoven klienta Azure nebo rozhraní REST API služeb úložiště Azure můžete vytvořit vlastní aplikaci pro migraci dat do účtu úložiště Blob. Azure Storage poskytuje množství knihoven klienta pro různé jazyky a platformy, jako například .NET, Java, C++, Node.JS, PHP, Ruby nebo Python. Knihovny klienta nabízí pokročilé možnosti a funkce, jako je například logika opakovaných pokusů, protokolování a paralelní ukládání. Můžete také psát aplikace přímo na rozhraní REST API, které může zavolat jakýkoli jazyk schopný vytvářet požadavky přes HTTP/HTTPS.

Další informace najdete v tématu [Začínáme s úložištěm Azure Blob](storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Objekty blob šifrované na straně klienta ukládají metadata šifrování uložená s objektem blob. Je absolutně nezbytné, aby každý použitý mechanizmus kopírování zajistil, aby zůstala zachovaná metadata objektu blob, především metadata šifrování. Pokud objekty blob zkopírujete bez těchto metadat, obsah objektu blob bude nenávratně ztracený. Podrobnější informace o šifrování metadat najdete v článku o [Azure Storage a šifrování na straně klienta](storage-client-side-encryption.md).
> 
> 

## <a name="faqs"></a>Nejčastější dotazy
1. **Jsou existující účty úložiště stále dostupné?**
   
    Ano, existující účty úložiště jsou stále dostupné a jejich funkce ani cena se nemění.  Není u nich možné vybrat úroveň úložiště a tato možnost nebude ani v budoucnosti.
2. **Proč a kdy bych měl/a začít používat účty úložiště Blob?**
   
    Účty úložiště Blob se specializují na ukládání objektů blob a umožňují nám zavádět nové funkce pro objekty blob. Výhledově se pro ukládání objektů blob budou doporučovat účty úložiště Blob, protože budou mít funkce hierarchie a úrovní úložiště podle typu účtu. Kdy budete chtít migrovat ale záleží na vás a vašich obchodních potřebách.
3. **Můžu převést svůj existující účet úložiště na účet úložiště Blob?**
   
    Ne. Účet úložiště Blob je jiný druh účtu. Budete si muset vytvořit nový a do něj pak migrovat existující data, jak jsme vysvětlili výše.
4. **Můžu na jednom účtu ukládat objekty do obou úrovní úložiště?**
   
    Atribut *Úroveň přístupu*, který indikuje úroveň úložiště, se nastavuje v rámci účtu, a platí tak pro všechny objekty na tom účtu. Atribut úrovně přístupu se nedá nastavit zvlášť pro jednotlivé objekty.
5. **Můžu účtu úložiště Blob změnit úroveň úložiště?**
   
    Ano. Úroveň úložiště můžete změnit nastavením atributu *Úroveň přístupu* na svém účtu úložiště. Změna úrovně úložiště se projeví u všech objektů uložených na účtu. Úroveň úložiště můžete z horké na studenou změnit bezplatně, ale změna ze studené na horkou je zpoplatněna částkou, která odpovídá přečtení všech dat v účtu podle platné sazby za GB.
6. **Jak často můžu účtu úložiště Blob změnit úroveň úložiště?**
   
    Možnost měnit úroveň úložiště v tomto směru neomezujeme, ale vezměte na vědomí, že změna úrovně úložiště ze studené na horkou s sebou nese značné náklady. Nedoporučujeme měnit úroveň úložiště často.
7. **Budou se objekty blob ve studené úrovni úložiště chovat jinak než objekty blob v horké úrovni úložiště?**
   
    Objekty blob v horké úrovni úložiště mají stejnou latenci jako objekty blob v účtech úložiště pro obecné účely. Objekty blob ve studené úrovni úložiště mají podobnou latenci (v řádech milisekund) jako objekty blob v účtech úložiště pro obecné účely.
   
    Objekty blob ve studené úrovni úložiště budou mít trochu nižší dostupnost služby (SLA) než objekty blob uložené v horké úrovni úložiště. Další informace najdete v tématu [SLA pro úložiště](https://azure.microsoft.com/support/legal/sla/storage).
8. **Můžu do účtů úložiště Blob ukládat objekty blob stránky a disky virtuálních počítačů?**
   
    Účty úložiště Blob podporují pouze objekty blob bloku a doplňovací objekty blob, nepodporují objekty blob stránky. Disky virtuálních počítačů Azure se opírají o objekty blob stránky, proto se účty úložiště Blob nedají použít k uložení disků virtuálních počítačů. Zálohy disků virtuálních počítačů se ale dají do účtu úložiště Blob uložit jako objekty blob bloku.
9. **Budu muset změnit svoje existující aplikace, aby používaly účty úložiště Blob?**
   
    Účty úložiště Blob mají rozhraní API 100% konzistentní s účty úložiště pro obecné účely pro objekty blob bloku a doplňující objekty blob. Pokud aplikace používá objekty blob bloku nebo objekty blob připojení a vy používáte verzi rozhraní [služby úložiště REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) z 14. 2. 2014 nebo novější, měla by aplikace fungovat. Pokud používáte starší verzi protokolu, budete muset svoji aplikaci aktualizovat, aby používala novou verzi, a mohla tak pracovat s oběma typy účtů úložiště zároveň. Celkově vždy doporučujeme používat nejnovější verzi bez ohledu na to, který typ účtu úložiště používáte.
10. **Budu muset něco dělat jinak?**
    
    Účty úložiště Blob jsou velmi podobné účtům úložiště pro obecné účely pro ukládání objektů blob bloku a doplňovacích objektů blob a podporují všechny klíčové funkce Azure Storage, včetně vysoké odolnosti a dostupnosti, škálovatelnosti, výkonnosti a zabezpečení. Kromě funkcí a omezení charakteristických pro účty a úrovně úložiště Blob, jak jsme popsali výše, všechno ostatní zůstává při starém.

## <a name="next-steps"></a>Další kroky
### <a name="evaluate-blob-storage-accounts"></a>Posouzení účtů úložiště Blob
[Ověření dostupnosti účtů úložiště Blob v jednotlivých oblastech](https://azure.microsoft.com/regions/#services)

[Zapnutí metrik Azure Storage a vyhodnocení používání aktuálních účtů úložiště](storage-enable-and-view-metrics.md)

[Posouzení cen úložiště Blob v jednotlivých oblastech](https://azure.microsoft.com/pricing/details/storage/)

[Posouzení cen přenosu dat](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Začátek používání účtů úložiště Blob
[Začínáme s úložištěm Azure Blob](storage-dotnet-how-to-use-blobs.md)

[Přesunutí dat z Azure Storage a do Azure Storage](storage-moving-data.md)

[Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)

[Procházení a prozkoumání účtů úložiště](http://storageexplorer.com/)


