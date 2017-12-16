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
ms.openlocfilehash: 78b539136971aa282e5447d7882ecb02f73f346b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/16/2017
---
# <a name="embed-azure-cloud-shell"></a>Vložení prostředí cloudu Azure

Vložení cloudové prostředí umožňuje vývojářům a obsahu zapisovače přímo z vyhrazené adresy URL, otevřete prostředí cloudu [shell.azure.com](https://shell.azure.com). To přináší okamžitě potenciál ověřování cloudové prostředí, nástrojů, a aktuální Azure CLI nebo Azure PowerShell nástroje pro vaše uživatele.

[![](https://shell.azure.com/images/launchcloudshell.png "Spusťte prostředí cloudu Azure")](https://shell.azure.com)

## <a name="how-to"></a>Postupy

Integrace tlačítko spustit prostředí cloudu do soubory markdown tak, že zkopírujete následující:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

Zde je kód HTML pro vložení automaticky otevírané okno prostředí cloudu:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Přizpůsobení prostředí

Nastavte možnosti specifické prostředí rozšířit vaše adresa URL.
|Prostředí   |ADRESA URL   |
|---|---|
|Naposledy použité prostředí   |Shell.Azure.com           |
|Bash                       |Shell.Azure.com/bash       |
|PowerShell                 |Shell.Azure.com/PowerShell |

## <a name="next-steps"></a>Další kroky
[Bash v prostředí cloudu rychlý start](quickstart.md)<br>
[Prostředí PowerShell v prostředí cloudu rychlý start](quickstart-powershell.md)
