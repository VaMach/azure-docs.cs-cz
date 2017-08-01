---
title: "Povolení funkce Azure Event Hubs Capture prostřednictvím portálu | Dokumentace Microsoftu"
description: "K povolení funkce Event Hubs Capture použijte web Azure Portal."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 1f3d373944b909db290f6cf2da7bf12a8a00e1c5
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017


---

# <a name="enable-event-hubs-capture-using-the-azure-portal"></a>Povolení funkce Event Hubs Capture prostřednictvím webu Azure Portal

Ke konfiguraci funkce Capture v době vytvoření centra událostí můžete použít [Azure Portal](https://portal.azure.com). Funkci Capture povolíte kliknutím na tlačítko **Zapnout** v okně portálu **Vytvořit centrum událostí**. Potom nakonfigurujete účet úložiště a kontejner kliknutím na část **Kontejner** tohoto okna. Vzhledem k tomu, že Event Hubs Capture u úložiště využívá ověřování služba-služba, není potřeba zadávat připojovací řetězec úložiště. Výběr prostředku automaticky vybere URI prostředku pro váš účet úložiště. Pokud používáte Azure Resource Manager, musíte tento identifikátor URI explicitně zadat jako řetězec.

Výchozí časový interval je 5 minut. Minimální hodnota je 1, maximální hodnota je 15. Okno **Velikost** má rozsah 10 až 500 MB.

![][1]

## <a name="adding-capture-to-an-existing-event-hub"></a>Přidání funkce Capture do existujícího centra událostí

Funkci Capture je možné nakonfigurovat v existujících centrech událostí, která jsou v oborech názvů služby Event Hubs. Tato funkce není dostupná pro starší typy oborů názvů **Zasílání zpráv** nebo **Smíšené**. Pokud chcete funkci Capture povolit v existujícím centru událostí nebo chcete změnit její nastavení, klikněte na příslušný obor názvů, aby se načetlo okno **Základy**, a potom klikněte na centrum událostí, pro které chcete povolit funkci Capture nebo změnit její nastavení. Nakonec klikněte v otevřeném okně na část **Vlastnosti**, jak ukazuje následující obrázek:

![][2]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png

## <a name="next-steps"></a>Další kroky

Ke konfiguraci funkce Event Hubs Capture můžete také použít šablony Azure Resource Manageru. Další informace najdete v tématu věnovaném [povolení funkce Capture pomocí šablony Azure Resource Manageru](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).

