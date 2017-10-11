---
title: "Azure vysokou dostupnost služby Analysis Services | Microsoft Docs"
description: "Zajišťuje vysokou dostupnost služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 84b4c59bac1feeb8611b3a8d783d093ba073e532
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="analysis-services-high-availability"></a>Vysoká dostupnost Analysis Services
Tento článek popisuje, zajištění vysoké dostupnosti pro servery Azure Analysis Services. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Zajištění vysoké dostupnosti během přerušení služby
Když je taková situace vzácná, může mít datové centrum Azure k výpadku. Pokud dojde k výpadku, budou přerušení obchodní, který může trvat několik minut nebo může trvat hodiny. Vysoká dostupnost nejčastěji dosáhnout s nadbytečností serveru. S Azure Analysis Services můžete dosáhnout vytvořením další, sekundární servery v oblastech jeden nebo více redundance. Při vytváření redundantních serverů, aby zajistil, data a metadata na těchto serverech je v synchronizaci se serverem v oblasti, který přešel do stavu offline, můžete:

* Modely nasazení na redundantní servery v jiných oblastech. Tato metoda vyžaduje zpracování dat na primární server i redundantní servery v paralelní, zajištění všechny servery jsou v synchronizaci.

* Zálohování databází z primárního serveru a obnovení na redundantní serverech. Například můžete automatizovat noční zálohování do úložiště Azure a obnovení na jiné servery redundantní v jiných oblastech. 

V obou případech Pokud primární server dojde k výpadku, musíte změnit připojovací řetězce v generování sestav klientům připojit se k serveru v jiném datovém centru místní. Tato změna považovat za poslední možnost a dochází pouze v případě výpadku center závažné místní data. Je pravděpodobnější, výpadku datacentra hostování primární server by dostane zpět online, než může aktualizovat připojení na všechny klienty. 



## <a name="related-information"></a>Související informace
[Zálohování a obnovení](analysis-services-backup.md)   
[Správa služby Azure Analysis Services](analysis-services-manage.md) 

