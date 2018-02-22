---
title: Registrace aplikace Azure Active Directory | Microsoft Docs
description: "Tento článek popisuje, jak zaregistrovat aplikaci s Azure Active Directory pomocí portálu Azure"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mtillman
editor: 
ms.assetid: 7dc7b89f-653f-405a-b5f4-2c1288720c15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: priyamo
ms.reviewer: elisol
ms.openlocfilehash: 8f5d4ba82fcf3c963373b0e90b707a7d86fc0fea
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="register-your-application-with-your-azure-active-directory-tenant"></a>Registrace aplikace pomocí klienta služby Azure Active Directory

Na portálu Azure můžete aplikaci zaregistrovat u klienta služby Azure Active Directory (Azure AD). Tím se vytvoří ID aplikací pro aplikaci a umožní, aby přijímat tokeny.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V levém navigačním podokně zvolte **všechny služby**, klikněte na tlačítko **registrace aplikace**a klikněte na tlačítko **přidat**.
4. Postupujte podle zobrazených výzev a vytvořte novou aplikaci. Pokud chcete konkrétní příklady pro webové aplikace nebo nativních aplikací, podívejte se na naše [– elementy QuickStart](active-directory-developers-guide.md).
  * Pro webové aplikace, zadejte **přihlašovací adresa URL**, což je základní adresu URL aplikace, kde uživatelé se mohou přihlásit v např `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * U nativních aplikací, zadejte **identifikátor URI pro přesměrování**, které Azure AD se používá k vrácení odpovědi tokenu. Zadejte konkrétní hodnotu pro vaši aplikace, např. `http://MyFirstAADApp`.
5. Po dokončení registrace Azure AD přiřadí aplikace identifikátor jedinečných klientských ID aplikace.

## <a name="update-application-settings-from-the-azure-portal"></a>Aktualizovat nastavení aplikace z portálu Azure

Můžete snadno upravit existující aplikaci nastavení pomocí portálu Azure. Můžete například nakonfigurovat adresa URL odpovědi, což je, kde Azure AD vydá token odpovědi. Můžete také nakonfigurovat oprávnění k ostatním aplikacím, například umožňuje aplikaci přístup k Microsoft Graph API. Můžete k tomu všechny prostřednictvím stránky nastavení aplikace.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V levém navigačním podokně zvolte **všechny služby**, klikněte na tlačítko **registrace aplikace**a vyberte aplikaci ze seznamu.
4. Klikněte na tlačítko **nastavení** otevřete stránku nastavení pro aplikaci.
  * **Vlastnosti** stránce lze upravit obecné informace pro aplikace. To zahrnuje název aplikace, adresa URL přihlašování a adresy URL odhlašovací.
  * **Adresy URL odpovědí** stránky umožňuje přidat adresu URL odpovědi, která je, kde Azure AD odešle token odpovědi.
  * **Vlastníky** stránky umožňuje přidat vlastníci aplikace.
  * **Požadovaná oprávnění** stránce umožňuje nakonfigurovat oprávnění pro aplikaci. Například pro přístup k rozhraní Graph API společnosti Microsoft, klikněte na možnost **přidat** a vyberte **Microsoft Graph** v modulu pro výběr rozhraní API, zvolte oprávnění požadované, například **čtení dat adresáře**.
  * **Klíče** stránky umožňuje přidat aplikace tajných klíčů. Tajný klíč se zobrazí pouze po okamžitě po vytvoření, tak zkontrolujte, zda a zkopírujte ho pro další použití.

## <a name="use-the-inline-manifest-editor"></a>Použití editoru vložené manifestu

Editor manifestu vložené můžete upravit některé vlastnosti aplikace, které nejsou vystaveny přímo na portálu Azure. Například můžete ho změnit identifikátor ID URI aplikace aplikace nebo povolit implicitní tok OAuth2.0 místo tok výchozí udělení autorizačního kódu.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Výběrem účtu v pravém horním rohu stránky zvolte klientovi Azure AD.
3. V levém navigačním podokně zvolte **všechny služby**, klikněte na tlačítko **registrace aplikace**a vyberte aplikaci ze seznamu.
4. Klikněte na tlačítko **Manifest** ze stránky aplikace k otevření editoru vloženého manifestu.
5. Můžete přímo měnit manifest a uložte ho, až budete připraveni. Alternativně můžete stáhnout manifest otevřít ji ve svém oblíbeném editoru a odeslat aktualizované manifestu.

## <a name="next-steps"></a>Další kroky

1. Podívejte se [– elementy QuickStart](active-directory-developers-guide.md) pro podrobné návody pro aplikace pro ověřování pomocí služby Azure AD.
2. Podívejte se na naše úplný seznam ukázky kódu v [Githubu](https://github.com/azure-samples).
