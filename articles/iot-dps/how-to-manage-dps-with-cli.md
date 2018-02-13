---
title: "Použití Azure CLI 2.0 a rozšíření IoT ke správě zařízení zřizování služby | Microsoft Docs"
description: "Další informace o použití Azure CLI 2.0 a rozšíření IoT ke správě službami zřizování zařízení"
services: iot-dps
keywords: 
author: chrissie926
ms.author: menchi
ms.date: 01/17/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 674245f1e284e7308474fed0f6c53b350ec1c819
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-azure-cli-20-and-the-iot-extension-to-manage-device-provisioning-services"></a>Použití Azure CLI 2.0 a rozšíření IoT ke správě zařízení zřizování služeb

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) je open source pro různé platformy nástroj příkazového řádku pro správu prostředků Azure, jako je například IoT okraj. Azure CLI 2.0 je k dispozici v systému Windows, Linux a systému MacOS. Azure CLI 2.0 umožňuje spravovat prostředky Azure IoT Hub, instance služby zřizování zařízení a propojené rozbočovače mimo pole.

Rozšíření IoT vylepšuje 2.0 rozhraní příkazového řádku Azure s funkcemi, jako je Správa zařízení a úplných možností IoT okraj.

V tomto kurzu nejprve dokončit postup instalace 2.0 rozhraní příkazového řádku Azure a IoT rozšíření. Pak zjistíte, jak spouštět příkazy příkazového řádku k provedení operací základní zařízení zřizování služby. 

## <a name="installation"></a>Instalace 

### <a name="step-1---install-python"></a>Krok 1 – instalace Python

[Python 2.7 x nebo Python 3.x](https://www.python.org/downloads/) je vyžadován.

### <a name="step-2---install-azure-cli-20"></a>Krok 2 – nainstalovat rozhraní příkazového řádku Azure 2.0

Postupujte podle [pokyny k instalaci](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) nastavení Azure CLI 2.0 ve vašem prostředí. Minimálně musí být vaší verzi 2.0 rozhraní příkazového řádku Azure 2.0.24 nebo vyšší. Použití `az –version` k ověření. Tato verze podporuje az rozšíření příkazy a zavádí rozhraní Knack příkaz. Jeden způsob, jak nainstalovat v systému Windows je ke stažení a instalaci [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Krok 3: instalace IoT rozšíření

[V souboru readme rozšíření IoT](https://github.com/Azure/azure-iot-cli-extension) popisuje několik způsobů, jak nainstalovat rozšíření. Nejjednodušší způsob, jak se má spustit `az extension add --name azure-cli-iot-ext`. Po instalaci, můžete použít `az extension list` ověření aktuálně nainstalovaná rozšíření nebo `az extension show --name azure-cli-iot-ext` zobrazíte podrobnosti o rozšíření IoT. Chcete-li odebrat rozšíření, můžete použít `az extension remove --name azure-cli-iot-ext`.


## <a name="basic-device-provisioning-service-operations"></a>Základní zařízení zřizování operací služby
Tento příklad ukazuje, jak přihlásit k účtu Azure, vytvořte skupinu prostředků Azure (kontejner, který obsahuje související prostředky pro řešení s Azure), vytvoření služby IoT Hub, vytvoření zřizování službu v zařízení, zobrazí seznam stávajících zařízení zřizování služby a vytvoření propojené IoT hub pomocí rozhraní příkazového řádku. 

Kroky instalace popsané dříve než začnete. Pokud nemáte účet Azure, ale můžete [vytvořit bezplatný účet](https://azure.microsoft.com/free/?v=17.39a) ještě dnes. 


### <a name="1-log-in-to-the-azure-account"></a>1. Přihlaste se k účtu Azure
  
    az login

![přihlášení][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Vytvořte skupinu prostředků IoTHubBlogDemo v eastus

    az group create -l eastus -n IoTHubBlogDemo

![Vytvoření skupiny prostředků][2]


### <a name="3-create-two-device-provisioning-services"></a>3. Vytvořte dva službami zřizování zařízení

    az iot dps create --resource-group IoTHubBlogDemo --name demodps

![Vytvoření distribučních bodů][3]

    az iot dps create --resource-group IoTHubBlogDemo --name demodps2

### <a name="4-list-all-the-existing-device-provisioning-services-under-this-resource-group"></a>4. Zobrazí seznam všech stávajících zařízení zřizování služeb v rámci následující skupiny prostředků

    az iot dps list --resource-group IoTHubBlogDemo

![Seznam distribučních bodů][4]


### <a name="5-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>5. Vytvoření IoT Hub blogDemoHub ve skupině nově vytvořený prostředek

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![Vytvoření centra IoT][5]

### <a name="6-link-one-existing-iot-hub-to-a-device-provisioning-service"></a>6. Propojit jeden stávající služby IoT Hub zařízení zřizování služby

    az iot dps linked-hub create --resource-group IoTHubBlogDemo --dps-name demodps --connection-string <connection string> -l westus

![Odkaz rozbočovače][5]

<!-- Images -->
[1]: ./media/how-to-manage-dps-with-cli/login.jpg
[2]: ./media/how-to-manage-dps-with-cli/create-resource-group.jpg
[3]: ./media/how-to-manage-dps-with-cli/create-dps.jpg
[4]: ./media/how-to-manage-dps-with-cli/list-dps.jpg
[5]: ./media/how-to-manage-dps-with-cli/create-hub.jpg
[6]: ./media/how-to-manage-dps-with-cli/link-hub.jpg


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Registrace zařízení
> * Spuštění zařízení
> * Ověřte, zda že je zařízení zaregistrované

Přechodu na v dalším kurzu se dozvíte, jak zřídit víc zařízení přes centra Vyrovnávání zatížení sítě. 

> [!div class="nextstepaction"]
> [Zřízení zařízení napříč Vyrovnávání zatížení sítě centra IoT](./tutorial-provision-multiple-hubs.md)
