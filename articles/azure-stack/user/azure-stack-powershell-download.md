---
title: "Stažení nástroje Azure zásobníku z Githubu | Microsoft Docs"
description: "Zjistěte, jak si chcete stáhnout nástroje potřebné pro práci s Azure zásobníku."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: sngun
ms.openlocfilehash: 69ca030d1b7601df424fa9446e1d194a3f6bd50a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="download-azure-stack-tools-from-github"></a>Stažení nástroje Azure zásobníku z Githubu

AzureStack nástroje je úložiště GitHub, který je hostitelem moduly Powershellu, které můžete použít ke správě a nasazení prostředků do protokolů Azure. Můžete stáhnout a použít tyto moduly Powershellu Development Kit zásobník Azure nebo na externí klienta se systémem windows, pokud máte v úmyslu navázat připojení VPN. Chcete-li získat tyto nástroje, naklonujte úložiště GitHub nebo AzureStack nástroje složka pro stahování. 

Klonovat úložiště, stáhněte si [Git](https://git-scm.com/download/win) pro systém Windows, otevřete okno příkazového řádku a spusťte následující skript:

```PowerShell
# Change directory to the root directory 
cd \

# clone the repository
git clone https://github.com/Azure/AzureStack-Tools.git --recursive

# Change to the tools directory
cd AzureStack-Tools
```

Pokud chcete stáhnout nástroje pro složku, spusťte následující skript:

```PowerShell
# Change directory to the root directory 
cd \

# Download the tools archive
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files
expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory
cd AzureStack-Tools-master

```

## <a name="functionalities-provided-by-the-modules"></a>Funkce poskytované moduly

Úložiště AzureStack nástroje obsahuje moduly Powershellu, které podporují následující funkce pro Azure zásobníku:  

| Funkce | Popis | Tento modul, který můžete použít? |
| --- | --- | --- |
| [Možností cloudu](azure-stack-validate-templates.md) | Tento modul slouží k získání možností cloudu cloudu. Například můžete získat možností cloudu například verze rozhraní API, prostředky Azure Resource Manager, rozšíření virtuálního počítače atd. pro zásobník Azure a Azure cloudy pomocí tohoto modulu. | Cloud správců a uživatelů. |
| [Správce prostředků zásady pro Azure zásobníku](azure-stack-policy-module.md) | Tento modul slouží ke konfiguraci předplatné Azure nebo skupinu prostředků Azure pomocí stejné verze a služba dostupnosti jako zásobník Azure. | Cloud správců a uživatelů |
| [Připojení k Azure zásobníku](azure-stack-connect-azure-stack.md) | Tento modul slouží k připojení k instanci zásobník Azure pomocí prostředí PowerShell a ke konfiguraci připojení VPN do protokolů Azure. | Cloud správců a uživatelů |
| [Ověření šablony](azure-stack-validate-templates.md) | Tento modul slouží k ověření, pokud existující nebo nové šablony lze nasadit do protokolů Azure. | Cloud správců a uživatelů |


## <a name="next-steps"></a>Další kroky
* [Konfigurace prostředí PowerShell Azure zásobník uživatele](azure-stack-powershell-configure-user.md)   
* [Připojení k Azure zásobníku Development Kit přes síť VPN](azure-stack-connect-azure-stack.md)  
