---
title: Azure Active Directory Identity Protection playbook | Microsoft Docs
description: "Zjistěte, jak Azure AD Identity Protection umožňuje omezit možnost útočník zneužít ohroženými identity nebo zařízení a zabezpečit identity nebo zařízení, která byla dříve by mohly vzbuzovat podezření nebo známé došlo k narušení."
services: active-directory
keywords: "ochrany identit Azure active directory, cloud app discovery,. Správa aplikací, zabezpečení, rizik, úroveň rizika, ohrožení zabezpečení, zásady zabezpečení"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 2ecd07faed785fa6aa179ac1cca35a70d965e1dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure seznam strategií ochrany identit Active Directory
Tato playbook vám pomůže:

* Naplnění dat v prostředí Identity Protection simulace rizikových událostech a ohrožení zabezpečení
* Nastavit zásady podmíněného přístupu na základě riziko a otestovat vliv těchto zásad

## <a name="simulating-risk-events"></a>Simulaci rizikových událostí
Tato část vám poskytne kroky pro simulaci následující typy událostí rizika:

* Přihlášení z anonymních IP adres (snadno)
* Přihlášení z neznámých míst (střední)
* Nemožná cesta do netypických míst (obtížné)

Další události riziko nelze simulated zabezpečeným způsobem.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Přihlášení z anonymních IP adres
Tento typ události riziko identifikuje uživatele, kteří mají úspěšném přihlášení z IP adresy, která byla zjištěna jako IP adresa anonymního proxy serveru. Tyto servery proxy jsou používány osob, které chcete skrýt IP adresu své zařízení a mohou být použity pro zlými úmysly.

**Pro simulaci přihlášení z anonymních IP adresy, proveďte následující kroky**:

