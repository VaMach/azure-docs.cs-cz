---
title: "Připojení webové aplikace ke službě Content Delivery Network | Dokumentace Microsoftu"
description: "Připojte webovou aplikaci ke službě Content Delivery Network, abyste doručovali statické soubory z hraničních uzlů."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: a12eab1f7bc4177f659771d58a58f749507c994c
ms.lasthandoff: 04/12/2017

---
# <a name="connect-a-web-app-to-a-content-delivery-network"></a>Připojení webové aplikace ke službě Content Delivery Network

V tomto kurzu vytvoříte profil Azure CDN a koncový bod Azure CDN, aby poskytovaly statické soubory z vaší webové aplikace pomocí lokalit POP v Azure CDN.

> [!TIP]
> Zkontrolujte aktuální seznam [lokalit POP v Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations).
>

## <a name="step-1---login-to-azure-portal"></a>Krok 1 – přihlášení k webu Azure Portal

Nejprve otevřete svůj oblíbený prohlížeč a přejděte na web [Azure Portal](https://portal.azure.com).

## <a name="step-2---create-a-cdn-profile"></a>Krok 2 – Vytvoření profilu CDN

Klikněte na tlačítko `+ New` v levém navigačním panelu a pak klikněte na **Web a mobilní zařízení**. V kategorii Web a mobilní zařízení vyberte **CDN**.

Zadejte **Název**, **Umístění**, **Skupina prostředků** a **Cenová úroveň** a pak klikněte na **Vytvořit**.

Otevřete centrum skupin prostředků z levého navigačního panelu a vyberte **myResourceGroup**. Ze seznamu prostředků vyberte **myCDNProfile**.

![azure-cdn-profile-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Krok 3 – Vytvoření koncového bodu CDN

Klikněte na `+ Endpoint` z příkazů vedle pole hledání, a tím otevřete okno pro vytvoření koncového bodu.

Zadejte **Název**, **Typ původu** a **Původní název hostitele** a pak klikněte na **Přidat**.

Vytvoří se koncový bod, a jakmile se vytvoří koncový bod Content Delivery Network, stav se zaktualizuje na **spuštění**.

![azure-cdn-endpoint-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---leveraging-cdn"></a>Krok 4 – Využívání CDN

Když je spuštěný koncový bod, ověříme, že je obsah k dispozici na serveru POP, a to přechodem na webovém serveru ke statickému souboru a následnou změnou názvu hostitele z `http://<app_name>.azurewebsites.net/path/to-static-file` na `http://<endpoint_name>.azureedge.net/path/to-static-file`.

![app-service-web-url-to-cdn-endpoint-url](media/app-service-web-tutorial-content-delivery-network/app-service-web-url-to-cdn-endpoint-url.png)

## <a name="next-steps"></a>Další kroky


