---
title: "Přizpůsobit styl stránky na portálu pro vývojáře Azure API Management | Microsoft Docs"
description: "Postupujte podle tento rychlý start pro přizpůsobení stylu elementů na portál pro vývojáře Azure API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: f427663ba1c437785c8c521925d9f733c45cb40d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="customize-the-style-of-the-developer-portal-pages"></a>Přizpůsobit styl na stránkách portálu pro vývojáře

Existují tři nejběžnější způsoby, jak přizpůsobit portál pro vývojáře ve službě Azure API Management:
 
* [Upravit obsah statické stránky a stránky rozložení elementů](api-management-modify-content-layout.md)
* Aktualizovat styly používané pro prvky na stránce přes portál pro vývojáře (popsáno v této příručce)
* [Úprava šablony použité pro stránky vytvořený portál](api-management-developer-portal-templates.md) (například dokumentace rozhraní API, produktů, ověření uživatele)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Přizpůsobení stylu prvků na stránkách **vývojáře** portálu
> * Zobrazit změny

![přizpůsobení stylu](./media/modify-developer-portal-style/developer_portal.png)

## <a name="prerequisites"></a>Požadavky

+ Dokončete následující rychlý start: [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).
+ Navíc kurzu: [importu a publikování vašeho prvního rozhraní API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="customize-the-developer-portal"></a>Přizpůsobení portálu pro vývojáře

1. Vyberte **přehled**.
2. Klikněte na tlačítko **portál pro vývojáře** tlačítka v horní části **přehled** okno. Případně můžete kliknout na **portál pro vývojáře URL** odkaz.
3. Na levé straně horní části obrazovky zobrazí ikonu skládá ze dvou štětců. Podržte ukazatel nad ikonu otevřete nabídku přizpůsobení portálu.

    ![přizpůsobení stylu](./media/modify-developer-portal-style/modify-developer-portal-style01.png)
4. Vyberte **styly** v nabídce otevřete podokno přizpůsobení stylu.

    Všechny elementy, které můžete přizpůsobit pomocí **styly** zobrazí na stránce
5. Zadejte "headings-color" **měnit hodnoty proměnných k přizpůsobení vzhledu portálu vývojáře:** pole.

     **@headings-color**  Prvek se zobrazuje na stránce. Tato proměnná Určuje barvu textu.

    ![přizpůsobení stylu](./media/modify-developer-portal-style/modify-developer-portal-style02.png)
    
6. Klikněte na pole pro  **@headings-color**  proměnné. 
    
    Otevře se výběr barvy rozevíracího seznamu.
7. Výběr barvy rozevíracího seznamu vyberte novou barvu.

    > [!TIP]
    > V reálném čase preview je k dispozici pro všechny změny. V horní části podokna přizpůsobení se zobrazuje indikátor průběhu. Text záhlaví se změní za několik sekund barevně na nově vybrané.

8. Vyberte **publikovat** z levého dolního rohu v nabídce panelu přizpůsobení.
9. Vyberte **publikovat vlastní nastavení** provést změny veřejně dostupné.

## <a name="view-your-change"></a>Zobrazit změny

1. Přejděte na portál pro vývojáře.
2. Můžete zobrazit změny, která jste udělali.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Přizpůsobení stylu prvků na stránkách **vývojáře** portálu
> * Zobrazit změny

> [!div class="nextstepaction"]
> [Přizpůsobení portálu pro vývojáře Azure API Management pomocí šablon](api-management-developer-portal-templates.md)