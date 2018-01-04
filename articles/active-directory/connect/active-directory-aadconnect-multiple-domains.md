---
title: "Azure AD Connect více domén"
description: "Tento dokument popisuje nastavení a konfiguraci víc domén nejvyšší úrovně s O365 a Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: db4cfe91b8d27b5336763eff7c6f22f0f345caf2
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Podpora více domén pro federaci s Azure AD
Následující dokumentace obsahuje pokyny k použití více nejvyšší úrovně domén a subdomén při federaci s Office 365 nebo Azure AD domény.

## <a name="multiple-top-level-domain-support"></a>Podpora více domén nejvyšší úrovně
Federaci několika, domény nejvyšší úrovně s Azure AD vyžaduje určitou další konfiguraci, který není nutný, pokud federaci s jednu doménu nejvyšší úrovně.

Když domény federovaný pomocí služby Azure AD, několik vlastností, které jsou nastavené na doménu v Azure.  Jeden z nich důležité je IssuerUri.  Toto je identifikátor URI, který se používá Azure AD k identifikaci domény, který je přidružený token.  Identifikátor URI nepotřebuje přeložit na cokoli ale musí být platný identifikátor URI.  Ve výchozím nastavení, Azure AD Nastaví tato hodnota identifikátor federační služby v místní služby AD FS konfigurace.

> [!NOTE]
> Identifikátor služby FS je identifikátor URI, který jednoznačně identifikuje federační služby.  Služby federation service je instance služby AD FS, která slouží jako služba tokenů zabezpečení.
>
>

