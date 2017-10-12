---
title: "Zásady přístupu k datům v Azure Time Series Insights | Dokumentace Microsoftu"
description: "V tomto kurzu se naučíte spravovat zásady přístupu k datům v Time Series Insights."
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: omravi
ms.openlocfilehash: 5258bf5de6f7aa1ea246f1235e7d362b1b7d0181
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Udělení přístupu k datům prostředí Time Series Insights pomocí webu Azure Portal

V prostředích Time Series Insights jsou dva nezávislé typy zásad přístupu:

* Zásady přístupu ke správě
* Zásady přístupu k datům

Oba typy zásad udělují objektům zabezpečení Azure Active Directory (uživatelům a aplikacím) různá oprávnění ke konkrétnímu prostředí. Objekty zabezpečení (uživatelé a aplikace) musí patřit ke stejné službě Active Directory (neboli tenant Azure) přidružené k předplatnému, které obsahuje prostředí.

Zásady přístupu ke správě udělují oprávnění související s konfigurací prostředí, jako je například
*   vytvoření nebo odstranění prostředí, zdrojů událostí nebo referenčních datových sad a
*   správa zásad přístupu k datům.

Zásady přístupu k datům udělují oprávnění k vydávání dotazů na data, zpracování referenčních dat v rámci prostředí a sdílení uložených dotazů a perspektiv přidruženým k danému prostředí.

Tyto dva typy zásad umožňují jasné oddělení přístupu ke správě prostředí od přístupu k datům v prostředí. Například je možné nastavit prostředí tak, aby vlastník nebo tvůrce prostředí neměl přístup k datům. Stejně tak je možné neudělit přístup ke konfiguraci prostředí uživatelům a službám s oprávněním číst data z prostředí.

## <a name="grant-data-access"></a>Udělení přístupu k datům
Následující postup ukazuje, jak udělit přístup k datům objektu zabezpečení uživatele:

1.  Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2.  V nabídce na levé straně webu Azure Portal klikněte na Všechny prostředky.
3.  Vyberte vaše prostředí Time Series Insights.

  ![Správa zdroje Time Series Insights – prostředí](media/data-access/getstarted-grant-data-access1.png)

4.  Vyberte Přístup k rovině dat a klikněte na Přidat.

  ![Správa zdroje Time Series Insights – přidat](media/data-access/getstarted-grant-data-access2.png)

5.  Klikněte na Vybrat uživatele.
6.  Podle e-mailu vyhledejte a vyberte uživatele.
7.  Klikněte na Vybrat v okně Vybrat uživatele.

  ![Správa zdroje Time Series Insights – vybrat uživatele](media/data-access/getstarted-grant-data-access3.png)

8.  Klikněte na Vybrat roli.
9.  Vyberte možnost Přispěvatel, pokud chcete uživateli umožnit provádění změn referenčních dat a sdílení uložených dotazů a perspektiv s ostatními uživateli prostředí. Jinak vyberte možnost Čtenář, čímž uživateli umožníte dotazování na data v prostředí a ukládání osobních (ne sdílených) dotazů v prostředí.
10. Klikněte na Ok v okně Vybrat roli.

  ![Správa zdroje Time Series Insights – vybrat roli](media/data-access/getstarted-grant-data-access4.png)

11. Klikněte na Ok v okně Vybrat roli uživatele.
12. Měli byste vidět tohle:

  ![Správa zdroje Time Series Insights – výsledky](media/data-access/getstarted-grant-data-access5.png)

## <a name="next-steps"></a>Další kroky

* [Vytvoření zdroje událostí](time-series-insights-add-event-source.md)
* [Odesílání událostí](time-series-insights-send-events.md) do zdroje událostí
* Zobrazení prostředí na [portálu Time Series Insights](https://insights.timeseries.azure.com)
