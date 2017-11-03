---
title: "Více geograficky pomůže dokumentace | Microsoft Docs"
description: "Naučte se vytvořit pracovní prostor a publikovat webovou službu v různých z – jih centrální USA (SCUS) oblasti Azure oblast Azure."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: e4941ccf8c6d7a0c77527e9c1d722bc3a770114a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="multi-geo-help-documentation"></a>Dokumentace nápovědy k více geografickým oblastem
Tento článek popisuje, jak můžete vytvořit pracovní prostor a publikovat webovou službu v různých oblastech Azure.  [Produkty Azure podle oblasti stránky](https://azure.microsoft.com/en-us/regions/services/) obsahuje seznam oblastí, kde je k dispozici Azure Machine Learning.

## <a name="create-a-workspace"></a>Vytvoření pracovního prostoru
1. Přihlaste se k portálu Azure Classic.
2. Klikněte na tlačítko **+ nový** > **datové služby** > **MACHINE LEARNING** > **rychle vytvořit**.  V části **umístění** vybrat jinou oblast, například **jihovýchodní Asie**.
   ![Více geograficky pomůže obrázek 1][1]
3. Vyberte pracovní prostor a potom klikněte na **přihlášení k ML Studio**.
   ![Více geograficky pomůže obrázek 2][2]
4. Nyní máte pracovního prostoru v jiné oblasti, které můžete používat stejně jako ostatní pracovního prostoru. Přepnout mezi pracovní prostory, podívejte se na pravé horní části obrazovky. Klikněte na rozevírací nabídce vyberte oblast a pak vyberte pracovní prostor. Všechno, co je místní pro oblast pracovního prostoru.  Například všechny vaše webové služby vytvořené z jiného pracovního prostoru bude ve stejné oblasti, které je umístěn v pracovním prostoru.
   ![Více geograficky pomůže image 3][3]

## <a name="open-an-experiment-from-gallery"></a>Mohli experiment otevřít z Galerie
Pokud jste mohli experiment otevřít z galerie, můžete také vybrat které oblasti, kterou chcete zkopírovat experiment a.

![Více geograficky pomůže obrázek 4][4a]

## <a name="web-service-management"></a>Správa webové služby
Pro programovou správu webové služby, například pro retraining, použijte adresu oblast:

* https://asiasoutheast.Management.azureml.NET
* https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Všimněte
1. Kopírovat můžete pouze experimenty mezi pracovní prostory, které patří do stejné oblasti tímto způsobem. Pokud je nutné zkopírovat experimentu napříč pracovní prostory v různých oblastech, můžete použít [prostředí PowerShell](http://aka.ms/amlps) příkaz [ *kopie AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) provést tuto akci. Další alternativní řešení, je publikovat experimentu do Galerie v seznamu neuvedené režimu, potom ho otevřete v pracovním prostoru z jiné oblasti.
2. Výběr oblasti zobrazí pouze pracovní prostory pro jednu oblast najednou.  
3. Vytváření a hostované ve Jižní střední USA volného prostoru nebo prostoru přístup hosta (anonymní)  
4. Webové služby z pracovního prostoru v jihovýchodní Asie nasazena se hostují taky v jihovýchodní Asie.  

## <a name="more-information"></a>Další informace
Zeptejte se na [fórum Azure Machine Learning](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/multi-geo/multi-geo_1.png
[2]: ./media/multi-geo/multi-geo_2.png
[3]: ./media/multi-geo/multi-geo_3.png
[4a]: ./media/multi-geo/multi-geo_4a.png
