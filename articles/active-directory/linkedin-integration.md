---
title: "Povolit nebo zakázat LinkedIn integrace pro aplikace Office v Azure Active Directory | Microsoft Docs"
description: "Vysvětluje, jak povolit nebo zakázat LinkedIn integrace pro aplikace společnosti Microsoft v Azure Active Directory"
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 02/28/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: cdfb5458b020e9d3a3f33cecbeb0ee7b9a48909d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="linkedin-integration-for-office-applications"></a>Integrace LinkedIn pro aplikace systému Office
Tento článek vysvětluje, jak omezit uživatele, kterým je LinkedIn integrace součástí Azure Active Directory (Azure AD). Integrace LinkedIn je povolena ve výchozím nastavení při přidání klienta, což umožňuje uživatelům používat veřejná data LinkedIn v některé z aplikace Microsoft. Každý uživatel může vybrat nezávisle připojit svůj pracovní nebo školní účet k účtu mají LinkedIn.

> [!IMPORTANT]
> Integrace LinkedIn není nasazované na všechny klienty Azure AD ve stejnou dobu. Jakmile je vrátit ke klientovi Azure, LinkedIn integrace ve výchozím nastavení zapnutá. Integrace LinkedIn není k dispozici pro místní přejít, suverénní a government klienty. Aktuální zobrazení informací o zavedení, najdete v článku [plán Office 365](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc) stránky.

## <a name="linkedin-integration-from-the-user-perspective"></a>Integrace LinkedIn z pohledu uživatele
Pokud uživatelé ve vaší organizaci připojit jejich LinkedIn účtu ke svým pracovním nebo školním účtem [umožňují LinkedIn poskytující data](https://www.linkedin.com/help/linkedin/answer/84077) mají být použity v Microsoft aplikace a služby, které poskytuje vaší organizace. [Uživatele můžete odpojit účty](https://www.linkedin.com/help/linkedin/answer/85097), které odebere oprávnění ke sdílení dat s Microsoft LinkedIn. Integrace LinkedIn používá veřejně dostupné informace o profilu LinkedIn. [Uživatelé můžou řídit, jak zobrazit svůj vlastní profil LinkedIn](https://www.linkedin.com/help/linkedin/answer/83) pomocí nastavení ochrany osobních údajů LinkedIn, včetně toho, jestli svůj profil lze zobrazit v aplikacích společnosti Microsoft.

## <a name="privacy-considerations"></a>Aspekty ochrany osobních údajů
Povolení integrace LinkedIn ve službě Azure AD umožňuje aplikace společnosti Microsoft a službám přístup k některé z vašich uživatelů LinkedIn informací. Pro čtení [prohlášení o ochraně osobních údajů Microsoft](https://privacy.microsoft.com/privacystatement/) Další informace o aspektech ochrany osobních údajů při povolování LinkedIn integrace ve službě Azure AD. 

## <a name="manage-linkedin-integration"></a>Správa integrace LinkedIn
Integrace LinkedIn podnikům, které je povolena ve výchozím nastavení ve službě Azure AD. Povolení integrace LinkedIn umožňuje všem uživatelům ve vaší organizaci používat funkce LinkedIn v rámci služby společnosti Microsoft, například zobrazení LinkedIn profily v aplikaci Outlook. Zakázání integrace LinkedIn odebere funkce LinkedIn z aplikace Microsoft a služby a zastavuje data sdílení mezi LinkedIn a vaše organizace prostřednictvím služeb Microsoft.

### <a name="enable-or-disable-linkedin-integration-for-your-organization-in-the-azure-portal"></a>Povolit nebo zakázat LinkedIn integrace pro vaši organizaci na portálu Azure

1. Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com/) pomocí účtu, který je globální správce pro tenanta Azure AD.
2. Vyberte **uživatelé**.
3. Na **uživatelé** vyberte **uživatelská nastavení**.
4. V části **LinkedIn integrace**, vyberte **Ano** nebo **ne** k povolení nebo zakázání integrace LinkedIn.
   ![Povolení integrace LinkedIn](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="enable-or-disable-linkedin-integration-for-your-organizations-office-2016-apps-using-group-policy"></a>Povolit nebo zakázat LinkedIn integrace pro aplikace Office 2016 vaší organizace pomocí zásad skupiny

1. Stažení [souborů šablon pro správu Office 2016 (ADMX nebo ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extrahování **ADMX** soubory a zkopírujte je do vašeho **centrálním úložišti**.
3. Otevřete správu zásad skupiny.
4. Vytvoření objektu zásad skupiny s následujícím nastavením: **konfigurace uživatele** > **šablony pro správu** > **Microsoft Office 2016**  >  **Různé** > **povolit integraci LinkedIn**.
5. Vyberte **povoleno** nebo **zakázané**.
  * Pokud tato zásada je **povoleno**, **LinkedIn zobrazit funkce v aplikacích Office** je povoleno nastavení v dialogovém okně Možnosti Office 2016. Také to znamená, že uživatelé ve vaší organizaci můžete použít funkce LinkedIn ve svých aplikacích Office.
  * Pokud tato zásada je **zakázáno**, **LinkedIn zobrazit funkce v aplikacích Office** nastavení nalezených v možnostech Office 2016 dialogové okno nastavena na zakázaném stavu, a toto nastavení nelze změnit koncovým uživatelům. Uživatelé ve vaší organizaci LinkedIn funkce nelze používat ve svých aplikacích Office 2016. 

Tato zásada skupiny ovlivní pouze aplikace Office 2016 pro místní počítač. Uživatelé mohou vidět LinkedIn funkcí v profilu karty v rámci služeb Office 365, i když se zakážou LinkedIn ve svých aplikacích Office 2016. 

### <a name="learn-more"></a>Další informace 
* [Informace o LinkedIn a funkcí ve vašich aplikacích společnosti Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centrum pro nápovědu LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Další postup
Pomocí následujícího odkazu zobrazíte aktuální nastavení integrace LinkedIn na portálu Azure:

[Konfigurace integrace LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 