Můžete zobrazit pomocí příkazu Powershellu IssuerUri `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Problému dojde, pokud chcete přidat více než jedné domény nejvyšší úrovně.  Předpokládejme například, že máte instalace federace mezi službou Azure AD a místní prostředí.  K tomuto dokumentu používám bmcontoso.com.  Teď přidali domény druhé, nejvyšší úrovně, bmfabrikam.com.

![Domény](./media/active-directory-multiple-domains/domains.png)

Když jsme se pokusí převést naše bmfabrikam.com domény na federovanou, jsme k chybě.  Důvodem je, Azure AD má omezení, který neumožňuje vlastnost IssuerUri do mají stejnou hodnotu pro více než jedné domény.  

![Chyba Federation](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>Parametr SupportMultipleDomain
Chcete-li vyřešit, je potřeba přidat jiné IssuerUri, což lze provést pomocí `-SupportMultipleDomain` parametr.  Tento parametr se používá s následující rutiny:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Tento parametr umožňuje nakonfigurovat IssuerUri tak, aby je založen na názvu domény služby Azure AD.  Toto je jedinečný mezi adresářů ve službě Azure AD.  Pomocí parametru umožňuje příkaz prostředí PowerShell úspěšně dokončit.

![Chyba Federation](./media/active-directory-multiple-domains/convert.png)

Prohlížení nastavení naší nové bmfabrikam.com domény můžete v tomto tématu:

![Chyba Federation](./media/active-directory-multiple-domains/settings.png)

Všimněte si, že `-SupportMultipleDomain` nezmění ostatní koncové body, které jsou stále nakonfigurované tak, aby odkazoval na našem federační službu na adfs.bmcontoso.com.

Dalším krokem, `-SupportMultipleDomain` nemá je, že zajišťuje, že v systému služby AD FS obsahuje správnou hodnotu vystavitele v tokeny vydané pro Azure AD. Dělá to tak, že část domény uživatelů (UPN) Toto nastavení jako doménu v IssuerUri, tj. přípona https://{upn} / adfs/services/vztah důvěryhodnosti.

Proto při ověřování do služby Azure AD nebo Office 365, element IssuerUri v token uživatele se používá k vyhledání domény ve službě Azure AD.  Pokud odpovídající nelze nalézt, ověření se nezdaří.

Například pokud uživatele (UPN) je bsimon@bmcontoso.com, IssuerUri element v tokenu AD FS problémů bude nastavena pro http://bmcontoso.com/adfs/services/trust. To bude odpovídat konfiguraci Azure AD a ověřování bude úspěšný.

Toto je pravidlo vlastní deklarace identity, který implementuje tuto logiku:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Chcete-li použít přepínač - SupportMultipleDomain při pokusu přidat nové nebo převést již přidání domény, musíte mít vaše federovaného vztahu důvěryhodnosti, které je podporují původně instalační program.  
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Postup aktualizace vztah důvěryhodnosti mezi AD FS a Azure
Pokud jste nenastavili federovaný vztah důvěryhodnosti mezi AD FS a vaší instanci Azure AD, musíte znovu vytvořit tohoto vztahu důvěryhodnosti.  Důvodem je, že když je původně instalační program bez `-SupportMultipleDomain` parametr, výchozí hodnotou je nastaven IssuerUri.  Na snímku obrazovky níže můžete zobrazit, jestli že issueruri nastavený na https://adfs.bmcontoso.com/adfs/services/trust.

Takže teď, když jsme úspěšně přidali nové domény na portálu Azure AD a pak se pokusíte převést pomocí `Convert-MsolDomaintoFederated -DomainName <your domain>`, jsme zobrazí následující chyba.

![Chyba Federation](./media/active-directory-multiple-domains/trust1.png)

Pokud se pokusíte přidat `-SupportMultipleDomain` přepínač jsme se zobrazí následující chyba:

![Chyba Federation](./media/active-directory-multiple-domains/trust2.png)

Jednoduše pokusu o spuštění `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` v původní doméně také dojde chybě.

![Chyba Federation](./media/active-directory-multiple-domains/trust3.png)

Použijte uvedený postup pro přidání další domény nejvyšší úrovně.  Pokud jste již přidali domény a nepoužili `-SupportMultipleDomain` parametr začněte s kroky pro odebírání a aktualizace vašeho původního domény.  Pokud jste nepřidali doména nejvyšší úrovně ještě můžete začít s kroky pro přidání domény pomocí prostředí PowerShell Azure AD Connect.

Pomocí následujících kroků odeberte vztah důvěryhodnosti Microsoft Online a aktualizujte vaší původní doméně.

1. Na federačním serveru služby AD FS otevřete **Správa služby AD FS.**
2. Na levé straně, rozbalte položku **vztahy důvěryhodnosti** a **vztahy důvěryhodnosti předávající strany**
3. Na pravé straně, odstraňte **Microsoft Office 365 Identity Platform** položku.
   ![Odeberte Online Microsoft](./media/active-directory-multiple-domains/trust4.png)
4. Na počítači, který má [Azure Active Directory modul pro prostředí Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) nainstalován spusťte následující příkaz: `$cred=Get-Credential`.  
5. Zadejte uživatelské jméno a heslo pro globálního správce pro doménu služby Azure AD, které jsou federaci s
6. V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`
7. V prostředí PowerShell zadejte `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Je to pro původní doméně.  Proto pomocí výše uvedených domén, které má být:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

Použijte následující postup pro přidání nové domény nejvyšší úrovně pomocí prostředí PowerShell

1. Na počítači, který má [Azure Active Directory modul pro prostředí Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) nainstalován spusťte následující příkaz: `$cred=Get-Credential`.  
2. Zadejte uživatelské jméno a heslo pro globálního správce pro doménu služby Azure AD, které jsou federaci s
3. V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`
4. V prostředí PowerShell zadejte`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Použijte následující postup pro přidání nové domény nejvyšší úrovně pomocí služby Azure AD Connect.

1. Spusťte Azure AD Connect z plochy a nabídky start
2. Zvolte "Přidat další domény Azure AD" ![přidat další doménu služby Azure AD](./media/active-directory-multiple-domains/add1.png)
3. Zadejte služby Azure AD a pověření služby Active Directory
4. Vyberte druhý doménu, kterou chcete nakonfigurovat pro federaci.
   ![Přidat další doménu služby Azure AD](./media/active-directory-multiple-domains/add2.png)
5. Kliknutím na tlačítko Instalovat

### <a name="verify-the-new-top-level-domain"></a>Ověření nové domény nejvyšší úrovně
Pomocí příkazu prostředí PowerShell `Get-MsolDomainFederationSettings -DomainName <your domain>`můžete zobrazit aktualizované IssuerUri.  Následující snímek obrazovky ukazuje federace, nastavení bylo aktualizováno na našem původní http://bmcontoso.com/adfs/services/trust domény

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

A IssuerUri v naší nové domény byla nastavena na https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a>Podpora pro subdomén
Když přidáte subdomény, z důvodu domén způsobem Azure AD, které jsou zpracovány, zdědí nastavení nadřazeného objektu.  To znamená, že IssuerUri musí odpovídat nadřazené položky.

To umožňuje vyslovte například, že mám bmcontoso.com a poté přidejte corp.bmcontoso.com.  To znamená, že bude nutné IssuerUri pro uživatele z corp.bmcontoso.com **http://bmcontoso.com/adfs/services/trust.**  Ale standardní pravidlo implementována výše pro Azure AD, vygeneruje token s vystavitele jako **http://corp.bmcontoso.com/adfs/services/trust.** ověření se nezdaří a která nebude shodovala s doménou vyžadované hodnotu.

### <a name="how-to-enable-support-for-sub-domains"></a>Povolení podpory pro subdomén
Chcete-li tento problém obejít služby AD FS je třeba aktualizovat vztah důvěryhodnosti předávající strany pro Microsoft Online.  To pokud chcete udělat, musíte nakonfigurovat vlastní pravidlo deklarace identity, aby ji odstraní vypnout všechny subdomén od uživatele (UPN) příponu při vytváření vlastní hodnotu vystavitele.

Následující deklarace identity se to udělat:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
Poslední číslo v regulárním výrazu nastavit kolik nadřazené domény v kořenové doméně. I tady mají bmcontoso.com, takže jsou potřebné dvě nadřazené domény. V případě, že tři nadřazené domény byly budou muset zůstat (tj.: corp.bmcontoso.com), pak číslo by byl tři. Nakonec rozsah můžete zadat, shody bude vždy provést tak, aby odpovídala maximální počet domén. "{2,3}" bude odpovídat dvě až tři domény (např: bmfabrikam.com a corp.bmcontoso.com).

Pomocí následujících kroků přidáte vlastních deklarací identity pro podporu dílčím doménám domény.

1. Správa služby otevřete AD FS
2. Klikněte pravým tlačítkem na vztah důvěryhodnosti Microsoft Online RP a zvolte pravidla upravit deklarace identity
3. Vyberte třetí pravidlo deklarace identity a nahraďte ![úpravy deklarací identity](./media/active-directory-multiple-domains/sub1.png)
4. Nahraďte aktuální deklarace identity:

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));

       with

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Nahraďte deklarace identity](./media/active-directory-multiple-domains/sub2.png)

5. Klikněte na tlačítko Ok.  Kliknutím na tlačítko použít.  Klikněte na tlačítko Ok.  Zavřete správu služby AD FS.
