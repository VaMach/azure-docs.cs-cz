---
title: "Převede uzel odkazy a Proxy aplikace Azure AD adresy URL | Microsoft Docs"
description: "Popisuje základní informace o Azure AD Application Proxy konektory."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: d5d704dac58d65dd7d62bc3eca400f9541714d5d
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Přesměrování pevně zakódované odkazy k aplikacím publikovaným pomocí proxy aplikace služby Azure AD

Proxy aplikace služby Azure AD umožňuje místní aplikace k dispozici pro uživatele, kteří jsou vzdálené nebo na jejich vlastní zařízení. Některé aplikace, ale měla vyvinuté pomocí místních odkazů vložených v kódu HTML. Tyto odkazy správně nefungují, pokud je aplikace používat vzdáleně. Pokud máte několik místní aplikace, přejděte na sobě navzájem, uživatelé očekávají, že na odkazy pokračovat v práci při jejich nejste v kanceláři. 

Ke konfiguraci externí adresy URL aplikací být stejné jako jejich interní adresy URL je nejlepší způsob, jak se ujistěte, že odkazy fungovat stejně uvnitř i vně podnikové sítě. Použití [vlastní domény](active-directory-application-proxy-custom-domains.md) nakonfigurovat tak, aby měl název vaší firemní domény místo výchozí domény proxy aplikace vašeho externí adresy URL.

Pokud nemůžete použít vlastní domény ve vašem klientovi, funkci překlad odkazů proxy aplikací zajišťuje vaše odkazy práce bez ohledu na to, kde jsou vaši uživatelé. Pokud máte aplikace, které přejděte přímo na vnitřních koncových bodů nebo porty, můžete namapovat tyto interní adresy URL publikované externí URL Proxy aplikace. Pokud je povoleno překlad odkaz, a Proxy aplikace hledá v kódu HTML a CSS publikované interní odkazy, služba Proxy aplikace přeloží je tak, aby uživatelé získají bez přerušení prostředí.

>[!NOTE]
>Funkce překladu odkaz je pro klienty, kteří ať důvodu, nelze použít vlastní domény tak, aby měl stejné interní a externí adresy URL pro svoje aplikace. Před povolením této funkce najdete v části Pokud [vlastních domén v Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) může fungovat pro vás.
>
>Nebo, pokud je aplikace, budete muset nakonfigurovat s odkazem překlad služby SharePoint, najdete v části [konfigurace mapování alternativních adres URL pro službu SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx) jiný způsob mapování odkazy.

## <a name="how-link-translation-works"></a>Jak funguje překlad propojení

Po ověření pokud proxy server předává data aplikací pro uživatele, Proxy aplikace vyhledá aplikace pro pevně zakódované odkazy a nahradí je jejich odpovídajících publikovaná externí adresy URL.

Proxy aplikace se předpokládá, že aplikace jsou kódování ve formátu UTF-8. Pokud není tento případ, zadejte typ kódování v hlavičce http odpovědi jako `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Odkazy, které se vztahuje?

Funkci překlad odkazů pouze vyhledá odkazy, které jsou v kódu značek v těle aplikace. Proxy aplikace má samostatné funkce pro překlad adres URL nebo soubory cookie v záhlaví. 

Existují dva typy běžné interní odkazů v místním aplikacím:

- **Relativní odkazy interní** , přejděte na příkaz sdíleného prostředku do místního souboru struktury jako `/claims/claims.html`. Tyto odkazy se automaticky fungovat v aplikacích, které jsou publikované prostřednictvím Proxy aplikace a pokračovat v práci s nebo bez překladu odkaz. 
- **Vnitřní propojení pevně zakódované** do jiných aplikací místní jako `http://expenses` nebo publikovat soubory, jako jsou `http://expenses/logo.jpg`. Funkci překlad odkazů na interní odkazy pevně zakódované funguje a je tak, aby odkazoval na externí adresy URL, které je třeba projít vzdálení uživatelé změní.

### <a name="how-do-apps-link-to-each-other"></a>Jak aplikace propojit k sobě navzájem?

Překlad odkaz je povolena pro každou aplikaci tak, že budete mít kontrolu nad činnost koncového uživatele na úrovni pro aplikaci. Zapnout překlad odkaz pro aplikaci, když chcete, aby odkazy *z* aplikaci k převodu, není odkazy *k* tuto aplikaci. 

Předpokládejme například, že máte tři aplikacích publikovaných prostřednictvím Proxy aplikace všech propojení mezi sebou: výhody, náklady a cesta. Je čtvrtá aplikaci a zpětnou vazbu, která není publikována prostřednictvím Proxy aplikace.

Když povolíte překlad odkaz pro aplikaci výhody, odkazy na výdaje a cesta přesměrování na externí adresy URL pro tyto aplikace, ale odkaz zpětná vazba není přesměrován, protože neexistuje žádná externí adresa URL. Nefungují odkazy z výdajů a cesta zpět na výhody, protože odkaz překlad nebyla povolena pro tyto dvě aplikace.

![Odkazy z výhody do jiných aplikací, pokud je povoleno překlad odkaz](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Odkazy, které nejsou přeloženy?

Pro zvýšení výkonu a zabezpečení, nejsou přeložit některé odkazy:

- Odkazy není uvnitř značky kódu. 
- Odkazy není v HTML nebo šablon stylů CSS. 
- Vnitřní propojení otevřít z jiných aplikací. Odkazy budou odesílat prostřednictvím e-mailu nebo pomocí rychlé zprávy nebo součástí jiné dokumenty, nebude možné přeložit. Uživatelé musí vědět, přejít na externí adresu URL.

Pokud potřebujete podporovat některý z těchto dvou scénářů, použijte místo odkaz překlad stejné interní a externí adresy URL.  

## <a name="enable-link-translation"></a>Povolit překlad odkaz

Začínáme s překlad odkaz je stejně snadná jako kliknutí na tlačítko:

1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Přejděte na **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** > vyberte aplikaci, kterou chcete spravovat > **proxy aplikace**.
3. Zapnout **překládat adresy URL v textu aplikace** k **Ano**.

   ![Vyberte možnost Ano překládat adresy URL v textu aplikace](./media/application-proxy-link-translation/select_yes.png).
4. Vyberte **Uložit** proveďte změny.

Teď když vaši uživatelé přístup k této aplikaci, proxy server automaticky vyhledá interní adresy URL, které byly publikovány prostřednictvím Proxy aplikace na klientovi.

## <a name="send-feedback"></a>Odeslat zpětnou vazbu

Chceme, abyste Ujistěte se, tato funkce fungovat pro všechny aplikace. Jsme hledání více než 30 značky v kódu HTML a CSS. Pokud máte příkladem generovaného odkazy, které nejsou se překlad vztahuje, pošlete fragment kódu do [zpětnou vazbu Proxy aplikací](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Další postup
[Použití vlastních domén s Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) mít stejnou interní a externí adresu URL

[Konfigurace mapování alternativních adres URL pro službu SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx)
