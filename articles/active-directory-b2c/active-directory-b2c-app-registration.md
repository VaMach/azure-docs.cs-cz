---
title: 'Azure Active Directory B2C: Registrace aplikace | Dokumentace Microsoftu'
description: "Postup registrace aplikace pomocí Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/06/2016
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 0b87ec728891d4f64bb7b2e4b7b4f0f2276b1ffc
ms.openlocfilehash: ca75ddf8d099f411c4ae1af00e922f26e145f939


---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrace vaší aplikace
## <a name="prerequisite"></a>Požadavek
Chcete-li sestavit aplikaci, která podporuje registrace a přihlašování uživatelů, musíte aplikaci nejprve zaregistrovat pomocí klienta Azure Active Directory B2C. Vlastního klienta získáte pomocí návodu v tématu [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md). Po provedení všech kroků v tomto tématu budete mít okno s funkcemi B2C připnuté na Úvodním panelu.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Přejděte do okna s funkcemi B2C.
Pokud máte okno s funkcemi B2C připnuté na Úvodním panelu, okno se zobrazí na [portálu Azure](https://portal.azure.com/) ihned po přihlášení jako Globální správce klienta B2C.

Přejít do okna je možné také kliknutím na **Procházet** a poté na **Azure AD B2C** v levém navigačním podokně na [portálu Azure](https://portal.azure.com/).

> [!IMPORTANT]
> Pro přístup k oknu s funkcemi B2C musíte být Globální správce klienta B2C. Globální správce jiného klienta ani uživatel jakéhokoli klienta nemají k oknu přístup.  Můžete přepnout na svého klienta B2C pomocí přepínače klienta v pravém horním rohu portálu Azure.
> 
> 

## <a name="register-an-application"></a>Registrace aplikace
1. V okně s funkcemi B2C na portálu Azure klikněte na **Aplikace**.
2. Klikněte na **Přidat** v horní části okna.
3. Zadejte **Název** aplikace, který popíše aplikaci uživatelům. Můžete například zadat „Aplikace Contoso B2C“.
4. Pokud píšete webovou aplikaci, přepněte přepínač **Zahrnout webovou aplikaci / webové rozhraní API** na **Ano**. **Adresy URL odpovědí** jsou koncové body, kam Azure AD B2C vrátí všechny tokeny, které vaše aplikace požaduje. Zadejte například `https://localhost:44321/`. Pokud vaše webová aplikace také bude volat některé webové rozhraní API zabezpečené pomocí Azure AD B2C, budete chtít vytvořit **Tajný klíč aplikace** a také kliknout na tlačítko **Vygenerovat klíč**.
   
   > [!NOTE]
   > **Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.
   > 
   > 
5. Pokud píšete mobilní aplikaci, přepněte přepínač **Zahrnout nativního klienta** na **Ano**. Poznamenejte si výchozí **Identifikátor URI přesměrování**, který se automaticky vytvoří.
6. Pro registraci aplikace klikněte na **Vytvořit**.
7. Klikněte na aplikaci, kterou jste právě vytvořili, a poznamenejte si globálně jedinečné**ID klienta aplikace**, které použijete později ve svém kódu.

> [!IMPORTANT]
> Aplikace vytvořené v okně funkcí B2C musí být spravované ve stejném umístění. Pokud upravujete aplikace B2C pomocí PowerShellu nebo jiného portálu, stanou se nepodporované a nebudou pravděpodobně s Azure AD B2C pracovat.
> 
> 

## <a name="build-a-quick-start-application"></a>Sestavení aplikace Rychlý start
Nyní, když máte aplikaci registrovanou v Azure AD B2C, můžete dokončit jeden z našich kurzů pro rychlý start pro uvedení do provozu. Zde je několik doporučení:

[!INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]




<!--HONumber=Jan17_HO4-->


