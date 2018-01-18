---
title: "Synchronizace obsahu ze složky cloudu do služby Azure App Service"
description: "Informace o nasazení aplikace do služby Azure App Service prostřednictvím synchronizace obsahu ze složky cloudu."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 8e132e4d4a65588d57e3cfb969e785f5a164206c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronizace obsahu ze složky cloudu do služby Azure App Service
V tomto kurzu se dozvíte, jak synchronizovat svůj obsah do [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) z oblíbených cloudových úložiště služby, jako je Dropbox a OneDrive. 

## <a name="overview"></a>Přehled nasazení obsahu synchronizace
Používá technologii nasazení synchronizace obsahu na vyžádání [modul nasazení Kudu](https://github.com/projectkudu/kudu/wiki) integrované se službou App Service. V [portál Azure](https://portal.azure.com), můžete určit složku v cloudovém úložišti, pracovat s kódu aplikace a obsah v této složce a kliknutím tlačítko Synchronizovat do služby App Service. Synchronizace obsahu využívá pro proces Kudu pro sestavení a nasazení. 

## <a name="contentsync"></a>Postup povolení nasazení obsahu synchronizace
Povolit synchronizaci obsahu z [portál Azure](https://portal.azure.com), postupujte takto:

1. Na stránce vaší aplikace na portálu Azure, klikněte na tlačítko **nastavení** > **zdroj nasazení**. Klikněte na tlačítko **zvolit zdroj**, pak vyberte **OneDrive** nebo **Dropbox** jako zdroj pro nasazení. 
   
    ![Synchronizace obsahu](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > Kvůli základním rozdílům v rozhraní API **Onedrivu pro firmy** v tuto chvíli není podporován. 
   > 
   > 
2. Dokončení autorizace pracovní postup povolení služby App Service pro přístup k cestě určené pro konkrétní předem definovaná pro OneDrive nebo Dropbox, kde všechny služby App Service obsahu se ukládají. Po autorizaci platformě App Service vám dává možnost vytvořit složku obsahu v určené cestě obsahu, nebo vybrat existující složku obsahu v této cestě určené obsahu. Určené cesty obsahu v rámci vaší cloudové úložiště účty používané pro synchronizaci služby App Service jsou následující:  
   
   * **OneDrive**:`Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. Po počáteční synchronizaci obsahu lze inicializovat synchronizace obsahu na vyžádání z portálu Azure. Historie nasazení je k dispozici na **nasazení** stránky.
   
    ![Historie nasazení](./media/app-service-deploy-content-sync/onedrive_sync.png)

Další informace o nasazení Dropbox je k dispozici v části [nasazení ze Dropbox] (https://azure.microsoft.com/en-in/blog/new-deploy-to-windows-azure-web-sites-from-dropbox/).

