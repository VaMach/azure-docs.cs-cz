---
title: "Vytvořte svůj první pracovní postup spojující cloudové aplikace a cloudové služby – Azure Logic Apps | Dokumentace Microsoftu"
description: "Přečtěte si, jak automatizovat firemní procesy pro systémovou integraci a integraci podnikových aplikací (EAI) pomocí vytváření a spouštění pracovních postupů v Azure Logic Apps."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
keywords: "pracovní postup, cloudové aplikace, cloudové služby, firemní procesy, systémová integrace, integrace podnikových aplikací, enterprise application integration, EAI"
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/31/2017
ms.author: jehollan; estfan; LADocs
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 6634f0ee88e68f2fcb09fd7534a88677e8efa029
ms.lasthandoff: 04/03/2017

---

# <a name="create-your-first-logic-app-workflow-to-automate-processes-between-cloud-apps-and-cloud-services"></a>Vytvořte svůj první pracovní postup aplikace logiky pro automatizaci procesů mezi cloudovými aplikacemi a cloudovými službami

Bez psaní jakéhokoli kódu můžete snáze a rychleji automatizovat firemní procesy díky vytváření a spouštění pracovních postupů v [Azure Logic Apps](logic-apps-what-are-logic-apps.md). Tento první příklad ukazuje, jak vytvořit základní aplikaci logiky, která podle informačního kanálu RSS kontroluje nový obsah na webu. Jakmile se v informačním kanálu webu objeví nové položky, aplikace logiky odešle e-mailu z účtu v Outlooku nebo Gmailu.

K vytvoření a spuštění aplikace logiky potřebujete tyto věci:

