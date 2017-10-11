---
title: "Cvičení obnovení po havárii databáze SQL | Microsoft Docs"
description: "Další pokyny a osvědčené postupy pro používání Azure SQL Database k provedení projde obnovení po havárii zajistit, aby byl váš zvláště důležitých podnikových aplikací odolné selhání a výpadky."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: 1b1d65a41a794a566287dcffe3581ac58e2a965f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="performing-disaster-recovery-drill"></a>Provedením postupu zotavení po havárii
Doporučujeme, aby se pravidelně provádí ověřování připravenosti aplikace pro pracovní postup obnovení. Ověření chování aplikace a dopad ztráty dat nebo přerušení zahrnuje tento převzetí služeb při selhání je dobrým zvykem engineering. Je také požadavek ve většině oborové standardy jako součást Certifikační kontinuity firmy.

Provádění postupu zotavení po havárii zahrnuje:

* Simulace výpadku datové vrstvy
* Obnovení
* Ověření obnovení post integritu aplikací

V závislosti na tom, jak můžete [určená aplikace pro kontinuitu podnikových procesů](sql-database-business-continuity.md), pracovní postup spuštění podrobné se může lišit. Níže budeme popisují doporučené postupy provádění postupu zotavení po havárii v kontextu databáze SQL Azure.

## <a name="geo-restore"></a>Geografické obnovení
Aby se zabránilo potenciální ztrátě dat při provádění postupu zotavení po havárii, doporučujeme provádění procházení testovacím prostředí pomocí vytvořit kopii produkčního prostředí a používat jej ověření pracovní postup převzetí služeb při selhání aplikace.

#### <a name="outage-simulation"></a>Simulace výpadku
Pro simulaci se výpadek, můžete odstranit nebo přejmenovat zdrojové databáze. To způsobí selhání připojení aplikace.

#### <a name="recovery"></a>Obnovení
* Provést geografické obnovení databáze do jiného serveru, jak se popisuje [zde](sql-database-disaster-recovery.md).
* Změna konfigurace aplikace pro připojení k obnovené databáze a postupujte podle [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) průvodce dokončí obnovení.

#### <a name="validation"></a>Ověření
* Dokončete podrobné sestavy pomocí ověření obnovení post integrity aplikace (včetně připojovací řetězce, přihlášení, základní funkce testování nebo jinou ověření část postupy signoffs standardní aplikace).

## <a name="geo-replication"></a>Geografická replikace
Pro databáze, který je chráněný pomocí geografická replikace zahrnuje cvičení procházení plánované převzetí služeb při selhání pro sekundární databázi. Plánované převzetí služeb při selhání zaručuje, že primární a sekundární databáze zachovány synchronizované při přepínání role. Na rozdíl od neplánované převzetí služeb při selhání tato operace není způsobit ztrátu dat, takže podrobné sestavy lze provést v provozním prostředí.

#### <a name="outage-simulation"></a>Simulace výpadku
Pro simulaci se výpadek, můžete zakázat webové aplikace nebo virtuální počítač připojen k databázi. Pro klienty webového výsledkem chyby připojení.

#### <a name="recovery"></a>Obnovení
* Zajistěte, aby konfigurace aplikace v oblasti zotavení po Havárii odkazuje na sekundární dřívějším, který se stane plně dostupný nový primární.
* Provedení [plánované převzetí služeb při selhání](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) aby sekundární databázi nový primární
* Postupujte podle [nakonfigurovat databázi po obnovení](sql-database-disaster-recovery.md) průvodce dokončí obnovení.

#### <a name="validation"></a>Ověření
* Dokončete podrobné sestavy pomocí ověření obnovení post integrity aplikace (včetně připojovací řetězce, přihlášení, základní funkce testování nebo jinou ověření část postupy signoffs standardní aplikace).

## <a name="next-steps"></a>Další kroky
* Další informace o obchodních scénářů kontinuity najdete v tématu [kontinuity scénáře](sql-database-business-continuity.md)
* Další informace o Azure SQL Database automatizované zálohování najdete v tématu [automatizované zálohování SQL Database](sql-database-automated-backups.md)
* Další informace o použití automatizované zálohování pro obnovení, najdete v části [obnovit databázi ze zálohy spouštěná služba](sql-database-recovery-using-backups.md)
* Další informace o možnosti rychlejší obnovení najdete v tématu [aktivní geografickou replikaci](sql-database-geo-replication-overview.md)  
