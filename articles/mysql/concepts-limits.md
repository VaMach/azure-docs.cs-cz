---
title: "Omezení v Azure databáze pro databázi MySQL | Microsoft Docs"
description: "Popisuje omezení verze preview ve službě Azure Database pro databázi MySQL."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 12/09/2017
ms.openlocfilehash: e16982e4e57ba9f2f11e9ee59f88f24b3fe3fe3f
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Omezení v Azure databáze pro databázi MySQL
Databáze Azure pro službu MySQL je ve verzi public preview. Následující části popisují kapacity, modul úložiště s podporou, oprávnění podpory, podpora příkaz manipulaci dat a funkční omezení ve službě databázového. Viz také [obecná omezení](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) vztahuje k modulu databáze MySQL.

## <a name="service-tier-maximums"></a>Maximální hodnoty úroveň služby
Azure databáze MySQL, má více úrovní služeb, které lze vybírat při vytváření serveru. Další informace najdete v tématu [co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md).  

Je maximální počet připojení, výpočetní jednotky a úložiště v jednotlivých úrovních služeb verzi Preview, následujícím způsobem: 

|                            |                   |
| :------------------------- | :---------------- |
| **Maximální počet připojení**        |                   |
| Základní 50 výpočetní jednotky     | 50 připojení    |
| Základní 100 výpočetní jednotky    | 100 připojení   |
| Standardní 100 výpočetní jednotky | 200 připojení   |
| Standardní 200 výpočetní jednotky | 400 připojení   |
| Standardní 400 výpočetní jednotky | 800 připojení   |
| Standardní 800 výpočetní jednotky | připojení 1 600  |
| **Maximální počet výpočetní jednotky**      |                   |
| Úroveň služeb Basic         | 100 výpočetní jednotky |
| Úroveň služeb Standard      | 800 výpočetní jednotky |
| **Maximální počet úložiště**            |                   |
| Úroveň služeb Basic         | 1 TB              |
| Úroveň služeb Standard      | 1 TB              |

Když se dosáhne příliš mnoha připojení, může se zobrazit chybová zpráva:
> Chyba 1040 (08004): Příliš mnoho připojení

## <a name="storage-engine-support"></a>Podpora modulu úložiště

### <a name="supported"></a>Podporováno
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [PAMĚŤ](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Nepodporováno
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [SMĚROVAČE BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIV](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDEROVANÉ](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Podpora oprávnění

### <a name="unsupported"></a>Nepodporováno
- DBA role mnoho severu parametry a nastavení nechtěně snížit výkon serveru nebo negate ACID vlastnosti databázového systému. Jako takový zachování naše služby integrity a SLA na úrovni produktu není zveřejňujeme roli DBA zákazníkům. Výchozí uživatelský účet, který je vytvořený, když je vytvořena nová instance databáze, umožňuje zákazníkům provést většinu příkazy DDL a jazyk DML instance spravované databáze. 
- Podobně SUPER oprávnění [SUPER oprávnění](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) je také omezen.

## <a name="data-manipulation-statement-support"></a>Podpora příkaz manipulace dat

### <a name="supported"></a>Podporováno
- DATA VSTUPNÍ_SOUBOR zatížení - podporována, ale musíte zadat parametr [místní], který směřuje na cestu UNC (úložiště Azure připojené prostřednictvím XSMB).

### <a name="unsupported"></a>Nepodporováno
- VYBERTE... DO VÝSTUPNÍ_SOUBOR

## <a name="preview-functional-limitations"></a>Funkční omezení verze Preview

### <a name="scale-operations"></a>Operace škálování
- Dynamické škálování serverů mezi úrovně služeb není aktuálně podporováno. To znamená přepínání mezi úrovně služeb Basic a Standard.
- Dynamické zvětšování na vyžádání úložiště na předem vytvořené serveru není aktuálně podporováno.
- Zmenšuje velikost úložiště serveru není podporována.

### <a name="server-version-upgrades"></a>Upgrady verze serveru
- Automatické migrace mezi verzemi modul hlavní databáze není aktuálně podporována.

### <a name="point-in-time-restore"></a>Obnovení do bodu v čase
- Obnovení na jinou službu vrstvy nebo výpočetní jednotky a velikost úložiště není povoleno.
- Obnovení odstraněného server není podporováno.

## <a name="functional-limitations"></a>Funkční omezení

### <a name="subscription-management"></a>Správa předplatného
- Dynamicky přesunutí předem vytvořené serverů mezi předplatné a skupina prostředků není aktuálně podporováno.

## <a name="current-known-issues"></a>Aktuální známé problémy:
- Instance serveru MySQL zobrazí verze nesprávný serveru po navázání připojení. Správný server verze instance, použijte vyberte version(); příkaz příkazového řádku MySQL.

## <a name="next-steps"></a>Další postup
- [Co je dostupné na jednotlivých úrovních služby](concepts-service-tiers.md)
- [Podporované verze databáze MySQL](concepts-supported-versions.md)