1. Stažení [Tor prohlížeče](https://www.torproject.org/projects/torbrowser.html.en).
2. Pomocí prohlížeče Tor, přejděte na [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3. Zadejte přihlašovací údaje účtu, který se má zobrazit v **přihlášení z anonymních IP adres** sestavy.

Přihlášení se zobrazí na řídicím panelu ochrany identit během pěti minut. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Přihlášení z neznámých míst
Riziko neznámých míst je v reálném čase vyhodnocení přihlášení mechanismus, který zvažuje po přihlášení umístění (IP, zeměpisné šířky nebo zeměpisnou délku a ASN) k určení nového / neznámého umístění. Systém ukládá předchozí IP adresy, zeměpisné šířky nebo zeměpisnou délku a čísla ASN uživatele a tyto být známé umístění zvažuje. Umístění přihlášení je považován za obeznámeni, pokud umístění přihlášení neodpovídá žádnému z existující známé umístění.

Ochrany identit Azure Active Directory:  

* má po počáteční learning dobu 14 dnů, za které není příznak jiných umístění jako neznámých míst.
* ignoruje přihlášení ze známé zařízení a umístění, které jsou geograficky blízko existující známé umístění.

Pro simulaci neznámých míst, budete muset přihlásit z umístění a zařízení, které účet nebyl přihlášení z před. 

**Pro simulaci přihlášení z neznámého umístění, proveďte následující kroky**:

1. Vyberte účet, který má aspoň 14 dnů přihlášení historie. 
2. Proveďte:
   
   a. Při použití sítě VPN, přejděte na [https://myapps.microsoft.com](https://myapps.microsoft.com) a zadejte přihlašovací údaje účtu, kterou chcete simulovat riziko událost.
   
   b. Požádejte přidružení v jiném umístění se přihlásit pomocí přihlašovacích údajů účtu (nedoporučuje se).

Přihlášení se zobrazí na řídicím panelu ochrany identit během pěti minut.

### <a name="impossible-travel-to-atypical-location"></a>Nemožná cesta do netypických umístění
Simulaci podmínku neuskutečnitelná cesta je složité, protože algoritmus využívá strojové učení k odstraňování plevele na hodnotu false pozitivních třeba neuskutečnitelná cesta ze známé zařízení, nebo přihlášení z sítě VPN, které se používají jinými uživateli v adresáři. Kromě toho algoritmus vyžaduje 3 až 14 dní historii přihlášení pro uživatele před zahájením generování rizikových událostí.

**Pro simulaci nemožná cesta do netypických umístění, proveďte následující kroky**:

1. Pomocí standardní prohlížeč, přejděte do [https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Zadejte přihlašovací údaje účtu, pomocí kterého chcete generovat událost riziko neuskutečnitelná cesta pro.
3. Změna uživatelského agenta. Můžete změnit uživatelský agent v aplikaci Internet Explorer z nástrojů pro vývojáře, nebo změnit váš uživatelský agent v Firefox nebo Chrome pomocí doplňku přepínači uživatelského agenta.
4. Změníte IP adresu. Můžete změnit IP adresu pomocí sítě VPN, rozšíření Tor nebo otáčí nový počítač v Azure v různých datových center.
5. Přihlaste se do [https://myapps.microsoft.com](https://myapps.microsoft.com) pomocí stejných přihlašovacích údajů jako před a během několika minut po předchozí přihlášení.

Přihlášení se zobrazí v řídicím panelu ochrany identit v rámci 2 – 4 hodiny.<br>
Z důvodu složitých strojového učení modely související se situací je možné, že se ho nebude získat převzata.<br> Můžete chtít replikovat tyto kroky pro více účtů Azure AD.

## <a name="simulating-vulnerabilities"></a>Simulaci ohrožení zabezpečení
Ohrožení zabezpečení jsou slabá místa v prostředí Azure AD, které může zneužít objektu actor chybný. Aktuálně jsou prezentované 3 typy ohrožení zabezpečení v Azure AD Identity Protection, které využít další funkce služby Azure AD. Tyto chyby zabezpečení se zobrazí na řídicím panelu ochrany identit automaticky po tyto funkce jsou nastavené.

* Azure AD [služby Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="user-compromise-risk"></a>Riziko ohrožení zabezpečení pro uživatele
**K testování riziko ohrožení zabezpečení pro uživatele, proveďte následující kroky**:

1. Přihlaste se do [https://portal.azure.com](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.
2. Přejděte na **ochrany identit**. 
3. V hlavním **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**. 
4. Na **nastavení portálu** okno, v části **pravidla zabezpečení**, klikněte na tlačítko **riziko ohrožení zabezpečení uživatele**. 
5. Na **přihlášení riziko** okně zapnout **Povolit pravidlo** vypnout a potom klikněte na **Uložit** nastavení.
6. Pro daný uživatelský účet, simulovat neznámého umístění nebo anonymní IP riziko událost. To bude zvýšit úroveň rizika uživatele pro daného uživatele k **střední**.
7. Počkejte několik minut a ověřte, že úrovni uživatele pro vaše uživatele je **střední**.
8. Přejděte na **nastavení portálu** okno.
9. Na **riziko ohrožení zabezpečení uživatele** okno, v části **Povolit pravidlo**, vyberte **na** . 
10. Vyberte jednu z následujících možností:
    
    a. Blok, vyberte **střední** pod **bloku přihlásit**.
    
    b. Pokud chcete vynutit zabezpečené heslo změnit, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.
11. Klikněte na **Uložit**.
12. Nyní můžete otestovat podmíněného přístupu na základě riziko podle přihlášení pomocí uživatel s oprávněním vyšší úrovně rizika úrovní. Pokud je uživatel riziko střední, v závislosti na konfiguraci zásad, vaše přihlášení je buď zablokuje nebo je vynucen ke změně hesla. 
    <br><br>
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    <br>

## <a name="sign-in-risk"></a>Riziko přihlášení
**K testování znaménkem v riziko, proveďte následující kroky:**

1. Přihlaste se do [https://portal.azure.com ](https://portal.azure.com) pomocí přihlašovacích údajů globálního správce pro vašeho klienta.
2. Přejděte na **ochrany identit**.
3. V hlavním **Azure AD Identity Protection** okně klikněte na tlačítko **nastavení**. 
4. Na **nastavení portálu** okno, v části **pravidla zabezpečení**, klikněte na tlačítko **přihlášení riziko**.
5. Na ** přihlásit riziko ** vyberte **na** pod **Povolit pravidlo**. 
6. Vyberte jednu z následujících možností:
   
   a. Blokování, vyberte **střední** pod **bloku přihlášení**
   
   b. Pokud chcete vynutit zabezpečené heslo změnit, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.
7. Blokování, vyberte v části přihlášení bloku střední.
8. Chcete-li vynutit ověřování Multi-Factor authentication, vyberte **střední** pod **vyžadovat vícefaktorové ověřování**.
9. Klikněte na **Uložit**.
10. Nyní můžete otestovat podmíněného přístupu na základě riziko tak, že simuluje neznámých míst nebo anonymní IP riziko události, protože jsou oba **střední** rizik události.


![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")


## <a name="see-also"></a>Viz také
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md)

