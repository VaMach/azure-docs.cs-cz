---
title: "Publikování-WebApplicationWebSite (skript prostředí Windows PowerShell) | Microsoft Docs"
description: "Zjistěte, jak publikovat webového projektu do webové stránky Azure. Tento skript vytvoří požadované prostředky ve vašem předplatném Azure, pokud ještě neexistují."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 07d21b7ce6cd8aee1cff704d316e7a2ca8c00437
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publikování-WebApplicationWebSite (skript prostředí Windows PowerShell)
## <a name="syntax"></a>Syntaxe
Publikuje webového projektu na web Azure. Pokud ještě neexistují, vytvoří skript ve vašem předplatném Azure požadované prostředky.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfigurace
Cesta k souboru konfigurace JSON, který popisuje podrobnosti o nasazení.

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Vyžaduje? |Hodnota TRUE |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="subscriptionname"></a>Název_předplatného
Název předplatného Azure, který chcete vytvořit web v.

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="webdeploypackage"></a>WebDeployPackage
Cesta k balíčku pro nasazení webu k publikování na web. Tento balíček můžete vytvořit pomocí Průvodce Publikovat Web v sadě Visual Studio. Další informace najdete v tématu [Začínáme s Azure Cloud Services a technologií ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
Uživatelské jméno a heslo pro databázi SQL v Azure.

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
V případě hodnoty true tiskových zpráv ze skriptu do výstupního datového proudu.

| Parametr | Výchozí hodnota |
| --- | --- |
| Aliasy |None |
| Vyžaduje? |False |
| Pozice |S názvem |
| Výchozí hodnota |False |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="remarks"></a>Poznámky
Podrobné informace o tom, jak vytvořit pomocí skriptu najdete v prostředí vývoje a testování [pomocí skriptů prostředí PowerShell systému Windows k publikování pro vývoj a testovací prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).

Konfigurační soubor JSON Určuje podrobnosti co je k nasazení. Obsahuje informace, kterou jste zadali při vytváření projektu, například název a uživatelské jméno pro web. Zahrnuje také databázi a zřizovat, pokud existuje. Následující kód ukazuje konfigurační soubor JSON příklad:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Můžete upravit konfigurační soubor JSON změnit, co je nasazen. Části webu není požadována, ale část databáze je volitelný.

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [Publish-WebApplicationVM (skript prostředí Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)

