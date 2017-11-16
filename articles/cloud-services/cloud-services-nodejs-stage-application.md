---
title: "Příprava nasazení cloudové služby (Node.js) | Microsoft Docs"
description: "Zjistěte, jak nasadit aplikaci Azure k testovacím prostředí, a potom nasazení v produkčním prostředí pomocí prohození virtuální IP (VIP)."
services: cloud-services
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: 
ms.assetid: d65d26a6-b424-49cd-a88c-7ef46bb112a8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: 307741d0792b34332d98bfa4f2d62c9fd1cf07a1
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="staging-an-application-in-azure"></a>Pracovní aplikace v Azure
Zabalené aplikace lze nasadit do fázovacího prostředí v Azure má být testována, před přesunutím do produkčního prostředí, ve kterém je aplikace přístupné z Internetu. Je pracovní prostředí je úplně stejně jako provozní prostředí, ale jenom přístup k dvoufázové instalace aplikace pomocí zkomolené adresy URL, který je generovaný Azure. Po ověření, že aplikace funguje správně, dá se nasadit do produkčního prostředí provedením prohození virtuální IP (VIP).

> [!NOTE]
> Kroky v tomto článku se uplatní jenom na uzlu aplikace hostované jako cloudová služba Azure.
> 
> 

## <a name="step-1-stage-an-application"></a>Krok 1: Příprava aplikace
Tato úloha vysvětluje postup dvoufázové instalace aplikace pomocí **Microsoft Azure PowerShell**.

1. Při publikování služby, stačí předat **-slotu** parametru **Publish-AzureServiceProject** rutiny.
   
   ```powershell
   Publish-AzureServiceProject -Slot staging
   ```
2. Přihlaste se na [portál Azure classic] a vyberte **cloudové služby**. Po cloudu vytvoření služby a **pracovní** stav sloupce byl aktualizovaný, aby **systémem**, klikněte na název služby.
   
   ![portál zobrazení spuštěnou službu][cloud-service]
3. Vyberte **řídicí panel**a potom vyberte **pracovní**.
   
   ![řídicí panel cloud service][cloud-service-dashboard]
4. Poznamenejte si hodnotu **adresa URL webu** položka vpravo. Název DNS je zkomolené interní ID, které generuje Azure.
   
    ![Adresa url webu][cloud-service-staging-url]

Nyní můžete ověřit, zda aplikace pracuje správně v testovacím prostředí pomocí pracovní adresa URL webu.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Krok 2: Upgrade aplikace v produkčním prostředí pomocí Prohazuje virtuální IP adresy
Po ověření upgradovanou verzi aplikace v testovacím prostředí, můžete rychle zpřístupnit jej v produkčním prostředí pomocí vzájemná záměna virtuálních IP adres (VIP), pracovní a provozní prostředí.

> [!NOTE]
> Tento krok předpokládá, že jste už nasadit aplikace do produkčního prostředí a připraví upgradovanou verzi aplikace.
> 
> 

1. Přihlaste se [portál Azure classic], klikněte na tlačítko **cloudové služby** a pak vyberte název služby.
2. Z **řídicí panel**, vyberte **pracovní**a potom klikněte na **odkládacího souboru** v dolní části stránky. Otevře se dialogové okno prohození virtuálních IP adres.
   
   ![Dialogové okno prohození virtuální IP adresy][vip-swap-dialog]
3. Zkontrolujte informace a pak klikněte na tlačítko **OK**. Dvě nasazení začněte aktualizace jako pracovní nasazení se přepne do produkčního prostředí a produkční nasazení se přepne do přípravy.

Úspěšně jste připravený nasazení a upgradovat produkční nasazení přes vzájemná záměna virtuální IP adresy s nasazením v pracovní.

## <a name="additional-resources"></a>Další zdroje
* [Vzájemná záměna virtuální IP adresy v Azure nasazení upgradu služby do produkčního prostředí]

[portál Azure classic]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Vzájemná záměna virtuální IP adresy v Azure nasazení upgradu služby do produkčního prostředí]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
