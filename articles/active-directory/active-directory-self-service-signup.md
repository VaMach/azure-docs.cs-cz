---
title: "Co je Samoobslužná registrace pro Azure? | Dokumentace Microsoftu"
description: "Přehled samoobslužné registrace pro Azure, jak spravovat proces registrace a jak převzít kontrolu nad název domény DNS."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 511088156d3546e2e0f3ac40e72bf2b8e4ae2cb9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-self-service-signup-for-azure"></a>Co je Samoobslužná registrace pro Azure?
Toto téma vysvětluje proces samoobslužné registrace a postup převzít kontrolu nad název domény DNS.  

## <a name="why-use-self-service-signup"></a>Proč používat samoobslužné registrace?
* Získejte zákazníků ke službám, které mají být rychlejší.
* Vytvořte e mailové nabídky pro službu.
* Vytvořte e mailové registrace toky, které rychle povolit uživatelům vytvářet identit pomocí jejich aliasy snadno zapamatovat pracovní e-mailu.
* Nespravované Azure adresáře mohou být provedeny později do spravovaných adresáře a znovu použít pro jiné služby.

## <a name="terms-and-definitions"></a>Termíny a definice
* **Samoobslužná registrace do**: Toto je metoda, podle kterého uživatel zaregistruje cloudovou službu a má identitu automaticky vytvoří pro ně v Azure Active Directory (Azure AD), na základě jejich e-mailovou doménu.
* **Nespravované Azure directory**: Toto je adresář, kde se má vytvořit danou identitu. Nespravované adresáře se adresář, který má žádný globální správce.
* **Ověřit e-mailu uživatel**: Jedná se o typ uživatelského účtu ve službě Azure AD. Uživatel, který má identity vytvoří automaticky po registraci na nabídku samoobslužné služby se označuje jako uživatel s ověřenou e-mailu. Ověření e-mailu uživatel je členem regulární adresáře označené creationmethod = EmailVerified.

## <a name="user-experience"></a>Činnost koncového uživatele
Předpokládejme například, že uživatele, jejichž e-mailu je Dan@BellowsCollege.com obdrží citlivé soubory e-mailem. Soubory chráněné pomocí Azure Rights Management (Azure RMS). Ale Dana pro organizaci, univerzity, kterou vlnovce, nebyl zaregistrovali do služby Azure RMS, ani nasadil Active Directory RMS. V takovém případě Dana můžete si zaregistrovat bezplatné předplatné RMS pro jednotlivce aby bylo možné číst chráněné soubory.

Pokud Dana první uživatele s e-mailovou adresu z BellowsCollege.com zaregistrovat tento samoobslužný nabídky, pak nespravované adresář bude vytvořen pro BellowsCollege.com ve službě Azure AD. Pokud jiných uživatelů z domény BellowsCollege.com registraci této nabídky nebo nabídku podobné samoobslužné služby, budou mít i e-mailu ověřit uživatelské účty vytvořené ve stejném adresáři nespravované v Azure.

## <a name="admin-experience"></a>Z pohledu správce
Správce, který vlastní název domény DNS adresář nespravované Azure můžete převzít kontrolu nad nebo sloučení adresáři po prokázání vlastnictví. Další části popisují pohledu správce podrobněji, ale tady je Shrnutí:

* Pokud převezmete adresář nespravované Azure, jednoduše stát globální správce adresáře, nespravované. Někdy se označuje jako interní převzetí.
* Pokud sloučení adresář Azure nespravované, přidejte název domény DNS nespravované adresáře do adresáře spravované Azure a vytvoření mapování uživatelů prostředků proto uživatelé nadále pro přístup ke službám bez přerušení. Někdy se označuje jako externí převzetí.

## <a name="what-gets-created-in-azure-active-directory"></a>Co se vytvoří v Azure Active Directory?
#### <a name="directory"></a>Adresář
* Adresář služby Azure Active Directory pro doménu je vytvořen, jeden adresář v každé doméně.
* Adresář Azure AD má žádné globální správce.

#### <a name="users"></a>Uživatelé
* Pro každého uživatele, která se zaregistruje je vytvořen objekt uživatele v adresáři služby Azure AD.
* Každý objekt uživatele je označena jako externí.
* Každý uživatel přístup ke službě, která jejich registraci aplikace.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Jak deklarace samoobslužné služby Azure AD adresář pro doménu I vlastní?
Samoobslužné služby Azure AD můžete deklarace adresář tak, že provedete ověření domény. Ověření domény prokáže, že jste vlastníkem domény tak, že vytvoříte záznamy DNS.

