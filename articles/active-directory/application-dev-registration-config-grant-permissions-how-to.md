---
title: "Postup udělení oprávnění k aplikaci zákaznických | Microsoft Docs"
description: "Postup udělení oprávnění pro aplikace vyvinuté vlastní pomocí portálu Azure AD nebo parametr adresy URL"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 0f13efd5e629bde592a3ab3a3e2db4a22180b5bb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a>Postup udělení oprávnění k aplikaci zákaznických

Chcete-li k udělení souhlasu ho preventivně ve vaší aplikaci nebo jsou spuštěné aplikace došlo k chybě, které nebyly souhlas, zkuste níže uvedeného postupu.

## <a name="how-to-perform-admin-consent-for-your-application"></a>Jak provádět souhlas správce pro vaši aplikaci

To má za následek udělení souhlasu do aplikace pro všechny uživatele ve vaší organizaci.

1. Přejděte na **registrace aplikace** jako **globálního správce**, vyberte aplikaci.

2. Vyberte **požadovaných oprávnění**a nakonec klikněte **udělit oprávnění** tlačítka v horní části okna.

Alternativně můžete vytvořit žádost o *login.microsoftonline.com* s konfigurací vaší aplikace a připojte na *& výzva = správce\_souhlas*. Po přihlášení pomocí přihlašovacích údajů správce, aplikace byl udělen souhlas pro všechny uživatele.

## <a name="how-to-force-user-consent-for-your-application"></a>Jak vynutit souhlas uživatele pro vaši aplikaci

* Připojit do žádosti o ověření *& výzva = souhlasu* vyžadující koncovým uživatelům souhlas pokaždé, když ověření.

## <a name="next-steps"></a>Další kroky

[Souhlasu a integrace aplikací AzureAD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

[Souhlasu a systému oprávnění rolích pro AzureAD v2.0 konvergované aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
