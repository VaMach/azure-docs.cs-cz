---
title: "Vytvoření, odkaz, odstranit nebo přesunout integrace účtu v Azure logic apps | Microsoft Docs"
description: "Postup vytvoření účtu integrace a propojit s logic apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 716e7b5bab8725dea0fd2b760d0e46e8e892c5b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-an-integration-account"></a>Co je integrace účet?

Účet integrace umožňuje integračních aplikací ke správě artefaktů, včetně schémat, map, certifikáty, partneři a smlouvy enterprise. Všechny integrace aplikace, které vytvoříte použije integrace účet pro přístup tyto schémat, map, certifikáty a tak dále.

## <a name="create-an-integration-account"></a>Vytvoření účtu integrace

1.  Přihlaste se na web [Azure Portal](http://portal.azure.com "Azure Portal"). V nabídce vlevo vyberte **další služby**.

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Do vyhledávacího pole zadejte "integrace" filtru. V seznamu výsledků vyberte **účty pro integraci**.

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. V horní části stránky, zvolte **přidat**.

    ![Vyberte Přidat](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Název účtu integrace a vyberte předplatné Azure, kterou chcete použít. Můžete buď vytvořit novou **skupiny prostředků** nebo vyberte existující skupinu prostředků. Vyberte **umístění** pro hostování účtu integrace a **cenová úroveň**. 

    Až budete připraveni, zvolte **vytvořit**.

    ![Zadejte podrobnosti pro váš účet integrace](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    Azure zřídí účtu integrace ve vybraném umístění, které by se měla dokončit během 1 minuty.

5. Aktualizujte stránku. Zobrazí váš nový účet integrace, které jsou uvedené.

    ![Zobrazí se váš nový účet integrace](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

V dalším kroku propojte integrace účet, který jste vytvořili pro svou aplikaci logiky. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Účet integrace propojit aplikaci logiky

Umožnit logiky aplikace přístup k mapám, schémata, smlouvy a artefaktů ve vašem účtu integrace propojit účet integrace do aplikace logiky.

### <a name="prerequisites"></a>Požadavky

* Účet integrace
* Aplikace logiky

> [!NOTE] 
> Ujistěte se, že integrace účet a logiku aplikace jsou *stejného umístění Azure* před zahájením.


1. Na portálu Azure vyberte svou aplikaci logiky a zkontrolujte umístění svou aplikaci logiky.

    ![Vyberte svou aplikaci logiky, zkontrolujte umístění](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. V části **nastavení**, vyberte **integrace účet**.

    ![Vyberte "Účet integrace"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Z **vyberte účet, integrace** seznamu, vyberte integrace účet, který chcete propojit do aplikace logiky. K dokončení propojení, zvolte **Uložit**.

    ![Vyberte svůj účet integrace](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    Zobrazí se oznámení, že zobrazuje svoji integraci do aplikace logiky je propojený účet a všechny artefakty ve vašem účtu integrace jsou nyní k dispozici pro svou aplikaci logiky.

    ![Aplikace logiky je propojený s vaším účtem integrace](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Teď, když je propojené účtu integrace aplikace logiky, můžete použít konektory B2B ve vašich logic apps. Některé běžné konektory B2B zahrnují ověření XML a plochý soubor kódování nebo dekódování.  

## <a name="delete-your-integration-account"></a>Odstranění účtu integrace

1. Vyberte **další služby**.

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Do vyhledávacího pole zadejte "integrace" filtru. V seznamu výsledků vyberte **účty pro integraci**.

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Vyberte integrační účet, který chcete odstranit.

    ![Vyberte účet integrace odstranit](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. V nabídce zvolte **odstranit**.

    ![Zvolte "Odstranit"](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. Potvrďte volbu Odstranit účet integrace.

## <a name="move-your-integration-account"></a>Přesunutí účtu integrace

Pokud chcete přesunout účet integrace Azure jiné předplatné nebo prostředek skupiny, postupujte podle těchto kroků.

> [!IMPORTANT]
> Je nutné aktualizovat všechny skripty používat nový prostředek ID po přesunutí účet integrace.

1. Vyberte **další služby**.

    ![Vyberte možnost "Další služby"](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Do vyhledávacího pole zadejte "integrace" filtru. V seznamu výsledků vyberte **účty pro integraci**.

    ![Filtrovat podle "integraci", vyberte "Účty pro integraci"](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. Vyberte integrační účet, který chcete přesunout. V části **nastavení**, zvolte **vlastnosti**.

    ![Vyberte účet integrace přesunout. V části Nastavení vyberte možnost Vlastnosti](./media/logic-apps-enterprise-integration-accounts/move.png)

5. Změňte skupinu prostředků nebo předplatného Azure, který je spojen s vaším účtem integrace.

    ![Vyberte skupiny prostředků změnu nebo změnu předplatného](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Další kroky
* [Další informace o smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md "Další informace o integraci smlouvy enterprise")  

