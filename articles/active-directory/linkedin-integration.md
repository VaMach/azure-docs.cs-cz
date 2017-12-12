---
title: "Konfigurace integrace LinkedIn ve službě Azure AD | Microsoft Docs"
description: "Vysvětluje, jak povolit nebo zakázat LinkedIn integrace pro aplikace společnosti Microsoft v Azure Active Directory."
services: active-directory
author: jeffgilb
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 15c161542d6826e6aeb5f708a0d9c3fa1f1885e3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="enabling-linkedin-integration-in-azure-active-directory"></a>Povolení integrace LinkedIn v Azure Active Directory
Povolení integrace LinkedIn umožní uživatelům přistupovat k data i veřejného LinkedIn a pokud se rozhodnete, jejich osobní LinkedIn síti ze v rámci aplikací společnosti Microsoft. Každý uživatel může vybrat nezávisle pro připojení k účtu mají LinkedIn svého pracovního účtu.

### <a name="linkedin-integration-from-your-end-users-perspective"></a>Integrace LinkedIn z hlediska vašich koncových uživatelů
Když koncoví uživatelé ve vaší organizaci připojit své účty LinkedIn k jejich pracovních účtů, jsou moct lépe identifikovat lidé, se kterými pracují s uvnitř i vně organizace. Připojení dvě účtů umožňuje připojení LinkedIn a data profilu, který se má použít v aplikacích Microsoft vaší organizace uživatele, ale jejich můžete vždy chodit odebráním oprávnění pro LinkedIn mohou sdílet data. Integrace se používá také informace o profilu veřejně dostupné a mohli uživatelé vybrat, jestli se má sdílet své veřejné informace profilu a sítě s aplikací společnosti Microsoft prostřednictvím nastavení ochrany osobních údajů LinkedIn.

>[!NOTE]
> Povolení integrace LinkedIn ve službě Azure AD umožňuje aplikacím a službám přístup k některé z informací vašich koncových uživatelů. Pro čtení [prohlášení o ochraně osobních údajů Microsoft](https://privacy.microsoft.com/privacystatement/) Další informace o aspektech ochrany osobních údajů při povolování LinkedIn integrace ve službě Azure AD. 

## <a name="enable-linkedin-integration"></a>Povolit integraci LinkedIn
Integrace LinkedIn podnikům, které je povolena ve výchozím nastavení ve službě Azure AD. Ano když tato funkce je k dispozici pro váš klient, všichni uživatelé ve vaší organizaci můžete zobrazit funkce LinkedIn a profily. Povolení integrace LinkedIn umožňuje uživatelům ve vaší organizaci používat funkce LinkedIn v rámci služby společnosti Microsoft, například zobrazení profily při příjmu e-mailu v Outlooku. Zakázáním této funkce zabraňuje přístupu k funkcím LinkedIn a ukončí uživatelských účtu připojení a sdílení mezi LinkedIn a vaše organizace prostřednictvím služeb Microsoft dat.

> [!IMPORTANT]
> Tato funkce není k dispozici pro místní přejít, suverénní a government klienty. Kromě toho služba Azure AD aktualizací, jako je LinkedIn integračních funkcí, se nenasadí na všechny klienty Azure ve stejnou dobu. Nelze povolit LinkedIn integraci s Azure AD, dokud nebude tato funkce byla vrácena ke klientovi Azure.

1. Přihlaste se k [centra pro správu Azure Active Directory](https://aad.portal.azure.com/) pomocí účtu, který je globální správce adresáře.
2. Vyberte **uživatelů a skupin**.
3. Na **uživatelů a skupin** vyberte **uživatelská nastavení**.
4. V části **LinkedIn integrace**, vyberte možnost Ano nebo ne k povolení nebo zakázání integrace LinkedIn.
   ![Povolení integrace LinkedIn](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="learn-more"></a>Další informace 
* [Informace o LinkedIn a funkcí ve vašich aplikacích společnosti Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centrum pro nápovědu LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Další kroky
Na následující odkaz můžete použít k povolení nebo zakázání LinkedIn integraci s Azure AD na portálu Azure:

[Konfigurace integrace LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 