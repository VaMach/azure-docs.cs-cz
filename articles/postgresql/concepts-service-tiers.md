---
title: "Cenové úrovně v Azure databázi PostgreSQL"
description: "Cenové úrovně v Azure databázi PostgreSQL"
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: article
ms.date: 05/31/2017
ms.openlocfilehash: 59ac187e50527517c402a5cc2aa871717dde5650
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-options-and-performance-understand-whats-available-in-each-pricing-tier"></a>Databáze Azure pro výkon a možnosti PostgreSQL: co je k dispozici v jednotlivých cenových úrovní
Když vytvoříte databázi Azure pro PostgreSQL server, rozhodněte tři hlavní možnosti konfigurace prostředky přidělené pro tento server. Tyto možnosti vliv na výkon a škálování serveru.
- Cenová úroveň
- Výpočetní jednotky
- Storage (GB)

Každá cenová úroveň má rozsah (výpočetní jednotky) úrovně výkonu můžete vybírat, v závislosti na požadavcích vaší úlohy. Vyšší úrovně výkonu poskytnout další prostředky pro váš server, která poskytuje vyšší propustnost. Můžete změnit úroveň výkonu serveru v rámci cenovou úroveň s téměř žádné výpadky aplikací.

> [!IMPORTANT]
> Když služba je ve verzi public preview, není zaručené smlouvy úroveň služeb (SLA).

V rámci Azure Database pro PostgreSQL server může mít jednu nebo více databází. Můžete se rozhodnout vytvořit jednu databázi na server pro využití všech prostředků nebo vytvořit několik databází, které budou prostředky sdílet. 

## <a name="choose-a-pricing-tier"></a>Zvolte cenovou úroveň.
Zatímco ve verzi preview, databáze Azure pro PostgreSQL nabízí dvě cenové úrovně: Basic a Standard. Úroveň Premium zatím není k dispozici, ale brzy. 

Následující tabulka obsahuje příklady cenovou úrovní služeb vhodné pro různé zátěže a aplikace.

| Cenová úroveň | Cílová zátěž |
| :----------- | :----------------|
| Basic | Nejvhodnější pro malé úlohy, které vyžadují škálovatelných výpočetních prostředků a úložiště bez zaručit IOPS. Mezi příklady patří servery, které jsou používány pro vývoj nebo testování nebo méně rozsáhlá zřídka používané aplikace. |
| Standard | Přejděte k možnosti pro cloudové aplikace, které potřebují IOPS zaručit při vysoké propustnosti. Mezi příklady patří webové nebo analytických aplikací. |
| Premium | Nejvhodnější pro úlohy, které je třeba s nízkou latencí pro transakce a vstupně-výstupní operace. Poskytuje nejlepší podporu pro více souběžných uživatelů. Platí pro databáze podporující kritické podnikové procesy.<br />Cenová úroveň Premium není k dispozici ve verzi preview. |

Při rozhodování o cenovou úroveň, nejprve spusťte tak, že určíte, pokud vaše úlohy potřebuje záruku IOPS. Pokud ano, použijte cenová úroveň Standard.

| **Funkce cenové úrovně** | **Basic** | **Standard** |
| :------------------------ | :-------- | :----------- |
| Maximální výpočetní jednotky | 100 | 800 | 
| Maximální celková velikost úložiště | 1 TB | 1 TB | 
| Záruku IOPS úložiště | Není k dispozici | Ano | 
| Maximální velikost úložiště IOPS | Není k dispozici | 3000 | 
| Doba uchovávání záloh databáze | 7 dní | 35 dní | 

Období preview nelze změnit cenovou úroveň, jakmile je vytvořena serveru. V budoucnu bude možné upgradovat nebo starší verzi serveru z jednu cenovou úroveň na jinou vrstvu.

