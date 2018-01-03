---
title: "POST announcment do testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Naučte se přidávat oznámení do testovacího prostředí v Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: v-craic
ms.openlocfilehash: d376909a46da11ac1b6b1fa968e53ebef8f3dbf7
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Odeslat oznámení do testovacího prostředí v Azure DevTest Labs

Jako správce a testovacího prostředí můžete účtovat vlastní oznámení existující testovacího prostředí a upozorněte uživatele o nedávné změny nebo přidání do testovacího prostředí. Například můžete chtít informovat uživatele o:

- Nové velikosti virtuálních počítačů, které jsou k dispozici
- Bitové kopie, které jsou aktuálně nepoužitelná
- Aktualizace zásad testovacího prostředí

Po odeslání, oznámení se zobrazí na stránce Přehled v prostředí a uživatele můžete vybrat další podrobnosti.

Funkce oznámení je určen pro použití pro dočasné oznámení.  Oznámení můžete snadno zakázat po již není potřeba.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Kroky při odesílání oznámení v existující testovacím prostředí

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. V případě potřeby vyberte **všechny služby**a potom vyberte **DevTest Labs** ze seznamu. (Testovacího prostředí může zobrazit již na řídicím panelu v části **všechny prostředky**).
1. Ze seznamu labs vyberte testovací prostředí, ve kterém chcete odeslat oznámení.  
1. V tomto prostředí **přehled** oblasti, vyberte **konfiguraci a zásady**.  

    ![Tlačítko konfiguraci a zásady](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. Na levé straně v části **nastavení**, vyberte **testovacím oznámení**.

    ![Tlačítko oznámení testovacího prostředí](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Chcete-li vytvořit zprávu pro uživatele v tomto testovacím prostředí, nastavte **povoleno** k **Ano**.

1. Zadejte **nadpis oznámení** a **text oznámení**.

   Název může být maximálně 100 znaků a se zobrazí uživatele na stránku přehled v prostředí. Pokud uživatel vybere nadpis, text oznámení se zobrazí.

   Text oznámení přijme markdownu. Při zadávání textu oznámení se zobrazí zpráva v oblasti náhledu v dolní části obrazovky.

    ![Oznámení obrazovky testovacího prostředí pro vytvoření zprávy.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Vyberte **Uložit** po odesíláním sdělení.

Pokud již nechcete zobrazit tato oznámení uživatelům prostředí, vraťte se do **testovacím oznámení** stránky a nastavte **povoleno** k **ne**.

## <a name="steps-for-users-to-view-an-announcement"></a>Kroky pro uživatele k zobrazení oznámení

1. Z [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), vyberte testovacím prostředí.

1. Pokud testovací prostředí má ho odeslat oznámení, oznámení informace se zobrazí v horní části stránky přehled v prostředí. Toto upozornění je oznámení produktu, který byl zadán při vytvoření oznámení.

    ![Oznámení testovacího prostředí na stránku přehled](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. Uživatele můžete vybrat zprávu zobrazíte celý oznámení.

    ![Další informace o hlášení testovacího prostředí](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další postup
* Pokud změníte nebo nastavení zásad testovacího prostředí, můžete odeslat oznámení k informování uživatelů. [Nastavit zásady a plány](devtest-lab-set-lab-policy.md) poskytuje informace o použití omezení a pravidla týkající se vašeho předplatného pomocí vlastních zásad.
* Prozkoumejte [Galerie šablon DevTest Labs Azure Resource Manager QuickStart](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
