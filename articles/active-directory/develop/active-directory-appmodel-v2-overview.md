---
title: "Azure Active Directory koncového bodu v2.0 | Microsoft Docs"
description: "Úvod do vytváření aplikace Microsoft Account a Azure Active Directory přihlášení."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1e286044fb1a1b367fcac2dc14c47f68d5ed120d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Přihlášení uživatele Azure AD v jediné aplikaci & Account Microsoft
V minulosti vývojář aplikace, kteří chtěli podporovat i osobní účty Microsoft a pracovní účty ze služby Azure Active Directory vyžaduje integrovat dvou samostatných systémech.  **Koncového bodu Azure AD v2.0** zavádí novou verzi ověřování rozhraní API, která umožňuje přihlásit oba typy účtů pomocí jednoho Jednoduchá integrace.  Aplikace, které používají koncového bodu v2.0 můžete také používat rozhraní REST API z [Microsoft Graph](https://graph.microsoft.io) pomocí buď typu účtu.

## <a name="getting-started"></a>Začínáme
Vyberte oblíbenou platformu z následujícího seznamu k sestavení aplikace pomocí našich opensourcové knihovny & architektury.  Alternativně můžete dokumentaci protokol OAuth 2.0 & OpenID Connect odesílat a přijímat zprávy protokolu přímo bez použití knihovny ověřování.

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Novinky
Zde uvedené informace budou užitečné porozumět, co je & co není možné s koncovým bodem v2.0.

* Další informace o [typy aplikací můžete vytvořit s koncovým bodem v2.0](active-directory-v2-flows.md).
* Pochopení [omezení, omezení a omezení](active-directory-v2-limitations.md) s koncovým bodem v2.0.
* Podívejte se na tento přehled – video pro koncový bod v2.0:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a>Referenční informace
Následující odkazy budou užitečné při prozkoumávání platformou podrobněji:

* [referenční informace o protokolu v2.0](active-directory-v2-protocols.md)
* [Odkaz tokenu v2.0](active-directory-v2-tokens.md)
* [Referenční příručka knihovny v2.0](active-directory-v2-libraries.md)
* [Obory a souhlasu v koncového bodu v2.0](active-directory-v2-scopes.md)
* [Microsoft Graph](https://graph.microsoft.io)

## <a name="help--support"></a>Nápověda a podpora
Na těchto místech získáte nejlepší pomoc s vývojem v Azure Active Directory.

* [Značky `azure-active-directory` a `adal` na Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [Zpětná vazba k Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> Pokud potřebujete přihlásit pracovní a školní účty ze služby Azure Active Directory, měli byste začít s naše [Příručka pro vývojáře Azure AD](active-directory-developers-guide.md).  Koncový bod v2.0 je určená pro vývojáře, kteří explicitně se muset přihlásit osobní účty Microsoft.

