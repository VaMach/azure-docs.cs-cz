---
title: "Nasaďte kontejner ASP.NET Core Linux Docker vzdáleného hostitele Docker | Microsoft Docs"
description: "Další informace o použití nástroje sady Visual Studio pro Docker nasazení webové aplikace ASP.NET Core na kontejner Docker spuštěna na virtuálním počítači Azure Docker hostiteli systému Linux"
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 4a87ee69f23779bf4f6f5db40bc05edbcfc7668d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Nasaďte kontejner ASP.NET vzdáleného hostitele Docker
## <a name="overview"></a>Přehled
Docker je modul lightweight kontejneru, podobně jako v některých způsobech virtuální počítač, který můžete použít k hostování aplikací a služeb.
Tento kurz vás provede pomocí [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/articles/core/docker/visual-studio-tools-for-docker) rozšíření pro nasazení aplikace ASP.NET Core do hostitelů Docker v Azure pomocí prostředí PowerShell.

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto kurzu se vyžaduje následující text:

* Vytvoření virtuálního počítače Azure Docker hostitele, jak je popsáno v [použití docker počítače s Azure](virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Nainstalujte nejnovější verzi [Visual Studio](https://www.visualstudio.com/downloads/)
* Stažení [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
* Nainstalujte [Docker pro Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Vytvoření webové aplikace ASP.NET Core
Následující postup vás provede vytvořením základní aplikace ASP.NET Core, který se použije v tomto kurzu.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Přidání podpory Docker
[!INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Pomocí skriptu prostředí PowerShell DockerTask.ps1
1. Otevřete příkazovém řádku prostředí PowerShell do kořenového adresáře projektu. 
   
   ```
   PS C:\Src\WebApplication1>
   ```
2. Ověření vzdáleném hostiteli spuštěn. Měli byste vidět stav = spuštěná 
   
   ```
   docker-machine ls
   NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
   MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
   ```
   
3. Sestavení aplikace pomocí parametru - sestavení
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
   ```  

   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
   > ```  
   > 
   > 
4. Spuštění aplikace, pomocí parametru - spustit
   
   ```
   PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
   ```
   
   > ```
   > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
   > ```
   > 
   > 
   
   Po dokončení docker, měli byste vidět výsledky podobné následujícím:
   
   ![Zobrazit aplikaci][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
