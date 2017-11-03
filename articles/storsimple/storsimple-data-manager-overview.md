---
title: "Přehled Microsoft Azure StorSimple Data Manager | Microsoft Docs"
description: "Obsahuje přehled služby StorSimple Manager dat (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: aedb44610fe57055851538b9dbdb810e66e58d73
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a>Přehled Data Manager zařízení StorSimple (soukromém náhledu).

## <a name="overview"></a>Přehled

Microsoft Azure StorSimple je řešení hybridní cloudové úložiště, které řeší složitosti nestrukturovaných dat běžně spojovaných se sdílenými složkami. StorSimple používá úložiště v cloudu jako rozšíření v případě místních řešení a automaticky úrovně dat mezi místní úložiště a úložiště v cloudu. Integrovat ochranu dat, místní a cloudových snímků, eliminuje potřebu rozrůstající infrastruktury úložiště. Archivace a zotavení po havárii je také bezproblémové s cloudem, který funguje jako mimo pracoviště.

Služba transformace dat, která Představujeme v tomto dokumentu, můžete bezproblémově přistupovat k StorSimple dat v cloudu. Tato služba poskytuje rozhraní API extrahovat data z StorSimple a prezentovat si ho k jiným službám Azure ve formátech, můžete snadno využívat. Formátů podporovaných v této verzi preview jsou Azure BLOB a prostředky Azure Media Services. Tato transformace umožňuje snadno propojit až službám, jako je Azure Media Services, Azure HDInsight, Azure Machine Learning a Azure Search fungovat data na místní zařízení řady StorSimple 8000.

Podrobný Blokový diagram ilustrující to jsou uvedeny níže.

![Vysokoúrovňový diagram](./media//storsimple-data-manager-overview/high-level-diagram.png)

Tento dokument popisuje, jak zaregistrovat privátní Preview verzi této služby. Také vysvětluje, jak lze pomocí této služby pro psaní aplikací, které používají StorSimple data a jinými službami Azure v cloudu.

## <a name="sign-up-for-data-manager-preview"></a>Zaregistrujte si Data Manager preview
Ještě než si zaregistrujete do služby Data Manager, přečtěte si následující požadavky.

### <a name="prerequisites"></a>Požadavky

Toto cvičení předpokládá, že máte
* Aktivní předplatné Azure.
* přístup k registrované zařízení StorSimple 8000 řady zařízení
* všechny klíče přidružené k zařízení řady StorSimple 8000.

### <a name="sign-up"></a>Registrace

Data Manager zařízení StorSimple je v privátní Preview verzi. Proveďte následující kroky se zaregistrovat v privátní Preview verzi této služby:

1.  Přihlaste se k portálu Azure s příponou StorSimple Manager dat na: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager). Přihlaste se pomocí přihlašovacích údajů Azure.

2.  Klikněte  **+**  ikona vytvoření služby. Klikněte na tlačítko **úložiště** a pak klikněte na **najdete v článku všechny** v okně, které se otevře.

    ![Ikona Data Manager StorSimple vyhledávání](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. Zobrazí ikona StorSimple Data Manager.

    ![Vyberte ikonu pro StorSimple Data Manager](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. Klikněte na ikonu StorSimple Manager dat a pak klikněte na **vytvořit**. Vyberte předplatné, které chcete povolit privátní Preview verzi a potom klikněte na **zapsat se!**

    ![Zapsat se](./media/storsimple-data-manager-overview/sign-me-up.png)

5. Tím se odešle požadavek na základní desce můžete. Nemůžeme se zaváděním je co nejdříve. Když je vaše předplatné povolená, můžete vytvořit služby StorSimple Data Manager.

6. Snadno přístup ke službě StorSimple Manager dat, klikněte na ikonu hvězdičky. Chcete-li připnout do vašich oblíbených položek.

    ![Správci StorSimple Data Access](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a>Další kroky

[Data Manager zařízení StorSimple pomocí uživatelského rozhraní pro transformaci dat](storsimple-data-manager-ui.md).