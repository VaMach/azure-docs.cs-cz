---
title: "Azure AD v2 Windows Desktop získávání spuštěno – konfigurace | Microsoft Docs"
description: "Aplikace Windows Desktop .NET (XAML) jak získat přístupový token a volat rozhraní API chráněné službou Azure Active Directory v2 koncový bod."
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 922fd0e24334eb45995b66dfaaa49d3d4f835e9a
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Přidat informace o registraci aplikace do aplikace
V tomto kroku budete muset do projektu přidejte Id aplikace.

1.  Otevřete `App.xaml.cs` a nahraďte řádek obsahující `ClientId` se:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Co je další

[!INCLUDE  [Test and Validate](..\..\..\..\includes\active-directory-develop-guidedsetup-windesktop-test.md)]
