---
title: "Vytvoření Power BI Embedded kapacity na portálu Azure | Microsoft Docs"
description: "Tento článek vás provede vytvoření Power BI Embedded kapacity v Microsoft Azure."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: bf7eb967760338626cd7e0465b16dc570e5f582a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-power-bi-embedded-capacity-in-the-azure-portal"></a>Vytvoření Power BI Embedded kapacity na portálu Azure

Tento článek vás provede vytvoření Power BI Embedded kapacity v Microsoft Azure. Power BI Embedded zjednodušuje možnosti Power BI dokážete rychle přidáte poutavých vizuální prvky, sestavy a řídicí panely do svých aplikací.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

> [!VIDEO https://www.youtube.com/embed/aXrvFfg_iSk]

## <a name="before-you-begin"></a>Než začnete

K dokončení tohoto rychlého startu je potřeba:

* **Předplatné Azure:** navštivte [bezplatná zkušební verze Azure](https://azure.microsoft.com/free/) vytvořit účet.
* **Azure Active Directory:** předplatného musí být přidružen klienta služby Azure Active Directory (AAD). A, ***musíte být přihlášeni do Azure s účtem v něm***. Účty Microsoft nejsou podporovány. Další informace najdete v tématu ověřování a uživatel oprávnění.
* **Power BI klienta:** alespoň jeden účet ve vašem tenantovi AAD, musíte mít zaregistrovaný pro Power BI.
* **Skupina prostředků:** už máte skupinu prostředků nebo [vytvořte novou](../azure-resource-manager/resource-group-overview.md).

## <a name="create-a-capacity"></a>Vytvoření kapacity

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

2. Vyberte **vytvořit prostředek** > **Data + analýzy**.

3. Do vyhledávacího pole Hledat *Power BI Embedded*.

4. V rámci Power BI Embedded vyberte **vytvořit**.

5. Zadejte požadované informace a potom vyberte **vytvořit**.

    ![Pole k vyplnění k vytvoření nové kapacity](media/create-capacity/azure-portal-create-power-bi-embedded.png)

    |Nastavení |Popis |
    |---------|---------|
    |**Název prostředku**|Název pro identifikaci kapacitu. Název prostředku se zobrazí v rámci portálu správce Power BI kromě portálu Azure.|
    |**Předplatné**|Předplatné, můžete chtít vytvořit kapacitu proti.|
    |**Skupina prostředků**|Skupinu prostředků, která obsahuje toto nové kapacity. Vyberte z existující skupinu prostředků nebo vytvořte jiný. Další informace naleznete v tématu [Přehled Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).|
    |**Správce služby Power BI kapacity**|Správci kapacity Power BI můžete zobrazit kapacitu na portálu správy Power BI a dát přiřazení oprávnění ostatním uživatelům. Ve výchozím nastavení je správce kapacity účtu. Správce kapacita musí být v rámci klienta služby Power BI.|
    |**Umístění**|Umístění je hostitelem Power BI pro vašeho klienta. Toto nastavení se vyřeší automaticky, že nelze vybrat jiné umístění.|
    |**Cenová úroveň**|Vyberte SKU (počet a paměti v základní velikost), která vyhovuje vašim potřebám.  Podrobnosti najdete v tématu [Power BI Embedded ceny](https://azure.microsoft.com/pricing/details/power-bi-embedded/)|

6. Vyberte **Vytvořit**.

Vytváření obvykle trvá za minutu; často jenom pár sekund. Pokud jste vybrali **připnout na řídicí panel**, přejděte na řídicí panel. zobrazíte nové kapacity. Nebo přejděte na **všechny služby** > **Power BI Embedded** chcete zobrazit, pokud je vaše kapacita připraven.

![Řídicí panel portálu Azure s Power BI Embedded kapacity](media/create-capacity/azure-portal-dashboard.png)

## <a name="next-steps"></a>Další postup

Pokud chcete používat vaše nové Power BI Embedded kapacity, přejděte na portál správce Power BI přiřadit pracovní prostory. Další informace najdete v tématu [Správa kapacit v Power BI Premium a Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-admin-premium-manage/).

Pokud nemusíte používat tuto kapacitu, pozastavte jej zastavit fakturace. Další informace najdete v tématu [pozastavení a spusťte vaše Power BI Embedded kapacita v portálu Azure](pause-start.md).

Pokud chcete začít, vkládání obsah Power BI v rámci vaší aplikace, najdete v části [postup vložení váš Power BI řídicí panely, sestavy a dlaždice](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Chcete se ještě na něco zeptat? [Zkuste požádat komunitě Power BI](http://community.powerbi.com/)