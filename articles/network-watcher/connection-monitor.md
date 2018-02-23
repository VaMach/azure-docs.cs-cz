---
title: "Sledování síťových připojení k Azure sledovací proces sítě - portálu Azure | Microsoft Docs"
description: "Naučte se monitorovat připojení k síti s sledovací proces sítě Azure pomocí portálu Azure."
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2018
ms.author: jdial
ms.openlocfilehash: beaa458d727a05eccf496933deb3c998828868cd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="monitor-network-connections-with-azure-network-watcher-using-the-azure-portal"></a>Monitorování připojení k síti s sledovací proces sítě Azure pomocí portálu Azure

Naučte se používat monitorování připojení k monitorování síťové připojení mezi virtuální počítač Azure a IP adresu. IP adresu můžete přiřadit k jiné prostředků Azure, nebo na Internetu nebo místní prostředek.

## <a name="prerequisites"></a>Požadavky

Před dokončením kroků v tomto článku, musí splňovat následující požadavky:

* Instance sledovací proces sítě v oblasti, kterou chcete sledovat pro připojení. Pokud jste již nemáte, můžete vytvořit jeden pomocí kroků v [vytvoření instance sledovací proces sítě Azure](network-watcher-create.md).
* Virtuální počítač z monitorování.
* Máte `AzureNetworkWatcherExtension` nainstalovat na virtuální počítače, kterou chcete sledovat připojení z. Chcete-li nainstalovat rozšíření virtuálního počítače s Windows, přečtěte si téma [rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) a nainstalovat rozšíření najdete v tématu virtuálního počítače Linux [Azure sítě sledovacích procesů agenta rozšíření virtuálního počítače pro Linux](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

## <a name="sign-in-to-azure"></a>Přihlášení k Azure 

Přihlaste se k [portálu Azure](http://portal.azure.com).

## <a name="create-a-connection-monitor"></a>Vytvoření připojení monitorování

1. Na levé straně na portálu, vyberte **další služby**.
2. Začněte psát *sledovací proces sítě*. Když **sledovací proces sítě** se zobrazí ve výsledcích hledání, vyberte ho.
3. V části **monitorování**, vyberte **monitorování připojení (Preview)**. Funkce ve verzi preview nemají stejnou úroveň spolehlivosti nebo dostupnost v oblastech jako funkce v hlavní verze.
4. Vyberte **+ přidat**.
5. Zadejte nebo vyberte příslušné informace pro připojení, které chcete monitorovat a potom vyberte **přidat**. V tomto příkladu připojení mezi *myVmSource* a *myVmDestination* virtuálních počítačů je monitorována přes port 80.
    
    |  Nastavení                                 |  Hodnota               |
    |  -------------------------------------   |  ------------------- |
    |  Název                                    |  myConnectionMonitor |
    |  Zdrojový virtuální počítač                  |  myVmSource          |
    |  Zdrojový port                             |                      |
    |  Cíl, vyberte virtuální počítač   |  myVmDestination     |
    |  Cílový port                        |  80                  |

6. Monitorování začne. Monitorování připojení sondy každých 60 sekund.
7. Monitorování připojení zobrazí průměrnou dobu odezvy sondy čas a procento, které nesplní. Data sledování můžete zobrazit v mřížce, nebo v grafu.

## <a name="next-steps"></a>Další postup

- Informace o automatizaci paketu zachytává se virtuální počítač výstrahy podle [vytváření zachycení aktivuje výstraha paketu](network-watcher-alert-triggered-packet-capture.md).

- Určení, zda některé provoz povolen v nebo z virtuálního počítače pomocí [IP tok ověření](network-watcher-check-ip-flow-verify-portal.md).