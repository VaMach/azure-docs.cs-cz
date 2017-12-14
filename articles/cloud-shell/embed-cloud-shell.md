---
title: "Vložení prostředí cloudu Azure | Microsoft Docs"
description: "Postup vložení prostředí cloudu Azure."
services: cloud-shell
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 7c06a51e7f9f402b2ec10e440ca98125a7f2a7cf
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="embed-azure-cloud-shell"></a>Vložení prostředí cloudu Azure

Vložení cloudové prostředí umožňuje vývojářům a obsahu zapisovače přímo z vyhrazené adresy URL, otevřete prostředí cloudu [shell.azure.com](https://shell.azure.com). To odemkne potenciál ověřování cloudové prostředí, nástrojů a aktuální nástroje Azure CLI nebo Azure PowerShell.

## <a name="how-to"></a>Postupy

Integrace tlačítko spustit prostředí cloudu do soubory markdown tak, že zkopírujete následující:

```markdown
[![Launch Cloud Shell](https:shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

Zde je kód HTML pro vložení automaticky otevírané okno prostředí cloudu:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="next-steps"></a>Další kroky
[Bash v prostředí cloudu rychlý start](quickstart.md)<br>
[Prostředí PowerShell v prostředí cloudu rychlý start](quickstart-powershell.md)