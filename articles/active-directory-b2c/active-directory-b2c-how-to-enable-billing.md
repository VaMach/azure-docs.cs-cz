---
title: "Postup propojení předplatné Azure s Azure AD B2C | Microsoft Docs"
description: "Podrobný průvodce povolit fakturace pro klienta Azure AD B2C do předplatného Azure."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: joroja
ms.openlocfilehash: 5b9955b2af7f20a79981315fa33a0eb5380a5465
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="linking-an-azure-subscription-to-an-azure-b2c-tenant-to-pay-for-usage-charges"></a>Propojování předplatné Azure do Azure B2C klienta k platit poplatky za používání

K předplatnému Azure se účtují poplatky za probíhající používání pro Azure Active Directory B2C (nebo Azure AD B2C). Je nezbytné pro správce klienta explicitně propojení klienta Azure AD B2C k předplatnému Azure po vytvoření klienta B2C, sám sebe.  Tento odkaz je dosaženo vytvořením Azure AD "Klienta B2C" prostředků v cílové předplatné Azure. Velký počet klientů B2C se dá propojit jedno předplatné Azure společně s dalším prostředkům služby Azure (například virtuální počítače, úložiště dat, LogicApps)


> [!IMPORTANT]
> Nejnovější informace o využití fakturaci a cenách pro B2C je na následující stránce: [cenová Azure AD B2C](
https://azure.microsoft.com/pricing/details/active-directory-b2c/)

## <a name="step-1---create-an-azure-ad-b2c-tenant"></a>Krok 1 – Vytvoření klienta Azure AD B2C
Vytvoření klienta B2C musíte nejprve dokončit. Tento krok přeskočte, pokud jste již vytvořili cílových klienta B2C. [Začínáme s Azure AD B2C](active-directory-b2c-get-started.md)

## <a name="step-2---open-azure-portal-in-the-azure-ad-tenant-that-shows-your-azure-subscription"></a>Krok 2 – otevřete portál Azure v klientovi Azure AD, který ukazuje vašeho předplatného Azure
Přejděte na [Azure Portal](https://portal.azure.com). Umožňuje přepnout do klienta Azure AD, který obsahuje předplatné Azure, které že chcete použít. Tento klient Azure AD se liší od klienta B2C. V rámci portálu Azure klikněte na název účtu v pravém horním rohu stránky řídicího panelu a vyberte klienta Azure AD. Chcete-li pokračovat je potřeba předplatné Azure. [Získání předplatného Azure.](https://account.windowsazure.com/signup?showCatalog=True)

![Přepnutí na klientovi služby Azure AD](./media/active-directory-b2c-how-to-enable-billing/SelectAzureADTenant.png)

## <a name="step-3---create-a-b2c-tenant-resource-in-azure-marketplace"></a>Krok 3 – vytvoření klienta B2C prostředků v Azure Marketplace
Klepnutím na ikonu Marketplace, nebo výběrem zeleným "+" v levém horním rohu řídicího panelu, otevřete Marketplace.  Vyhledejte a vyberte Azure Active Directory B2C. Vyberte možnost vytvořit.

![Vyberte Marketplace.](./media/active-directory-b2c-how-to-enable-billing/marketplace.png)

![Hledání AD B2C](./media/active-directory-b2c-how-to-enable-billing/searchb2c.png)

Prostředek Azure AD B2C vytvořit dialogové okno obsahuje následující parametry:

1. Klienta Azure AD B2C – vyberte klienta Azure AD B2C z rozevíracího seznamu.  Zobrazit pouze oprávněné klienty Azure AD B2C.  Oprávněné klienty B2C splňovat tyto podmínky: jste globální správce klienta B2C a klienta B2C není aktuálně přidružené k předplatnému Azure

2. Azure AD B2C prostředků název – je předem vybrali, aby odpovídal názvu domény klienta B2C

3. Předplatné -, ve kterém se správce nebo spolusprávce aktivní předplatné Azure.  Několik klientů Azure AD B2C, mohou být přidány do jednoho předplatného Azure

4. Umístění skupiny prostředků a skupina prostředků – tento artefaktů umožňuje uspořádat několik prostředků Azure.  Tato volba nemá žádný vliv na umístění klienta B2C, výkon nebo stav fakturace

5. Připnout na řídicí panel pro nejjednodušší přístup k vaší B2C klienta fakturační informace a nastavení klienta B2C ![vytvořit prostředek B2C](./media/active-directory-b2c-how-to-enable-billing/createresourceb2c.png)

## <a name="step-4---manage-your-b2c-tenant-resources-optional"></a>Krok 4 – spravovat prostředky klienta B2C (volitelné)
Po dokončení nasazení nového prostředku "Klienta B2C" se vytvoří v cílové skupině prostředků a související předplatného Azure.  Měli byste vidět nový prostředek typu "Klienta B2C" přidat spolu s vašich prostředků Azure.

![Vytvořte prostředek B2C](./media/active-directory-b2c-how-to-enable-billing/b2cresourcedashboard.png)

Kliknutím na prostředek klienta B2C, budete moci
- Klikněte na název odběru, chcete-li zkontrolovat fakturační informace. Viz fakturaci a využití.
- Klikněte na tlačítko Nastavení Azure AD B2C, otevřete novou kartu prohlížeče přímo v okně Nastavení klienta B2C
- Odeslání žádosti o podporu
- Přesunutí prostředku klienta B2C do jiného předplatného Azure nebo do jiné skupiny prostředků.  Tato volba změny, které předplatné obdrží poplatky za používání.

![Nastavení prostředků B2C](./media/active-directory-b2c-how-to-enable-billing/b2cresourcesettings.png)

## <a name="known-issues"></a>Známé problémy
- Odstranění klienta B2C. Pokud klienta B2C je vytvořen, odstraní a znovu vytvoří se stejným názvem domény, prosím také odstranit a znovu vytvořit "Propojování" prostředek se stejným názvem domény.  Tento prostředek "Propojování" v části "Všechny prostředky" zjistí v klientovi předplatné prostřednictvím portálu Azure.
- Samoobslužné uložena omezení umístění místních prostředků.  Ve výjimečných případech může uživatel navázat místní omezení pro vytváření prostředků Azure.  Toto omezení by mohlo zabránit vytvoření propojení mezi předplatné Azure a klienta B2C. Pokud chcete zmírnit, prosím toto omezení zmírnit.

## <a name="next-steps"></a>Další kroky
Po dokončení pro každou z vašich klientů B2C těchto kroků se vašeho předplatného Azure se fakturuje v souladu s podrobností o Azure přímý nebo smlouvu Enterprise.
- Zkontrolujte využití a fakturace ve vašem vybraném předplatném Azure
- Přečtěte si podrobné informace o použití ve dnech sestav pomocí [API pro vytváření sestav využití](active-directory-b2c-reference-usage-reporting-api.md)
