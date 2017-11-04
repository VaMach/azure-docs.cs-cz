---
title: "Stream Analytics: Otáčení protokolu pověření pro vstupy a výstupy | Microsoft Docs"
description: "Zjistěte, jak aktualizovat přihlašovací údaje pro Stream Analytics vstupy a výstupy."
keywords: "přihlašovací údaje"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: a1a927fa9c34b38e54fdb22782e80fd13bf430c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Otočit přihlašovací údaje pro vstupy a výstupy úlohy Stream Analytics
## <a name="abstract"></a>Abstraktní
Azure Stream Analytics dnes neumožňuje nahrazení přihlašovacích údajů na vstupu a výstupu při běhu úlohy.

Zatímco Azure Stream Analytics podporuje obnovení úlohy z posledního výstupu, jsme chtěli sdílet celý proces pro minimalizaci prodleva mezi zastavení a spuštění úlohy a otáčení přihlašovací údaje.

## <a name="part-1---prepare-the-new-set-of-credentials"></a>Část 1 – Příprava novou sadu přihlašovacích údajů:
Tato část se vztahuje na následující vstupy/výstupy:

* Blob Storage
* Event Hubs
* SQL Database
* Table Storage

Pro ostatní vstupy/výstupy pokračujte část 2.

### <a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště/tabulky
1. Přejdete na rozšíření úložiště na portálu pro správu Azure:  
   ![graphic1][graphic1]
2. Vyhledejte úložiště používá vaše úloha a přejděte do ní:  
   ![graphic2][graphic2]
3. Klikněte na příkaz Spravovat přístupové klíče:  
   ![graphic3][graphic3]
4. Mezi primární přístupový klíč a sekundární přístupový klíč **vyberte ten není používá vaše úloha**.
5. Stiskněte tlačítko znovu vygenerovat:  
   ![graphic4][graphic4]
6. Zkopírujte nově vygenerovaný klíč:  
   ![graphic5][graphic5]
7. Pokračujte částí 2.

### <a name="event-hubs"></a>Služba Event hubs
1. Přejdete na Service Bus rozšíření na portálu pro správu Azure:  
   ![graphic6][graphic6]
2. Vyhledejte Namespace Service Bus, používá vaše úloha a přejděte do ní:  
   ![graphic7][graphic7]
3. Pokud vaše úlohy používá zásady sdíleného přístupu na Namespace sběrnice služby, přejít na krok 6  
4. Přejděte na kartu centra událostí:  
   ![graphic8][graphic8]
5. Vyhledejte centra událostí, které používá vaše úloha a přejděte do ní:  
   ![graphic9][graphic9]
6. Přejděte na kartu konfigurace:  
   ![graphic10][graphic10]
7. Na název zásady rozevíracího seznamu vyhledejte zásady sdíleného přístupu používá vaše úloha:  
   ![graphic11][graphic11]
8. Mezi primární klíč a sekundární klíč **vyberte ten není používá vaše úloha**.  
9. Stiskněte tlačítko znovu vygenerovat:  
   ![graphic12][graphic12]
10. Zkopírujte nově vygenerovaný klíč:  
   ![graphic13][graphic13]
11. Pokračujte částí 2.  

### <a name="sql-database"></a>SQL Database
> [!NOTE]
> Poznámka: budete muset připojit ke službě SQL Database. Přidáme ukazují, jak to provést pomocí prostředí pro správu na portálu pro správu Azure, ale můžete použít některé klientské nástroje, jako je SQL Server Management Studio také.
>
> 

1. Přejděte do databáze SQL rozšíření na portálu pro správu Azure:  
   ![graphic14][graphic14]
2. Vyhledejte databázi SQL používanou nástrojem úlohu a **klikněte na server** odkaz na stejném řádku:  
   ![graphic15][graphic15]
3. Klikněte na příkaz Spravovat:  
   ![graphic16][graphic16]
4. Hlavní databáze typu:  
   ![graphic17][graphic17]
5. Zadejte uživatelské jméno, heslo a klikněte na protokolu:  
   ![graphic18][graphic18]
6. Klikněte na nový dotaz:  
   ![graphic19][graphic19]
7. Typ v následujícím dotazu nahraďte < login_name > vaše uživatelské jméno a nahraďte <enterStrongPasswordHere> pomocí nového hesla:  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. Klikněte na tlačítko spustit:  
   ![graphic20][graphic20]
9. Vraťte se ke kroku 2 a tuto dobu, klepněte na databázi:  
   ![graphic21][graphic21]
10. Klikněte na příkaz Spravovat:  
   ![graphic22][graphic22]
11. Zadejte uživatelské jméno, heslo a klikněte na možnost přihlášení:  
   ![graphic23][graphic23]
