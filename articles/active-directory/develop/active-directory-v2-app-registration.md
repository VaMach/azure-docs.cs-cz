---
title: "Zaregistrovat aplikaci s koncovým bodem v2.0 Azure AD pomocí portálu | Microsoft Docs"
description: "Postup registrace aplikace se společností Microsoft pro povolení přihlášení a přístup ke službám Microsoft pomocí koncového bodu v2.0"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mtillman
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: eba8ecd27542b23676c08b8ce072c91134d27fa5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Postup registrace aplikace s koncovým bodem v2.0
Vytvořit aplikaci, která přijímá MSA & Azure AD přihlášení, nejprve musíte zaregistrovat aplikaci se společností Microsoft.  V tomto okamžiku nebudete moci používat všechny existující aplikace, které jste uzavřeli s Azure AD nebo MSA - budete muset vytvořit nové.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0.  Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="visit-the-microsoft-app-registration-portal"></a>Přejděte na portál pro registraci aplikace Microsoft
Nejdůležitější - první návštěvě [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Toto je nový portál registrace aplikace, kde můžete spravovat aplikace Microsoft.

Přihlaste se pomocí buď osobní nebo pracovní nebo školní účet Microsoft.  Pokud nemáte buď, zaregistrujte si nový osobní účet. Pokračujte, nebude trvat dlouho – budete čekáme sem.

Provést? By měl nyní se díváte v seznamu aplikací Microsoftu, což je pravděpodobně prázdná.  Umožňuje změnit.

Klikněte na tlačítko **přidat aplikaci**a pojmenujte ho.  Na portálu přiřadí globálně jedinečné Id aplikace, které budete používat později v kódu aplikace.  Pokud vaše aplikace obsahuje serverovou komponentu potřebného přístupových tokenů pro volání rozhraní API (vezměte v úvahu: Office, Azure nebo vlastní webové rozhraní API), budete chtít vytvořit **tajný klíč aplikace** i zde.

Dál přidejte platformy, které vaše aplikace bude používat.

* Pro webové aplikace, zadejte **identifikátor URI pro přesměrování** kde lze odesílat zprávy přihlášení.
* Identifikátor uri je automaticky vytvořen pro přesměrování pro mobilní aplikace, poznamenejte si výchozí.

Volitelně můžete přizpůsobit vzhled a chování stránky přihlášení v části profilu.  Ujistěte se, klikněte na **Uložit** než budete pokračovat.

> [!NOTE]
> Při vytváření aplikace pomocí [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), aplikace bude zaregistrována v domácí klientovi účtu, který používáte k přihlášení k portálu.  To znamená, že nemůže zaregistrovat aplikaci v klientovi služby Azure AD pomocí osobního účtu Microsoft.  Pokud chcete explicitně zaregistrovat aplikaci v konkrétní klienta, přihlaste se pomocí účtu, který původně vytvořil v něm.
> 
> 

## <a name="build-a-quick-start-app"></a>Sestavení aplikace rychlý start
Teď, když máte aplikaci Microsoft, můžete dokončit jeden z našich kurzů v2.0 rychlý start.  Zde je několik doporučení:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

