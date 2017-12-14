---
title: "Vysoká dostupnost – služba Azure SQL Database | Microsoft Docs"
description: "Další informace o možnosti vysokou dostupnost služby Azure SQL Database a funkcí"
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
ms.assetid: 
ms.service: sql-database
ms.custom: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 12/13/2017
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: c0a140c959f14c2e8ceaddad5d323f0900be5d2f
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="high-availability-and-azure-sql-database"></a>Vysoká dostupnost a Azure SQL Database
Od zahájení nabídky PaaS databáze SQL Azure společnost Microsoft vyvinula potenciálu naše zákazníky, kteří vysoké dostupnosti (HA) je součástí služby a zákazníků se nebudou muset pracovat, přidejte speciální logiku pro nebo rozhodnutí ohledně HA. Společnost Microsoft nabízí zákazníkům SLA a udržovat plnou kontrolu nad konfigurace systému HA a operace. HA SLA platí pro databáze SQL v oblasti a neposkytuje ochranu v případech selhání celkový oblasti, které jsou faktory, které mimo naše přiměřené ovládací prvek (například přírodní katastrofě war, funguje teroristický útok, povstáním, government akce nebo k síti nebo selhání zařízení mimo našich datacentrech, včetně na zákazníka nebo mezi web zákazníka a naše datového centra).

Pro zjednodušení problém prostor HA, společnost Microsoft používá následující předpoklady:
1.  Selhání hardwaru a softwaru jsou nevyhnutelné
2.  Provozní pracovníci uděláte chyby, které vést k chybám
3.  Plánované údržby operace způsobit výpadky 

Když tyto jednotlivé události jsou jen zřídka, v cloudovém měřítku, budou každý týden, pokud není každý den. 

## <a name="fault-tolerant-sql-databases"></a>Odolné proti chybám databáze SQL
Zákazníci se nejvíc zajímat odolnost vlastní databáze a jsou méně zájem o odolnost službě SQL Database jako celek. 99,99 % dostupnost služby je smysl, pokud "databáze" je součástí 0,01 % databází, které jsou vypnuté. Každé databáze musí být odolné proti chybám a zmírnění chyby by nikdy dojít ke ztrátě potvrzené transakce. 

Databáze SQL pro data, používá místní úložiště (LS) založené na přímé připojené disky nebo virtuální pevné disky a vzdálené úložiště (r) založené na objekty BLOB stránky Azure Premium Storage. 
- Místní úložiště se používá v Premium databáze a fondy, které jsou určeny pro OLTP aplikace s vysokou požadavky IOPS. 
- Vzdálené úložiště se používá pro úrovně služeb Basic a Standard, určený pro malé, cold nebo velké databáze, které vyžadují úložiště a výpočetního výkonu nezávisle škálovat. Objekt blob jednostránkové používají pro databázi a soubory protokolu a mechanismy pro replikaci a převzetí služeb při selhání integrovanou úložiště.

V obou případech replikace, detekce chyb a převzetí služeb při selhání mechanismy databáze SQL jsou plně automatizované a pracovat i bez lidského zásahu. Tato architektura umožňuje potvrdit data musí být nikdy ztraceny a že odolnost dat má přednost před všechno ostatní.

Klíčové výhody:
- Zákazníci plně využívat replikované databází bez nutnosti konfigurovat nebo spravovat složitá hardware, software, operačního systému nebo prostředích virtualizace.
- Úplné ACID vlastnosti relačních databází je udržováno v systému.
- Převzetí služeb při selhání jsou plně automatizované bez ztráty dat. všechny potvrzené.
- Směrování připojení k primární replice dynamicky spravuje službu s žádné logiku aplikace vyžaduje.
- Vysoká úroveň automatické redundance je k dispozici bez dalších poplatků.

> [!NOTE]
> Architektura popsané vysoké dostupnosti se mohou změnit bez předchozího upozornění. 

## <a name="data-redundancy"></a>Data redundancy

Řešení vysoké dostupnosti v SQL Database je založeno na [AlwaysON](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) technologie v systému SQL Server a udělá z něj fungovat pro LS a RS databáze s minimálním rozdíly. V konfiguraci LS je AlwaysON používané pro trvalost v RS se používá k zajištění dostupnosti (nízkou RTO). 

