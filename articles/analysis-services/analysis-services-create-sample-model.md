---
title: "Přidejte ukázkový tabulkový model pro váš server Azure Analysis Services | Microsoft Docs"
description: "Informace o postupu přidání ukázkový model v Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/28/2018
ms.author: owend
ms.openlocfilehash: df83f5dd86d1edf857378ae69b16a86b57f9a2fe
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="tutorial-add-a-sample-model"></a>Kurz: Přidání ukázkových modelu

V tomto kurzu přidáte model ukázkové společnosti Adventure Works ke svému serveru. Ukázkový model je dokončené verzi (1200) dat společnosti Adventure Works Internet prodej modelování kurzu. Ukázkový model je užitečné pro testování model správy, připojení pomocí nástrojů a klientské aplikace a dotazování dat modelu.

## <a name="before-you-begin"></a>Než začnete

Pro absolvování tohoto kurzu potřebujete:

- Serveru Azure Analysis Services
- Oprávnění správce serveru

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-sample-model"></a>Vytvoření ukázkové modelu

1. V serveru **přehled**, klikněte na tlačítko **nový model**.

    ![Vytvoření ukázkové modelu](./media/analysis-services-create-sample-model/aas-create-sample-new-model.png)

2. V **nový model** > **vyberte zdroj dat**, ověřte **vzorová data** je vybrána a potom klikněte na **přidat**.

    ![Vyberte ukázková data](./media/analysis-services-create-sample-model/aas-create-sample-data.png)

3. V **přehled**, ověřte `adventureworks` ukázka je vytvořena.

    ![Vyberte ukázková data](./media/analysis-services-create-sample-model/aas-create-sample-verify.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Ukázkový model používá mezipaměť paměťových prostředků. Pokud nepoužíváte ukázkový model pro testování, byste měli odebrat ze serveru.

> [!NOTE]
> Tyto kroky popisují, jak odstranit modelu ze serveru pomocí aplikace SSMS. Můžete také odstranit model pomocí preview návrháře funkce webu.

1. V aplikaci SSMS > **Průzkumník objektů**, klikněte na tlačítko **připojit** > **služby Analysis Services**.

2. V **připojit k serveru**, vložte název serveru, pak v **ověřování**, zvolte **Universal s podpora vícefaktorového ověřování služby Active Directory -**, zadejte své uživatelské jméno a potom klikněte na **Připojit**.

    ![Přihlásit](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-signin.png)

3. V **Průzkumník objektů**, klikněte pravým tlačítkem myši `adventureworks` ukázkové databáze a pak klikněte na tlačítko **odstranit**.

    ![Odstranit ukázkové databáze](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-delete.png)

## <a name="next-steps"></a>Další postup 

[Připojení v Power BI Desktop](analysis-services-connect-pbi.md)   
[Spravovat role databáze a uživatele](analysis-services-database-users.md)


