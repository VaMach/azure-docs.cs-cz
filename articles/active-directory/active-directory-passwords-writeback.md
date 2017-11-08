---
title: "Azure AD SSPR se zpětným zápisem hesel | Microsoft Docs"
description: "Pomocí Azure AD a Azure AD Connect zpětný zápis hesla do místního adresáře"
services: active-directory
keywords: "Správa hesel služby Active directory, správou hesel Azure AD samoobslužném resetování hesla služby"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 9733774570f3148e0092f42c1321b4fac1c80b54
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="password-writeback-overview"></a>Přehled zpětný zápis hesla

Zpětný zápis hesla můžete konfigurovat Azure AD pro zpětný zápis hesel do služby Active Directory v místě. Odebere potřeba nastavit a spravovat složitá místním řešením pro resetování hesla pomocí samoobslužné služby a nabízí pohodlný způsob cloudu pro uživatelům resetovat vlastní hesla na místě, kde se nacházejí. Zpětný zápis hesla je součástí [Azure Active Directory Connect](./connect/active-directory-aadconnect.md) , lze povolit a používat aktuální odběratele Premium [edice služby Azure Active Directory](active-directory-editions.md).

Zpětný zápis hesla poskytuje následující funkce:

* **Nula zpětnou vazbu zpoždění** -zpětný zápis hesla je asynchronní operace. Uživatelé jsou okamžitě upozorněni, pokud své heslo nesplňuje zásady nebo se nepodařilo resetovat nebo změnit z jakéhokoli důvodu.
* **Podporuje resetování hesla pro uživatele, kteří používají službu AD FS nebo jiné technologie federation** -s zpětný zápis hesla, tak dlouho, dokud federované uživatelské účty jsou synchronizovány do vašeho klienta Azure AD je bylo možné spravovat své místní AD hesla z cloudu.
* **Podporuje resetování hesla pro uživatele, kteří používají [synchronizace hodnot hash hesel](./connect/active-directory-aadconnectsync-implement-password-synchronization.md)**  – Pokud službu resetování hesla zjistí, že účet synchronizované uživatele je povoleno pro synchronizace hodnot hash hesel, nemůžeme resetovat současně tento účet místní i cloudové heslo.
* **Změna hesla z přístupového panelu a Office 365 podporuje** – když federovaný nebo heslo, které jsou synchronizované uživatele pocházet ke změně hesla vypršela platnost, nebo jiný vypršela platnost, můžeme tyto zpětnému zápisu do místního prostředí AD.
* **Podporuje zápis zpět hesla, když je správce resetovat z portálu Azure** – vždy, když správce obnoví heslo uživatele v [portál Azure](https://portal.azure.com), pokud je Federovaná tohoto uživatele nebo heslo synchronizován, vytvoříme heslo správce vybere na vaše místní AD, také. To není aktuálně podporováno na portálu správy Office.
* **Vynucuje místní zásady hesel AD** – Pokud uživatel resetuje heslo, zajišťujeme, že splňuje místní zásady služby Active Directory před potvrzením do tohoto adresáře. To zahrnuje historie, složitost, stáří, filtrů hesla a další omezení heslo, které jste definovali ve vaší místní AD.
* **Nevyžaduje žádná pravidla brány firewall pro příchozí** -zpětný zápis hesla předávání přes Azure Service Bus používá jako základní komunikační kanál, což znamená, že nemáte, otevřete žádné příchozí porty v bráně firewall pro tuto funkci používat.
* **Není podporována pro uživatelské účty, které existují v rámci chráněné skupiny ve službě Active Directory vaší místní** – Další informace o chráněné skupiny, najdete v tématu [chráněné účty a skupiny ve službě Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Jak funguje zpětný zápis hesla

Když se federovaný nebo heslo hash synchronizovaný uživatel dodává můžete resetovat nebo změnit své heslo v cloudu, dojde k následující položky:

1. Jsme zkontrolujte jaký typ hesla, uživatel má.
    * Pokud vidíte heslo je spravovaný místně
        * Jsme zaškrtněte, pokud je služba zpětný zápis a spuštěna, pokud se jedná, jsme mohl uživatel pokračovat
        * Pokud není služba zpětný zápis nahoru, jsme říct uživatelům, že nyní nelze resetovat svoje heslo
2. Uživatel potom předá odpovídající ověřovací brány a dosáhne obrazovce resetování hesla.
3. Uživatel vybere nové heslo a potvrzení ho.
4. Po kliknutí na tlačítko Odeslat, jsou šifrovány heslo jako prostý text s symetrický klíč, který jste vytvořili během procesu instalace zpětný zápis.
5. Po šifrování hesla, jsme její zahrnutí do datové části, která se odešlou přes kanál HTTPS do vaší předávání přes konkrétního klienta service bus, (který také nastavíme pro vás během procesu instalace zpětný zápis). Tato předávání je chráněn náhodně generované heslo, které zná pouze místní instalace.
6. Jakmile zpráva dosáhne služby service bus, koncový bod resetování hesla automaticky probudí a uvidí, že má čekající žádost o resetování.
7. Služba poté hledat uživatele dotyčném pomocí atribut kotvy cloudu. Pro toto vyhledávání úspěšné:

    * Objekt uživatele, musí existovat v prostoru konektoru AD
    * Objekt uživatele musí být propojena s odpovídající objektu MV
    * Objekt uživatele musí být propojena na odpovídající objekt konektoru AAD.
    * Odkaz z AD connector objektu MV musí mít synchronizační pravidlo `Microsoft.InfromADUserAccountEnabled.xxx` na odkaz. <br> <br>
    Při volání dodává z cloudu, synchronizační modul používá atribut cloudAnchor k vyhledání místa objekt konektoru AAD, odkaz zpět do objektu MV způsobem a potom následuje odkaz zpět na objekt AD. Vzhledem k tomu může existovat více objektů AD (více doménovými strukturami) pro stejného uživatele, synchronizační modul spoléhá na `Microsoft.InfromADUserAccountEnabled.xxx` odkaz vybrat tu správnou.

    > [!Note]
    > V důsledku této logiky musí být schopen komunikovat s emulátoru primárního řadiče domény pro zpětný zápis hesla pro práci Azure AD Connect. Pokud potřebujete povolit ručně, se můžete připojit Azure AD Connect na emulátoru primárního řadiče domény pomocí pravým tlačítkem myši na **vlastnosti** synchronizace konektoru služby Active Directory, pak výběrem **konfigurace oddílů adresáře**. Tady vyhledejte část **nastavení připojení řadiče domény** a zaškrtněte políčko s názvem **použít pouze upřednostňované řadiče domény**. I když upřednostňované řadič domény není emulátor primárního řadiče domény, Azure AD Connect se pokusí připojit k primární řadič domény pro zpětný zápis hesla.

8. Jakmile je nalezen uživatelský účet, jsme pokus o resetování hesla přímo v příslušné doménové struktuře AD.
9. Pokud operace nastavení hesla úspěšné, jsme informace pro uživatele, že jejich heslo bylo změněno.
    > [!NOTE]
    > V případě, že když hesla se synchronizují do Azure AD pomocí synchronizace hesel, existuje pravděpodobnost, že místní zásady hesel je nižší než zásady hesel cloudu. V takovém případě jsme stále vynucovat ať zásady místního je a místo toho povolit synchronizaci hodnoty hash hesla k synchronizaci hodnoty hash hesla. Tím se zajistí, že vaše místní zásady se vynucují v cloudu, bez ohledu používáte synchronizace hesel nebo federování zajistit jednotné přihlašování.

10. Pokud heslo nastavené operace selže, jsme vrátí chybu pro uživatele a dát jim zkuste to znovu.
    * Operaci může selhat z důvodu následující
        * Služba byla dolů
        * Heslo, které vybral nesplňuje zásady organizace
        * Nepovedlo se nám najít uživatele v místní AD

    Jsme konkrétní zprávu pro mnohá z těchto případů a informace pro uživatele co dělají řešení tohoto problému.

## <a name="configuring-password-writeback"></a>Konfigurace zpětný zápis hesla

Doporučujeme použít funkci Automatické aktualizace [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) Pokud chcete použít zpětný zápis hesla.

DirSync a Azure AD Sync již nejsou podporované znamená povolení zpětného zápisu hesla v článku [upgradu z nástroje DirSync a Azure AD Sync](connect/active-directory-aadconnect-dirsync-deprecated.md) obsahuje další informace o pomoci při váš přechod.

Následující postup předpokládá, že jste již nakonfigurovali Azure AD Connect v prostředí pomocí [Express](./connect/active-directory-aadconnect-get-started-express.md) nebo [vlastní](./connect/active-directory-aadconnect-get-started-custom.md) nastavení.

1. Ke konfiguraci a povolení protokolu zpětného zápisu hesla v na server Azure AD Connect a spustit **Azure AD Connect** Průvodce konfigurací.
2. Na úvodní obrazovce klikněte na tlačítko **konfigurace**.
3. Na obrazovce další úlohy, klikněte na tlačítko **přizpůsobit možnosti synchronizace** a potom zvolte **Další**.
4. Na obrazovce připojení ke službě Azure AD, zadejte přihlašovací údaje globálního správce a zvolte **Další**.
5. Na připojení adresáře a domény a organizační jednotky filtrování obrazovky, můžete zvolit **Další**.
6. Na obrazovce volitelných funkcí, zaškrtněte políčko vedle **zpětný zápis hesla** a klikněte na tlačítko **Další**.
   ![Povolení zpětného zápisu hesla ve službě Azure AD Connect][Writeback]
7. Na stránce Připraveno ke konfiguraci obrazovce klikněte na tlačítko **konfigurace** a počkejte na dokončení procesu.
8. Až se zobrazí konfigurace dokončit, klikněte na tlačítko **ukončení**

Související s zpětný zápis hesla běžných úloh odstraňování potíží naleznete v části [řešení zpětného zápisu hesla](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) v našem článek o odstraňování potíží.

## <a name="active-directory-permissions"></a>Oprávnění služby Active Directory

Účet zadaný v nástroj Azure AD Connect musí mít resetovat heslo, změnit heslo, oprávnění k zápisu na lockoutTime a oprávnění k zápisu na pwdLastSet, rozšířené práv na buď na kořenový objekt **každou doménu** v tom, že doménové struktury **nebo** na uživatele, které chcete být v oboru pro SSPR organizační jednotky.

Pokud si nejste jisti, který účet výše odkazuje, otevřete konfigurace Azure Active Directory Connect uživatelského rozhraní a klikněte na možnost zobrazení aktuální konfigurace. Účet, který je nutné přidat oprávnění je uveden v části "Synchronizovat adresáře"

Nastavení těchto oprávnění umožní účtu služby MA pro každou doménovou strukturu spravovat hesla v zastoupení uživatelských účtů v této doménové struktuře. **Pokud toto přiřazení oprávnění neprovedete, pak i když má být konfigurována správně, zobrazí se zpětný zápis uživatelů dojde k chybám při pokusech o správu svých místních hesel z cloudu.**

> [!NOTE]
> To může trvat až jednu hodinu nebo více těchto oprávnění k replikaci pro všechny objekty ve vašem adresáři.
>

Nastavit příslušná oprávnění pro zpětný zápis hesla hesla

1. Otevřete pomocí účtu, který má oprávnění pro správu příslušné domény Active Directory Users and Computers
2. Z nabídky Zobrazit zkontrolujte, zda že je zapnutý pokročilé funkce
3. V levém panelu klikněte pravým tlačítkem na objekt, který reprezentuje kořen domény a vyberte možnost Vlastnosti
    * Klikněte na kartu zabezpečení
    * Klikněte na tlačítko Upřesnit.
4. Na kartě oprávnění klikněte na tlačítko Přidat
5. Vyberte účet, který oprávnění se používají pro (z instalace Azure AD Connect)
6. V platí pro rozbalení pole vyberte podřízené uživatelské objekty
7. V části oprávnění zaškrtněte políčka pro následující
    * Trvale platné heslo
    * Resetování hesla
    * Změnit heslo
    * Zápis lockoutTime
    * Zápis pwdLastSet
8. Klikněte na tlačítko použít/OK prostřednictvím Pokud chcete použít a ukončete všechna otevřená dialogová.

## <a name="licensing-requirements-for-password-writeback"></a>Licenční požadavky pro zpětný zápis hesla

Informace týkající se licencování, viz [licence potřebné pro zpětný zápis hesla](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) nebo následující weby

* [Azure Active Directory ceny lokality](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Zabezpečené produktivní Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Místní režimy ověřování, které jsou podporované pro zpětný zápis hesla

Zpětný zápis hesla funguje pro následující typy heslo uživatele:

* **Uživatelé cloudové**: zpětný zápis hesla nelze použít v této situaci, protože neexistuje žádné místní heslo
* **Synchronizovat hesla uživatelů**: podporované zpětný zápis hesla
* **Federovaní uživatelé**: podporované zpětný zápis hesla
* **Předávací ověřování uživatelů**: podporované zpětný zápis hesla

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>Operace uživatele a správce podporované pro zpětný zápis hesla

Hesla jsou zapsány zpět v následujících situacích:

* **Podporované operace koncového uživatele**
  * Všechny koncového uživatele samoobslužné služby dobrovolném změnit operace hesla
  * Všechny koncového uživatele samoobslužné služby vynutit změnu hesla operace (například vypršení platnosti hesla)
  * Všechny koncových uživatelů samoobslužného resetování hesel pocházející z [portálu pro resetování hesel](https://passwordreset.microsoftonline.com)
* **Podporované operace správce**
  * Všechny dobrovolném samoobslužné služby správce změnit operace hesla
  * Všechny správce samoobslužné služby vynutit změnu hesla operace (například vypršení platnosti hesla)
  * Jakékoli správce samoobslužného resetování hesel pocházející z [portálu pro resetování hesel](https://passwordreset.microsoftonline.com)
  * Resetování z jakékoli hesla iniciované správcem koncového uživatele [portál Azure classic](https://manage.windowsazure.com)
  * Resetování z jakékoli hesla iniciované správcem koncového uživatele [portálu Azure](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Operace uživatele a správce není podporováno pro zpětný zápis hesla

Hesla jsou **není** zapisovat zpátky v následujících situacích:

* **Nepodporovaná operace koncového uživatele**
  * Všichni koncoví uživatelé resetovat vlastní hesla pomocí prostředí PowerShell v1, v2 nebo Azure AD Graph API
* **Nepodporovaná operace správce**
  * Resetování z jakékoli hesla iniciované správcem koncového uživatele [portálu pro správu Office](https://portal.office.com)
  * Žádné z prostředí PowerShell v1, v2 nebo Azure AD Graph API pro vytvoření nového hesla iniciované správcem koncového uživatele

Při pracujeme odstranit tato omezení, konkrétní časové osy, kterou jsme můžete sdílet ještě nemáme.

## <a name="password-writeback-security-model"></a>Model zabezpečení zpětný zápis hesla

Zpětný zápis hesla je vysoce zabezpečených služba.  Chcete-li ujistit, jestli že je chránit vaše informace, jsme povolit model vrstvené 4 zabezpečení, který je popsán níže.

* **Předávání přes service-bus konkrétního klienta**
  * Při nastavování služby nastavíme předávání sběrnice služby konkrétního klienta, který je chráněný pomocí náhodně generované silné heslo, které společnost Microsoft nikdy nemá přístup k.
* **Uzamčení dolů, kryptograficky silné heslo šifrovacího klíče**
  * Po vytvoření předávání přes service bus vytvoříme silné symetrického klíče, které používáme k šifrování hesla při přechodu do prostřednictvím sítě. Tento klíč je umístěn pouze ve vaší společnosti tajný úložiště v cloudu, což je výraznou uzamčené a auditovat, stejně jako jakékoli jiné heslo v adresáři.
* **Oborový standard TLS**
  1. Pokud heslo resetovat nebo změnit operace probíhá v cloudu, jsme trvat heslo jako prostý text a šifrování pomocí veřejného klíče.
  2. Jsme pak umístit, do zprávy protokolu HTTPS, která se posílají přes šifrovaný kanál pomocí certifikátů SSL společnosti Microsoft k vaší předávání přes service bus.
  3. Po příchodu zprávy do služby Service Bus, vaše místní agent probudí a ověřuje na Service Bus pomocí silné heslo, který byl vytvořen dříve.
  4. Místní agent převezme šifrované zprávy, dešifruje ji pomocí soukromého klíče, který jsme vygenerovat.
  5. Místní agent pokusí k nastavení hesla prostřednictvím rozhraní API SetPassword AD DS.
     * Tento krok je co umožňuje vynucovat vaše místní zásady hesel služby AD (složitost, stáří, historie, filtry atd.) v cloudu.
* **Zásady vypršení platnosti zpráv** 
  * Pokud zpráva je umístěna v Service Bus, protože vaše místní služba není spuštěna, bude vypršení časového limitu a odeberou po několika minutách ke zvýšení zabezpečení ještě víc.

### <a name="password-writeback-encryption-details"></a>Podrobnosti o šifrování zpětný zápis hesla

Kroky šifrování prochází žádost o resetování hesla, až uživatel odešle, než dorazí v prostředí místní zajistit spolehlivost maximální služby a zabezpečení jsou popsány níže.

* **Krok 1 – heslo šifrování pomocí klíče RSA 2048 bitů** – když uživatel odešle heslo k možné zpětně zapsat do místní, nejprve odeslaná heslo samotné je šifrován klíčem RSA 2048 bitů.
* **Krok 2 – šifrování na úrovni balíčku s AES-GCM** - pak celý balíček (heslo + požadovaná metadata) jsou šifrována pomocí standardu AES-GCM. Toto šifrování zabraňuje každý, kdo má přímý přístup do základní sběrnice kanálu v zobrazení/manipulaci s obsahem.
* **Krok 3 – veškerá komunikace probíhá přes protokol TLS / SSL** -veškerá komunikace se sběrnice probíhá kanál SSL/TLS. Toto šifrování zabezpečuje obsah z neoprávněným třetím stranám.
* **Automatickou výměnu klíče každých šest měsíců** – každých 6 měsíců, nebo pokaždé, když zpětný zápis hesla je zakázána a znovu povolena na Azure AD Connect, můžeme automaticky všechny výměny klíčů zajistit zabezpečení Maximální služby a bezpečnost.

### <a name="password-writeback-bandwidth-usage"></a>Využití šířky pásma zpětný zápis hesla

Zpětný zápis hesla je služba malou šířkou pásma, která odešle požadavky zpět do agenta místně jenom v následujících případech:

1. Dvě zprávy po povolení nebo zakázání funkce přes Azure AD Connect.
2. Tak dlouho, dokud je služba spuštěná, je odeslána zpráva jeden každých pět minut jako služba prezenčního signálu.
3. Dvě zprávy, které se odesílají každý čas odeslání nové heslo
    * První zprávu jako požadavek k provedení operace.
    * Druhá zpráva, která obsahuje výsledek operace a jsou zasílány v následujících případech:
        * Pokaždé, když nové heslo je odesíláno během resetování hesla pomocí samoobslužné služby uživatele.
        * Pokaždé, když nové heslo je odesíláno během operace změny hesla uživatele.
        * Pokaždé, když nové heslo je odesíláno během spouštěná správce uživatelské heslo resetovat (z Azure správce portálů pouze).

#### <a name="message-size-and-bandwidth-considerations"></a>Aspekty zpráva velikost a šířky pásma

Velikost jednotlivých zpráv popsané výše je obvykle v části 1 kb, i v rámci extrémně zatížení, samotnou službu zpětný zápis hesla spotřebovává několik kilobity za sekundu šířky pásma. Vzhledem k tomu, že každá zpráva odeslána v reálném čase, pouze v případě, že operace aktualizace a heslo a vzhledem k tomu, že velikost zprávy je tak malé využití šířky pásma pro zpětný zápis schopnosti je efektivně příliš malá mít žádné skutečných měřitelný dopad.

## <a name="next-steps"></a>Další kroky

* [Jak dokončit úspěšné zavedení SSPR?](active-directory-passwords-best-practices.md)
* [Resetování nebo změna hesla](active-directory-passwords-update-your-own-password.md)
* [Registrace samoobslužného resetování hesla](active-directory-passwords-reset-register.md)
* [Máte dotaz k licencování?](active-directory-passwords-licensing.md)
* [Jaká data používá SSPR a která data byste měli naplnit pro vaše uživatele?](active-directory-passwords-data.md)
* [Které metody ověřování jsou dostupné pro uživatele?](active-directory-passwords-how-it-works.md#authentication-methods)
* [Jaké jsou možnosti zásad se SSPR?](active-directory-passwords-policy.md)
* [Jak hlásit aktivitu v SSPR?](active-directory-passwords-reporting.md)
* [Jaké jsou všechny možnosti v SSPR a co znamenají?](active-directory-passwords-how-it-works.md)
* [Myslím, že je něco poškozené. Jak řešit problémy SSPR?](active-directory-passwords-troubleshoot.md)
* [Mám otázku, která není zodpovězená jinde](active-directory-passwords-faq.md)

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Povolení zpětného zápisu hesla ve službě Azure AD Connect"
