---
title: "Pomocí portálu Azure ke konfiguraci nahrávání souborů | Microsoft Docs"
description: "Postup konfigurace služby IoT hub, chcete-li povolit nahrávání souborů z připojených zařízení pomocí portálu Azure. Obsahuje informace o konfiguraci cílového účtu úložiště Azure."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: 149dd84d7d8f4ff9cd30f9fc649ced3cb364cfb7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Konfigurace centra IoT nahrávání souborů pomocí portálu Azure

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Nahrávání souborů

Použít [souboru nahrávání funkce IoT hub][lnk-upload], je třeba nejprve přidružit účet služby Azure Storage pomocí centra. Vyberte **nahrávání souborů** zobrazíte seznam vlastností nahrávání souboru pro službu IoT hub, která je upravována.

![Zobrazení IoT Hub soubor nahrát nastavení na portálu][13]

**Kontejner úložiště**: pomocí portálu Azure vyberte kontejner objektů blob v účtu Azure Storage v aktuálním předplatném Azure přidružit služby IoT Hub. Pokud potřeby můžete vytvořit účet úložiště Azure na **účty úložiště** okno a objektů blob kontejneru na **kontejnery** okno. IoT Hub automaticky vygeneruje identifikátory URI SAS s oprávněními k zápisu do tohoto kontejneru objektů blob pro zařízení se má použít při odesílají soubory.

![Zobrazit kontejnery úložiště pro nahrávání souborů na portálu][14]

**Přijímat oznámení pro nahraném soubory**: povolení nebo zakázání oznámení o odeslání souboru přes přepínač.

**Hodnota TTL SAS**: Toto nastavení je time-to-live identifikátorů URI SAS, vrácený do zařízení IoT Hub. Ve výchozím nastavení má jednu hodinu, ale můžete přizpůsobit tak, aby ostatní hodnoty pomocí posuvníku.

**Upozornění na nastavení výchozí hodnota TTL souboru**: time-to-live souboru odeslat oznámení, než vyprší jejich platnost. Ve výchozím nastavení má jeden den, ale můžete přizpůsobit tak, aby ostatní hodnoty pomocí posuvníku.

**Soubor doručení maximální počet oznámení**: počet, kolikrát se Centrum IoT pokusí doručit soubor odeslat oznámení. Ve výchozím nastavení má 10, ale můžete přizpůsobit tak, aby ostatní hodnoty pomocí posuvníku.

![Konfigurace služby IoT Hub nahrávání souborů na portálu][15]

## <a name="next-steps"></a>Další kroky

Další informace o možnostech nahrávání souboru Centrum IoT najdete v tématu [nahrání souborů ze zařízení] [ lnk-upload] v příručce pro vývojáře IoT Hub.

Další informace o správě Azure IoT Hub na následujících odkazech:

* [Hromadné spravovat zařízení IoT][lnk-bulk]
* [Metriky služby IoT Hub][lnk-metrics]
* [Monitorování operací][lnk-monitor]

Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s hranou IoT][lnk-iotedge]
* [Zabezpečení řešení IoT od základů nahoru][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
