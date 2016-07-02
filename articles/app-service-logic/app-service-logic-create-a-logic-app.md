<properties
    pageTitle="Vytvoření aplikace logiky | Microsoft Azure"
    description="Podívejte se, jak vytvořit aplikaci logiky propojením služeb SaaS."
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="app-service\logic"
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/16/2016"
    ms.author="stepsic"/>

# Vytvoření nové aplikace logiky propojením služeb SaaS

| Stručná referenční příručka |
| --------------- |
| [Jazyk pro definování Logic Apps](https://msdn.microsoft.com/library/azure/mt643789.aspx) |
| [Dokumentace ke konektoru Logic Apps](../connectors/apis-list.md) |
| [Fórum Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

Toto téma popisuje, jak během několika minut začít s [App Services Logic Apps](app-service-logic-what-are-logic-apps.md). Projdeme si workflow, který umožňuje doručovat do poštovní schránky sadu tweetů, které vás zajímají.

Abyste mohli použít tento scénář, potřebujete:

- Předplatné Azure
- Účet na Twitteru
- Účet Office 365

## Vytvoření nové aplikace logiky k posílání tweetů e-mailem

1. Na řídicím panelu Portálu Azure vyberte **Marketplace**. 
2. V části Všechno vyhledejte „aplikace logiky“ a vyberte **Aplikace logiky (Preview)**. Můžete také vybrat **Nový**, **Web + mobilní zařízení** a potom **Aplikace logiky (Preview)**. 
3. Zadejte název aplikace logiky, vyberte plán služby App Service a zvolte **Vytvořit**.  
    V tomto kroku předpokládáme, že máte plán služby App Service a jste obeznámeni s požadovanými vlastnostmi. Pokud ne, buďte bez obav. Můžete začít u [podrobného přehledu plánů Azure App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md). 

4. Až se aplikace logiky poprvé otevře, budete potřebovat trigger. Do vyhledávacího pole triggeru zadejte **twitter** a vyberte jej.

7. Teď zadáte, které klíčové slovo chcete na Twitteru vyhledat. 
    ![Vyhledávání na Twitteru](./media/app-service-logic-create-a-logic-app/twittersearch.png)

5. Vyberte znaménko plus a zvolte **Přidat akci** nebo **Přidat podmínku**:  
    ![Plus](./media/app-service-logic-create-a-logic-app/plus.png)
6. Když vyberete **Přidat akci**, zobrazí se všechny konektory spolu s jejich dostupnými akcemi. Pak si můžete vybrat, který konektor a akci chcete přidat do aplikace logiky. Vybrat si můžete například **Office 365 – Poslat e-mail** a další akce Office 365:  
    ![Akce](./media/app-service-logic-create-a-logic-app/actions.png)

7. Nyní je třeba vyplnit požadované parametry pro e-mail:  ![Parametry](./media/app-service-logic-create-a-logic-app/parameters.png)

8. Nakonec můžete vybrat **Uložit** a tím aplikaci logiky uvést do provozu.

## Správa aplikace logiky po vytvoření

Nyní je vaše aplikace logiky spuštěná a funkční. Pokaždé, kdy se spustí naplánovaný workflow, vyhledá tweety s konkrétním hashtagem. Pokud najde odpovídající tweet, vloží jej do vaší schránky. Nakonec uvidíte, jak je možné aplikaci vypnout nebo jak zkontrolovat její funkčnost.

1. Klikněte na **Procházet** na levé straně obrazovky a vyberte **Logic Apps**.

2. Klikněte na novou aplikaci logiky, kterou jste právě vytvořili: zobrazí se aktuální stav a obecné informace.

3. Pokud chcete svou novou aplikaci logiky upravit, klikněte na **Triggery a akce**.

5. Aplikaci deaktivujete tak, že na panelu příkazů kliknete na **Vypnout**.

Za méně než 5 minut jste dokázali nastavit jednoduchou aplikaci logiky, která běží v cloudu. Další informace o používání funkcí Logic Apps najdete v tématu o [Používání funkcí aplikací logiky]. Informace o definicích samotných aplikaci logiky najdete v tématu o [vytváření definic Logic Apps](app-service-logic-author-definitions.md).

<!-- Shared links -->
[Portál Azure]: https://portal.azure.com
[Používání funkcí aplikací logiky]: app-service-logic-create-a-logic-app.md



<!--HONumber=Jun16_HO2-->


