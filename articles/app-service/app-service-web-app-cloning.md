---
title: "Webové aplikace klonování pomocí prostředí PowerShell"
description: "Zjistěte, jak vytvořit kopii vaší webové aplikace do nové webové aplikace pomocí prostředí PowerShell."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: dc252903571857b5fc89d1d9a2c63cd6b44e9021
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Aplikační služby Azure klonování pomocí prostředí PowerShell
Verze služby Microsoft Azure PowerShell verze 1.1.0 byla přidána nová možnost na nový AzureRMWebApp, kterého by uživatel možnost klonování stávající webovou aplikaci do nově vytvořené aplikace v jiné oblasti nebo ve stejné oblasti. To vám umožní zákazníkům snadno a rychle nasadit počet aplikací v různých oblastech.

Klonování aplikace je momentálně podporovaná jenom pro plány služby app úrovně premium. Nová funkce používá stejná omezení jako funkci zálohování webové aplikace, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Klonování existující aplikace
Scénář: Stávající webovou aplikaci v oblasti jihu USA, uživatel chtěli klonovat obsah do nové webové aplikace v oblasti severní jihu USA. To můžete udělat pomocí Azure Resource Manager verzi rutiny prostředí PowerShell k vytvoření nové webové aplikace s parametrem - SourceWebApp.

Znalost, název skupiny prostředků, který obsahuje ke zdrojové aplikaci pro webové, jsme se získat informace o zdrojové webové aplikaci (v takovém případě s názvem zdrojového webapp) použijte následující příkaz prostředí PowerShell:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Pokud chcete vytvořit, nový plán služby App Service, můžeme použít příkaz New-AzureRmAppServicePlan jako v následujícím příkladu

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Pomocí příkazu New-AzureRmWebApp, můžeme vytvořit novou webovou aplikaci v oblasti severní centrální nám a tie existující úrovně premium plán služby App Service, kromě toho můžeme použít stejné skupině prostředků jako zdroj webovou aplikaci nebo definovat novou skupinu prostředků, následující ukazuje:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Klonování existující webovou aplikaci, včetně všech přidružených nasazení přihrádek, uživatel bude muset použít parametr IncludeSourceWebAppSlots, následující příkaz prostředí PowerShell demonstruje použití tohoto parametru pomocí příkazu New-AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

Klonovat stávající webovou aplikaci v rámci stejné oblasti, bude uživatel muset vytvořit novou skupinu prostředků a nové služby app service plánu ve stejné oblasti a potom pomocí následujícího příkazu prostředí PowerShell klonovat webové aplikace

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonování stávající aplikace pro služby App Service Environment
Scénář: Stávající webovou aplikaci v oblasti jihu USA, uživatel by chtěli klonovat obsah do nové webové aplikace do existující aplikace služby prostředí (App Service Environment).

Znalost, název skupiny prostředků, který obsahuje ke zdrojové aplikaci pro webové, jsme se získat informace o zdrojové webové aplikaci (v takovém případě s názvem zdrojového webapp) použijte následující příkaz prostředí PowerShell:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Znalost App Service Environment název a název skupiny prostředků, které patří App Service Environment, uživatel může použít příkaz New-AzureRmWebApp k vytvoření nové webové aplikace v existující App Service Environment, následující ukazuje:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Parametr umístění není požadována z důvodu starší verze, ale v případě vytváření aplikace v App Service Environment se budou ignorovat. 

## <a name="cloning-an-existing-app-slot"></a>Klonování existujícího slotu aplikace
Scénář: Uživatel chtěli klonovat existující Slot webové aplikace do buď nové webové aplikace nebo nový slot webové aplikace. Nové webové aplikace může být ve stejné oblasti jako původní slot webové aplikace nebo v jiné oblasti.

Znalost, název skupiny prostředků, který obsahuje ke zdrojové aplikaci pro webové, jsme zdrojový slot webové aplikace na informace, které (v takovém případě s názvem zdrojového webappslot) a vázané na webovou aplikaci zdroj webapp použijte následující příkaz prostředí PowerShell:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Následující část ukazuje vytvoření kopie zdrojové webové aplikace do nové webové aplikace:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Konfigurace Traffic Manager během klonování aplikace
Vytvoření více oblast webové aplikace a konfigurace Azure Traffic Manager pro směrování provozu pro tyto webové aplikace, je scénáři n důležité zajistit, že aplikace zákazníků vysoce dostupný, při klonování stávající webovou aplikaci máte možnost připojit i webové aplikace na nový profil správce provozu nebo stávající - Všimněte si, že verze nástroje Traffic Manager je podporována pouze Azure Resource Manager.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Vytvoření nového profilu Traffic Manageru během klonování aplikace
Scénář: Uživatel chtěli klonovat webové aplikace do jiné oblasti, při konfiguraci profil správce provozu Azure Resource Manager, které zahrnují oba webové aplikace. Následující část ukazuje vytvoření kopie zdrojové webové aplikace do nové webové aplikace při konfiguraci nového profilu Traffic Manageru:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Přidání nových klonovat webovou aplikaci do existujícího profilu Správce provozu
Scénář: Uživatel již mít profil správce provozu Azure Resource Manager, kterou chcete přidat i webové aplikace jako koncové body. K tomu, musíme nejprve sestavte stávající id profilu Správce provozu, budeme potřebovat id odběru, název skupiny prostředků a stávající název profilu Správce provozu.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Po s id Manager provoz, následující ukazuje vytvoření kopie zdrojové webové aplikace do nové webové aplikace při jejich přidáním do existující profil Traffic Manageru:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Aktuální omezení
Tato funkce je aktuálně ve verzi preview, pracujeme na přidání nové funkce v čase, v následujícím seznamu jsou známé omezení v aktuální verzi klonování aplikace:

* Nastavení automatického škálování nejsou klonovat.
* Nastavení plánu zálohování nejsou klonovat.
* Nastavení sítě VNET nejsou klonovat.
* Přehled aplikace nejsou automaticky na nastavit cílové webové aplikace
* Nejsou klonovat snadno nastavení ověřování
* Kudu rozšíření nejsou klonovat.
* TiP pravidla nejsou klonovat.
* Nejsou klonovat databáze obsahu

### <a name="references"></a>Odkazy
* [Klonování webové aplikace](app-service-web-app-cloning.md)
* [Zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md)
* [Azure Resource Manager podpora pro Azure Traffic Manager Preview](../traffic-manager/traffic-manager-powershell-arm.md)
* [Úvod do prostředí App Service](environment/intro.md)
* [Použití Azure PowerShellu s Azure Resource Managerem](../azure-resource-manager/powershell-azure-resource-manager.md)