Existují dva způsoby, jak provést převzetí DNS z adresáře služby Azure AD:

* interní převzetí (Správce zjistí adresář nespravované Azure a chce zapnout do spravovaných adresáře)
* externí převzetí (správce pokusí přidat novou doménu do jejich spravované Azure directory)

Může zajímat ověření vlastní domény, protože adresář nespravované jsou převzetí po uživatel provedl samoobslužné registrace, nebo může být přidání nové domény do existujícího spravovaného adresáře. Například máte doménu s názvem contoso.com a chcete přidat novou doménu s názvem contoso.co.uk nebo contoso.uk.

## <a name="what-is-domain-takeover"></a>Co je převzetí domény?
Tato část obsahuje informace o ověření, že jste vlastníkem domény

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Co je ověření domény a proč slouží?
Abyste mohli provádět operace v adresáři, Azure AD vyžaduje, abyste ověřili vlastnictví domény DNS.  Ověření domény umožňuje deklarace identity adresář a buď zvýšit úroveň adresář samoobslužné služby, který má spravovaný adresář nebo sloučení adresáři samoobslužných služeb do existujícího spravovaného adresáře.

## <a name="examples-of-domain-validation"></a>Příklady ověření domény
Existují dva způsoby, jak provést převzetí DNS adresáře:

* interní převzetí (například správce zjistí adresář samoobslužné služby, nespravované a chce zapnout do spravovaných adresáře)
* externí převzetí (například k oprávnění správce se pokusí přidat novou doménu do spravovaných adresáře)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Interní převzetí - podporovat samoobslužné služby, nespravované adresáři, který bude spravovaný adresář
Když provedete interní převzetí, získá adresáři převést z adresář nespravované na spravované adresáře. Je potřeba provést ověření názvu domény DNS, kde v zóně DNS vytvořit záznam MX nebo záznam TXT. Tato akce:

* Ověří, že jste vlastníkem domény
* Umožňuje spravovat adresář
* Umožňuje vám globální správce adresáře

Řekněme, že správce IT z vlnovce univerzity, kterou zjistí, že uživatelé z školy zaregistrovali pro samoobslužné služby nabídky. Jako registrovaný vlastník DNS název BellowsCollege.com, správce IT může ověřit vlastnictví název DNS v Azure a pak proveďte přes adresáři nespravované. Adresář se pak stane spravovaný adresář a správce IT má přiřazenou roli globálního správce pro adresář BellowsCollege.com.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externí převzetí - sloučit adresář samoobslužné služby do existující spravované adresář
V externí převzetí již máte spravovaný adresář a chcete, aby všichni uživatelé a skupiny z nespravovaných adresáře pro připojení k tomuto adresáři spravované místo vlastní dva samostatné adresáře.

Jako správce adresáře spravované přidání domény a domény se stane, tak, aby měl adresář nespravované s ním spojená.

Řekněme například, jsou správce IT a už máte spravovaný adresář pro doménu Contoso.com, název domény, které je registrované pro vaši organizaci. Zjistíte, že uživatelé z vaší organizace provedli Samoobslužná registrace až pro nabídky pomocí doménového názvu e-mailu user@contoso.co.uk, což je jiný název domény, který vaše organizace vlastní. Tito uživatelé nyní mají účty v nespravované adresář pro contoso.co.uk.

Nechcete spravovat dva samostatné adresáře, aby sloučit nespravované adresář pro contoso.co.uk do existujícího adresáře spravované IT pro doménu contoso.com.

Externí převzetí postupuje stejně DNS ověření jako interní převzetí.  Přičemž rozdíl: uživatelů a služeb jsou mapovány na IT spravovat adresář.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Co je dopad provedení externí převzetí?
S externí převzetí se vytvoří mapování uživatelů prostředků, uživatelé mohou i nadále přístup ke službám bez přerušení. Mnoho aplikací, včetně služby RMS pro jednotlivce, zpracování a mapování uživatelů prostředků a uživatelé mohou i nadále přístup k těchto služeb bez změn. Pokud aplikace efektivně nezpracovává mapování uživatelů na prostředky, může být externí převzetí explicitně blokována tak, aby uživatelé z nízký prostředí.

#### <a name="directory-takeover-support-by-service"></a>Podpora převzetí Directory službou
Následující služby v současné době podporují převzetí:

* RMS

Následující služby bude brzy k dispozici podpora převzetí:

* PowerBI

Následující nepodporují a vyžadovat další správce akce k migraci dat uživatele po externí převzetí.

