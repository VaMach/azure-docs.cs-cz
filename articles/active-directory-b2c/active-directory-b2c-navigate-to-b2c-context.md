---
title: "Azure Active Directory B2C: Přepnutí na tenanta B2C | Dokumentace Microsoftu"
description: "Postup přepnutí do kontextu tenanta Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 0eb1b198-44d3-4065-9fae-16591a8d3eae
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/13/2017
ms.author: parakhj
ms.openlocfilehash: 6c1fd08c52f33a062d06e0593cbbe00346bb44f1
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="switching-to-your-azure-ad-b2c-tenant"></a>Přepnutí na tenanta Azure AD B2C

Abyste mohli konfigurovat Azure AD B2C, musíte být v kontextu tenanta Azure AD B2C.

## <a name="log-into-azure-ad-b2c-tenant"></a>Přihlášení k tenantovi Azure AD B2C

Abyste mohli přejít do tenanta Azure AD B2C, musíte být přihlášeni na webu Azure Portal jako globální správce tenanta Azure AD B2C.

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
1. Tenanty můžete přepínat kliknutím na vaši e-mailovou adresu nebo váš obrázek v horním pravém rohu.
1. V seznamu `Directory`, který se zobrazí, vyberte tenanta Azure AD B2C, kterého chcete spravovat.

Azure Portal se aktualizuje.  Nyní jste na webu Azure Portal přihlášeni v kontextu vašeho tenanta Azure AD B2C.

## <a name="navigate-to-the-b2c-features-blade"></a>Přejděte do okna s funkcemi B2C.

1. Klikněte na **Procházet** v levém navigačním panelu.
1. Klikněte na **> Další služby** a pak v levém navigačním panelu vyhledejte `Azure AD B2C`.  (Pro připnutí na Úvodní panel vlevo klikněte na hvězdičku nalevo od Azure AD B2C)
1. Kliknutím na **Azure AD B2C** otevřete okno s funkcemi B2C.
   
    ![Snímek obrazovky s přechodem do okna s funkcemi B2C](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> Pro přístup k oknu s funkcemi B2C musíte být Globální správce klienta B2C. Globální správce jiného klienta ani uživatel jakéhokoli klienta nemají k oknu přístup.  Můžete přepnout na svého klienta B2C pomocí přepínače klienta v pravém horním rohu webu Azure Portal.
