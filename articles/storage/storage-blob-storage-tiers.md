<properties
    pageTitle="Studené úložiště Azure pro objekty Blob | Microsoft Azure"
    description="Úrovně úložiště Blob  nabízejí cenově výhodné úložiště pro data objektu podle vzorů přístupu. Studené úložiště Azure je optimalizované pro data, která se využívají méně často."
    services="storage"
    documentationCenter=""
    authors="sribhat-msft"
    manager=""
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/09/2016"
    ms.author="sribhat"/>


# Azure Blob Storage: Horká a studená úroveň úložiště

## Přehled

Azure Storage teď nabízí dvě úrovně úložiště Blob (úložiště objektů), abyste náklady na ukládání dat mohli přizpůsobit tomu, jak často je využíváte. **Úroveň horkého úložiště** Azure je optimalizovaná pro ukládání dat, která se využívají často. **Úroveň studeného úložiště** Azure je optimalizovaná pro ukládání dat, která se nevyužívají často. U dat ve studeném úložišti se může tolerovat trochu horší dostupnost, ale přesto se u nich vyžaduje vysoká odolnost a podobná přístupová a přenosová rychlost jako u horkých dat. Úložiště pro studená data je výrazně levnější, ovšem za cenu mírně horší dostupnosti a vyšších nákladů na přístup.

Data uložená v cloudu dnes narůstají exponenciální rychlostí. Pokud chcete mít pod kontrolou náklady na rostoucí potřeby úložiště, je čím dál výhodnější svoje data třídit třeba podle toho, jak často jsou potřeba nebo jak dlouho se mají zachovat. Data uložená v cloudu se můžou výrazně lišit podle toho, jak se vytvářejí, zpracovávají a využívají. Některá data se během svojí existence využívají nebo mění často. Některá data se využívají hlavně krátce po svém vytvoření a přístupy k nim později velmi rychle klesnou. Některá data se po uložení v cloudu využívají zřídka, pokud vůbec někdy.

Pro každý z těchto scénářů využívání je vhodná jiná úroveň úložiště, která je optimalizovaná pro určitý vzor přístupu. S novými úrovněmi horkého a studeného úložiště vám teď úložiště Azure Blob umožní zohlednit tuto potřebu různých druhů úložišť na různých cenových úrovních.

## Účty úložiště Blob

**Účty úložiště Blob** jsou účty specializovaného úložiště pro ukládání nestrukturovaných dat jako blobů (objektů) v úložišti Azure Storage. S účty úložiště Blob si teď můžete vybrat mezi úrovněmi horkého a studeného přístupu – málo používaná data si ukládejte do studeného úložiště s nižší cenou za uložení, často používaná data si ukládejte do horkého úložiště s nižší cenou za přístup. Účty úložiště Blob jsou podobné účtům úložiště pro obecné účely a mají stejnou vysokou odolnost, dostupnost, škálovatelnost a výkonnost, a navíc mají 100% konzistentnost rozhraní API pro objekty blob bloku a doplňovací objekty blob.

> [AZURE.NOTE] Účty úložiště Blob podporují pouze objekty blob bloku a doplňovací objekty blob, nepodporují objekty blob stránky.

Účty úložiště Blob zpřístupňují atribut **Úroveň přístupu**, který vám umožní označit úroveň úložiště jako **Horkou** nebo **Studenou** podle toho, jaká data jsou v účtu uložená. Mezi úrovněmi přístupu můžete kdykoli přepnout, třeba pokud pro vaše data existuje vzor používání.

> [AZURE.NOTE] Se změnou úrovně přístupu můžou být spojené další poplatky. Další informace najdete v části [Ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing).

Mezi scénáře použití pro úroveň přístupu horkého úložiště můžou patřit například tyto:

- Data, která se aktivně používají nebo se k nim očekává častý aktivní přístup (čtení a zápis).
- Data, která jsou určená ke zpracování a eventuální migraci na úroveň studeného úložiště.

Mezi scénáře použití pro studenou úroveň přístupu můžou patřit například tyto:

