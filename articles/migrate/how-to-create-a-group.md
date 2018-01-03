---
title: "Skupina počítačů pro hodnocení se migrovat Azure | Microsoft Docs"
description: "Popisuje, jak seskupit počítačů před spuštěním posouzení se službou Azure migrovat."
author: rayne-wiselman
ms.service: azure-migrate
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: raynew
ms.openlocfilehash: f42b184cddb3274d7ee0163c10cac002ccfbef62
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="group-machines-for-assessment"></a>Skupiny počítačů pro vyhodnocení

Tento článek popisuje, jak vytvořit skupiny počítačů pro vyhodnocení podle [Azure migrovat](migrate-overview.md). Azure migrací vyhodnocuje počítače ve skupině, zkontrolujte, jestli jste vhodné pro migraci na Azure a poskytuje velikost a cenově odhady pro spuštění na počítač v Azure.


## <a name="create-a-group"></a>Vytvoření skupiny

1. V **přehled** projektu Azure migrace, v části Správa, klikněte na tlačítko **skupiny** > **+ skupiny**a zadejte název skupiny.
2. Přidejte jeden nebo více počítačů do skupiny a klikněte na **vytvořit**. 
3. Volitelně můžete vybrat spustit nové vyhodnocení pro skupinu. 

    ![Vytvoření skupiny](./media/how-to-create-a-group/create-group.png)

Po vytvoření skupiny, můžete ho upravit tak, že vyberete skupinu na **skupiny** stránky a přidáním nebo odebráním počítače.

## <a name="next-steps"></a>Další kroky

- Další informace o použití [mapování závislostí počítače](how-to-create-group-machine-dependencies.md) k vytvoření vysoce důvěryhodných skupin.
- [Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.
