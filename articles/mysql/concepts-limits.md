---
title: "Omezení v Azure databáze pro databázi MySQL"
description: "Tento článek popisuje omezení v Azure Database pro databázi MySQL, jako je například počet připojení a možnosti úložiště modul."
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 85e57170c1cbd977d2de6e7e614916333c79e047
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Omezení v Azure databáze pro databázi MySQL
Databáze Azure pro službu MySQL je ve verzi public preview. Následující části popisují kapacity, modul úložiště s podporou, oprávnění podpory, podpora příkaz manipulaci dat a funkční omezení ve službě databázového. Viz také [obecná omezení](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) vztahuje k modulu databáze MySQL.

## <a name="service-tier-maximums"></a>Maximální hodnoty úroveň služby
Azure databáze MySQL, má více úrovní služeb, které lze vybírat při vytváření serveru. Další informace najdete v tématu [Azure Database pro databázi MySQL cenové úrovně](concepts-pricing-tiers.md).  

Je maximální počet připojení, výpočetní jednotky a úložiště v jednotlivých úrovních služeb verzi Preview, následujícím způsobem: 

|**Cenová úroveň**| **Výpočetní generování**|**vCore(s)**| **Maximální počet připojení**|
|---|---|---|---|
|Basic| Gen 4| 1| 50|
|Basic| Gen 4| 2| 100|
|Basic| Gen 5| 1| 50|
|Basic| Gen 5| 2| 100|
|Obecné použití| Gen 4| 2| 200|
|Obecné použití| Gen 4| 4| 400|
|Obecné použití| Gen 4| 8| 800|
|Obecné použití| Gen 4| 16| 1600|
|Obecné použití| Gen 4| 32| 3200|
|Obecné použití| Gen 5| 2| 200|
|Obecné použití| Gen 5| 4| 400|
|Obecné použití| Gen 5| 8| 800|
|Obecné použití| Gen 5| 16| 1600|
|Obecné použití| Gen 5| 32| 3200|
|Paměťově optimalizované| Gen 5| 2| 600|
|Paměťově optimalizované| Gen 5| 4| 1250|
|Paměťově optimalizované| Gen 5| 8| 2500|
|Paměťově optimalizované| Gen 5| 16| 5000|
|Paměťově optimalizované| Gen 5| 32| 10000| 

Když se dosáhne příliš mnoha připojení, může se zobrazit chybová zpráva:
> Chyba 1040 (08004): Příliš mnoho připojení

## <a name="storage-engine-support"></a>Podpora modulu úložiště

### <a name="supported"></a>Podporováno
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Nepodporovaný
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIV](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDEROVANÉ](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Podpora oprávnění

### <a name="unsupported"></a>Nepodporovaný
- DBA role: mnoha parametry serveru a nastavení můžete nechtěně snížit výkon serveru nebo negate ACID vlastnosti databázového systému. Jako takový zachování integrity služby a SLA na úrovni produktu, tato služba nevystavuje roli správce databáze. Výchozí uživatelský účet, který je vytvořený, když je vytvořena nová instance databáze, umožňuje provádět většinu příkazů DDL a jazyk DML instance spravované databáze. 
- Oprávnění SUPERUŽIVATELE: podobně [SUPER oprávnění](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) je také omezen.

## <a name="data-manipulation-statement-support"></a>Podpora příkaz manipulace dat

### <a name="supported"></a>Podporováno
- DATA VSTUPNÍ_SOUBOR zatížení - podporována, ale musíte zadat parametr [místní], který směřuje na cestu UNC (úložiště Azure připojené prostřednictvím XSMB).

### <a name="unsupported"></a>Nepodporovaný
- VYBERTE... DO VÝSTUPNÍ_SOUBOR

## <a name="preview-functional-limitations"></a>Funkční omezení verze Preview

### <a name="scale-operations"></a>Operace škálování
- Dynamické škálování serverů napříč cenové úrovně se aktuálně nepodporuje. To znamená přepínání mezi Basic, obecné účely a paměťově optimalizované cenové úrovně.
- Zmenšuje velikost úložiště serveru není podporována.

### <a name="server-version-upgrades"></a>Upgrady verze serveru
- Automatické migrace mezi verzemi modul hlavní databáze není aktuálně podporována.

### <a name="point-in-time-restore"></a>Obnovení do bodu v čase
- Obnovení na jinou službu vrstvy nebo výpočetní jednotky a velikost úložiště není povoleno.
- Obnovení odstraněného server není podporováno.

## <a name="functional-limitations"></a>Funkční omezení

### <a name="subscription-management"></a>Správa předplatného
- Dynamicky přesunutí předem vytvořené serverů mezi předplatné a skupina prostředků není aktuálně podporováno.

## <a name="current-known-issues"></a>Aktuální známé problémy
- Instance serveru MySQL zobrazí verze nesprávný serveru po navázání připojení. Správný server verze instance, použijte vyberte version(); příkaz příkazového řádku MySQL.

## <a name="next-steps"></a>Další postup
- [Co je dostupné na jednotlivých úrovních služby](concepts-pricing-tiers.md)
- [Podporované verze databáze MySQL](concepts-supported-versions.md)
