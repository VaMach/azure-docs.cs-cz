---
title: "Nasazení do Azure IoT hraniční zařízení model Azure Machine Learning | Microsoft Docs"
description: "Tento dokument popisuje, jak se dá modely Azure Machine Learning nasadit na zařízení Azure IoT okraj."
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: garyericson, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/10/2017
ms.openlocfilehash: 924766aee7486f0cf5006dd89d5d77b83753833b
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-and-azure-machine-learning-model-to-an-azure-iot-edge-device"></a>Nasazení a model Azure Machine Learning k Azure IoT hraniční zařízení

Všechny modely Azure Machine Learning kontejnerizované jako na základě Docker webové služby, můžete spustit také na Azure IoT hraniční zařízení. Další skripty a pokyny najdete v [AI nástrojů pro Azure IoT Edge](http://aka.ms/AI-toolkit).

## <a name="operationalize-the-model"></a>Zprovoznit model
Váš model zprovoznit podle pokynů v [nasazení služby webové Model Azure Machine Learning Management](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy) vytvoření bitové kopie Docker se svým modelem.

## <a name="deploy-to-azure-iot-edge"></a>Nasazení do Azure IoT Edge
Azure IoT Edge přesune cloudu analýzy a vlastní obchodní logiku do zařízení. Všechny modely Machine Learning můžete spustit na IoT hraniční zařízení. V dokumentaci k nastavení IoT hraniční zařízení a vytváření nasazení naleznete na [aka.ms/azure-iot-edge-doc](https://aka.ms/azure-iot-edge-doc).

Následují další skutečnosti, které je Poznámka:.

### <a name="add-registry-credentials-to-the-edge-runtime-on-your-edge-device"></a>Přidejte pověření registru do hraniční runtime v hraniční zařízení
Na počítači, kde spouštíte IoT Edge přidáte přihlašovací údaje v registru, aby modul runtime může mít přístup ke kontejneru pro vyžádání obsahu.

Pro systém Windows spusťte následující příkaz:
```cmd/sh
iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```
Pro Linux spusťte následující příkaz:
```cmd/sh
sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password>
```

### <a name="find-the-machine-learning-container-image-location"></a>Vyhledejte umístění bitové kopie kontejneru Machine Learning
Je třeba umístění image kontejneru Machine Learning. Vyhledání kontejneru umístění obrázku:

1. Přihlaste se [portál Azure](http://portal.azure.com/).
2. V **registru kontejner Azure**, vyberte registru, které chcete prověřit.
3. V registru, klikněte na tlačítko **úložiště** zobrazíte seznam všech úložiště a jejich obrázků.