12. Klikněte na nový dotaz:  
   ![graphic24][graphic24]
13. Zadejte následující dotaz nahrazování < uživatelské_jméno > s názvem, podle kterého chcete identifikovat toto přihlášení v kontextu této databáze (můžete zadat stejnou hodnotu, který jste zadali pro < login_name >, třeba) a nahraďte < login_name > nové uživatelské jméno:  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klikněte na tlačítko spustit:  
   ![graphic25][graphic25]
15. Nový uživatel by měl poskytovat teď se stejnou rolí a oprávnění měl původního uživatele.
16. Pokračujte částí 2.

## <a name="part-2-stopping-the-stream-analytics-job"></a>Část 2: Ukončení úlohy Stream Analytics
1. Přejdete na rozšíření Stream Analytics na portálu pro správu Azure:  
   ![graphic26][graphic26]
2. Najděte úlohu a přejděte do ní:  
   ![graphic27][graphic27]
3. Přejděte na kartu vstupů nebo výstupů založené na tom, jestli jsou otáčení přihlašovací údaje na vstup nebo výstup.  
   ![graphic28][graphic28]
4. Klikněte na příkaz k ukončení a potvrďte, že úloha byla zastavena:  
   ![graphic29][graphic29] počkejte na zastavení úlohy.
5. Vyhledejte vstupu a výstupu, který chcete otočit na přihlašovací údaje a přejděte do ní:  
   ![graphic30][graphic30]
6. Přejděte k části 3.

## <a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Část 3: Úprava přihlašovací údaje u úlohy Stream Analytics
### <a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště/tabulky
1. Najít pole klíč účtu úložiště a vložte do něj nově vygenerovaný klíč:  
   ![graphic31][graphic31]
2. Klikněte na příkaz Uložit a potvrďte uložení změn:  
   ![graphic32][graphic32]
3. Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že je úspěšně byla úspěšná.
4. Přejděte k části 4.

### <a name="event-hubs"></a>Služba Event hubs
1. Najít pole klíče události rozbočovače zásady a vložit vaše nově vygenerovaný klíč do ní:  
   ![graphic33][graphic33]
2. Klikněte na příkaz Uložit a potvrďte uložení změn:  
   ![graphic34][graphic34]
3. Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že má úspěšně úspěšně.
4. Přejděte k části 4.

### <a name="power-bi"></a>Power BI
1. Klikněte na tlačítko Obnovit autorizace:  

   ![graphic35][graphic35]
2. Zobrazí se po potvrzení:  

   ![graphic36][graphic36]
3. Klikněte na příkaz Uložit a potvrďte uložení změn:  
   ![graphic37][graphic37]
4. Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že úspěšně úspěšně prošel.
5. Přejděte k části 4.

### <a name="sql-database"></a>SQL Database
1. Najít pole uživatelské jméno a heslo a vkládání do nich vaše nově vytvořenou sadu přihlašovacích údajů:  
   ![graphic38][graphic38]
2. Klikněte na příkaz Uložit a potvrďte uložení změn:  
   ![graphic39][graphic39]
3. Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že má úspěšně úspěšně.  
4. Přejděte k části 4.

## <a name="part-4-starting-your-job-from-last-stopped-time"></a>Část 4: Od poslední zastaven úlohu
1. Opustit vstupu a výstupu:  
   ![graphic40][graphic40]
2. Klikněte na příkaz ke spuštění:  
   ![graphic41][graphic41]
3. Vyberte naposledy zastaveno a klikněte na tlačítko OK:  
   ![graphic42][graphic42]
4. Přejděte k části 5.  

## <a name="part-5-removing-the-old-set-of-credentials"></a>Část 5: Odebrání původní sadu přihlašovacích údajů
Tato část se vztahuje na následující vstupy/výstupy:

* Blob Storage
* Event Hubs
* SQL Database
* Table Storage

### <a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště/tabulky
Část 1 opakujte pro přístupový klíč, který byl dříve používán vaše úloha obnovení nyní nepoužívané přístupový klíč.

### <a name="event-hubs"></a>Služba Event hubs
Část 1 opakujte pro klíč, který byl dříve používán vaše úloha obnovíte klíč, teď nepoužívá.

### <a name="sql-database"></a>SQL Database
1. Přejděte zpět do okna dotazu z část 1 kroku 7 a zadejte následující dotaz, nahraďte < previous_login_name > uživatelské jméno, který byl dříve používán vaše úlohy:  
   `DROP LOGIN <previous_login_name>`  
2. Klikněte na tlačítko spustit:  
   ![graphic43][graphic43]  

Měli byste obdržet následující potvrzení: 

    Command(s) completed successfully.

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod do služby Azure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

