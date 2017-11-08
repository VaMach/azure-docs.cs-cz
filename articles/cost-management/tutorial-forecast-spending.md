---
title: "Prognózy výdajů s náklady na správu Azure | Microsoft Docs"
description: "Prognózy výdajů pomocí historie využití a výdaje data."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: d8b0cd2a3e5f9829f0844783aad22d375eb9d7a8
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="forecast-future-spending"></a>Předpovídat budoucí výdaje

Azure náklady na správu Cloudyn umožňuje předpovídat budoucí výdaje pomocí historie využití a výdaje data. Cloudyn sestavy slouží k zobrazení všech dat projekce náklady. V příkladech v tomto kurzu vás provede procesem kontrola náklady projekce pomocí sestav. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Předpovídat budoucí výdaje

## <a name="forecast-future-spending"></a>Předpovídat budoucí výdaje

Cloudyn zahrnuje náklady projekce sestav usnadňujících prognózy výdajů na základě vašeho využití v čase. Jejich primárním účelem je vám pomohou zajistit, že vaše trendy nákladů nepřekračují očekávání vaší organizace. Sestavy, které můžete použít jsou aktuální měsíc projektovat náklady a náklady na roční projekci. Jak zobrazit předpokládané budoucí výdaje Pokud vaše využití zůstává relativně konzistentní s vaší posledních 30 dní využití.

Plánované náklady na aktuální měsíc sestava obsahuje náklady na vašich služeb. Zobrazit předpokládané náklady na na základě náklady od začátku měsíce a předchozího měsíce. V nabídce sestavy v horní části portálu, klikněte na tlačítko **náklady** > **projekce a nároky** > **projekci náklady na aktuální měsíc**. Následující obrázek ukazuje příklad.

![plánované náklady aktuálního měsíce.](./media/tutorial-forecast-spending/project-month01.png)

V příkladu se zobrazí, služby, které stráví nejvíc. Náklady na Azure byly nižší než AWS náklady. Pokud chcete zobrazit podrobnosti o nákladech projekce pro virtuální počítače Azure, v **filtru** seznamu, vyberte **virtuální počítač Azure nebo**.

![Virtuální počítač Azure aktuálního měsíce plánované náklady](./media/tutorial-forecast-spending/project-month02.png)

Použijte stejný základní předchozí postup se podívat na měsíční náklady na projekce pro jiné služby, které vás zajímají.

Roční náklady projektovat sestava zobrazí extrapolované náklady na vašich služeb za další 12 měsíců.

V nabídce sestavy v horní části portálu, klikněte na tlačítko **náklady** > **projekce a nároky** > **roční náklady projektovat**. Následující obrázek ukazuje příklad.

![roční sestava plánované náklady](./media/tutorial-forecast-spending/project-annual01.png)

V příkladu se zobrazí, služby, které stráví nejvíc. Podobně jako v příkladu měsíční náklady na Azure byly nižší než AWS náklady. Pokud chcete zobrazit podrobnosti o nákladech projekce pro virtuální počítače Azure, v **filtru** seznamu, vyberte **virtuální počítač Azure nebo**.

![roční předpokládané náklady na virtuální počítače](./media/tutorial-forecast-spending/project-annual02.png)

Na obrázku výše je roční předpokládané náklady na virtuálních počítačích Azure $28,374.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Předpovídat budoucí výdaje


Přechodu na v dalším kurzu se dozvíte, jak spravovat náklady se sestavami, náklady na přidělení a kompletní přehled nákladů.

> [!div class="nextstepaction"]
> [Řízení nákladů se sestavami, náklady na přidělení a kompletní přehled nákladů.](tutorial-manage-costs.md)