- Záloha, archivace a datové sady pro zotavení po havárii.
- Starší obsah a média, které se již nezobrazují často, ale které by však měly být na vyžádání okamžitě dostupné.
- Velké datové sady, které je potřeba levně uložit, zatímco se shromažďují další data pro budoucí zpracování. (*např.* dlouhodobé uložení vědeckých dat, nezpracovaná telemetrická data z výrobního závodu)
- Původní (hrubá, nezpracovaná) data, která je potřeba zachovat i po jejich zpracování do konečné, použitelné podoby. (*např.* původní záznamy médií po překódování do jiných formátů)
- Data pro soulad a archivaci, které je potřeba uchovat po dlouhou dobu a téměř se k nim nepřistupuje. (*např.* záznamy z bezpečnostních kamer, staré rentgenové snímky / snímky magnetické rezonance pro zdravotnická zařízení, zvukové záznamy a přepisy zákaznických hovorů pro finanční služby)

Další informace o účtech úložiště najdete v tématu [Účty Azure Storage](storage-create-storage-account.md).

Pro aplikace, které potřebují jen úložiště objektů blob bloku a doplňovacích objektů blob doporučujeme použít účty úložiště Blob a využít tak výhody specializovaného cenového modelu úložiště děleného na úrovně. Chápeme ale, že to někdy nemusí být možné a použití účtů úložiště pro obecné účely by bylo vhodnější, třeba v těchto situacích:

- Potřebujete používat tabulky, fronty nebo soubory a chcete mít svoje objekty blob uložené ve stejném účtu úložiště. Všimněte si, že kromě stejných sdílených klíčů nemá jejich uložení ve stejném účtu žádnou jinou technickou výhodu.
- Stejně budete muset použít model nasazení Classic. Účty úložiště Blob jsou dostupné jen přes model nasazení Azure Resource Manager.
- Potřebujete použít objekty blob stránky. Účty úložiště Blob nepodporují objekty blob stránky. Pokud nemáte nějaký zvláštní nebo konkrétní důvod používat objekty blob stránky, obvykle doporučujeme používat objekty blob bloku.
- Používáte verzi rozhraní [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx), která je starší než 14.2.2014, nebo klientskou knihovnu verze nižší než 4.x a nemůžete svoji aplikaci upgradovat.

