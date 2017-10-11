---
title: "Úlohy, které jsou povoleny v různých stavů nebo stavy, které jsou ve službě BizTalk Services | Microsoft Docs"
description: "Akce nebo operace v jiný stav MABS povoleny: zastavit, spustit, restartovat, pozastavit, obnovit, odstranit, škálovat, aktualizace konfigurace a základní nahoru"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: aea738f3-ec76-4099-a41b-e17fea9e252f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2016
ms.author: mandia
ms.openlocfilehash: 918154852cf4ee1b80792eaddb588f5b0237da7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="what-you-can-and-cant-do-using-the-biztalk-service-state"></a>Můžete a nemůže provádět pomocí stavu služby BizTalk

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

V závislosti na aktuální stav služby BizTalk jsou operace, které lze nebo nelze provést na službu BizTalk.

Například můžete zřídit nové služby BizTalk v portálu Azure classic. Po úspěšném dokončení, služba BizTalk je v `active` stavu. V aktivním stavu můžete zastavit, pozastavit a odstranit službu BizTalk. Pokud zastavíte službu BizTalk a zastavení selže, pak službu BizTalk přejde na `StopFailed` stavu. V `StopFailed` stavu, můžete restartovat službu BizTalk. Pokud se pokusíte operaci, která není povolena, jako je obnovení, došlo k následující chybě:

`Operation not allowed`

## <a name="view-the-possible-states"></a>Zobrazit možné stavy

V následujících tabulkách jsou uvedeny operace nebo akce, které lze provést, pokud služba BizTalk je v určitém stavu. ✔ znamená, že operace je povolena v tomto stavu. Prázdná položka znamená, že v tomto stavu nelze provést operaci.

| Stav služby | Start | Zastavit | Restartování | Pozastavit | Obnovit | Odstranění | Měřítko | Aktualizace <br/> Konfigurace | Zálohování |
| --- | --- | --- | --- | --- | --- | --- |--- | --- | --- |
| Aktivní |  | ✔ | ✔ | ✔ |  | ✔ |✔ |✔ |✔ |
| Zakázáno |  |  |  |  |  | ✔ | |  |  | 
| pozastaveno |  |  |  |  | ✔ | ✔ | |  | ✔ |
| Zastaveno | ✔ |  | ✔ |  |  | ✔ | |  | ✔ |
| Aktualizace služby se nezdařila. |  |  |  |  |  | ✔ | |  |  | 
| DisableFailed |  |  |  |  |  | ✔ | |  |  | 
| EnableFailed |  |  |  |  |  | ✔ | |  |  | 
| StartFailed <br/> StopFailed <br/> RestartFailed | ✔ | ✔ | ✔ |  |  | ✔ | | ✔ | |
| SuspendedFailed <br/> ResumeFailed|  |  |  | ✔ | ✔ | ✔ | |  |  | 
| CreatedFailed <br/> RestoreFailed |  |  |  |  |  | ✔ | |  |  | 
| ConfigUpdateFailed  |  |  | ✔ |  |  | ✔ | |✔ | |
| ScaleFailed |  |  |  |  |  | ✔ |✔ | |  |  | 



## <a name="see-also"></a>Viz také
* [Vytvoření služby BizTalk pomocí portálu Azure classic](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [Co můžete dělat na řídicí panel, sledování a škálování kartách ve službě BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [Získat s edice Developer, Basic, Standard a Premium ve službě BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Postup zálohování a obnovení služby BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [Omezení podrobně BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
* [Načtení služby Service Bus a řízení přístupu vystavitele název vystavitele klíčových hodnot a pro vaši službu BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
* [Jak začít používat sadu SDK Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=302335)

