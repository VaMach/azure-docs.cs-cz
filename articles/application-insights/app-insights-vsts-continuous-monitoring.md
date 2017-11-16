---
title: "Nepřetržité monitorování vaší verze kanálu DevOps s služby VSTS a Azure Application Insights | Microsoft Docs"
description: "Poskytuje pokyny k rychlému nastavení nepřetržitého monitorování pomocí Application Insights"
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 11/13/2017
ms.service: application-insights
ms.topic: article
manager: carmonm
ms.openlocfilehash: 6ced5941d06c02a2576bf7f561bd535810e7ffd9
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Přidat nepřetržité monitorování do kanálu verze

Visual Studio Team Services (VSTS) se integruje s Azure Application Insights umožňuje nepřetržité monitorování vaší verze kanálu DevOps v průběhu životního cyklu softwaru. 

Služby VSTS teď podporuje nepřetržité monitorování, jímž kanálů verze můžete začlenit data monitorování z Application Insights a dalším prostředkům služby Azure. Když se detekuje výstrahu Application Insights, nasazení může zůstat ověřovaného vrácení nebo vrácena zpět, dokud výstraha vyřeší. Pokud všechny kontroly skončí úspěchem, nasazení můžete pokračovat automaticky z testu úplně do produkčního prostředí bez nutnosti ruční zásah. 

## <a name="configure-continuous-monitoring"></a>Nakonfigurovat nepřetržité monitorování

1. Vyberte existující projekt služby VSTS.

2. Pozastavte ukazatel myši nad **sestavení a verze** > vyberte **verze** > klikněte na tlačítko **znaménko plus** > **vytvořit verze definice** > Vyhledejte **monitorování** > **nasazení služby Azure App Service pomocí nepřetržité monitorování.**

   ![Nová verze definice služby VSTS](.\media\app-insights-continuous-monitoring\001.png)

3. Klikněte na tlačítko **použít.**

4. Vedle červeného vykřičníku vyberte text modře k **zobrazit úlohy prostředí.**

   ![Úlohy v zobrazení prostředí](.\media\app-insights-continuous-monitoring\002.png)

   Zobrazí se pole konfigurace, použijte v následující tabulce k vyplnění vstupních polí.

    | Parametr        | Hodnota |
   | ------------- |:-----|
   | **Název prostředí**      | Název, který popisuje prostředí definice verze |
   | **Předplatné Azure** | Rozevírací seznam se naplní s kterýkoli odběr služby Azure propojí s účtem služby VSTS|
   | **Název služby App Service** | Ruční zadání nová hodnota může být nutný pro toto pole v závislosti na jiných výběr |
   | **Skupina prostředků**    | Rozevírací seznam se naplní s dostupných skupin prostředků |
   | **Název prostředku Application Insights** | Rozevírací seznam se naplní ke všem prostředkům Application Insights, které odpovídají dříve vybrané skupiny prostředků.

5. Vyberte **výstrahy konfigurovat Application Insights**

6. Výchozí pravidla výstrah, vyberte **Uložit** > zadejte komentář > klikněte na tlačítko **OK**

## <a name="modify-alert-rules"></a>Upravit pravidla výstrah

1. Chcete-li upravit předdefinovaná nastavení výstrah, klikněte na pole s **výpustky...**  napravo od **pravidla výstrah.**

   (Out-of-box čtyři pravidla výstrah, které se nacházejí: dostupnost, neúspěšných žádostí, doba odezvy serveru, serveru výjimkami.)

2. Kliknutím na symbol rozevíracího seznamu vedle **dostupnosti.**

3. Úprava dostupnost **prahová hodnota** pro splnění požadavků na úrovni služby.

   ![Upravit výstrahu](.\media\app-insights-continuous-monitoring\003.png)

4. Vyberte **OK** > **Uložit** > zadejte komentář > klikněte na tlačítko **OK.**

## <a name="add-deployment-conditions"></a>Přidání podmínek nasazení

1. Klikněte na tlačítko **kanálu** > vyberte **před** nebo **po nasazení podmínky** symbol v závislosti na fázi, která vyžaduje bránu průběžné monitorování.

   ![Před nasazením podmínky](.\media\app-insights-continuous-monitoring\004.png)

2. Nastavit **brány** k **povoleno** > **schválení brány**> klikněte na tlačítko **přidat.**

3. Vyberte **Azure monitorování** (Tato možnost vám dává možnost na výstrahy přístup jak z monitorování Azure a Application Insights)

    ![Azure Monitor](.\media\app-insights-continuous-monitoring\005.png)

4. Zadejte **vypršení časového limitu brány** hodnotu.

5. Zadejte **Interval vzorkování.**

## <a name="deployment-gate-status-logs"></a>Protokoly stav nasazení brány

Po přidání brány nasazení je výstraha ve službě Application Insights, který přesahuje vaší dříve definovanou prahovou hodnotu, chrání vaše nasazení z nežádoucí verze povýšení. Po vyřešení výstrahy nasazení můžete pokračovat automaticky.

Toto chování zachovávají, vyberte **verze** > vydání klikněte pravým tlačítkem na název **otevřete** > **protokoly.**

![Logs](.\media\app-insights-continuous-monitoring\006.png)

## <a name="next-steps"></a>Další kroky

Další informace o vytvoření služby VSTS a verzi vyzkoušet [– elementy quickstart.](https://docs.microsoft.com/en-us/vsts/build-release/)