## <a name="local-storage"></a>Místní úložiště

V případě LS každou databázi do režimu online pomocí služby správy (MS) v rámci prstenec ovládacího prvku. Jedna primární replika a alespoň dvě sekundární repliky (kvora set) jsou umístěné v rámci prstenec klienta, která zahrnuje tři nezávislých fyzických subsystémů v rámci stejného datového centra. Všechny čtení a zápisy se odesílají bránou (GW) na primární repliku a zápisů asynchronně replikovány na sekundárních replikách. Databáze SQL používá schéma na základě kvora potvrzení, kde data se zapisují do primárním serverem a nejméně jedna sekundární replika před potvrzení transakce.

[Service Fabric](/azure/service-fabric/service-fabric-overview.md) převzetí služeb při selhání systému automaticky znovu vytvoří repliky jako uzly selžou a udržuje sadu kvora členství jako uzly odchylují a připojení k systému. Plánované údržby je pečlivě koordinována, aby se zabránilo kvora set směrem dolů nižší než počet minimální repliky (obvykle 2). Tento model funguje dobře u prémiových databází, ale vyžaduje redundance součástí výpočetních operací a úložiště a má za následek vyšší náklady.

## <a name="remote-storage"></a>Vzdálené úložiště

Pro vzdálené úložiště konfigurace (úrovně Basic a Standard) se přesně jedna kopie databáze udržuje v vzdálené úložiště objektů blob, využití funkcí systémy úložiště odolnosti, redundanci a bit kroužkovitosti detekce. 

Architektura vysoké dostupnosti je znázorněno v následujícím diagramu:
 
![Architektura vysoké dostupnosti](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection--recovery"></a>Detekce chyb & obnovení 
Ve velkém měřítku distribuovaného systému musí systém detekce vysoce spolehlivé selhání, který může zjistit selhání spolehlivě, rychle a co nejblíže k odběrateli. Pro databázi SQL je tento systém založen na Azure Service Fabric. 

S primární replikou je okamžitě zřejmé Pokud primární replice se nezdařila a pracovní nemůže pokračovat, protože všechny čtení a zápisu dochází v primární replice nejprve. Tento proces povýšení sekundární repliky se stavem primární má cíli času obnovení (RTO) = 30 sekund a plánovaného bodu obnovení (RPO) = 0. Aby se minimalizoval vliv 30 sekund RTO, osvědčeným postupem je pokuste se znovu připojit s menší doba čekání nezdařených pokusů o připojení.

Pokud se nezdaří sekundární repliku, databáze je dolů minimální kvora sadu, s žádné k výměně za chodu. Služba fabric zahájí se proces opětovné konfigurace podobný procesu, který následuje selhání primární repliky, takže po krátké čekání na-li určit, zda je trvalé, selhání jiné sekundární repliky se vytvoří. V případech dočasné stavu out-of-service, například k selhání operačního systému nebo upgrade není novou repliku vytvořená s umožňující uzlu se nezdařilo restartovat místo toho okamžitě. 

Databáze SQL pro konfiguraci vzdáleného úložiště používá funkci AlwaysON k databázím převzetí služeb při selhání během upgrady. K tomu, novou instanci SQL se spouštějí předem jako součást upgradu události plánované a připojí a obnoví se soubor databáze ze vzdáleného úložiště. V případě mimoprocesových nebo jiných neplánovaných událostí technologie Windows Fabric spravuje dostupnost instance a jako poslední krok obnovení připojí vzdálený databázový soubor.

## <a name="conclusion"></a>Závěr
Azure SQL DB je úzce integrovaná s platformou Azure a vysoce závisí na Service Fabric pro zjištění selhání a obnovení a objektů BLOB služby Azure Storage pro ochranu dat. Ve stejnou dobu Azure SQL database používá technologii AlwaysOn z pole produktu SQL Server pro replikaci a převzetí služeb při selhání. Kombinace těchto technologií umožňuje aplikacím plně výhody modelu smíšený úložiště a podporu nejnáročnější SLA. 

## <a name="next-steps"></a>Další kroky

- Další informace o [Service Fabric](/azure/service-fabric/service-fabric-overview.md)
- Další informace o [Azure Traffic Manager](/traffic-manager/traffic-manager-overview.md) 
