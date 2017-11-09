---
title: "Azure vyhrazené prostředků název chyby | Microsoft Docs"
description: "Popisuje postup řešení chyb při zadávání názvu prostředku, který zahrnuje vyhrazené slovo."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 2f06b7bc65ea309bc5374b41f402f8e7bc4d2a38
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="resolve-reserved-resource-name-errors"></a>Vyřešte chyby název vyhrazený prostředek

Tento článek popisuje chyby, ke které dojde při nasazení na prostředek, který obsahuje vyhrazené slovo v jeho názvu.

## <a name="symptom"></a>Příznaky

Při nasazení na prostředek, který je k dispozici prostřednictvím veřejný koncový bod, může se zobrazit chybová zpráva:

```
Code=ReservedResourceName;
Message=The resource name <resource-name> or a part of the name is a trademarked or reserved word.
```

## <a name="cause"></a>Příčina

Prostředky, které mají veřejný koncový bod nelze použít vyhrazená slova nebo ochranné známky v názvu.

Slova jsou vyhrazené:

* PŘÍSTUP
* AZURE
* BING
* BIZSPARK
* BIZTALK
* CORTANY
* ROZHRANÍ DIRECTX
* DOTNET.
* DYNAMICS
* APLIKACE EXCEL
* EXCHANGE
* FOREFRONT
* GROOVE
* HOLOLENS
* HYPER-V
* SENZOR KINECT
* APLIKACE LYNC
* MSDN
* O365
* OFFICE
* OFFICE 365
* ONEDRIVE
* APLIKACE ONENOTE
* APLIKACE OUTLOOK
* POWERPOINT
* SLUŽBY SHAREPOINT
* SKYPE
* APLIKACE VISIO
* VISUAL STUDIO

Tato slova nelze použít jako celá slova nebo substring v názvu:

* PŘIHLÁŠENÍ
* MICROSOFT
* WINDOWS
* XBOX

## <a name="solution"></a>Řešení

Zadejte název, který nepoužívá jeden z vyhrazených slov.