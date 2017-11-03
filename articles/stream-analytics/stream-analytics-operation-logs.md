---
title: "Ladění pomocí operace a protokoly služby ve Stream Analytics | Microsoft Docs"
description: "Postupy: použití protokoly operací Stream Analytics"
keywords: "protokoly služby"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 2ee0871752dc2a3da345339fb826340d44ae48d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Ladění pomocí služby a operace protokoly úlohy Stream Analytics
Všech služeb Azure, zadejte provozní protokolování zpráv uživatelům, aby podrobnosti záznamu týkající se operací správy. V Azure Stream Analytics tyto informace slouží k ladění účely, například zobrazení stavu úlohy, průběh úlohy a zprávy o selhání sledovat průběh úlohy v čase, z začít zpracování na výstup.

## <a name="find-operation-logs-in-the-azure-portal"></a>Najít operaci protokoly na portálu Azure
Protokoly operací je přístupná dvěma způsoby:  

* Řídicí panel úlohy Stream Analytics  
* Správa služeb v portálu Azure classic  

## <a name="dashboard-of-the-stream-analytics-job"></a>Řídicí panel úlohy Stream Analytics
Odkaz na odpovídající protokoly úlohy Stream Analytics se zobrazí na kartě úlohy řídicího panelu. Pokud kliknete na tento odkaz, nastaví filtry tak, že se zobrazí nejnovější protokoly pro tuto konkrétní úlohu.

  ![Vyberte protokoly služby správy](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Služby správy
Ručně přejděte na protokoly operací pro Stream Analytics a dalších služeb v portálu Azure classic:

1. Klikněte na **Management Services** v [portál Azure classic](https://manage.windowsazure.com).
2. Vyberte **Stream Analytics** pro **typ** a název úlohy pro **název služby**.  
   
   ![Vyberte služby Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Vyhledání protokolů auditu na portálu Azure
Chcete-li najít operační protokoly pro úlohu služby Stream Analytics na portálu Azure, klikněte na tlačítko **Procházet** a pak vyberte **protokoly auditu**.

  ![Portálu Azure vyberte Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Otevře zobrazení, zobrazení událostí z posledních sedmi dnů pro všechny prostředky ve vašem předplatném.  Můžete filtrovat, zobrazí se události zadejte typ nebo časového rámce kliknutím **filtru** příkaz.

  ![Portálu Azure vyberte Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Získat podrobnosti protokolu
Můžete filtrovat podle časové rozmezí a stav chcete zobrazit protokoly pro úlohu.

Na portálu Azure klikněte na **podrobnosti** tlačítko v dolní části okna zobrazíte další podrobnosti o zvolenou událost. 

  ![Vyberte podrobnosti](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Na portálu Azure klikněte na položky protokolu chcete zobrazit podrobné události v ho.

  ![Portálu Azure vyberte podrobnosti](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Odtud můžete otevřít **podrobností** zobrazit kliknutím na události.

  ![Portálu Azure vyberte podrobnosti](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Ladění neúspěšnou úlohu
Na portálu Azure klikněte na ikonu hledání a typ "se nezdařilo". To dává výsledku všechny protokoly s chybami. 

  ![Ladění neúspěšnou úlohu](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Na portálu Azure můžete filtrovat podle úrovně zprávu zobrazíte **kritický** události.

  ![Portál Azure ladění](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Můžete vybrat některý z chyby a klikněte na **podrobnosti** Další informace o chybě.  Některé chybové zprávy také poskytují informace o tom, jak zmírnit problém. 

  ![Podrobnosti o operaci](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

V případě, že budete muset kontaktovat [podporu](https://azure.microsoft.com/support/options/) nebo zadejte informace o týmu prostřednictvím [fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), je potřeba počítat s provozní údaje, konkrétně **ID korelace**. 

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

