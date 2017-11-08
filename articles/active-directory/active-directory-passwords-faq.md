---
title: "Nejčastější dotazy: Azure AD SSPR | Microsoft Docs"
description: "Nejčastější dotazy o hesla pomocí samoobslužné služby Azure AD resetovat"
services: active-directory
keywords: "Správa hesel služby Active directory, správou hesel Azure AD samoobslužném resetování hesla služby"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 14c565bb67480681e1d398a0a21a11448f405e4e
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="password-management-frequently-asked-questions"></a>Nejčastější dotazy se správou hesel

Níže jsou uvedeny některé nejčastější dotazy pro všechny věci týkající se vytvoření nového hesla.

Pokud máte obecný dotaz týkající se Azure AD a hesla pomocí samoobslužné služby resetování, která není zde zodpovězena, můžete požádat komunitou o pomoc na [fóra Azure Ad](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD). Členové komunity služby zahrnují Engineers, správce produktu, MVP a hlavě nehodí Odborníci v oblasti IT.

Tyto nejčastější dotazy je rozdělená do následujících částí:

* [**Dotazy o registraci k resetování hesla**](#password-reset-registration)
* [**Dotazy o resetování hesla**](#password-reset)
* [**Dotazy týkající se změna hesla**](#password-change)
* [**Sestavy dotazy týkající se správou hesel**](#password-management-reports)
* [**Dotazy týkající se zpětný zápis hesla**](#password-writeback)

## <a name="password-reset-registration"></a>Registrace pro resetování hesla

* **Otázka: je možné mé uživatelé registrovat svá vlastní data resetování hesla?**

  > **Odpověď:** Ano, jak dlouho, dokud je povoleno obnovení hesla a udělení licence, mohou přejít na portál registraci pro resetování hesla na adrese http://aka.ms/ssprsetup k registraci jejich informace o ověřování. Uživatelé mohou také registrovat přejdete k přístupovému panelu na adrese http://myapps.microsoft.com, kliknutím na kartu profil a kliknutím na registrace pro resetování hesla možnost.
  >
  >
* **Otázka: je možné definovat data resetování hesel jménem Moje uživatelů?**

  > **Odpověď:** Ano, můžete tak učinit službou Azure AD Connect, prostředí PowerShell [portál Azure](https://portal.azure.com), nebo portál pro správu Office. Další informace najdete v článku [dat používá Azure AD funkce samoobslužného resetování hesla](active-directory-passwords-data.md).
  >
  >
* **Otázka: je možné synchronizovat data pro bezpečnostní otázky z místně?**

  > **Odpověď:** to není možné ještě dnes.
  >
  >
* **Otázka: je možné zaregistrovat vlastní uživatelé dat tak, že další uživatelé nemohou zobrazit tato data?**

  > **Odpověď:** Ano, když uživatelé zaregistrovat dat pomocí registrace portálu pro resetování hesel je uložen do polí privátní ověřování, která viditelné pouze globální správce a uživatele.
  >
  >
* **Otázka: Moji uživatelé mají k registraci, aby mohli používat resetování hesla?**

  > **Odpověď:** Ne, pokud jejich jménem definujete dostatek informací, ověřování, uživatelé nebudou mít k registraci. Tak dlouho, dokud máte správně naformátovaná data uložená v odpovídající pole v adresáři, resetování hesla funguje.
  >
  >
* **Otázka: je možné synchronizovat nebo nastavit telefon pro ověření, ověření e-mailu nebo alternativní telefon pro ověření pole jménem Moje uživatelů?**

  > **Odpověď:** to není možné ještě dnes.
  >
  >
* **Otázka: jak na portál pro registraci vědět, které možnosti zobrazíte Moji uživatelé?**

  > **Odpověď:** zobrazí pouze portálu registrace resetování hesel možností, že je povoleno pro vaše uživatele. Tyto možnosti se nacházejí v části zásady resetování hesel uživateli svého adresáře na kartě konfigurace. Například to znamená, že pokud nepovolíte bezpečnostní otázky, pak nebudou se uživatelé moct zaregistrovat pro tuto možnost.
  >
  >
* **Otázka: když se považuje za uživatele registrované?**

  > **Odpověď:** uživatel se považuje zaregistrovat pro SSPR, když registrace alespoň **několik metod, které jsou nutná k obnovení** , kterou jste nastavili [portál Azure](https://portal.azure.com).
  >
  >

## <a name="password-reset"></a>Resetování hesla

* **Otázka: jak dlouho má čekat pro příjem e-mailem, SMS nebo telefonní hovor z resetování hesla?**

  > **Odpověď:** e-mailu, zpráv SMS, a telefonních hovorů by měl přicházející do části jednu minutu, normální případem 5-20 sekund.
    >Pokud tento časový rámec neobdrží oznámení:
        > * Zkontrolujte své složky s nevyžádanou poštou.
        > * Číslo nebo e-mailu kontaktovaný je ta, kterou byste měli zkontrolovat.
        > * Zkontrolujte, zda že je správně naformátován ověřování dat v adresáři.
                >     * Příklad: "+ 1 4255551234" nebo "user@contoso.com"
  >
  >
* **Otázka: jaké jazyky jsou podporovány resetování hesla?**

  > **Odpověď:** heslo resetovat uživatelského rozhraní, zpráv SMS a hlasové hovory jsou lokalizované do stejné jazyky, které jsou podporovány v Office 365.
  >
  >
* **Otázka: jaký částí prostředí resetování hesla získat značky po nastavení organizační brandingu v adresáře Moje aktivity karta Konfigurace?**

  > **Odpověď:** portálu pro resetování hesla zobrazí logo vaší organizace a umožňuje vám nakonfigurovat kontakt odkaz na vaši správce tak, aby odkazovalo vlastní e-mailu nebo adresa URL. E-mailu, která se odešlou resetování hesla zahrnuje loga vaší organizace, barvy, název v textu e-mailu a přizpůsobit z názvu.
  >
  >
* **Otázka: jak můžete naučit Moje uživatele o tom, kde přejděte k resetování hesla?**

  > **Odpověď:** vyzkoušíme některá doporučení v našich [SSPR nasazení článku](active-directory-passwords-best-practices.md#email-based-rollout)
  >
  >
* **Otázka: je možné pomocí této stránky z mobilního zařízení?**

  > **Odpověď:** Ano, tato stránka funguje na mobilních zařízeních.
  >
  >
* **Otázka: podporujete odemykání účty místní služby active directory při resetování hesel uživatelů?**

  > **Odpověď:** Ano, pokud uživatel může resetovat své heslo a byla nasazena zpětného zápisu hesla pomocí služby Azure AD Connect, tento uživatelský účet je automaticky odemknout se obnovit své heslo.
  >
  >
* **Otázka: jak můžete integrovat přímo do daného uživatele plochy přihlašování resetování hesla?**

  > **Odpověď:** Pokud jste zákazník Azure AD Premium, můžete nainstalovat Microsoft Identity Manager bez dalších poplatků a nasazení v případě místních řešení resetování hesla ke splnění tohoto požadavku.
  >
  >
* **Otázka: je možné nastavit různé bezpečnostní otázky pro různá národní prostředí?**

  > **Odpověď:** to není možné ještě dnes.
  >
  >
* **Otázka: jak tolik otázek jsme nakonfigurovat pro možnost ověření bezpečnostních otázek?**

  > **Odpověď:** můžete nakonfigurovat až 20 vlastní bezpečnostních otázek v [portál Azure](https://portal.azure.com).
  >
  >
* **Otázka: jak dlouho může bezpečnostní otázky být?**

  > **Odpověď:** bezpečnostní otázky může být 3 až 200 znaků.
  >
  >
* **Otázka: jak dlouho může odpovědi na bezpečnostní otázky být?**

  > **Odpověď:** odpovědi může mít délku 3 až 40 znaků.
  >
  >
* **Otázka: je duplicitní odpovědi na bezpečnostní otázky odmítl?**

  > **Odpověď:** Ano, jsme odmítnout duplicitní odpovědi na bezpečnostní otázky.
  >
  >
* **Otázka: může uživatel na stejné bezpečnostní otázku zaregistrovat více než jednou?**

  > **Odpověď:** Ne, jakmile se uživatel zaregistruje na určitou otázku, se nemusí zaregistrovat pro tento dotaz ještě jednou.
  >
  >
* **Otázka: je možné nastavit minimální limit bezpečnostní otázky pro registraci a resetování?**

  > **Odpověď:** Ano, lze nastavit jeden limit pro registraci a druhý pro obnovení. bezpečnostní otázky 3 až 5 může být potřeba k registraci a 3 až 5 může být nutný pro obnovení.
  >
  >
* **Otázka: I nakonfigurované Moje zásady budou muset uživatelé použít bezpečnostní otázky pro resetování, ale pravděpodobně nakonfigurována jinak než Azure správci.**

  > **Odpověď:** jedná se o očekávané chování. Microsoft vynucuje silné výchozí zásad resetování hesel dvě brány pro žádnou roli Správce služby Azure. To zakáže správci pomocí bezpečnostních otázek. Další informace o této zásadě naleznete v článku [zásady hesel a omezení v Azure Active Directory](active-directory-passwords-policy.md#administrator-password-policy-differences)
  >
  >
* **Otázka: Pokud uživatel má více než maximální počet otázek vyžadovaných k resetování registrované, jak jsou bezpečnostní otázky vybrané během obnovení?**

  > **Odpověď:** N bezpečnostní otázky jsou náhodně vybrané mimo celkový počet otázek, které uživatel zaregistroval, kde N je **počet otázek vyžadovaných k resetování**. Například pokud má uživatel zaregistrován 5 bezpečnostní otázky, ale jenom 3 jsou nutná k obnovení, 3 5 jsou vybrán náhodně a uvedené v resetování. Pokud uživatel získá nesprávné odpovědi na otázky, aby se zabránilo ražením otázku opakovat procesu výběru.
  >
  >
* **Otázka: můžete zabránit uživatelům v pokusu o mnohokrát v krátkého časového období pro vytvoření nového hesla?**

  > **Odpověď:** Ano, jsou funkce zabezpečení, které jsou součástí pro ochranu před zneužitím pro vytvoření nového hesla. Uživatelé mohou zkuste jenom 5 resetování pokusů o zadání hesla v rámci jednu hodinu před uzamknutí 24 hodin. Uživatelé mohou pouze pokusí se ověřit telefonní číslo 5krát v rámci jednu hodinu před uzamknutí 24 hodin. Uživatelé mohou pouze pokusí metoda ověření jednotného 5krát v rámci jednu hodinu před uzamknutí 24 hodin.
  >
  >
* **Otázka: po tom, jak dlouho jsou e-mailu a SMS jednorázovým heslem platné?**

  > **Odpověď:** dobu platnosti relace pro resetování hesla je 15 minut. Od začátku operace resetování hesla, uživatel má 15 minut resetovat heslo. E-mailu a SMS jednorázové heslo jsou neplatné po vypršení tohoto časového období.
  >
  >
* **Otázka: je možné zablokovat uživatelům resetování hesla?**

  > **Odpověď:** Ano, pokud používáte skupinu povolit samoobslužné resetování hesla je můžete je odebrat ze skupiny, která by jim umožnila tuto možnost.
  >
  >

## <a name="password-change"></a>Změna hesla

* **Otázka: kde by měli Moji uživatelé přejít ke změně hesla?**

  > **A:** uživatelé mohou změnit své heslo kdekoli uvidí jejich profilový obrázek nebo ikonu (jako v pravém horním rohu jejich [Office 365](https://portal.office.com) nebo [přístupový Panel](https://myapps.microsoft.com) dojde. Uživatelé mohou změnit své heslo z [stránka přístupového panelu profil](https://account.activedirectory.windowsazure.com/r#/profile). Uživatelé mohou také vyzváni ke změně hesla na přihlašovací obrazovce Azure AD automaticky, pokud vypršela platnost hesla. Nakonec mohou uživatelé přejdou [portál změn hesel služby Azure AD](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) přímo Pokud chtějí změnit své heslo.
  >
  >
* **Otázka: je možné se svým uživatelům a upozornění na portálu Office, když vyprší platnost hesla pro místní?**

  > **Odpověď:** to je možné ještě dnes Pokud používáte služby AD FS podle pokynů tady: [odesílání deklarace zásady hesel se službou AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396). Pokud používáte synchronizaci hodnoty hash hesla, to není možné ještě dnes. Je to proto, že jsme není synchronizována zásady hesel z místní, takže není možné, že nám odeslat oznámení o vypršení platnosti do prostředí cloudu. V obou případech je také možné [upozorněte uživatele, jejichž hesla se vyprší pomocí prostředí PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).
  >
  >
* **Otázka: je možné blokovat uživatelům změnu hesla?**

  > **Odpověď:** pro uživatele jenom pro cloud tím, nejde blokovat. Pro místní uživatele můžete nastavit `User cannot change password` pro zaškrtnutí a tito uživatelé nebudou moci měnit své heslo.
  >
  >

## <a name="password-management-reports"></a>Sestavy správy hesel

* **Otázka: jak dlouho trvá pro data objeví na sestavy správy heslo?**

  > **Odpověď:** Data by se zobrazit na sestav správy hesel v rámci 5 až 10 minut. Ho některých případech může trvat jednu hodinu, než se objeví.
  >
  >
* **Otázka: jak můžete filtrovat sestavy správy heslo?**

  > **Odpověď:** sestav správy hesel můžete filtrovat kliknutím malé lupy extrémně napravo od popisky sloupců, do horní části sestavy. Pokud chcete provádět bohatší filtrování, můžete si stáhnout sestavu, aby se v aplikaci excel a vytvoření kontingenční tabulky.
  >
  >
* **Otázka: co je maximální počet událostí, které jsou uloženy v sestavy správy heslo?**

  > **Odpověď:** až než 75 000 resetování nebo heslo registraci k resetování hesla události jsou uložené v sestav správy hesel, pokrývání uzlů zpět až 30 dnů.  Pracujeme na rozbalte tento počet na obsahovat další události.
  >
  >
* **Otázka: jak daleko zpět sestav správy hesel přejděte?**

  > **Odpověď:** Správa hesel sestavy zobrazit operace, ke kterým došlo během posledních 30 dnů. Teď Pokud budete potřebovat k archivaci tato data můžete stáhnout sestavy pravidelně a uložit je do samostatných umístění.
  >
  >
* **Otázka: je maximální počet řádků, které se mohou objevit v sestavy správy heslo?**

  > **Odpověď:** Ano, nesmí být delší než 75 000 řádků mohou být na buď sestavy správy hesel, jestli se zobrazí v uživatelském rozhraní nebo stahování.
  >
  >
* **Otázka: je rozhraní API pro přístup k resetování hesla nebo registrační data pro vytváření sestav?**

  > **Odpověď:** Ano, naleznete v následující dokumentaci se dozvíte, jak přistupovat k resetování hesla, vytváření sestav datového proudu.  [Další informace o přístupu k resetování hesla události vytváření sestav prostřednictvím kódu programu](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).
  >
  >

## <a name="password-writeback"></a>Zpětný zápis hesla

* **Otázka: jak zpětný zápis hesla funguje na pozadí?**

  > **Odpověď:** najdete v části [jak zpětný zápis hesla funguje](active-directory-passwords-writeback.md) pro vysvětlení, co se stane, když zapnete zpětný zápis hesla a jak se data proudí prostřednictvím systému zpět do místního prostředí.
  >
  >
* **Otázka: jak dlouho zpětný zápis hesla trvá fungovat?  Je k dispozici ke zpoždění synchronizace jako se synchronizací hodnoty hash hesla?**

  > **Odpověď:** zpětný zápis hesla je rychlé. Je synchronní kanál, který funguje zásadně jinak než synchronizaci hodnoty hash hesla. Zpětný zápis hesla umožňuje uživatelům získat v reálném čase zpětnou vazbu o úspěšnosti jejich resetování hesla nebo změňte operaci. Průměrná doba pro úspěšné zpětný zápis hesla je v části 500 ms.
  >
  >
* **Otázka: Pokud Můj účet místní je, jak je můj účet nebo přístup do cloudu zneužitím?**

  > **Odpověď:** Pokud vaše místní ID je zakázaná, cloudu ID nebo přístup budou rovněž zakázány na další interval synchronizace prostřednictvím AAD Connect ve výchozím nastavení je to každých 30 minut.
  >
  >
* **Otázka: Pokud je omezené Můj účet místní zásady hesel místní služby Active Directory, SSPR orientují tuto zásadu po změně hesla?**

  > **Odpověď:** Ano, spoléhá na SSPR a dodržuje místní zásady hesel služby AD, včetně typické zásady hesel služby AD domény, stejně jako jakoukoli definované zásady podrobné heslo pro daného uživatele.
  >
  >
* **Otázka: jaký typy účtů zpětný zápis hesla funguje pro?**

  > **Odpověď:** zpětný zápis hesla funguje pro federovaný a synchronizuje hodnoty Hash hesla uživatele.
  >
  >
* **Otázka: zpětný zápis hesla vynutit zásady hesel Moje doména?**

  > **Odpověď:** Ano, zpětný zápis hesla vynucuje stáří hesla, historie, složitost, filtrů a další omezení, můžete mohou zavést na hesla v místní doméně.
  >
  >
* **Otázka: je zpětný zápis hesla zabezpečení?  Jak se může být jistí, že nebude získat hacker I?**

  > **Odpověď:** Ano, jsou zabezpečené zpětný zápis hesla. Číst informace o čtyři vrstvy zabezpečení implementované službu zpětný zápis hesla, podívejte se [model zabezpečení zpětný zápis hesla](active-directory-passwords-writeback.md#password-writeback-security-model) část v tom, jak zpětný zápis hesla funguje.
  >
  >

## <a name="next-steps"></a>Další kroky

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
