---
title: "Vytvořit pracovní prostor Machine Learning | Microsoft Docs"
description: "Postup vytvoření pracovního prostoru pro Azure Machine Learning Studio"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
ms.openlocfilehash: 4e1fa0a9abd4721d15a94923263ff2f521bceee8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Vytvoření a sdílení pracovního prostoru Azure Machine Learning
Tato nabídka odkazy na témata, které popisují, jak nastavit různé vědě prostředí data používá Cortana proces pro analýzu (CAP).

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

Pokud chcete používat Azure Machine Learning Studio, musíte mít pracovní prostor Machine Learning. Tento pracovní prostor obsahuje nástroje, které potřebujete k vytváření, správu a publikování experimenty.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

### <a name="to-create-a-workspace"></a>Vytvoření pracovního prostoru
1. Přihlaste se k [portálu Azure](https://portal.azure.com/)

    > [!NOTE]
    > Přihlaste se a vytvořit pracovní prostor, musíte být správce předplatného Azure. 
    >
    > 

2. Klikněte na tlačítko **+ nové**

3. Vyberte **Intelligence + analýzy**, klikněte na tlačítko **pracovní prostor Machine Learning**, pak klikněte na tlačítko **vytvořit**

4. Zadejte informace o pracovním prostoru

    - *Název pracovního prostoru* může být až 260 znaků, ne končí v prostoru. Název nesmí obsahovat tyto znaky:`< > * % & : \ ? + /`
    - *Plán webové služby* vyberete (nebo vytvořte), spolu s příslušnými *cenová úroveň* můžete vybrat, se používá při nasazování webových služeb z tohoto pracovního prostoru.

    ![Vytvořit nový pracovní prostor](./media/create-workspace/create-new-workspace.png)

5. Klikněte na **Vytvořit**

Po nasazení pracovním prostoru můžete ho otevřít v nástroji Machine Learning Studio.

1. Procházet a strojového učení Studio na [https://studio.azureml.net/](https://studio.azureml.net/).

2. V pravém horním rohu vyberte pracovní prostor.

    ![Vyberte pracovní prostor](./media/create-workspace/open-workspace.png)

3. Klikněte na tlačítko **Moje experimenty**.

    ![Otevřete experimenty](./media/create-workspace/my-experiments.png)

Informace o správě pracovního prostoru najdete v tématu [spravovat pracovní prostor služby Azure Machine Learning](manage-workspace.md).
Pokud narazíte na potíže při vytváření pracovního prostoru, přečtěte si téma [Průvodce odstraňováním potíží: Vytvořte a připojte se k pracovní prostor Machine Learning](troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Sdílení pracovní prostor služby Azure Machine Learning
Jednou Machine Learning při vytváření pracovního prostoru, můžete uživatele pozvat, do pracovního prostoru sdílet přístup do pracovního prostoru a všechny jeho experimenty, datové sady, poznámkové bloky, atd. Můžete přidat uživatele v jednom ze dvou rolí:

* **Uživatel** -prostoru uživatel může vytvořit, otevřít, upravit a odstranit experimenty, datové sady atd., v pracovním prostoru.
* **Vlastník** – vlastníka můžete pozvat a odebírání uživatelů v pracovním prostoru, kromě uživatele, můžete provést.

> [!NOTE]
> Účet správce, který vytvoří pracovním prostoru se automaticky přidá do pracovního prostoru jako pracovní prostor vlastníka. Ale jiní správci nebo uživatelé v tomto předplatném nejsou automaticky udělí přístup k pracovním prostoru – je potřeba je explicitně pozvat.
> 
> 

### <a name="to-share-a-workspace"></a>Chcete-li sdílet pracovního prostoru

1. Přihlaste se k strojového učení Studio na [https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. V levém panelu klikněte na tlačítko **nastavení**

3. Klikněte **uživatelé** karta

4. Klikněte na tlačítko **POZVAT uživatele více** v dolní části stránky

    ![Nastavení Studio](./media/create-workspace/settings.png)

5. Zadejte jeden nebo více e-mailové adresy. Uživatelé, potřebujete účet Microsoft nebo účet organizace (z Azure Active Directory).

6. Vyberte, zda chcete přidat uživatele jako vlastníka nebo uživatele.

7. Klikněte **OK** značku zaškrtnutí.

Každý uživatel, které přidáte obdrží e-mail s pokyny, jak se přihlásit do sdíleného pracovního prostoru.

> [!NOTE]
> Pro uživatele, abyste mohli nasadit nebo spravovat webových služeb v tomto pracovním prostoru musí být Přispěvatel nebo správce v rámci předplatného Azure. 



