---
title: "Pozastavení a spustit vaše Power BI Embedded kapacita na portálu Azure | Microsoft Docs"
description: "Tento článek vás provede způsob pozastavení a spuštění Power BI Embedded kapacity v Microsoft Azure."
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
ms.openlocfilehash: a7f86ebf3e79812eb50e58cbb320336cbd1149e6
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="pause-and-start-your-power-bi-embedded-capacity-in-the-azure-portal"></a>Pozastavení a spustit vaše Power BI Embedded kapacita na portálu Azure

Tento článek vás provede způsob pozastavení a spuštění Power BI Embedded kapacity v Microsoft Azure. Předpokladem je, že jste vytvořili Power BI Embedded kapacity. Pokud máte není, najdete v části [vytvořit Power BI na Embedded kapacity na portálu Azure](create-capacity.md) začít pracovat.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="pause-your-capacity"></a>Pozastavení vaše kapacita

Pozastavení vaše kapacita, nebudete se účtují. Pozastavení vaše kapacita je velmi užitečná, pokud není potřeba použít kapacitu v časovém intervalu. Pomocí následujících kroků můžete pozastavit vaše kapacita.

> [!NOTE]
> Pozastavení kapacitou může zabránit obsah k dispozici v rámci Power BI. Zajistěte, aby pro zrušení přiřazení pracovních prostorů z vaší kapacity před pauzou, aby se zabránilo přerušení.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

2. Vyberte **všechny služby** > **Power BI Embedded** zobrazíte vaší kapacity.

    ![Všechny služby v rámci portálu Azure](media/pause-start/azure-portal-more-services.png)

3. Vyberte kapacitu, kterou chcete pozastavit.

    ![Power BI Embedded kapacity seznamu v rámci portálu Azure](media/pause-start/azure-portal-capacity-list.png)

4. Vyberte **pozastavení** v rámci podrobnosti kapacity.

    ![Pozastavení vaše kapacita](media/pause-start/azure-portal-pause-capacity.png)

5. Vyberte **Ano**, který potvrdí, že chcete pozastavit kapacitu.

    ![Potvrďte pozastavení](media/pause-start/azure-portal-confirm-pause.png)

## <a name="start-your-capacity"></a>Spuštění vaší kapacity

Obnovte využití spuštěním vaše kapacita. Spouštění vaše kapacita také obnoví fakturace.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

2. Vyberte **všechny služby** > **Power BI Embedded** zobrazíte vaší kapacity.

    ![Všechny služby v rámci portálu Azure](media/pause-start/azure-portal-more-services.png)

3. Vyberte kapacitu, kterou chcete spustit.

    ![Power BI Embedded kapacity seznamu v rámci portálu Azure](media/pause-start/azure-portal-capacity-list.png)

4. Vyberte **spustit** v rámci podrobnosti kapacity.

    ![Spuštění vaší kapacity](media/pause-start/azure-portal-start-capacity.png)

5. Vyberte **Ano**, který potvrdí, že chcete spustit kapacitu.

    ![Potvrdit spuštění](media/pause-start/azure-portal-confirm-start.png)

Pokud žádný obsah je přiřazen k této kapacitu, je k dispozici po spuštění.

## <a name="next-steps"></a>Další postup

Pokud chcete škálovat vaše kapacita nahoru nebo dolů, přečtěte si téma [škálovat vaše Power BI Embedded kapacita](scale-capacity.md).

Pokud chcete začít, vkládání obsah Power BI v rámci vaší aplikace, najdete v části [postup vložení váš Power BI řídicí panely, sestavy a dlaždice](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Chcete se ještě na něco zeptat? [Zkuste požádat komunitě Power BI](http://community.powerbi.com/)