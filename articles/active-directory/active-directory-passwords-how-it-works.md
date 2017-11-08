---
title: Jak to funguje? Azure AD SSPR | Microsoft Docs
description: "Podrobné informace pro vytvoření nového hesla samoobslužné služby Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: fd9515120049dd3837a43c95de8a9b6822719e19
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Samoobslužné služby v Azure AD podrobné informace pro vytvoření nového hesla

Jak funguje SSPR? Co tato možnost znamená v rozhraní? Materiály pro další informace o hesla pomocí samoobslužné služby Azure AD resetovat.

## <a name="how-does-the-password-reset-portal-work"></a>Jak resetování hesla portálu pracovní

Když uživatel přejde na portál resetovat heslo, pracovní postup je spuštěna Chcete-li zjistit:

   * Jak by měly být lokalizovány stránky?
   * Je platný uživatelský účet?
   * Jaké organizace uživatel nepatří do?
   * Kde je spravované heslo uživatele?
   * Je daný uživatel licenci pro použití funkce?


Čtení následujícími kroky Další informace o logiku za heslo resetovat stránky.

1. Uživatel klikne na nelze přístup k vašemu účtu odkaz nebo přejde přímo na [https://aka.ms/sspr](https://passwordreset.microsoftonline.com).
2. Založené na prohlížeči národní prostředí vykresluje v příslušné jazykové možnosti. Prostředí resetování hesla je lokalizované do stejné jazyky jako podporuje Office 365.
3. Uživatel zadá ID uživatele a předá test captcha.
4. Azure AD ověří, zda je uživatel moci pomocí této funkce pomocí tohoto postupu následující kontroly:
   * Kontroluje, zda uživatel má povolení této funkce a přiřazenou licenci Azure AD.
     * Pokud uživatel nemá povolení této funkce nebo přiřazenou licenci, je uživatel požádán o obrátit na správce svého resetovat heslo.
   * Kontroluje, zda má uživatel práva challenge dat definovaných na svůj účet v souladu se zásadami správce.
     * Pokud zásady vyžaduje pouze jednu výzvu, pak je je zajistit, že uživatel má příslušná data definované pro minimálně jeden z problémů ve zásad správce povolené.
       * Pokud uživatel není nakonfigurováno, pak uživatel by měl obrátit na správce svého resetovat heslo.
     * Pokud zásady vyžaduje dva problémy, pak je je zajistit, že uživatel má příslušná data definované pro alespoň dvě z problémů ve zásad správce povolené.
       * Pokud uživatel není nakonfigurován, je jsme uživatele doporučujeme na obrátit na správce svého resetovat heslo.
   * Kontroluje, pokud je pro uživatele heslo spravovaný místně (federovaný nebo hodnoty hash hesla synchronizovat).
     * Pokud je nasazený zpětný zápis a heslo uživatele je spravovaný místně, uživatel je moci pokračovat k ověření a obnovit své heslo.
     * Pokud není nasazena zpětný zápis a heslo uživatele je spravovaný místně, je uživatel požádán o obrátit na správce svého resetovat heslo.
5. Pokud je zjištěno, že je uživatel moct úspěšně obnovit své heslo, pak může uživatel v průvodci procesem resetování.

## <a name="authentication-methods"></a>Metody ověřování

Pokud je povolená funkce samoobslužného resetování hesla (SSPR), musíte vybrat alespoň jeden z následujících možností pro metody ověřování. Někdy může dojít v tyto možnosti se označuje jako brány. Důrazně doporučujeme vybrat alespoň dvě metody ověřování, tak, aby vaši uživatelé větší flexibilitu.

* E-mail
* Mobilní telefon
* Telefon do kanceláře
* Bezpečnostní otázky

![Ověřování][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-authentication-data"></a>Jaké pole se používají v adresáři pro ověřování dat

* Telefon do kanceláře odpovídá telefonní číslo do kanceláře
    * Uživatelé se nemohou tuto možnost nastavíte pole sami musí být definován správcem
* Mobilní telefon odpovídá telefon pro ověření (nejsou viditelné veřejně) nebo mobilní telefon (veřejně viditelný)
    * Služby nejprve hledá telefon pro ověření, pak spadne zpět na mobilní telefon není-li k dispozici
* Alternativní e-mailová adresa odpovídá ověřování e-mailu (nejsou viditelné veřejně) nebo alternativní e-mailu
    * Služby nejprve hledá ověřování e-mailu a pak dojde k navrácení alternativní e-mailu

Ve výchozím nastavení jsou synchronizovány pouze cloudu atributy telefonní číslo do kanceláře a mobilní telefon do cloudového adresáře z vašeho místního adresáře pro data ověřování.

Uživatelé mohou pouze obnovit své heslo, pokud mají data obsažená v metod ověřování, které správce povolil a vyžaduje.

Pokud uživatelé nechcete, aby jejich číslo mobilního telefonu viditelné v adresáři, ale přesto chcete použít pro resetování hesla, Správci by neměl naplnit v adresáři a uživatel by měl naplnit jejich **telefon pro ověření** atribut prostřednictvím [portál pro registraci a resetování hesla](http://aka.ms/ssprsetup). Správci uvidí tyto informace v profilu uživatele, ale nebude publikován jinde.

### <a name="number-of-authentication-methods-required"></a>Počet požadované metody ověřování

Tato možnost určuje minimální počet dostupné metody ověření nebo brány, uživatel musí projít resetovat nebo jejich heslo pro odemknutí a lze ho nastavit na hodnotu 1 nebo 2.

Uživatelé mohou k poskytování další metody ověřování, pokud jsou povolené správcem.

Pokud uživatel nemá minimální požadované metody zaregistrovaný, zobrazí se jim chybovou stránku, která přesměruje je, aby vyžadovala resetovat heslo správce.

#### <a name="changing-authentication-methods"></a>Změna metody ověřování

Pokud začnete se zásadu, která obsahuje pouze jednu metodu ověřování požadované pro resetovat nebo odemknout zaregistrován a změníte, že ke dvěma co se stane?

| Počet metody zaregistrován | Počet metod požadovaných | výsledek |
| :---: | :---: | :---: |
| 1 nebo více | 1 | **Možnost** resetovat nebo odemknutí |
| 1 | 2 | **Nelze** resetovat nebo odemknutí |
| 2 nebo více | 2 | **Možnost** resetovat nebo odemknutí |

Pokud změníte typy metod ověřování, zda může uživatel používat můžete nechtěně přestat uživatelé nebudou moci používat SSPR, pokud nemají minimální množství dat, které jsou k dispozici.

Příklad: 
1. Původní zásady nakonfigurované s 2 metodami ověřování vyžaduje použití pouze office phone a bezpečnostní otázky. 
2. Správce změní zásady tak, aby už použít bezpečnostní otázky, ale povolí používání mobilního telefonu a alternativní e-mailu.
3. Uživatelé bez mobilní telefon a naplněno pole alternativní e-mailovou nelze resetovat jejich hesla.

### <a name="how-secure-are-my-security-questions"></a>Do jaké míry jsou mé bezpečnostní otázky

Pokud chcete použít bezpečnostní otázky, doporučujeme, abyste je používán pomocí jiné metody, které se méně bezpečné než jiné metody vzhledem k tomu, že někteří uživatelé mohou znát odpovědi na otázky jiným uživatelem.

> [!NOTE] 
> Bezpečnostní otázky jsou uloženy a s lepším zabezpečením na objekt uživatele v adresáři a pouze může být odpovědi uživatele během registrace. Neexistuje žádný způsob pro správce pro čtení nebo upravit uživatele otázek a odpovědí.
>

### <a name="security-question-localization"></a>Lokalizace bezpečnostní otázku

Všechny předdefinované otázky, které následují lokalizace do úplnou sadu Office 365 jazyky podle národního prostředí prohlížeče uživatele.

* V jakém městě jste potkali svého manžela nebo manželku (partnera nebo partnerku)?
* V jakém městě se potkali vaši rodiče?
* V jakém městě žije váš nejbližší sourozenec?
* V jakém městě se narodil váš otec?
* V jakém městě jste měli první práci?
* V jakém městě se narodila vaše matka?
* V jakém městě jste byli na Nový rok 2000?
* Jaké je příjmení vašeho oblíbeného učitele ze vysokou * školy?
* Jaký je název univerzity, na kterou jste se hlásili, ale nechodili na ni?
* Jak se jmenuje město, kde jste měli svatební hostinu?
* Jaký je oblíbený sport vašeho otce?
* Jaké je vaše oblíbené jídlo?
* Jaké je jméno a příjmení vaší babičky z matčiny strany?
* Jak se za svobodna jmenovala vaše matka?
* Co je narozeninách měsíci a roce vašeho nejstaršího sourozence? (Příklad: listopad 1985)
* Jaké je křestní jméno vašeho nejstaršího sourozence?
* Jaké je jméno a příjmení vašeho dědečka z otcovy strany?
* Jaké je křestní jméno vašeho nejmladšího sourozence?
* Do jaké školy jste chodili v šesté třídě?
* Jaké měl jméno a příjmení váš nejlepší přítel v dětství?
* Jaké měl jméno a příjmení váš první partner nebo partnerka?
* Jaké bylo příjmení vašeho oblíbeného učitele ze základní školy?
* Jaká byla značka a model vašeho prvního auta nebo motorky?
* Jaký byl název první školy, do které jste chodili?
* Jaký název měla nemocnice, ve které jste se narodili?
* Jak se jmenovala ulice, kde jste v dětství bydleli?
* Jak se jmenoval váš dětský idol?
* Jaké bylo jméno vašeho oblíbeného plyšáka?
* Jak se jmenovalo vaše první domácí zvířátko?
* Jakou jste měli v dětství přezdívku?
* Jaký byl váš oblíbený sport na střední škole?
* Jaká byla vaše první práce?
* Jaké byly poslední čtyři číslice vašeho telefonu v dětství?
* Jaké bylo v dětství vaše vysněné povolání?
* Jakou nejznámější osobnost jste kdy potkali?

### <a name="custom-security-questions"></a>Vlastní bezpečnostní otázky

Vlastní bezpečnostní otázky nejsou lokalizovány pro různá národní prostředí. Všechny vlastní otázky se zobrazují ve stejném jazyce, které jsou zapsány v rozhraní správce i v případě národního prostředí uživatele prohlížeče se liší. Pokud potřebujete lokalizovaná otázky, použijte prosím předdefinované dotazy.

Maximální délka vlastní bezpečnostní otázku je 200 znaků.

### <a name="security-question-requirements"></a>Požadavky na zabezpečení otázku

* Minimální odpovědí limit pro počet znaků je 3 znaky
* Odpověď maximální limit pro počet znaků je 40 znaků
* Uživatelé nemusí odpovězte na stejné otázku více než jednou.
* Uživatelé nemusí poskytovat stejné odpověď na otázku více než jeden
* Všechny znakovou sadu může použít k definování otázky a odpovědi, včetně znaky kódování Unicode
* Počet otázek definovaných musí být větší než nebo rovna počtu otázek vyžadovaných k registraci

## <a name="registration"></a>Registrace

### <a name="require-users-to-register-when-signing-in"></a>Při přihlášení vyžadovat registraci uživatelů

Povolením této možnosti vyžaduje, že uživatel, který je povolen pro resetování hesel k dokončení heslo resetovat registrace, pokud se přihlášení do aplikace pomocí služby Azure AD se přihlásit jako ty, které následují:

* Office 365
* portál Azure
* Přístupový panel
* Federovaným aplikacím
* Vlastních aplikací pomocí služby Azure AD

Pokud je toto zakázáno uživatelé můžou pořád spustit ručně zaregistrovat své kontaktní informace navštivte stránky [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) nebo kliknutím **zaregistrovat pro resetování hesla** odkaz na kartě profil v přístupový panel.

> [!NOTE]
> Uživatelé portálu pro registraci resetování hesla můžete zavřít kliknutím na tlačítko Storno nebo zavřením okna ale výzvy pokaždé, když se přihlásit, dokud nebude po dokončení registrace.
>
> Pokud jsou již přihlášení tím nebude přerušení připojení uživatele.

### <a name="number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Počet dní před vyzváním uživatelů k potvrzení ověřovacích informací

Tato možnost určuje dobu, po mezi nastavení a reconfirming informace o ověřování a je k dispozici, pokud povolíte pouze **vyžadovat, aby registraci při přihlašování uživatelé** možnost.

Platné hodnoty jsou 0-730 dní s 0 znamená nikdy požádejte uživatele o potvrzení jejich informace o ověřování

## <a name="notifications"></a>Oznámení

### <a name="notify-users-on-password-resets"></a>Upozornit uživatele na resetování hesla

Pokud tato možnost nastavená na Ano, uživatele, který je resetování hesla obdrží e-mail s upozorněním, že jejich heslo bylo změněno prostřednictvím portálu SSPR k jejich primární a alternativní e-mailové adresy na souboru ve službě Azure AD. Nikdo jiný po upozornění na resetování této události.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Upozornit všechny správce, když jiní správci resetování hesel

Pokud je tato možnost nastavená na hodnotu Ano, pak **všichni správci** obdrží e-mail na jeho primární e-mailovou adresu v souboru ve službě Azure AD, které jim, že jiný správce došlo ke změně hesla pomocí SSPR.

Příklad: Existují čtyři správci v prostředí. Resetuje heslo pomocí SSPR správce "A". Správci B, C a D dostane e-mail výstrahy je této situaci.

## <a name="on-premises-integration"></a>Místní integrace

Pokud jste nainstalovali, nakonfigurovat a povolili Azure AD Connect, máte následující další možnosti pro místní integrace. Pokud tyto možnosti jsou aktivní na více systémů a zpětný zápis nebyla nakonfigurována správně [konfigurace zpětný zápis hesla](active-directory-passwords-writeback.md#configuring-password-writeback) Další informace.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Zpětný zápis hesla do místního adresáře

Řídí, zda je povolen zpětný zápis hesla pro tento adresář a pokud zpětného zápisu na, označuje stav zpětného zápisu služby místně. To je užitečné, pokud chcete dočasně zakázat zpětný zápis hesla bez nutnosti měnit Azure AD Connect.

* Pokud přepínač je nastavena na hodnotu Ano, pak zpětný zápis povolen a federovaných a mohou uživatelé hash synchronizovat heslo resetovat jejich hesla.
* Pokud je přepínač nastavený Ne, pak zpětný zápis je zakázána a federovaných a uživatelé hash synchronizovat hesla nejsou moct resetovat jejich hesla.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Povolit uživatelům bez resetování hesla pro odemknutí účtů

Označuje, zda uživatelé, kteří navštíví portálu pro resetování hesla by měla mít možnost k odemčení svých místních účtů služby Active Directory bez resetování hesla. Ve výchozím nastavení Azure AD odemkne účty při resetování hesla, toto nastavení umožňuje oddělit těchto dvou operací. 

* Pokud nastavíte hodnotu "Ano", bude nabídnuta možnost obnovit své heslo a odemknout účet, nebo k odemčení bez resetování hesla.
* Pokud nastavena na hodnotu "Ne", pak jsou jen mít uživatelé provést kombinovaný heslo pro obnovení a operaci odemknutí účtu.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Jak resetování hesla práce B2B uživatelů?
Resetování hesla a změny jsou plně podporovány s všechny konfigurace B2B. V následujících třech případech jsou podporovány pro resetování hesla uživatele B2B.

1. **Uživatelé z partner org s existujícího klienta Azure AD** – Pokud má organizace jsou partnerství se existujícího klienta Azure AD, jsme **respektují aktivují zásady resetování hesel jsou povolené v něm**. Pro heslo resetovat pro práci partnera jenom potřeb organizace a ujistěte se, Azure AD SSPR je povoleno, které je bez dalších poplatků pro zákazníky O365, a lze je povolit pomocí následujících kroků v našem [Začínáme se správou hesel](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords)průvodce.
2. **Uživatelé, kteří zaregistrovali do služby pomocí [samoobslužné registrace](active-directory-self-service-signup.md)**  – Pokud organizace, která jsou partnerství se použité [samoobslužné registrace](active-directory-self-service-signup.md) funkci nahrát do klienta, jsme by jim umožnila resetován s e-mailu, budou registrována.
3. **Uživatelé B2B** – všechny nové uživatele B2B vytvořené pomocí nové [funkce Azure AD s B2B](active-directory-b2b-what-is-azure-ad-b2b.md) bude také moct resetovat vlastní hesla s e-mailu, budou během procesu pozvání zaregistrovány.

Tento scénář otestovat, přejděte na http://passwordreset.microsoftonline.com s jedním z těchto uživatelů partnera. Tak dlouho, dokud budou mít alternativní e-mailu nebo e-mailové ověřování definované, resetování hesla funguje podle očekávání.

> [!NOTE]
> Účty Microsoft, kterým byl udělen přístup hosta do služby Azure AD klienta jsou z Hotmail.com, Outlook.com, nebo jiné osobní e-mailové adresy nejsou moci používat Azure AD SSPR a bude nutné resetovat heslo pomocí informací v nalezen článek [při nemůžete se přihlásit ke svému účtu Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).

## <a name="next-steps"></a>Další kroky

Na následujících odkazech najdete další informace o resetování hesla pomocí Azure AD

* [Jak dokončit úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetování nebo změna hesla](active-directory-passwords-update-your-own-password.md)
* [Registrace samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Máte dotaz k licencování?](active-directory-passwords-licensing.md)
* [Jaká data používá SSPR a která data byste měli naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Které metody ověřování jsou dostupné pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad se SSPR?](active-directory-passwords-policy.md)
* [Co je zpětný zápis hesla a proč byste se o něj měli starat?](active-directory-passwords-writeback.md)
* [Jak hlásit aktivitu v SSPR?](active-directory-passwords-reporting.md)
* [Jaké jsou všechny možnosti v SSPR a co znamenají?](active-directory-passwords-how-it-works.md)
* [Myslím, že je něco poškozené. Jak řešit problémy SSPR?](active-directory-passwords-troubleshoot.md)
* [Mám otázku, která není zodpovězená jinde](active-directory-passwords-faq.md)

[Authentication]: ./media/active-directory-passwords-how-it-works/sspr-authentication-methods.png "Dostupné metody ověřování Azure AD a požadované množství"