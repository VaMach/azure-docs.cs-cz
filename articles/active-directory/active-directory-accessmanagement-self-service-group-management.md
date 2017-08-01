---
title: "Nastavení služby Azure Active Directory pro správu přístupu k samoobslužným aplikacím | Dokumentace Microsoftu"
description: "Samoobslužná správa skupin umožňuje uživatelům vytvářet a spravovat skupiny zabezpečení nebo skupiny Office 365 ve službě Azure Active Directory a nabízí uživatelům možnost žádat o členství ve skupině zabezpečení nebo ve skupině Office 365."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 904d5c70-c34a-46c4-a9a7-d1efecf4821c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/27/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 92681a42ff1eb7e9bfa834308833b96749cbd078
ms.contentlocale: cs-cz
ms.lasthandoff: 07/26/2017

---
# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Nastavení služby Azure Active Directory pro samoobslužnou správu skupin
Samoobslužná správa skupin uživatelům umožňuje vytvářet a spravovat skupiny zabezpečení nebo skupiny Office 365 ve službě Azure Active Directory (Azure AD). Uživatelé mohou také požádat o členství ve skupině zabezpečení nebo ve skupině Office 365. Vlastník skupiny pak může členství schválit nebo odepřít. Tímto způsobem můžete každodenní řízení členství ve skupině delegovat na uživatele, kteří chápou obchodní kontext takového členství. Funkce samoobslužné správy skupin jsou dostupné jenom pro skupiny zabezpečení a skupiny Office 365. Nejsou dostupné pro skupiny zabezpečení s povoleným e-mailem a pro distribuční seznamy.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.

Samoobslužná správa skupin v současné době obsahuje dva základní scénáře: delegovanou správu skupin a samoobslužnou správu skupin.

* **Delegovaná správa skupin** – příkladem je správce, který spravuje přístup k aplikaci SaaS, kterou používá jeho společnost. Správa těchto přístupových práv je čím dál náročnější, takže správce požádá majitele firmy, aby vytvořil novou skupinu. Správce přiřadí nové skupině přístup k aplikaci a do této skupiny přidá všechny uživatele, kteří už přístup k aplikaci mají. Majitel firmy potom může přidat další uživatele a tito uživatelé budou automaticky přiřazeni k aplikaci. Kvůli správě přístupu pro uživatele tak majitel firmy nemusí čekat na správce. Pokud správce udělí stejné oprávnění správci v jiné firemní skupině, potom tato osoba může spravovat také přístup svých vlastních uživatelů. Majitel firmy ani správce nemůžou zobrazit nebo spravovat uživatele toho druhého. Správce může stále vidět všechny uživatele, kteří mají přístup k aplikaci, a v případě potřeby je může zablokovat.
* **Samoobslužná správa skupin** – příkladem tohoto scénáře jsou dva uživatelé, kteří mají nezávisle nastavené weby SharePoint Online. Oba chtějí týmu toho druhého poskytnout přístup na svůj web. Aby toho dosáhli, můžou vytvořit jednu skupinu v Azure AD a potom v SharePointu Online každý z nich vybere tu skupinu, které chce poskytnout přístup na svůj web. Pokud chce uživatel získat přístup, požádá o něj na přístupovém panelu a po schválení získá přístup k oběma webům SharePoint Online automaticky. Později se jeden z nich rozhodne, že všichni uživatelé s přístupem k webu by měli získat přístup i k určité aplikaci SaaS. Správce aplikace SaaS může přidat přístupová práva k aplikacím na web SharePoint Online. Od toho okamžiku získají veškeré jím schválené žádosti přístup na oba weby SharePoint Online a také k této aplikaci SaaS.

## <a name="making-a-group-available-for-end-user-self-service"></a>Zpřístupnění skupiny pro samoobslužné funkce koncových uživatelů
1. Na [portálu Azure Classic](https://manage.windowsazure.com) otevřete adresář služby Azure AD.
2. Na kartě **Konfigurace** nastavte možnost**Delegovaná správa skupin** na Povoleno.
3. Nastavte možnost **Uživatelé můžou vytvářet skupiny zabezpečení** nebo **Uživatelé můžou vytvářet skupiny Office** na Povoleno.

Když je povolená možnost **Uživatelé můžou vytvářet skupiny zabezpečení**, mají všichni uživatelé v adresáři povoleno vytvářet nové skupiny zabezpečení a přidávat do těchto skupin členy. Tyto nové skupiny se zobrazí také všem ostatním uživatelům na přístupovém panelu. Pokud to nastavení zásad skupiny umožňuje, můžou ostatní uživatelé vytvářet žádosti o připojení k těmto skupinám. Pokud je zakázaná možnost **Uživatelé můžou vytvářet skupiny zabezpečení**, uživatelé nemůžou vytvářet skupiny a nemůžou měnit existující skupiny, ve kterých figurují jako vlastníci. Můžou však stále spravovat členství v těchto skupinách a schvalovat žádosti o členství od jiných uživatelů.

Možnost **Uživatelé, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** můžete použít k zajištění podrobnějšího řízení přístupu, který vám samoobslužná správa skupin nenabídne. Když je povolená možnost **Uživatelé můžou vytvářet skupiny**, mají všichni uživatelé v adresáři povoleno vytváření nových skupin a přidávání členů do těchto skupin. Současným nastavením možnosti **Uživatelé, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** na Některé omezíte správu skupin jenom na omezenou skupinu uživatelů. Pokud je tento přepínač nastavený na možnost Někteří, musíte uživatele nejprve přidat do skupiny SSGMSecurityGroupsUsers a teprve potom budou tito uživatelé moct vytvářet nové skupiny a přidávat do nich členy. Nastavením možnosti **Uživatelé, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** na Všichni povolíte všem uživatelům v adresáři vytváření nových skupin.

Můžete také použít políčko **Skupina, která může využívat samoobslužné funkce pro skupiny zabezpečení** a zadat do něj vlastní název skupiny, jejíž členové můžou samoobslužné funkce používat.

## <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)
* [Rutiny služby Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Představení služby Azure Active Directory](active-directory-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

