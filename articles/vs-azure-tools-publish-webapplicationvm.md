---
title: "Publikování WebApplicationVM | Microsoft Docs"
description: "Zjistěte, jak nasadit webovou aplikaci pro virtuální počítač. Tento skript vytvoří požadované prostředky ve vašem předplatném Azure, pokud ještě neexistují."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 2738fc1dff50a177a227ae2c7719bd9a192d82ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publikování-WebApplicationVM (skript prostředí Windows PowerShell)
Nasadí webovou aplikaci pro virtuální počítač. Pokud ještě neexistují, vytvoří skript ve vašem předplatném Azure požadované prostředky.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfigurace
Cesta k souboru konfigurace JSON, který popisuje podrobnosti o nasazení.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |Hodnota TRUE |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="subscriptionname"></a>Název_předplatného
Název předplatného Azure, ve kterém chcete vytvořit virtuální počítač.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |Použije první předplatné v souboru předplatného |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="webdeploypackage"></a>WebDeployPackage
Cesta k balíčku pro nasazení webu k publikování do virtuálního počítače. Tento balíček můžete vytvořit pomocí Průvodce Publikovat Web v sadě Visual Studio. V tématu [postupy: vytvoření balíčku pro nasazení webu v sadě Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="allowuntrusted"></a>AllowUntrusted
V případě hodnoty true povolte používání certifikátů, které nejsou podepsána důvěryhodnou kořenovou autoritou.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |False |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="vmpassword"></a>VMPassword
Pověření pro účet virtuálního počítače. Příklad: - VMPassword @{Name = "admin"; Heslo = "password"}

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
Přihlašovací údaje pro databázi SQL v Azure. Příklad: - DatabaseServerPassword @{Name = "admin"; Heslo = "password"}

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
V případě hodnoty true tiskových zpráv ze skriptu do výstupního datového proudu.

| Aliasy | None |
| --- | --- |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |False |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="remarks"></a>Poznámky
Podrobné informace o tom, jak vytvořit pomocí skriptu najdete v prostředí vývoje a testování [pomocí skriptů prostředí PowerShell systému Windows k publikování pro vývoj a testovací prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).

Konfigurační soubor JSON Určuje podrobnosti co je k nasazení. Obsahuje informace, které jste zadali při vytváření projektu, například název, skupinu vztahů, image virtuálního pevného disku a velikost virtuálního počítače. Také zahrnuje koncových bodů na virtuálním počítači, databází, které chcete zřídit, pokud existuje a webové parametrů nasazení. Následující kód ukazuje konfigurační soubor JSON příklad:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Můžete upravit konfigurační soubor JSON změnit, co je zřízený. Virtuální počítač a cloudové služby jsou povinné, ale části databáze je volitelný.

