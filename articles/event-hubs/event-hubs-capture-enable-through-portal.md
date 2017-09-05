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
ms.date: 08/28/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 5420c1cdefa99ff30320dd757e04aa0cafc792bc
ms.contentlocale: cs-cz
ms.lasthandoff: 08/29/2017

---

# <a name="enable-event-hubs-capture-using-the-azure-portal"></a>Povolení funkce Event Hubs Capture prostřednictvím webu Azure Portal

Azure [Event Hubs Capture][capture-overview] umožňuje automatické doručování streamovaných dat ve službě Event Hubs do služby [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) nebo do účtu [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/) podle vašeho výběru.

Ke konfiguraci funkce Capture v době vytvoření centra událostí můžete použít [Azure Portal](https://portal.azure.com). Data můžete zachytávat do kontejneru služby Azure [Blob Storage](https://azure.microsoft.com/services/storage/blobs/) nebo do účtu [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).

Další informace najdete v tématu [Přehled funkce Event Hubs Capture][capture-overview].

## <a name="capture-data-to-an-azure-storage-account"></a>Zachytávání dat do účtu služby Azure Storage  

Při vytváření centra událostí můžete povolit funkci Capture kliknutím na tlačítko **Zapnout** na obrazovce portálu **Vytvořit centrum událostí**. Potom zadáte účet úložiště a kontejner kliknutím na **Azure Storage** v poli **Zprostředkovatel zachytávání**. Vzhledem k tomu, že Event Hubs Capture u úložiště využívá ověřování služba-služba, není potřeba zadávat připojovací řetězec úložiště. Výběr prostředku automaticky vybere URI prostředku pro váš účet úložiště. Pokud používáte Azure Resource Manager, musíte tento identifikátor URI explicitně zadat jako řetězec.

Výchozí časový interval je 5 minut. Minimální hodnota je 1, maximální hodnota je 15. Okno **Velikost** má rozsah 10 až 500 MB.

![][1]

## <a name="capture-data-to-an-azure-data-lake-store-account"></a>Zachytávání dat do účtu Azure Data Lake Store

Pro zachytávání dat do služby Azure Data Lake Store vytvoříte účet Data Lake Store a centrum událostí:

### <a name="create-an-azure-data-lake-store-account-and-folders"></a>Vytvoření účtu Azure Data Lake Store a složek

1. Vytvořte účet Data Lake Store podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md). 
2. Vytvořte pod tímto účtem složku podle pokynů v části [Vytváření složek v účtu Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#createfolder).
3. Na stránce účtu Data Lake Store klikněte na **Průzkumník dat**.
4. Klikněte na **Přístup**.
5. Klikněte na tlačítko **Přidat**.
6. Do pole **Hledat podle jména nebo e-mailu** zadejte **Microsoft.EventHubs** a pak tuto možnost vyberte. 
7. Zobrazí se karta **Oprávnění**. Nastavte oprávnění tak, jak je znázorněno na následujícím obrázku:

    ![][6]

8. Klikněte na **OK**.
9. Nyní v kořenové složce vytvořte složku tak, že přejdete do cílové složky a kliknete na název složky.
10. Klikněte na **Přístup**.
11. Klikněte na tlačítko **Přidat**.
12. Do pole **Hledat podle jména nebo e-mailu** zadejte **Microsoft.EventHubs** a pak tuto možnost vyberte.
13. Znovu se zobrazí karta **Oprávnění**. Nastavte oprávnění tak, jak je znázorněno na následujícím obrázku:

    ![][5]

### <a name="create-an-event-hub"></a>Vytvoření centra událostí

1. Nezapomeňte, že centrum událostí musí být ve stejném předplatném Azure jako služba Azure Data Lake Store, kterou jste právě vytvořili. Vytvořte centrum událostí kliknutím na tlačítko **Zapnout** v části **Capture** na stránce portálu **Vytvořit centrum událostí**. 
2. Na stránce portálu **Vytvořit centrum událostí** v poli **Zprostředkovatel zachytávání** vyberte **Azure Data Lake Store**.
3. V části **Vybrat službu Data Lake Store** zadejte účet Data Lake Store, který jste vytvořili dříve, a do pole **Cesta Data Lake** zadejte cestu ke složce s daty, kterou jste vytvořili.

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>Přidání nebo konfigurace funkce Capture v existujícím centru událostí

Funkci Capture můžete nakonfigurovat v existujících centrech událostí, která jsou v oborech názvů služby Event Hubs. Pokud chcete funkci Capture povolit v existujícím centru událostí nebo chcete změnit její nastavení, klikněte na příslušný obor názvů, aby se načetla obrazovka **Základy**, a potom klikněte na centrum událostí, pro které chcete povolit funkci Capture nebo změnit její nastavení. Nakonec na otevřené stránce klikněte na část **Vlastnosti** a pak upravte nastavení funkce Capture, jak je znázorněno na následujících obrázcích:

### <a name="azure-blob-storage"></a>Azure Blob Storage

![][2]

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png
[5]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture5.png
[6]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture6.png

## <a name="next-steps"></a>Další kroky

- Další informace o funkci Event Hubs Capture najdete v tématu [Přehled funkce Event Hubs Capture][capture-overview].
- Ke konfiguraci funkce Event Hubs Capture můžete také použít šablony Azure Resource Manageru. Další informace najdete v tématu věnovaném [povolení funkce Capture pomocí šablony Azure Resource Manageru](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).

[capture-overview]: event-hubs-capture-overview.md
