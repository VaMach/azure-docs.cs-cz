---
title: "Přehled Azure databáze pro služba relační databáze MySQL | Microsoft Docs"
description: "Přehled databáze Azure pro služba relační databáze MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/20/2017
ms.custom: mvc
ms.openlocfilehash: 2a9efdd9285dfa5fca450ede64e5f6ee54cbc72b
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2017
---
# <a name="what-is-azure-database-for-mysql"></a>Co je Azure Database pro databázi MySQL?
Databáze pro databázi MySQL Azure je služba relační databáze v cloudu Microsoftu na základě [MySQL Community Edition](https://www.mysql.com/products/community/) databázového stroje. Tato služba je ve verzi public preview. Azure Database pro databázi MySQL nabízí:

- Integrovanou vysokou dostupnost s bez dalších nákladů.
- Předvídatelný výkon, pomocí včetně ceny průběžnými platbami.
- Škálování za chodu během několika sekund.
- Zabezpečená k ochraně citlivých dat na rest a v provozu.
- Automatické zálohování a bodu v čas – obnovení až 35 dnů.
- Podnikové úrovni zabezpečení a dodržování předpisů.

Tyto možnosti vyžadují téměř žádné správy a všechny jsou k dispozici bez dalších poplatků. Umožňují zaměřit na rychlý vývoj aplikací a urychlení doby uvedení na trh než přidělování drahocenný čas a prostředky se správou virtuálních počítačů a infrastruktury. Kromě toho můžete nadále vyvíjet aplikace s otevřeným zdrojem nástroje a platformy zvoleného zajistit vaše obchodní požadavky, všechny bez nutnosti další nové dovednosti s rychlostí a efektivitu.

![Databáze Azure pro koncepční diagram MySQL](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Tento článek je úvodem do databáze Azure pro MySQL základní koncepty a funkcí týkajících se výkonu, škálovatelnosti a spravovatelnosti, s odkazy na podrobnější informace. Viz tyto elementy QuickStart, které vám pomůžou začít:
- [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Vytvoření serveru Azure Database for MySQL pomocí Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md)

Sada ukázek Azure CLI najdete v části:
- [Ukázek Azure CLI pro databázi Azure pro databázi MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Úprava výkonu a škálování během několika sekund
Ve verzi preview, databáze Azure pro službu MySQL nabízí dvě úrovně služby: Basic a Standard. Každá úroveň nabízí různé výkon a možnosti pro podporu – od nejlehčích k těm nejnáročnějším. Můžete sestavit svoji první aplikaci s malou databází pro několik dolarů za měsíc a pak upravit měřítko podle potřeb vašeho řešení. Dynamické škálovatelnost umožňuje databázi transparentně reagovat na měnící se rychle požadavky na prostředky. Platíte jenom pro prostředky. můžete třeba a jenom v případě potřeby. V tématu [cenové úrovně](concepts-service-tiers.md) podrobnosti.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Jak můžete rozhodnout, kdy k vytočení nahoru a dolů? Použijete integrovanou výkonu monitorování a výstrah funkce v kombinaci s hodnocení výkonu na základě výpočetní jednotek. Používání těchto nástrojů, můžete rychle posoudit dopad výpočetní jednotky škálování nebo dolů na základě vaší aktuální nebo předpokládané výkonu potřeb. V tématu [výstrahy](howto-alert-on-metric.md) podrobnosti.

## <a name="keep-your-app-and-business-running"></a>Udržujte své aplikace a podnikáni v chodu
Azure špičkové 99,99 % dostupnost smlouvu o úrovni služeb (SLA) používá technologii globální sítě datových center spravovaných společností Microsoft, pomáhá udržet vaše aplikace s 24/7. S každou databází Azure pro server databáze MySQL můžete využít výhod integrované zabezpečení, odolnost proti chybám a ochrany dat, která by jinak muset koupit nebo návrh, vytvářet a spravovat. S Azure Database pro databázi MySQL můžete v okamžiku obnovení obnovit server do předchozího stavu, až 35 dnů.

## <a name="secure-your-data"></a>Zabezpečení dat
Služby Azure databáze mít tradici zabezpečení dat, které podporuje Azure Database pro databázi MySQL, s funkcemi, které omezit přístup, ochranu dat na rest a v provozu a vám pomůže sledovat činnost. Přejděte [Centrum zabezpečení Azure](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) informace o bezpečnosti samotné platformy Azure.

Databáze Azure pro službu MySQL používá šifrování úložiště pro data na rest. Data, včetně zálohování, jsou zašifrovaná na disku (s výjimkou dočasné soubory, které jsou vytvořené pomocí modulu při spuštění dotazů). Služba používá šifrování AES 256 bitů, který je součástí šifrování úložiště Azure a klíče je spravován systémem. Šifrování úložiště je vždycky zapnuté a nejde zakázat.

Ve výchozím nastavení je databáze Azure pro službu MySQL nakonfigurovaná tak, aby vyžadovala [zabezpečení připojení SSL](./concepts-ssl-connection-security.md) pro data za provozu v síti. Vynucení připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.  Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení k databázi služby, pokud klientská aplikace nepodporuje připojení SSL.

## <a name="next-steps"></a>Další kroky
Teď, když jste si přečetli Úvod do Azure Database pro databázi MySQL a znáte odpověď na otázku "Co je Azure databáze pro MySQL?", jste připraveni:
- Najdete na stránce s cenami pro porovnávání náklady a kalkulačky. [Ceny](https://azure.microsoft.com/pricing/details/mysql/)
- Začněte vytvořením první server. [Vytvoření serveru Azure Database for MySQL pomocí webu Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Vytvoření první aplikace pomocí preferovaný jazyk: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md)  |  [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [přejděte](./connect-go.md)
