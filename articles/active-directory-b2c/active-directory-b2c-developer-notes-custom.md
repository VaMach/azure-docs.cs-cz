---
title: "Azure Active Directory B2C: Poznámky pro vývojáře na pomocí vlastních zásad | Microsoft Docs"
description: "Poznámky pro vývojáře o konfiguraci a údržbu Azure AD B2C pomocí vlastních zásad"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: a5f222e5b11e05286152a9f1cc55d2c3fc27a9dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Poznámky k verzi pro verzi public preview služby Azure Active Directory B2C vlastních zásad
Sada funkcí vlastních zásad je nyní k dispozici pro vyhodnocení v rámci verze public preview pro všechny Azure Active Directory B2C zákazníků (Azure AD B2C). Tato sada funkcí je cílena na vývojáře pokročilé identity vytváření nejvíce komplexní řešení identity.  

Tato sada funkcí v současné době vyžadují vývojářům konfigurovat rozhraní Identity prostředí přímo prostřednictvím úpravy souborů XML. Tato metoda konfigurace je výkonná a komplexní. Pokročilé vývojáře, kteří používají rozhraní Identity prostředí měli naplánovat investovat chvíli dokončení návodů a čtení dokumenty referenční identity. 

## <a name="features-included-in-this-public-preview"></a>Funkce obsažené v této verzi public preview
Nové funkce, zavedená ve verzi public preview vývojáři můžete provádět následující úlohy:<br>

* Autor a nahrání vlastní ověřování uživatele cesty pomocí vlastních zásad. 
   * Popis uživatelské cesty podrobné jako výměny mezi zprostředkovatelem deklarací identity. 
   * Definujte podmíněného větvení v cesty uživatele. 
* Integrate povoleno rozhraní REST API služby ve vaší vlastní ověřování uživatele cesty.  
* Přidejte federaci se zprostředkovatelů identity, které jsou kompatibilní s standardní OpenIDConnect. <br>
* Přidejte federaci se zprostředkovatelů identity, které dodržovat protokolu SAML verze 2.0. 

## <a name="terms-of-the-public-preview"></a>Podmínky verze public Preview

* Doporučujeme vám používat nové funkce jenom pro účely hodnocení.<br>
* Nové funkce nejsou určeny pro použití v provozním prostředí.<br>
* Smlouvy o úrovni služeb (SLA) se nevztahují na nové funkce. <br>
* Žádosti o podporu můžete podává prostřednictvím regulární podpora kanálů. <br>
* Neexistuje žádné přislíbeném datum pro obecnou dostupnost.<br>
* Naše uvážení a z jakéhokoli důvodu můžete Microsoft příznak a odmítnout nebo omezit scénáře a cesty uživatele, které překračují oboru listinou produktu Azure AD B2C sloužit jako platforma správy (CIAM) přístupu a identit zákazníka.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Odpovědnosti vlastních zásad pro sadu funkcí vývojářů
Konfigurace zásad ruční uděluje nižší úrovně přístupu pro základní platformu Azure AD B2C a má za následek vytvoření rozhraní jedinečný plně přizpůsobitelná vztah důvěryhodnosti. Počet možných kombinací zprostředkovatelů vlastní identity, vztahy důvěryhodnosti, integrace s externích služeb a podrobné pracovních umístit větší nároky na pokročilé vývojáře využívají je.

Chcete-li plně využívat výhod verzi public preview, doporučujeme, aby vývojáři využívání sadu funkcí vlastní zásady dodržovat následující pokyny:
* Seznamte se s jazykem konfigurace modulu prostředí Identity a řízení tajné klíče nebo klíče.
* Převzít vlastnictví scénáře a vlastní integrace.
* Proveďte testování metodický scénář.
* Postupujte podle vývoj softwaru a pracovní osvědčené postupy s minimálně jeden vývoj a testování prostředí a jeden produkčního prostředí.
* Udržení informovanosti o novém vývoji z poskytovatelů identit a službách, které můžete integrovat. Například sledovat určité změny v tajných klíčů a plánovaných a neplánovaných změn ke službě.
* Nastavte aktivní monitorování a sledování odezvy provozní prostředí.
* Zachovat aktuální kontaktní e-mailové adresy a zůstat reaguje na e-mailů team Web live společnosti Microsoft.
* Trvat včas akce při nedoporučuje Uděláte to tak tým Microsoft live-site. 


>[!NOTE]
>Tyto funkce možná časem zahrneme v integrovaných zásad služby Azure AD, přitom přístupnější pro všechny vývojáře.

## <a name="next-steps"></a>Další kroky
[Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md).