## <a name="understand-the-price"></a>Pochopení za cenu.
Když vytvoříte novou databázi Azure pro PostgreSQL uvnitř [portálu Azure](https://portal.azure.com/#create/Microsoft.PostgreSQLServer), klikněte na tlačítko **cenová úroveň** okno a měsíční náklady se zobrazí na základě na vybrané možnosti. Pokud nemáte předplatné Azure, použijte Azure cenové kalkulačky získat odhadované ceny. Navštivte [Azure cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) web, pak klikněte na tlačítko **přidat položky**, rozbalte **databáze** kategorie a zvolte **databáze Azure pro PostgreSQL**  přizpůsobit možnosti.

## <a name="choose-a-performance-level-compute-units"></a>Zvolte si úroveň výkonu (výpočetní jednotky)
Po určení cenovou úroveň pro vaši databázi Azure pro PostgreSQL server, jste připraveni k určení úrovně výkonu výběrem počet jednotek výpočetní potřeby. Vhodná výchozí hodnota je 200 nebo 400 výpočetní jednotky pro aplikace, které vyžadují vyšší souběžnosti uživatele pro jejich web nebo analytické úlohy a postupně podle potřeby upravit. 

Výpočetní že jednotky jsou měřítkem propustnost zpracování procesoru, který zaručeně být k dispozici pro jednu databázi Azure pro PostgreSQL server. Výpočetní jednotka je kombinaci měření prostředků procesoru a paměti.  Další informace najdete v tématu [vysvětlením výpočetní jednotky](concepts-compute-unit-and-storage.md)

### <a name="basic-pricing-tier-performance-levels"></a>Základní cenovou úroveň úrovně výkonu:

| **Úroveň výkonu** | **50** | **100** |
| :-------------------- | :----- | :------ |
| Maximální počet výpočetní jednotky | 50 | 100 |
| Velikost zahrnuté úložiště | 50 GB | 50 GB |
| Maximální velikost úložiště serveru\* | 1 TB | 1 TB |

### <a name="standard-pricing-tier-performance-levels"></a>Standardní cenovou úroveň úrovně výkonu:

| **Úroveň výkonu** | **100** | **200** | **400** | **800** |
| :-------------------- | :------ | :------ | :------ | :------ |
| Maximální počet výpočetní jednotky | 100 | 200 | 400 | 800 |
| Velikost zahrnuté úložiště a zřízené IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS | 125 GB<br/> 375 IOPS |
| Maximální velikost úložiště serveru\* | 1 TB | 1 TB | 1 TB | 1 TB |
| Zřízení serveru maximální IOPS | 3000 IOPS | 3000 IOPS | 3000 IOPS | 3000 IOPS |
| Zřízení serveru maximální IOPS za GB | Opravené 3 IOPS za GB | Opravené 3 IOPS za GB | Opravené 3 IOPS za GB | Opravené 3 IOPS za GB |

\*Maximální velikost úložiště serveru odkazuje na maximální zřízené velikost úložiště pro váš server.

## <a name="storage"></a>Úložiště 
Konfigurace úložiště definuje množství kapacity úložiště, které jsou k dispozici pro databázi Azure pro PostgreSQL server. Úložiště používá služba zahrnuje soubory databáze, transakční protokoly a protokoly serveru PostgreSQL. Zvažte velikost úložiště potřebné pro hostování své databáze a požadavky na výkon (IOPS) při výběru konfigurace úložiště.

Některé kapacita úložiště je zahrnutý minimálně s každou cenovou úroveň, jsou popsány v předchozí jako "Zahrnuté velikost úložiště". Kapacita úložiště lze přidat, když je vytvořen serveru, v přírůstcích po 125 GB až do maximální povolené úložiště. Další úložnou kapacitu se dá nakonfigurovat nezávisle na výpočetní jednotky konfigurace. Změny ceny založenou na velikosti úložiště vybrané.

Konfigurace IOPS v každé úrovni výkonu má vztah k cenové úrovně a velikost úložiště vybrali. Základní úroveň neposkytuje záruku IOPS. V rámci standardní cenovou úroveň úměrně IOPS pro maximální velikost úložiště v pevný poměr 3:1. Zahrnuté úložiště 125 GB zaručuje 375 zřízené iops, každý s velikostí vstupně-výstupní operace až 256 kB. Další úložiště můžete 1 TB maximum zaručit 3000 zřízené IOPS.

Monitorování grafu metriky na portálu Azure nebo psát příkazy rozhraní příkazového řádku Azure k měření spotřeby úložiště a IOPS. Relevantní metriky pro monitorování jsou limit úložiště, procento úložiště, použité úložiště a vstupně-výstupní operace procent.

>[!IMPORTANT]
> Zatímco ve verzi preview, zvolte velikost úložiště na čas, kdy je vytvořena serveru. Změna velikosti úložiště na existující server se ještě nepodporuje. 

## <a name="scaling-a-server-up-or-down"></a>Škálování server nahoru nebo dolů
Původně zvolíte cenovou úroveň a úroveň výkonu, když vytvoříte databázi Azure pro PostgreSQL. Později, je možné škálovat výpočetní jednotky nahoru nebo dolů, dynamicky v rozsahu stejné cenovou úroveň. Na portálu Azure, posuňte výpočetní jednotky na serveru cenová úroveň okno, nebo pomocí skriptu podle tento příklad: [sledování a škálování jediného PostgreSQL serveru pomocí rozhraní příkazového řádku Azure](scripts/sample-scale-server-up-or-down.md)

Výpočetní jednotky škálování se provádí nezávisle na maximální velikost úložiště, které jste vybrali.

Na pozadí Změna úrovně výkonu databáze vytvoří repliku původní databázi na novou úroveň výkonu a pak přepne připojení k replice. Během tohoto procesu bude ztracena žádná data. Během krátké chvíli, když jsme přepnout na repliku je zakázána připojení k databázi, aby některé transakce na cestě může být vrácena zpět. Délka tohoto časového období je různá, ale v průměru nepřekračuje 4 sekundy a ve více než 99 % případů nepřekročí 30 sekund. Pokud v okamžiku zákazu připojení probíhá velké množství transakcí, může se toto časové období prodloužit.

Dobu trvání procesu celý škálování závisí na velikosti i cenová úroveň serveru před a po provedení změny. Například server, který mění výpočetní jednotky v rámci standardní cenovou úroveň, provést v rámci několik minut. Nové vlastnosti pro server se nepoužívají, dokud nebudou dokončeny změny.

## <a name="next-steps"></a>Další kroky
- Další informace o výpočetní jednotky, najdete v části [vysvětlením výpočetní jednotky](concepts-compute-unit-and-storage.md)
- Zjistěte, jak [sledování a škálování jediného PostgreSQL serveru pomocí rozhraní příkazového řádku Azure](scripts/sample-scale-server-up-or-down.md)
