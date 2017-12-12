---
title: "Správa aktualizací v zásobníku Azure přehled | Microsoft Docs"
description: "Další informace o správě aktualizací pro Azure zásobníku integrované systémy."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9b0781f4-2cd5-4619-a9b1-59182b4a6e43
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 4355e7a8c220dea03df5eda54ed4e94d6ebc5c94
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="manage-updates-in-azure-stack-overview"></a>Správa aktualizací v přehledu Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy*

Společnost Microsoft vydá balíčky aktualizací pro Azure zásobníku integrované systémy v pravidelných cadence, který patří obvykle čtvrté úterý v každém měsíci začínající na obecné dostupnosti. Požádejte výrobce OEM o jejich konkrétní oznámení procesu zajistit aktualizace oznámení reach vaší organizace, nebo toto políčko zaškrtněte v části systémy notes\Integrated Concepts\Release poznámky k verzi pro další informace o konkrétních verzích.

Jednotlivými verzemi aktualizací softwaru společnosti Microsoft je instalován jako balíček jednu aktualizaci. Jako operátor zásobník Azure můžete snadno importovat, instalace a sledujte průběh instalace těchto aktualizací balíčky z portálu správce. 

Dodavatele hardwaru, výrobce (OEM), bude také uvolnit aktualizace, jako jsou ovladače a firmware aktualizace. Tyto aktualizace se dodávají jako samostatné balíčky podle dodavatele hardwaru, od výrobců OEM a jsou spravovány samostatně z aktualizací společnosti Microsoft.

Aby byl váš systém v rámci podpory, je nutné zachovat zásobník Azure aktualizovat na úroveň konkrétní verzi. Ujistěte se, abyste si prošli [zásobník Azure obsluhy zásad](azure-stack-servicing-policy.md).

> [!NOTE]
> Balíčky aktualizací zásobník Azure nemůže použít na Azure zásobníku Development Kit. Balíčky aktualizací jsou navrženy pro integrované systémy.

## <a name="the-update-resource-provider"></a>Poskytovatel prostředku aktualizace

Azure zásobníku obsahuje poskytovatele prostředku aktualizace, které orchestrují použití aktualizací softwaru společnosti Microsoft. Tohoto poskytovatele prostředků zajistí, že se aktualizace napříč všechny fyzické hostitele, aplikace Service Fabric a moduly runtime a všechny infrastruktury virtuální počítače a jejich přidružených službách.

Jak nainstalovat aktualizace, můžete snadno zobrazit podrobný stav jako cíle proces aktualizace různé subsystémy v zásobníku Azure (například fyzických hostitelů a infrastrukturu virtuálních počítačů).

## <a name="plan-for-updates"></a>Plán pro aktualizace

Důrazně doporučujeme upozornit uživatele žádné operace údržby a naplánovat normální údržbu jiný pracovní dobu co nejvíc. Operace údržby může mít vliv na portálu operace a úlohy klientů.

## <a name="using-the-update-tile-to-manage-updates"></a>Pomocí dlaždice aktualizace ke správě aktualizací
Správa aktualizací z portálu správce je jednoduchý proces. Operátor zásobník Azure můžete přejít na aktualizace dlaždice na řídicím panelu na:

- zobrazit důležité informace, jako je aktuální verze.
- instalace aktualizací a sledovat průběh.
- Zkontrolujte historii aktualizací pro dříve nainstalované aktualizace.
 
## <a name="determine-the-current-version"></a>Zjistit aktuální verzi

Aktualizace dlaždice se zobrazuje aktuální verze Azure zásobníku. Na dlaždici aktualizace můžete získat pomocí některé z následujících metod v portálu správce:

- Na řídicím panelu zobrazovat v aktuální verzi **aktualizace** dlaždici.
 
   ![Aktualizace dlaždice na řídicím panelu výchozí](./media/azure-stack-updates/image1.png)
 
- Na **oblast správy** dlaždici, klikněte na název oblasti. Zobrazit aktuální verzi v **aktualizace** dlaždici.

## <a name="next-steps"></a>Další kroky

- [Údržba zásad Azure zásobníku](azure-stack-servicing-policy.md) 
- [Oblast správy v Azure zásobníku](azure-stack-region-management.md)     


