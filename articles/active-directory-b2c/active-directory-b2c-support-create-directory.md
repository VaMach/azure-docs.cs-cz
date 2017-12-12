---
title: "Azure Active Directory B2C: Řešení potíží s vytváření klienty | Microsoft Docs"
description: "Problémy a řešení pro vytvoření klienta služby Azure Active Directory nebo Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: 7ba4c6b2-161b-45b5-b3bd-ccb662f5d7a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: cfbcb9ad561320da8836086b1b8bff39f3fefb37
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Řešení potíží s vytváření klienta služby Azure Active Directory nebo Azure Active Directory B2C 

## <a name="create-an-azure-ad-tenant"></a>Vytvoření klienta Azure AD
Pokud při prvním pokusu nelze vytvořit klienta služby Azure Active Directory (Azure AD), zkuste to znovu. Pokud potíže potrvají, kontaktujte podporu Azure.

## <a name="create-an-azure-ad-b2c-tenant"></a>Vytvoření tenanta Azure AD B2C
Pokud narazíte na potíže při jste [vytvoření Azure Active Directory B2C klienta (Azure AD B2C)](active-directory-b2c-get-started.md), vyzkoušejte následující možnosti:

* Pokud klienta Azure AD B2C není zobrazena v seznamu klienty, zkuste to znovu vytvořit klienta.
* Pokud klienta Azure AD B2C objeví v seznamu klientů a zobrazí se následující chybová zpráva, odstraňte klienta a vytvořit znovu:

    "Nelze dokončit vytvoření klienta B2C 'contosob2c'. Navštivte toto [odkaz](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) další pokyny. "
* Existují známé problémy při odstranění existujícího klienta Azure AD B2C a znovu vytvořit pomocí stejného názvu domény. Při vytváření nového klienta Azure AD B2C, musíte použít jiný název domény.
* Pokud tato řešení nefungují, kontaktujte podporu Azure. Další informace najdete v tématu [soubor žádosti o podporu pro Azure AD B2C](active-directory-b2c-support.md).

