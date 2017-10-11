---
title: "Nastavení zásad replikace pro fyzický server replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které je třeba nastavit zásady replikace při replikaci místní fyzických serverů do úložiště Azure pomocí služby Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 9ce23382001b54e7b9b7a58b8dd9aa61b400826d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-to-azure"></a>Krok 8: Nastavení zásad replikace pro fyzický server replikaci do Azure.


Tento článek popisuje, jak nastavit zásadu replikace, když replikujete fyzických serverů Windows nebo Linuxem do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.


POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="configure-a-policy"></a>Konfigurace zásad

1. Klikněte na tlačítko **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.
2. V **vytvořit zásadu replikace**, zadejte název zásady.
3. V části **Prahová hodnota cíle bodu obnovení (RPO)** zadejte omezení RPO. Tato hodnota určuje, jak často jsou vytvořeny body obnovení data. Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.
4. V **uchování bodu obnovení**, zadejte (v hodinách), jak dlouho je okno uchování pro každý bod obnovení. Replikované virtuální počítače můžete obnovit do libovolného bodu v okně. Až 24 hodin uchování je podporována pro počítače, na které se replikují do úložiště úrovně premium a 72 hodin, než standardní úložiště.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi. Klikněte na tlačítko **OK** k vytvoření zásad.

    ![Zásady replikace](./media/physical-walkthrough-replication/gs-replication2.png)
8. Když vytvoříte novou zásadu je automaticky přiřazen s konfiguračním serverem. Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady. Například, pokud je zásada replikace **rep zásad** bude navrácení služeb po obnovení zásad **rep. zásady navrácení**. Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.

## <a name="next-steps"></a>Další kroky

Přejděte na [krok 9: instalace služby Mobility](physical-walkthrough-install-mobility.md)
