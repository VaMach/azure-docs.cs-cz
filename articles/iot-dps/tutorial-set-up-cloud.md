---
title: "Nastavení cloudu pro Azure IoT Hub zařízení zřizování služby portálu | Microsoft Docs"
description: "Zřizování automatické zařízení IoT Hub v portálu Azure"
services: iot-dps
keywords: 
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 088d127521ce89d3a82e30ad8797fe5746ae7e03
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>Konfigurace prostředků cloudu pro zřizování zařízení službou IoT Hub zařízení zřizování

Tento kurz ukazuje, jak nastavit cloudu pro automatické zařízení zřizování s využitím službu zřizování zařízení IoT Hub. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Pomocí portálu Azure k vytvoření služby zřizování zařízení IoT Hub a získat ID oboru
> * Vytvoření centra IoT
> * Propojit službu zřizování zařízení IoT hub
> * Nastavit zásady přidělení v rámci služby zřizování zařízení

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Vytvoření instance služby zřizování zařízení a získat ID oboru

Postupujte podle těchto kroků můžete vytvořit novou instanci služby zřizování zařízení.

1. V levém horním rohu portálu Azure klikněte na **nový**.
2. Do vyhledávacího pole zadejte **zřizování zařízení**. 
3. Klikněte na tlačítko **zařízení IoT Hub zřizování služby**.
4. Vyplňte **IoT Hub zařízení zřizování služby** formuláře s následujícími informacemi:
    
   | Nastavení       | Navrhovaná hodnota | Popis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Název** | Libovolný jedinečný název | -- | 
   | **Předplatné** | Vaše předplatné  | Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions). |
   | **Skupina prostředků** | myResourceGroup | Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Umístění** | Libovolné platné umístění | Informace o oblastech najdete v tématu [Oblasti služeb Azure](https://azure.microsoft.com/regions/). |   

   ![Zadejte základní informace o vaší distribučních bodů na portálu](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. Klikněte na možnost **Vytvořit**.
6. *ID oboru* slouží k identifikaci ID registrace a poskytuje záruku, že je jedinečné ID registrace. Chcete-li získat tuto hodnotu, klikněte na tlačítko **přehled** otevřete **Essentials** stránky pro službu zřizování zařízení. Kopírování **ID oboru** hodnotu do dočasného umístění pro pozdější použití.
7. Také si poznamenejte **koncový bod služby** hodnotu, nebo ho zkopírujte do dočasného umístění pro pozdější použití. 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili službu IoT Hub a máte název hostitele a připojovací řetězec služby IoT Hub, které potřebujete k dokončení kurzu.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Propojení služby zřizování zařízení do služby IoT hub

Dalším krokem je propojení služby zřizování zařízení a IoT hub, aby služba zřizování zařízení IoT Hub můžete zaregistrovat zařízení na daném rozbočovači. Službu můžete zřídit jenom zařízení do centra IoT, které byly spojeny s službu zřizování zařízení. Postupujte podle těchto kroků.

1. V **všechny prostředky** klikněte na instanci služby zřizování zařízení jste předtím vytvořili.
2. Na stránce služby zřizování zařízení, klikněte na tlačítko **propojené IoT hubs**.
3. Klikněte na tlačítko **Přidat**.
4. V **odkaz Přidat do služby IoT hub** stránka, pomocí přepínačů, chcete-li určit, zda propojené služby IoT hub se nachází v aktuálním předplatném nebo v jiném předplatném. Zvolte název centra IoT z **služby IoT hub** pole.
5. Klikněte na **Uložit**.

   ![Propojte název centra propojení s distribučních bodů na portálu](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Nastavit zásady přidělení v rámci služby zřizování zařízení

Přidělení zásady je nastavení služby zřizování zařízení IoT Hub, které určuje, jak zařízení se přiřadí do služby IoT hub. Existují tři podporované přidělení zásady: 

1. **Nejnižší latenci**: zřízení zařízení do služby IoT hub podle centrem s nejnižší latencí do zařízení.
2. **Rovnoměrně váha distribuce** (výchozí): propojené IoT hubs jsou stejně může mít zřízení k nim zařízení. Toto je výchozí nastavení. Pokud jsou zřizování zařízení, aby pouze jeden Centrum IoT, můžete ponechat toto nastavení. 
3. **Konfigurace statické prostřednictvím seznamu registrace**: specifikace požadované služby IoT hub v seznamu registrace přednost zásada přidělování zřizování zařízení úrovně služeb.

Nastavení zásad přidělení, v klikněte na stránce služby zřizování zařízení **spravovat zásady přidělení**. Ujistěte se, že je zásada přidělení nastavená **rovnoměrně váha distribuce** (výchozí). Pokud provedete změny, klikněte na tlačítko **Uložit** po dokončení.

![Spravovat přidělení zásad](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další kurzy v této kolekci stavějí tohoto kurzu. Pokud máte v úmyslu pokračovat v pro práci s několik rychlé spuštění nebo se kurzy, neprovádí vyčištění prostředky vytvořené v tomto kurzu. Pokud neplánujete, chcete-li pokračovat, pomocí následujících kroků můžete odstranit všechny prostředky, které jsou vytvořené v tomto kurzu na webu Azure portal.

1. Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **všechny prostředky** a pak vyberte instanci IoT Hub zařízení zřizování služby. V horní části **všechny prostředky** klikněte na tlačítko **odstranit**.  
2. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyberte své centrum IoT. V horní části **všechny prostředky** klikněte na tlačítko **odstranit**.
 
## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Pomocí portálu Azure k vytvoření služby zřizování zařízení IoT Hub a získat ID oboru
> * Vytvoření centra IoT
> * Propojit službu zřizování zařízení IoT hub
> * Nastavit zásady přidělení v rámci služby zřizování zařízení

Přechodu na v dalším kurzu se dozvíte, jak nastavit zařízení pro zřizování.

> [!div class="nextstepaction"]
> [Nastavit zařízení pro zřizování](tutorial-set-up-device.md)
