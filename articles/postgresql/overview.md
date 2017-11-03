---
title: "Přehled Azure databáze pro služba relační databáze PostgreSQL | Microsoft Docs"
description: "Poskytuje přehled databáze Azure pro služba relační databáze PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 10/20/2017
ms.openlocfilehash: 5b5da758e966cc5ca536d7b291be74409f02ca73
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/21/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>Co je Azure databáze PostgreSQL?

Azure databázi PostgreSQL je služba relační databáze v cloudu Microsoft, které jsou vytvořené pro vývojáře na základě komunity verze softwaru open source [PostgreSQL](https://www.postgresql.org/) databázového stroje. Tato služba je ve verzi public preview. Azure databázi PostgreSQL přináší:

- Integrovanou vysokou dostupnost s bez dalších nákladů.
- Předvídatelný výkon, pomocí včetně ceny průběžnými platbami.
- Škálování za chodu během několika sekund.
- Zabezpečená k ochraně citlivých dat na rest a v provozu.
- Automatické zálohování a bodu v čas – obnovení až 35 dnů.
- Podnikové úrovni zabezpečení a dodržování předpisů.

Všechny tyto možnosti vyžadují téměř žádné správy a všechny jsou k dispozici bez dalších poplatků. Tyto funkce umožňují zaměřit na rychlý vývoj aplikací a urychlení doby uvedení na trh, namísto přidělování drahocenný čas a prostředky se správou virtuálních počítačů a infrastruktury. Kromě toho můžete nadále vývoj aplikace s otevřeným zdrojem nástroje a platformy podle svého výběru a poskytovat s rychlostí a požadavky na vaše podnikání bez nutnosti další nové dovednosti efektivitu. 

Tento článek je úvodem do databáze Azure pro PostgreSQL základní koncepty a funkcí týkajících se výkonu, škálovatelnosti a spravovatelnosti. Viz tyto elementy QuickStart, které vám pomůžou začít:

- [Vytvoření databáze Azure pro PostgreSQL pomocí portálu Azure](quickstart-create-server-database-portal.md)
- [Vytvoření databáze Azure pro PostgreSQL pomocí rozhraní příkazového řádku Azure](quickstart-create-server-database-azure-cli.md)

Sada ukázek Azure CLI najdete v části:

- [Ukázek Azure CLI pro databázi Azure pro PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Úprava výkonu a škálování během několika sekund
Ve verzi preview, databáze Azure pro službu MySQL nabízí dvě úrovně služby: Basic a Standard. Každá úroveň nabízí různé výkon a možnosti pro podporu – od nejlehčích k těm nejnáročnějším. Můžete sestavit svoji první aplikaci s malou databází pro několik dolarů za měsíc a pak upravit měřítko podle potřeb vašeho řešení. Dynamické škálovatelnost umožňuje databázi transparentně reagovat na měnící se rychle požadavky na prostředky. Platíte jenom pro prostředky. můžete třeba a jenom v případě potřeby. V tématu [cenové úrovně](concepts-service-tiers.md) podrobnosti.

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Jak můžete rozhodnout, kdy k vytočení nahoru a dolů? Použijete integrovanou výkonu monitorování a výstrah funkce v kombinaci s hodnocení výkonu na základě výpočetní jednotek. Používání těchto nástrojů, můžete rychle posoudit dopad výpočetní jednotky škálování nebo dolů na základě vaší aktuální nebo předpokládané výkonu potřeb. V tématu [výstrahy](howto-alert-on-metric.md) podrobnosti.

## <a name="keep-your-app-and-business-running"></a>Udržujte své aplikace a podnikáni v chodu
Azure špičkové 99,99 % dostupnost (není k dispozici ve verzi preview) smlouvu o úrovni služeb (SLA) používá technologii globální sítě datových center spravovaných společností Microsoft, pomáhá udržet vaše aplikace s 24/7. S každou Azure databázi PostgreSQL serveru můžete využít výhod integrované zabezpečení, odolnost proti chybám a ochrany dat, která by jinak muset koupit nebo návrh, vytvářet a spravovat. Každá úroveň služeb s Azure Database pro PostgreSQL, nabízí komplexní sadu business kontinuitu podnikových procesů a možnosti, které můžete použít k zahájení a udržení tímto způsobem. Můžete využít [obnovení databáze do určitého bodu v čase](howto-restore-server-portal.md), a to až 35 dnů zpět. Kromě toho pokud datového centra hostujícího vaše databáze dojde k výpadku, můžete obnovit databáze z geograficky redundantní kopie nejnovější zálohy.

## <a name="secure-your-data"></a>Zabezpečení dat
Služba Azure databáze mají tradici zabezpečení dat, databáze Azure pro PostgreSQL zachovává s funkcemi, které omezit přístup, ochrana dat na rest a v provozu a umožňují sledovat aktivitu. Přejděte [Centrum zabezpečení Azure](https://www.microsoft.com/TrustCenter/Security/default.aspx) informace o bezpečnosti samotné platformy Azure.

Databáze Azure pro službu PostgreSQL používá šifrování úložiště pro data na rest. Data, včetně zálohy jsou zašifrovaná na disku (s výjimkou dočasné soubory vytvořené modulem při spuštění dotazů). Služba používá šifrování AES 256 bitů, který je součástí šifrování úložiště Azure a klíče je spravován systémem. Šifrování úložiště je vždycky zapnuté a nejde zakázat.

Ve výchozím nastavení je databáze Azure pro službu PostgreSQL nakonfigurovaná tak, aby vyžadovala [zabezpečení připojení SSL](./concepts-ssl-connection-security.md) pro data za provozu v síti. Vynucování připojení SSL mezi databázový server a klientských aplikací pomáhá chránit před útoky "man uprostřed" šifrování datový proud mezi serverem a aplikace.  Volitelně můžete zakázat, budete požadovat protokol SSL pro připojení k databázi služby, pokud klientská aplikace nepodporuje připojení SSL.

## <a name="next-steps"></a>Další kroky
- Najdete v článku [stránce s cenami](https://azure.microsoft.com/pricing/details/postgresql/) porovnávání náklady a kalkulačky.
- Začínáme [vytvoření první databáze Azure pro PostgreSQL](./quickstart-create-server-database-portal.md).
- Vytvoření první aplikace Python, PHP, Ruby, C\#, Java, Node.js: [knihovny připojení](./concepts-connection-libraries.md)