* SharePoint nebo OneDrive

## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Postup převzetí název domény DNS
Máte několik možností k provedení ověření domény (a pokud chcete provést převzetí):

1. Portál pro správu Azure

   Pomocí tohoto postupu přidání domény převzetím aktivaci.  Pokud adresář již existuje pro doménu, budete mít možnost provádět externí převzetí.

   Přihlaste se k portálu Azure pomocí svých přihlašovacích údajů.  Přejděte na existující adresář a potom na **přidáním domény**.
2. Office 365

   Použijete možnosti na [spravovat domény](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) stránky v Office 365 pro práci s doménách a záznamech DNS. V tématu [ověřte svoji doménu v Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).
3. Windows PowerShell

   Následující kroky jsou potřebná k provedení ověřování pomocí prostředí Windows PowerShell.

   | Krok | Pomocí rutiny |
   | --- | --- |
   | Vytvořit objekt přihlašovacích údajů |Get-Credential |
   | Připojení k Azure AD |Connect-MsolService |
   | získat seznam domén |Get-MsolDomain |
   | Vytvoření výzvu |Get-MsolDomainVerificationDns |
   | Vytvořit záznam DNS |To udělat na serveru DNS |
   | Ověření výzvy |Confirm-MsolEmailVerifiedDomain |

Například:

1. Připojení k Azure AD pomocí přihlašovacích údajů, které jste použili k reagovat na nabídku samoobslužné služby:

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. Získáte seznam domén:

    Get-MsolDomain
3. Poté spusťte rutinu Get-MsolDomainVerificationDns vytvořit výzvu:

    Get-MsolDomainVerificationDns – DomainName *your_domain_name* – DnsTxtRecord režimu

    Například:

    Get-MsolDomainVerificationDns – DomainName contoso.com – DnsTxtRecord režimu
4. Zkopírujte hodnotu (výzva), která je vrácena z tohoto příkazu.

    Například:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9
5. V oboru názvů veřejné DNS vytvořte záznam DNS txt, který obsahuje hodnotu, kterou jste zkopírovali v předchozím kroku.

    Název pro tento záznam je název nadřazené domény, tak pokud vytvoříte tento záznam o prostředku pomocí role DNS ze systému Windows Server, ponechte název prázdné, jenom vložení záznamu hodnotu do textového pole
6. Spusťte rutinu potvrdit MsolDomain ověření výzvy:

    Potvrďte MsolEmailVerifiedDomain - DomainName *your_domain_name*

    Například:

    Potvrďte MsolEmailVerifiedDomain - DomainName contoso.com

Úspěšné výzvy se vrátíte do příkazového řádku bez chyby.

## <a name="how-do-i-control-self-service-settings"></a>Jak řídit nastavení samoobslužné služby?
Správci mají dvou ovládacích prvků samoobslužné služby ještě dnes. Kontroly nad:

* Zda uživatelé mohou připojit adresáři e-mailem.
* Zda uživatelé mohou licence sami pro aplikace a služby.

### <a name="how-can-i-control-these-capabilities"></a>Jak můžete řídit tyto funkce?
Správce můžete nakonfigurovat tyto možnosti, pomocí těchto parametrů rutiny Set-MsolCompanySettings Azure AD:

* **AllowEmailVerifiedUsers** Určuje, jestli uživatel může vytvořit nebo připojit k nespravované adresáři. Pokud tento parametr nastavte na $false, žádní uživatelé ověřit e-mailu se můžete zapojit do adresáře.
* **AllowAdHocSubscriptions** řídí schopnost uživatelům provádět Samoobslužná registrace nahoru. Pokud tento parametr nastavte na $false, žádní uživatelé provést samoobslužné registrace.

### <a name="how-do-the-controls-work-together"></a>Jak ovládací prvky spolupracují?
Tyto dva parametry můžete použít ve spojení definovat přesnější kontrolu nad Samoobslužná registrace až. Například následující příkaz umožní uživatelům proveďte Samoobslužná registrace, ale pouze v případě, že tito uživatelé již máte účet ve službě Azure AD (jinými slovy, uživatele, kteří se potřebují účet ověřit e-mailu, který chcete vytvořit nelze provést Samoobslužná registrace nahoru):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Následující vývojový diagram vysvětluje všechny různé kombinace pro tyto parametry a výsledný podmínky k adresáři a Samoobslužná registrace nahoru.

![][1]

Další informace a příklady použití těchto parametrů najdete v tématu [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="see-also"></a>Viz také
* [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Referenční informace k rutinám Azure](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
