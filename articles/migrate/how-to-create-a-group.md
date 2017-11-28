---
title: "Skupina počítačů pro hodnocení se migrovat Azure | Microsoft Docs"
description: "Popisuje, jak seskupit počítačů před spuštěním posouzení se službou Azure migrovat."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5c279804-aa30-4946-a222-6b77c7aac508
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 966e0aebf56ab049e898d1787bfdfbb2ef23635e
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="group-machines-for-assessment"></a>Skupiny počítačů pro vyhodnocení

Tento článek popisuje, jak vytvořit skupiny počítačů pro vyhodnocení podle [Azure migrovat](migrate-overview.md). Azure migrací vyhodnocuje počítače ve skupině, zkontrolujte, jestli jste vhodné pro migraci na Azure a poskytuje velikost a cenově odhady pro spuštění na počítač v Azure.


## <a name="create-a-group"></a>Vytvoření skupiny

1. V **řídicí panel** projektu Azure migrovat, klikněte na tlačítko **skupiny** > **+ skupiny**a zadejte název skupiny.
2. Přidejte jeden nebo více počítačů do skupiny a klikněte na **vytvořit**. 
3. Volitelně můžete vybrat spustit nové vyhodnocení pro skupinu. 

    ![Vytvoření skupiny](./media/how-to-create-a-group/create-group.png)

Po vytvoření skupiny, můžete ho upravit tak, že vyberete skupinu na **skupiny** stránky a přidáním nebo odebráním počítače.

## <a name="next-steps"></a>Další kroky

- Další informace o použití [mapování závislostí počítač](how-to-create-group-machine-dependencies.md) vytvořit podrobnější skupiny.
- [Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.