> [AZURE.NOTE] Účty úložiště Blob se aktuálně podporují ve většině oblastí Azure, ke kterým se v budoucnu připojí další. Aktuální seznam dostupných oblastí najdete na stránce [Dostupnost Azure Services v jednotlivých oblastech](https://azure.microsoft.com/regions/#services).

## Srovnání úrovní přístupu

V této tabulce najdete srovnání obou úrovní přístupu:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Úroveň přístupu horkého úložiště</center></strong></td>
    <td><strong><center>Úroveň přístupu studeného úložiště</center></strong></td
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
    <td colspan="2"><center>neuvedeno</center></td>
</tr>
<tr>
    <td><strong><center>Minimální doba uložení<center></strong></td>
    <td colspan="2"><center>neuvedeno</center></td>
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

> [AZURE.NOTE] Účty úložiště Blob podporují stejnou škálovatelnost a cíle výkonnosti jako účty úložiště pro obecné účely. Další informace najdete v tématu [Škálovatelnost a cíle výkonnosti Azure Storage Scalability](storage-scalability-targets.md).

## Ceny a fakturace

Účty úložiště Blob používají nový cenový model pro úložiště objektů blob podle úrovně přístupu. Při použití účtu úložiště Blob je potřeba vzít v úvahu tyto fakturační podmínky:

- **Cena za uložení**: Vedle uloženého množství dat se ceny za uložení dat liší také podle úrovně přístupu. Pro úroveň přístupu studeného úložiště je sazba za gigabajt nižší než pro úroveň přístupu horkého úložiště.
- **Cena za přístup**: Přístup k datům uloženým na úrovni přístupu studeného úložiště je zpoplatněný podle sazby za gigabajt přečtených a zapsaných dat.
- **Cena za transakci**: Na obě úrovně se vztahuje poplatek za jednotlivé transakce. Cena za transakci je však pro úroveň přístupu studeného úložiště vyšší než pro úroveň přístupu horkého úložiště.
- **Cena za datové přenosy geografické replikace**: Ta se vztahuje jen na účty s nastavenou geografickou replikací, jako třeba GRS a RA-GRS. Přenos dat geografické replikace je zpoplatněný podle sazby za gigabajt.
- **Cena za odchozí datové přenosy**: Odchozí přenosy dat (dat přenesených směrem z oblasti Azure) jsou zpoplatněné podle využití šířky pásma sazbou za gigabajt, stejně jako je tomu u účtů úložiště pro obecné účely.
- **Změna úrovně přístupu**: Změna úrovně přístupu ze studené na horkou je zpoplatněná ve výši, která odpovídá přečtení všech dat v aktuálním účtu úložiště pro každý přechod. Naopak změna úrovně přístupu z horkého úložiště na studené je bezplatná.

> [AZURE.NOTE] Aby si zákazníci mohli vyzkoušet nové úrovně úložiště a ověřit funkčnost služby po jejím spuštění bude změna úrovně přístupu ze studeného úložiště na horké až do 30. června 2016 bezplatná. Od 1. července 2016 budou všechny přechody ze studeného úložiště na horké zpoplatněné. Další informace o cenovém modelu pro účty úložiště Blob najdete na stránce [Azure Storage – Ceny](https://azure.microsoft.com/pricing/details/storage/). Další informace o poplatcích za odchozí přenosy dat najdete na stránce [Informace o cenách datových přenosů](https://azure.microsoft.com/pricing/details/data-transfers/).

## Rychlý start

V této části si předvedeme následující scénáře s použitím Portálu Azure:

- Vytvoření účtu úložiště Blob.
- Správa účtu úložiště Blob.

### Použití Portálu Azure

#### Vytvoření účtu úložiště Blob pomocí Portálu Azure

1. Přihlaste se k [Portálu Azure](https://portal.azure.com).

2. V nabídce centra vyberte **Nový** -> **Data a úložiště** -> **Účet úložiště**.

3. Zadejte název nového účtu úložiště.

4. Jako model nasazení vyberte **Resource Manager**.

5. Jako typ účtu úložiště vyberte **úložiště Blob**.

6. Vyberte úroveň přístupu: **Horká ** nebo **Studená**. Výchozí hodnota je **Horká**.

7. Vyberte možnost replikace pro účet úložiště: **LRS**, **GRS** nebo **RA-GRS**. Výchozí hodnota je **RA-GRS**. Další informace o možnostech replikace pro Azure Storage najdete v tématu [Replikace Azure Storage](storage-redundancy.md).

8. Vyberte předplatné, ve kterém chcete vytvořit nový účet úložiště.

9. Zadejte novou skupinu prostředků nebo vyberte existující skupinu prostředků. Podrobnější informace o skupinách prostředků najdete v tématu [Použití Portálu Azure ke správě prostředků Azure](../azure-portal/resource-group-portal.md).

10. Vyberte zeměpisné umístění účtu úložiště.

11. Vytvořte účet kliknutím na **Vytvořit**.

#### Změna úrovně přístupu účtu úložiště Blob pomocí Portálu Azure

1. Přihlaste se do [Portálu Azure](https://portal.azure.com) a přejděte k účtu úložiště.

2. Klikněte na **Všechna nastavení** a potom na **Konfigurace**, tady můžete zobrazit a měnit konfiguraci účtu.

3. Zadejte požadovanou úroveň přístupu: **Horká ** nebo **Studená**.

    > [AZURE.NOTE] Se změnou úrovně přístupu můžou být spojené další poplatky. Další informace najdete v části [Ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing).

## Migrace na účty úložiště Blob

Cílem této části je pomoct uživatelům s hladkým přechodem na účty úložiště Blob. Účet úložiště Blob se specializuje pouze na ukládání objektů blob bloku a doplňovacích objektů blob. Existující účty úložiště pro obecné účely, ve kterých můžete ukládat tabulky, fronty, soubory a disky i objekty blob, se nedají převést na účty úložiště Blob. Pokud chcete využívat úrovně úložiště, budete muset vytvořit nové účty úložiště Blob a svoje existující data migrovat do těchto nových účtů.

### Plánování migrace existujících dat

Pokud přesouváte data do účtu úložiště Blob, asi budete chtít využít výhody úrovně studeného úložiště a ušetřit na datech, která se využívají méně často. Prvním krokem při plánování migrace dat do účtu úložiště Blob na studené úrovni přístupu je vyhodnotit svůj aktuální vzor používání a zjistit, jestli by se migrace na účet úložiště Blob vyplatila. Celkově vzato budete chtít znát:

- Vzor spotřeby vašeho úložiště – Kolik dat je uloženo a jak se toto množství měsíc od měsíce mění?
- Vzor přístupů k vašemu úložišti – Kolik dat se čte z účtu a zapisuje na účet (včetně nových dat)? Jaké transakce a kolik se jich používá pro přístup k datům?

Pokud chcete sledovat existující účty úložiště a shromažďovat tato data, přečtěte si téma [Povolení metrik Azure Storage a zobrazení údajů metrik](storage-enable-and-view-metrics.md). Tyto údaje teď můžete použít v [Cenové kalkulačce pro Azure Storage](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) a získat tak představu o nákladech.

### Migrace existujících dat

Následující metody můžete použít pro migraci existujících dat do účtů úložiště Blob z lokálních úložných zařízení, z cloudového úložiště jiných poskytovatelů nebo z existujícího účtu úložiště Azure pro obecné účely:

#### AzCopy

AzCopy je nástroj Windows, který se spouští z příkazového řádku a který je určený pro vysoce výkonné kopírování dat do Azure Storage a z Azure Storage. Pomocí nástroje AzCopy můžete kopírovat data do svého účtu úložiště Blob z existujících účtů úložiště pro obecné účely nebo ukládat data z lokálních úložných zařízení do svého účtu úložiště Blob.

Další informace najdete v tématu [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md).

#### Knihovna pro přesun dat

Knihovna pro přesun dat v Azure Storage pro .NET je založená na platformě pro přesun základních dat, kterou využívá nástroj AzCopy. Knihovna je určená pro vysoce výkonné, spolehlivé a snadné operace přesunu dat, podobně jako AzCopy. Díky tomu můžete naplno využívat výhody funkcí AzCopy ve svojí aplikaci nativně, bez nutnosti spouštět a sledovat externí instance nástroje AzCopy.

Další informace najdete v tématu [Knihovna pro přesun dat v Azure Storage pro .Net](https://github.com/Azure/azure-storage-net-data-movement).

#### Rozhraní REST API nebo knihovna klienta

Pomocí některé z knihoven klienta Azure nebo rozhraní REST API služeb úložiště Azure můžete vytvořit vlastní aplikaci pro migraci dat do účtu úložiště Blob. Azure Storage poskytuje množství knihoven klienta pro různé jazyky a platformy, jako například .NET, Java, C++, Node.JS, PHP, Ruby nebo Python. Knihovny klienta nabízí pokročilé možnosti a funkce, jako je například logika opakovaných pokusů, protokolování a paralelní ukládání. Můžete také psát aplikace přímo na rozhraní REST API, které může zavolat jakýkoli jazyk schopný vytvářet požadavky přes HTTP/HTTPS.

Další informace najdete v tématu [Začínáme s úložištěm Azure Blob](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Objekty blob šifrované na straně klienta ukládají metadata šifrování uložená s objektem blob. Je absolutně nezbytné, aby každý použitý mechanizmus kopírování zajistil, aby zůstala zachovaná metadata objektu blob, především metadata šifrování. Pokud objekty blob zkopírujete bez těchto metadat, obsah objektu blob bude nenávratně ztracený. Další informace o metadatech šifrování najdete v tématu [Šifrování Azure Storage na straně klienta](storage-client-side-encryption.md).

## Nejčastější dotazy

1. **Jsou existující účty úložiště stále dostupné?**

    Ano, existující účty úložiště jsou stále dostupné a jejich funkce ani cena se nemění.  Není u nich možné si vybrat úroveň přístupu a ani v budoucnu tuto možnost mít nebudou.

2. **Proč a kdy bych měl/a začít používat účty úložiště Blob?**

    Účty úložiště Blob se specializují na ukládání objektů blob a umožňují nám zavádět nové funkce pro objekty blob. Výhledově se pro ukládání objektů blob budou doporučovat účty úložiště Blob, protože budou mít funkce hierarchie a úrovní úložiště podle typu účtu. Kdy budete chtít migrovat ale záleží na vás a vašich obchodních potřebách.

3. **Můžu převést svůj existující účet úložiště na účet úložiště Blob?**

    Ne. Účet úložiště Blob je jiný druh účtu. Budete si muset vytvořit nový a do něj pak migrovat existující data, jak jsme vysvětlili výše.

4. **Můžu na jednom účtu ukládat objekty v obou úrovních přístupu?**

    Úroveň přístupu je atribut účtu a platí pro všechny objekty v daném účtu. Úroveň přístupu se nedá nastavit pro jednotlivé objekty zvlášť.

5. **Můžu změnit úroveň přístupu účtu úložiště Blob?**

    Ano. Úroveň přístupu můžete změnit pro účet úložiště. Změna úrovně přístupu na úrovni účtu se projeví na všech objektech uložených v daném účtu. Úroveň přístupu můžete z horké na studenou změnit bezplatně, ale změna ze studené na horkou je zpoplatněná částkou, která odpovídá přečtení všech dat v účtu podle platné sazby za gigabajt.

6. **Jak často můžu měnit úroveň přístupu svého účtu úložiště Blob?**

    Neuplatňujeme žádné omezení toho, jak často se smí měnit úroveň přístupu, ale musíte vzít na vědomí, že změna úrovně přístupu ze studené na horkou pro vás znamená výrazné náklady. Nedoporučujeme měnit úroveň přístupu často.

7. **Budou se objekty blob ve studené úrovni přístupu úložiště chovat jinak než objekty blob v horké úrovni přístupu úložiště?**

    Objekty blob v horké úrovni úložiště mají stejnou latenci jako objekty blob v účtech úložiště pro obecné účely. Objekty blob ve studené úrovni úložiště mají podobnou latenci (v řádech milisekund) jako objekty blob v účtech úložiště pro obecné účely.

    Objekty blob ve studené úrovni přístupu úložiště budou mít trochu nižší dostupnost služby (SLA) než objekty blob uložené v horké úrovni přístupu úložiště. Další informace najdete v tématu [SLA pro úložiště](https://azure.microsoft.com/support/legal/sla/storage).

8. **Můžu do účtů úložiště Blob ukládat objekty blob stránky a disky virtuálních počítačů?**

    Účty úložiště Blob podporují pouze objekty blob bloku a doplňovací objekty blob, nepodporují objekty blob stránky. Disky virtuálních počítačů Azure se opírají o objekty blob stránky, proto se účty úložiště Blob nedají použít k uložení disků virtuálních počítačů. Zálohy disků virtuálních počítačů se ale dají do účtu úložiště Blob uložit jako objekty blob bloku.

9. **Budu muset změnit svoje existující aplikace, aby používaly účty úložiště Blob?**

    Účty úložiště Blob mají rozhraní API 100% konzistentní s účty úložiště pro obecné účely pro objekty blob bloku a doplňující objekty blob. Pokud vaše aplikace používá objekty blob bloku nebo doplňující objekty blob a pokud používáte verzi rozhraní [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) z 14.2.2014 nebo novější, měla by vaše aplikace prostě fungovat. Pokud používáte starší verzi protokolu, budete muset svoji aplikaci aktualizovat, aby používala novou verzi, a mohla tak pracovat s oběma typy účtů úložiště zároveň. Celkově vždy doporučujeme používat nejnovější verzi bez ohledu na to, který typ účtu úložiště používáte.

10. **Budu muset něco dělat jinak?**

    Účty úložiště Blob jsou velmi podobné účtům úložiště pro obecné účely pro ukládání objektů blob bloku a doplňovacích objektů blob a podporují všechny klíčové funkce Azure Storage, včetně vysoké odolnosti a dostupnosti, škálovatelnosti, výkonnosti a zabezpečení. Kromě funkcí a omezení charakteristických pro účty úložiště Blob a jejich úrovní přístupu, jak jsme je popsali výše, by všechno ostatní mělo zůstat stejné.

## Další kroky

### Posouzení účtů úložiště Blob

[Ověření dostupnosti účtů úložiště Blob v jednotlivých oblastech](https://azure.microsoft.com/regions/#services)

[Zapnutí metrik Azure Storage a vyhodnocení používání aktuálních účtů úložiště](storage-enable-and-view-metrics.md)

[Posouzení cen úložiště Blob v jednotlivých oblastech](https://azure.microsoft.com/pricing/details/storage/)

[Posouzení cen přenosu dat](https://azure.microsoft.com/pricing/details/data-transfers/)

### Začátek používání účtů úložiště Blob

[Začínáme s úložištěm Azure Blob](storage-dotnet-how-to-use-blobs.md)

[Přesunutí dat z Azure Storage a do Azure Storage](storage-moving-data.md)

[Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)



<!--HONumber=Jun16_HO2-->


