---
title: "Nainstalovat Azure IoT Edge na jádro IoT | Microsoft Docs"
description: "Nainstalovat modul runtime Azure IoT Edge na jádro IoT Windows zařízení"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 11/17/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: d3ff260b4ac238ce7aaa2a63538dede7bd21a19c
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Nainstalovat modul runtime IoT Edge na jádro IoT Windows – náhled

Modulu Runtime Azure IoT Edge můžete spustit i na malá velikost počítače jeden Tabule (SBC) zařízení, které jsou velmi běžně se vyskytujícím v odvětví IoT. Tento článek vás provede zřizování modul runtime na [kambala velká MinnowBoard] [ lnk-minnow] vývoj Tabule systémem jádro IoT Windows.

## <a name="install-the-runtime"></a>Nainstalovat modul runtime

1. Nainstalujte [řídicího panelu Windows 10 IoT Core] [ lnk-core] v hostitelském systému.
1. Postupujte podle kroků v [nastavit vaše zařízení] [ lnk-board] ke konfiguraci vaší karty MinnowBoard kambala velká nebo maximální sestavení 16299 Image. 
1. Zařízení, povolit pak [přihlášení vzdáleně pomocí prostředí PowerShell][lnk-powershell].
1. V konzole PowerShell instalaci modulu runtime kontejneru: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-17.06.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >Tento modul runtime kontejneru je ze serveru sestavení projektu Moby a je určená jenom pro účely hodnocení. Ho má není otestovat, schválené nebo nepodporuje Docker.

1. Nainstalovat modul runtime IoT okraj a ověřte konfiguraci:

   ```powershell
   Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
   ```

   Tento skript obsahuje následující informace: 
   * Python 3.6
   * Skript IoT okraj ovládacího prvku (iotedgectl.exe)

Může se zobrazit informační výstup z nástroje iotedgectl.exe zeleně v okně vzdáleného prostředí PowerShell. To nutně neznamená chyby. 

## <a name="next-steps"></a>Další kroky

Teď, když máte zařízení se systémem runtime IoT Edge, zjistěte, jak [nasadit a monitorovat moduly IoT Edge ve velkém měřítku][lnk-deploy].

<!--Links-->
[lnk-minnow]: https://minnowboard.org/ 
[lnk-core]: https://docs.microsoft.com/windows/iot-core/connect-your-device/iotdashboard
[lnk-board]: https://developer.microsoft.com/windows/iot/Docs/GetStarted/mbm/sdcard/stable/getstartedstep2
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
