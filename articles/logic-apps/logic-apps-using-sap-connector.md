---
title: "Připojit do systému SAP místně v Azure Logic Apps | Microsoft Docs"
description: "Připojit do systému SAP místní z pracovní postup aplikace logiky přes bránu místní data"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 3fea93f558d5a4ef62550fd1f6486903cb812930
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-an-on-premises-sap-system-from-logic-apps-with-the-sap-connector"></a>Připojit do systému SAP místní z aplikace logiky s konektorem SAP 

Místní brána dat umožňuje spravovat data a bezpečný přístup k prostředkům, které jsou na místě. Toto téma ukazuje, jak připojit aplikace logiky do místního systému SAP. V tomto příkladu aplikace logiky požadavky IDOC přes protokol HTTP a odešle odpověď zpět.    

> [!NOTE]
> Aktuální omezení: 
> - Aplikace logiky časového limitu, pokud nemáte dokončit všechny kroky potřebné pro odpověď v rámci [časový limit požadavku](./logic-apps-limits-and-config.md). V tomto scénáři může získat požadavky blokovány. 
> - Nástroje pro výběr souborů nezobrazuje všechna dostupná pole. V tomto scénáři můžete ručně přidat cesty.

## <a name="prerequisites"></a>Požadavky

- Instalace a konfigurace nejnovější [místní brána dat](https://www.microsoft.com/download/details.aspx?id=53127) verze 1.15.6150.1 nebo novější. [Jak se připojit k bráně místní data v aplikaci logiky](http://aka.ms/logicapps-gateway) jsou uvedené kroky. Brány musí být nainstalován v místním počítači, abyste mohli pokračovat.

- Stáhněte a nainstalujte nejnovější klientské knihovny SAP ve stejném počítači, kam jste nainstalovali bránu data gateway. Použijte některou z následujících verzí SAP: 
    - Serveru SAP
        - Libovolný Server SAP, který podporují konektor .NET (NCo) 3.0
 
    - SAP klienta
        - Konektor .NET SAP (NCo) 3.0

## <a name="add-triggers-and-actions-for-connecting-to-your-sap-system"></a>Přidat triggery a akce pro připojení k systému SAP

Konektor SAP má akce, ale není aktivační události. Ano jsme muset použít jiné aktivační událost při spuštění pracovního postupu. 

1. Přidat aktivační událost požadavků a odpovědí a potom vyberte **nový krok**.

2. Vyberte **přidat akci**a potom vyberte konektor SAP zadáním `SAP` do pole hledání:    

     ![Vyberte Server aplikace SAP nebo Server zpráv SAP](media/logic-apps-using-sap-connector/sap-action.png)

3. Vyberte [ **SAP aplikační Server** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) nebo [ **Server zpráv SAP**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), podle vašeho nastavení SAP. Pokud nemáte existující připojení, zobrazí se výzva k jeho vytvoření.

   1. Vyberte **připojit prostřednictvím místní brána dat**a zadejte podrobnosti pro váš systém SAP:   

       ![Přidat připojovací řetězec k SAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. V části **brány**vyberte existující bránu, nebo pokud chcete nainstalovat novou bránu, **instalaci brány**.

        ![Instalace nové brány](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. Když zadáte všechny podrobnosti, vyberte **vytvořit**. 
   Služba Logic Apps nakonfiguruje a otestuje připojení, a ujistěte se, že připojení funguje správně.

4. Zadejte název pro připojení k prostředí SAP.

5. Různé možnosti SAP jsou nyní k dispozici. Najít váš IDOC kategorie, vyberte ze seznamu. Ručně zadejte cestu a vyberte odpověď HTTP v **textu** pole:

     ![Akce SAP](media/logic-apps-using-sap-connector/picture3.png)

6. Akce pro vytváření pro přidání **odpověď HTTP**. Zpráva odpovědi musí být z výstupu SAP.

7. Uložte svou aplikaci logiky. Otestujte zasláním IDOC prostřednictvím adresy URL protokolu HTTP aktivační události. Po odeslání IDOC Čekejte na odpověď z aplikace logiky:   

     > [!TIP]
     > Podívejte se na postup [sledování funkce Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Teď, když je konektor SAP přidal do aplikace logiky, prozkoumejte další funkce:

- BAPI
- DOKUMENT RFC

## <a name="get-help"></a>Podpora

Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Pokud chcete pomoci při vylepšování Azure Logic Apps a konektorů, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak chcete ověřit, transformace a dalších funkcí jako BizTalk v [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- [Připojení k místním datům](../logic-apps/logic-apps-gateway-connection.md) z aplikace logiky
