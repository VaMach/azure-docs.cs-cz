---
title: "Použití Azure IoT Hub zřizování služby zařízení pro zřizování zařízení, mezi zatížení vyrovnáváním centra IoT | Microsoft Docs"
description: "Zřizování automatické zařízení distribučních bodů mezi zatížení vyrovnáváním centra IoT na portálu Azure"
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
ms.openlocfilehash: 4842944cd0d980fb7e817165da23b9c3c4037e94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Zřízení zařízení napříč Vyrovnávání zatížení sítě centra IoT

Tento kurz ukazuje, jak zřídit zařízení ve více, Vyrovnávání zatížení sítě centra IoT pomocí služby pro zřizování zařízení (distribučních bodů). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Pomocí portálu Azure zřídit druhé zařízení do druhé služby IoT hub 
> * Přidání položky seznamu registrace do druhého
> * Nastavte zásady přidělení distribučních bodů na **i distribuce**
> * Odkaz novou službu IoT hub do distribučních bodů

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

V tomto kurzu vychází předchozí [zřízení zařízení k rozbočovači](tutorial-provision-device-to-hub.md) kurzu.

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>Pomocí portálu Azure zřídit druhé zařízení do druhé služby IoT hub

Postupujte podle kroků v [zřízení zařízení k rozbočovači](tutorial-provision-device-to-hub.md) kurzu ke zřízení druhé zařízení do jiné služby IoT hub.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>Přidání položky seznamu registrace do druhého

Registrace seznamu informuje distribučních bodů, jakou metodu ověření (metoda za potvrzení identity zařízení) používá se k zařízení. Dalším krokem je přidání položky seznamu registrace pro druhý zařízení. 

1. Na stránce pro vaše distribučních bodů, klikněte na tlačítko **spravovat registrace**. **Přidat položku seznamu registrace** se zobrazí stránka. 
2. V horní části stránky klikněte na tlačítko **přidat**.
2. Vyplňte pole a pak klikněte na tlačítko **Uložit**.

## <a name="set-the-dps-allocation-policy"></a>Nastavit zásady přidělení distribučních bodů

Přidělení zásady je nastavení distribučních bodů, které určuje, jak jsou přiřazeny zařízení do služby IoT hub. Existují tři podporované přidělení zásady: 

1. **Nejnižší latenci**: zřízení zařízení do služby IoT hub podle centrem s nejnižší latencí do zařízení.
2. **Rovnoměrně váha distribuce** (výchozí): propojené IoT hubs jsou stejně může mít zřízení k nim zařízení. Toto je výchozí nastavení. Pokud jsou zřizování zařízení, aby pouze jeden Centrum IoT, můžete ponechat toto nastavení. 
3. **Konfigurace statické prostřednictvím seznamu registrace**: specifikace požadované služby IoT hub v seznamu registrace přednost zásada přidělování úrovni distribučních bodů.

Postupujte podle těchto kroků můžete nastavte přidělení zásad:

1. Nastavení zásad přidělení, klikněte na stránce distribučních bodů na **spravovat zásady přidělení**.
2. Nastavení zásady přidělení pro **rovnoměrně váha distribuce**.
3. Klikněte na **Uložit**.

## <a name="link-the-new-iot-hub-to-dps"></a>Odkaz novou službu IoT hub do distribučních bodů

Propojte distribučních bodů a IoT hub, distribučních bodů můžete zaregistrovat zařízení na daném rozbočovači.

1. V **všechny prostředky** klikněte na tlačítko distribučních bodů, jste předtím vytvořili.
2. Na stránce distribučních bodů, klikněte na tlačítko **propojené IoT hubs**.
3. Klikněte na tlačítko **Přidat**.
4. V **odkaz Přidat do služby IoT hub** stránka, pomocí přepínačů, chcete-li určit, zda propojené služby IoT hub se nachází v aktuálním předplatném nebo v jiném předplatném. Zvolte název centra IoT z **služby IoT hub** pole.
5. Klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Pomocí portálu Azure zřídit druhé zařízení do druhé služby IoT hub 
> * Přidání položky seznamu registrace do druhého
> * Nastavte zásady přidělení distribučních bodů na **i distribuce**
> * Odkaz novou službu IoT hub do distribučních bodů

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
-->