* Předplatné Azure. Pokud předplatné nemáte, můžete [začít s bezplatným účtem Azure](https://azure.microsoft.com/free/). Jinak si můžete [zaregistrovat předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

  Vaše předplatné se používá pro účtování využívání aplikace logiky. Přečtěte si, jak funguje [měření využití](../logic-apps/logic-apps-pricing.md) a jaké jsou [ceny](https://azure.microsoft.com/pricing/details/logic-apps) Azure Logic Apps.

Tento příklad také vyžaduje následující položky:

* Účet Outlook.com, Office 365 Outlook nebo Gmail

    > [!TIP]
    > Pokud máte osobní [účet Microsoft](https://account.microsoft.com/account), máte účet Outlook.com. Pokud máte pracovní nebo školní účet Azure, máte účet **Office 365 Outlook**.

* Odkaz na informační kanál RSS nějakého webu. Tento příklad používá kanál RSS pro [web MSDN Channel 9](https://channel9.msdn.com/): `https://s.ch9.ms/Feeds/RSS`

## <a name="add-a-trigger-that-starts-your-workflow"></a>Přidání triggeru, který spustí váš pracovní postup

[*Trigger*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) je událost, která spustí pracovní postup vaší aplikace logiky a je první položkou, kterou aplikace logiky potřebuje.

1. Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal").

2. V levé nabídce zvolte **Nový** > **Podniková integrace** > **Aplikace logiky**, jak vidíte na tomto obrázku:

     ![Azure Portal, Nový, Podniková integrace, Aplikace logiky](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > Můžete také zvolit **Nový**, do vyhledávacího pole zadat `logic app` a stisknout klávesu Enter. Pak zvolte **Aplikace logiky** > **Vytvořit**.

3. Pojmenujte svoji novou aplikaci logiky a vyberte své předplatné Azure. Nyní vytvořte a vyberte skupinu prostředků Azure, která vám pomůže organizovat a spravovat související prostředky Azure. Nakonec vyberte umístění datacentra pro hostování aplikace logiky. Až budete hotovi, vyberte **Připnout na řídicí panel** a pak **Vytvořit**.

     ![Podrobnosti aplikace logiky](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > Když vyberte možnost **Připnout na řídicí panel**, vaše aplikace logiky se po nasazení objeví na řídicím panelu Azure a automaticky se otevře. Pokud se aplikace logiky na řídicím panelu neobjeví, na dlaždici **Všechny prostředky** zvolte **Zobrazit další** a vyberte svou aplikaci logiky. Nebo v nabídce vlevo klikněte na **Další služby**. V části **Podniková integrace** zvolte **Aplikace logiky** a vyberte svoji aplikaci logiky.

4. Při prvním otevření aplikace logiky vám Návrhář aplikací logiky nabídne šablony, které můžete použít pro začátek. Tentokrát klikněte na **Prázdná aplikace logiky**, abyste mohli aplikaci vytvořit od začátku.

    Otevře se Návrhář aplikace logiky a zobrazí dostupné služby a *triggery*, které můžete v aplikaci logiky použít.

5. Do vyhledávacího pole zadejte `RSS` a vyberte trigger **RSS – když se publikuje položka informačního zdroje**. 

    ![Trigger RSS](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. Zadejte odkaz na informační zdroj RSS webu, který chcete sledovat. 

     Můžete také změnit **frekvenci** a **interval**. 
     Tato nastavení určují, jak často má aplikace logiky kontrolovat nové položky a vracet všechny položky nalezené za určité období.

     V našem příkladu budeme přítomnost nových položek na webu MSDN Channel 9 kontrolovat každý den.

     ![Nastavení triggeru s informačním kanálem RSS, frekvencí a intervalem](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. Teď svoji práci uložte. (Na panelu příkazů Návrháře zvolte **Uložit**.)

   ![Uložení aplikace logiky](media/logic-apps-create-a-logic-app/save-logic-app.png)

   Uložením přejde aplikace logiky do aktivního provozu, ale v současné době pouze kontroluje nové položky v zadaném informačním kanálu RSS. 
   Aby byl tento příklad o něco užitečnější, přidáme akci, kterou aplikace logiky provede po splnění triggeru.

## <a name="add-an-action-that-responds-to-your-trigger"></a>Přidání akce, která reaguje na trigger

[*Akce*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) je úloha, kterou provádí pracovní postup vaší aplikace logiky. Když do aplikace logiky přidáte trigger, můžete přidat akci, která bude provádět určitou operaci s daty generovanými triggerem. V našem příkladu přidáme akci, která odešle e-mail, jakmile se v informačním kanálu RSS webu objeví nové položky.

1. V Návrháři pod triggerem zvolte **Nový krok** > **Přidat akci**, ukazuje tento obrázek:

   ![Přidání akce](media/logic-apps-create-a-logic-app/add-new-action.png)

   Návrhář zobrazí [dostupné konektory](../connectors/apis-list.md), ze kterých můžete vybrat akci, která se má spustit v reakci na trigger.

2. Podle typu vašeho e-mailového účtu postupujte podle kroků pro Outlook nebo Gmail.

   * Když chcete odeslat e-mail z účtu Outlooku, zadejte do vyhledávacího pole `outlook`. V části **Služby** zvolte **Outlook.com** pro osobní účty Microsoft nebo **Office 365 Outlook** pro pracovní nebo školní účty Azure. 
   V části **Akce** zvolte **Odeslat e-mail**.

       ![Výběr akce Outlooku „Odeslat e-mail“](media/logic-apps-create-a-logic-app/actions.png)

   * Když chcete odeslat e-mail z účtu Gmailu, zadejte do vyhledávacího pole `gmail`. 
   V části **Akce** zvolte **Odeslat e-mail**.

       ![Výběr akce Gmailu „Odeslat e-mail“](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. Když se zobrazí výzva k zadání přihlašovacích údajů, zadejte uživatelské jméno a heslo k e-mailovému účtu. 

4. Zadejte podrobnosti pro tuto akci, jako je cílová e-mailová adresa, a vyberte parametry dat, která se mají do e-mailu zahrnout, například:

   ![Výběr dat pro zahrnutí do e-mailu](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    Pokud jste zvolili Outlook, vaše aplikace logiky může vypadat podobně jako v tomto příkladu:

    ![Hotová aplikace logiky](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.    Uložte provedené změny. (Na panelu příkazů Návrháře zvolte **Uložit**.)

6. Nyní můžete aplikaci logiky ručně spustit a otestovat ji. Na panelu příkazů Návrháře zvolte **Spustit**. Můžete také nechat aplikaci logiky zkontrolovat zadaný informační kanál RSS podle nastaveného plánu.

   Pokud aplikace logiky najde nové položky, odešle e-mail obsahující požadovaná data. 
   Když žádné nové položky nenajde, přeskočí akci, která odesílá e-mail.

7. Ke sledování a kontrole činnosti a historie spouštění vaší aplikaci logiky vyberte v nabídce aplikace logiky možnost **Přehled**.

   ![Sledování a kontrola činnosti a historie spouštění aplikaci logiky](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Pokud nenajdete očekávaná data, na panelu příkazů zkuste vybrat možnost **Aktualizovat**.

   Další informace o stavu a historie spouštění vaší aplikace logiky a o její diagnostice najdete v článku [Odstraňovaní potíží s aplikací logiky](logic-apps-diagnosing-failures.md).

      > [!NOTE]
      > Aplikace logiky se bude spouštět dál, dokud ji nevypnete. Pokud chcete aplikaci dočasně vypnout, v nabídce aplikace logiky zvolte **Přehled**. Na panelu příkazů zvolte **Zakázat**.

Gratulujeme, právě jste vytvořili a spustili svoji první základní aplikaci logiky. Také jste se naučili, jak snadno vytvořit pracovní postupy pro automatizaci procesů, jak a integrovat cloudové aplikace a cloudové služby – a všechno bez programování.

## <a name="manage-your-logic-app"></a>Správa aplikací logiky

Při správě své aplikace můžete provádět úkoly jako je kontrola stavu, úpravy, zobrazení historie, vypnutí nebo odstranění aplikace logiky.

1. Přihlaste se na web [Azure Portal](https://portal.azure.com "Azure Portal").

2. V nabídce vlevo klikněte na **Další služby**. V části **Podniková integrace** zvolte **Logic Apps**. Vyberte svou aplikaci logiky. 

   V nabídce aplikace logiky najdete tyto úkoly správy:

   |Úkol|Kroky| 
   |:---|:---| 
   | Zobrazení stavu aplikace, historie spouštění a obecných informací| Zvolte **Přehled**.| 
   | Úprava aplikace | Zvolte **Návrhář aplikace logiky**. | 
   | Zobrazení definice JSON pracovního postupu vaší aplikace | Zvolte **Zobrazení kódu aplikace logiky**. | 
   | Zobrazení operací, které aplikace logiky provedla | Zvolte **Protokol aktivit**. | 
   | Zobrazení předchozích verzí aplikace logiky | Zvolte **Verze**. | 
   | Dočasné vypnutí aplikace | Zvolte **Přehled** a pak na panelu příkazů **Zakázat**. | 
   | Odstranění aplikace | Zvolte **Přehled** a pak na panelu příkazů **Odstranit**. Zadejte název aplikace logiky a zvolte **Odstranit**. | 

## <a name="get-help"></a>Podpora

Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

*  [Přidání podmínek a spuštění pracovních postupů](../logic-apps/logic-apps-use-logic-app-features.md)
*     [Šablony pro aplikace logiky](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Vytváření aplikací logiky ze šablon Azure Resource Manageru](../logic-apps/logic-apps-arm-provision.md)

