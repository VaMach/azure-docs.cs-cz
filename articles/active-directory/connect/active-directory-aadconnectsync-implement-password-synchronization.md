---
title: Implementace synchronizace hesel s Azure AD Connect sync | Microsoft Docs
description: Poskytuje informace o tom, jak funguje synchronizace hesel a jak nastavit.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 05f16c3e-9d23-45dc-afca-3d0fa9dbf501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2017
ms.author: billmath
ms.openlocfilehash: 048dfa812a5388b2fc47bafd2fae5d6e96414712
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/23/2017
---
# <a name="implement-password-synchronization-with-azure-ad-connect-sync"></a>Implementace synchronizace hesel s synchronizace Azure AD Connect
Tento článek obsahuje informace, které potřebujete synchronizovat hesla služby uživatele z místní instance služby Active Directory do Azure Active Directory (Azure AD) instance založené na cloudu.

## <a name="what-is-password-synchronization"></a>Co je synchronizace hesel
Pravděpodobnost, že jste se blokovat práce s aplikací z důvodu zapomenuté heslo souvisí se počet různých hesel, je třeba pamatovat. Více hesel, budete muset Pamatujte si, že čím vyšší pravděpodobnost zapomenutí jeden. Dotazy a volání o resetování hesla a další problémy související s hesly poptávky nejvíce zdrojů technickou podporu.

Synchronizace hesel je funkce používá k synchronizaci hesel uživatelů z místní instance služby Active Directory k Azure cloudové AD instance.
Tato funkce slouží k přihlášení ke službám Azure AD, jako je Office 365, Microsoft Intune, CRM Online a Azure Active Directory Domain Services (Azure AD DS). Přihlášení ke službě pomocí stejné heslo, které používáte pro přihlášení k vaší místní instancí Active Directory.

![Co je služba Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Protože se sníží počet hesel, uživatelé potřebovat udržovat na právě jeden. Synchronizace hesel vám pomůže:

* Zvýšení produktivity uživatelů.
* Snižte náklady na technickou podporu.  

Navíc pokud se rozhodnete použít [federační službou Active Directory Federation Services (AD FS)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), můžete volitelně můžete nastavit synchronizaci hesel jako záložní případ selhání infrastruktury služby AD FS.

Synchronizace hesel je rozšíření pro funkci synchronizace adresáře implementované synchronizace Azure AD Connect. Pokud chcete používat synchronizace hesel ve vašem prostředí, budete muset:

* Instalovat Azure AD Connect.  
* Konfigurace synchronizace adresářů mezi vaší místní instancí Active Directory a vaše instance služby Azure Active Directory.
* Povolení synchronizace hesel.

Podrobnější informace najdete v článku [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

> [!NOTE]
> Další podrobnosti o Azure Active Directory Domain Services nakonfigurovaná pro synchronizaci FIPS a heslo najdete v části "synchronizace hesel a FIPS" později v tomto článku.
>
>

## <a name="how-password-synchronization-works"></a>Princip funkce Synchronizace hesla
Služba Active Directory domain Services ukládá ve formě reprezentace hodnotu hash hesla skutečného uživatelského hesla. Hodnota hash je výsledek jednosměrné matematické funkce ( *algoritmus hash*). Neexistuje žádná metoda vrátit výsledek jednosměrné funkce na prostý text verzi heslo. Hodnoty hash hesla nelze použít pro přihlášení k síti na pracovišti.

Synchronizace Azure AD Connect Pokud chcete synchronizovat hesla, extrahuje vaše hodnoty hash hesla z místní instance služby Active Directory. Další bezpečnostní zpracování se použije pro hodnoty hash hesla před synchronizací k ověřovací službě Azure Active Directory. Hesla se synchronizují na jednotlivé uživatele a v chronologickém pořadí.

Samotný datový tok proces synchronizace hesla je podobná synchronizace uživatelských dat, jako je například DisplayName nebo e-mailové adresy. Ale hesla se synchronizují častěji, než standardní directory okna synchronizace pro ostatní atributy. Proces synchronizace hesel spustí každé dvě minuty. Nelze změnit frekvenci tohoto procesu. Při synchronizaci hesla, přepíše stávající heslo cloudu.

Při prvním povolení funkce Synchronizace hesla, provede počáteční synchronizaci hesel všech uživatelů v oboru. Nemůže explicitně definovat podmnožinu uživatelská hesla, které chcete synchronizovat.

Pokud změníte heslo k místní, aktualizované heslo je synchronizován, nejčastěji během několika minut.
Funkce Synchronizace hesla automaticky opakovat pokusy o synchronizaci se nezdařilo. Pokud dojde k chybě při pokusu o synchronizaci hesla, je váš prohlížeč událostí přihlášení k chybě.

Synchronizace hesla nemá žádný vliv na uživatele, který je aktuálně přihlášený.
Aktuální relace cloudové služby nemá vliv okamžitě změnu hesla synchronizované, ke kterému dochází, když jste přihlášení ke cloudové službě. Ale při Cloudová služba vyžaduje, abyste pokus o ověření, musíte zadat nové heslo.

Uživatel musí zadat svoje podnikové přihlašovací údaje ještě jednou k ověření do služby Azure AD, bez ohledu na to, jestli jsou přihlášení k podnikové síti. Tyto vzor můžete minimalizovat, ale pokud uživatel vybere zachovat mi podepsané v políčko (KMSI) při přihlašování. Tento výběr nastaví soubor cookie relace, který obchází ověřování na krátkou dobu. Chování KMSI můžete povolit nebo zakázat pomocí Správce Azure AD.

> [!NOTE]
> Synchronizace hesla je podporována pouze pro uživatele typu objektu ve službě Active Directory. Nepodporuje pro typ objektu iNetOrgPerson.

### <a name="detailed-description-of-how-password-synchronization-works"></a>Podrobný popis toho, jak funguje synchronizace hesel
Následující text popisuje podrobný Princip funkce Synchronizace hesla mezi služby Active Directory a Azure AD.

![Tok podrobné heslo](./media/active-directory-aadconnectsync-implement-password-synchronization/arch3.png)


1. Každé dvě minuty, agent synchronizace hesel na server AD Connect požadavky hodnot hash hesel uložených (atribut unicodePwd) z řadiče domény přes standardní [MS-DRSR](https://msdn.microsoft.com/library/cc228086.aspx) replikace protokol použitý k synchronizaci dat mezi Řadiče domény. Účet služby musí mít replikovat změny adresáře a replikovat Directory změny všechna AD oprávnění (udělena na instalaci ve výchozím nastavení) k získání hodnoty hash hesla.
2. Před odesláním, řadič domény šifruje hodnota hash MD4 hesla pomocí klíče, který je [MD5](http://www.rfc-editor.org/rfc/rfc1321.txt) hash klíče relace RPC a salt. Potom odesílá výsledek agent synchronizace hesel pomocí protokolu RPC. Řadič domény také předá salt agent synchronizace s využitím protokolu replikace řadiče domény, tak, že agent bude moci dešifrovat obálku.
3.  Po agent synchronizace hesel je má šifrované obálky, používá [MD5CryptoServiceProvider](https://msdn.microsoft.com/library/System.Security.Cryptography.MD5CryptoServiceProvider.aspx) a salt chcete vygenerovat klíč pro dešifrování přijatých dat. zpět do původního MD4. Na žádný bod agent synchronizace hesel mít přístup k hesla v nešifrovaném textu. Agent synchronizace hesel použití algoritmu MD5 je určené výhradně pro kompatibilitu s protokolem replikace se serverem DC a používá se pouze místní mezi řadič domény a agent synchronizace hesel.
4.  Agent synchronizace hesel rozšíří hodnota hash hesla binární 16 bajtů na 64 bajtů převedením první hodnotu hash na hexadecimální řetězec 32 bajtů, pak převedete tento řetězec zpět do binární kódování UTF-16.
5.  Agent synchronizace hesel přidá salt, skládající se z délka v bajtech 10 salt, na binární soubor 64 bajtů dalším stupněm ochrany původní hodnota hash.
6.  Agent synchronizace hesel pak kombinuje hodnota hash MD4 plus salt a vstup do [PBKDF2](https://www.ietf.org/rfc/rfc2898.txt) funkce. 1 000 iterací [HMAC SHA256](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) klíčovaný algoritmus hash se používá. 
7.  Agent synchronizace hesel trvá výsledná hodnota hash 32 bajtů, zřetězí salt a počet iterací SHA256 k němu (pro použití služby Azure AD) a pak přes protokol SSL přenáší řetězec z Azure AD Connect do služby Azure AD.</br> 
8.  Když se uživatel pokusí přihlásit k Azure AD a zadá své heslo, je heslo spusťte pomocí stejné MD4 + řetězce salt + PBKDF2 + HMAC SHA256 procesu. Pokud výsledná hodnota hash odpovídající hodnotě hash uložené ve službě Azure AD, uživatel zadá správné heslo a je ověřen. 

>[!Note] 
>Původní hodnota hash MD4 není přenést do služby Azure AD. Místo toho je hodnota hash SHA256 původní hodnota hash MD4 nepřenáší. Výsledkem je pokud je hodnota hash uložené ve službě Azure AD získali, ho nelze použít při útoku pass-the-hash místně.

### <a name="how-password-synchronization-works-with-azure-active-directory-domain-services"></a>Princip funkce Synchronizace hesla s Azure Active Directory Domain Services
Pomocí funkce Synchronizace hesla můžete také synchronizovat hesla služby místně do [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). V tomto scénáři instanci Azure Active Directory Domain Services se všechny metody dostupné v místní instanci služby Active Directory ověřuje vaši uživatelé v cloudu. Možnosti tohoto scénáře je podobná pomocí migrace nástroj ADMT (Active Directory) v místním prostředí.

### <a name="security-considerations"></a>Aspekty zabezpečení
Při synchronizaci hesel, není pro funkci Synchronizace hesel do služby Azure AD, ani žádný z přidružených službách zpřístupněná heslo ve formátu prostého textu.

Ověření uživatele probíhá služby Azure AD a nikoli proti instanci Active Directory organizace. Pokud má vaše organizace pochybnostmi data hesel v žádném formuláři a místním, třeba vzít v úvahu, že data heslo SHA256 uložená ve službě Azure AD – hodnota hash původní hodnota hash MD4 – je výrazně bezpečnější než co je uložený ve službě Active Directory. Dál platí protože tato hodnota hash SHA256 nelze dešifrovat, nemohou být brzy vrátit do prostředí služby Active Directory organizace a zobrazí jako platné uživatelské heslo v rámci útoku pass-the-hash.





### <a name="password-policy-considerations"></a>Aspekty zásad hesel
Existují dva typy zásad hesel, které jsou ovlivněné povolení synchronizace hesel:

* Zásady složitost hesel
* Zásad vypršení platnosti hesla

#### <a name="password-complexity-policy"></a>Zásady složitost hesel  
Pokud je povolená synchronizace hesel, zásady složitost hesel v místní instanci služby Active Directory přepsat zásady složitost v cloudu pro synchronizované uživatele. Všechny platné hesel můžete použít z vaší místní instancí Active Directory pro přístup ke službám Azure AD.

> [!NOTE]
> Hesla pro uživatele, které jsou vytvořené přímo v cloudu se stále vztahují zásady pro hesla, jak jsou definovány v cloudu.

#### <a name="password-expiration-policy"></a>Zásad vypršení platnosti hesla  
Pokud je uživatel v rozsahu synchronizace hesel, heslo k účtu cloudu nastavená na *nikdy nevyprší*.

Můžete se přihlásit ke cloudovým službám pomocí synchronizované heslo, které je vypršení její platnosti v místním prostředí. Heslo cloudu je aktualizována při příštím změny hesla v místním prostředí.

#### <a name="account-expiration"></a>Vypršení platnosti účtu
Pokud vaše organizace používá atribut AccountExpires zadává jako součást Správa uživatelských účtů, mějte na paměti, že tento atribut není synchronizována do Azure AD. V důsledku toho vypršenou platností účet služby Active Directory v prostředí nakonfigurovaná pro synchronizaci hesla bude nadále aktivní ve službě Azure AD. Doporučujeme vám, že pokud účet vypršela platnost, by měly aktivovat akce pracovního postupu, skript prostředí PowerShell, která zakáže účtu uživatele Azure AD. Naopak když účet je zapnuté, instancí Azure AD by měl být zapnut.

### <a name="overwrite-synchronized-passwords"></a>Přepsat synchronizované hesla
Správce můžete ručně obnovit heslo pomocí prostředí Windows PowerShell.

V takovém případě nové heslo přepíše synchronizované heslo a všechny zásady hesel, které jsou definované v cloudu se použijí k nové heslo.

Pokud změníte heslo místní znovu nové heslo se synchronizují do cloudu, a přepíše ručně aktualizované heslo.

Synchronizace hesla nemá žádný vliv na Azure uživatele, který je přihlášený. Aktuální relace cloudové služby nemá vliv okamžitě změnu hesla synchronizované, ke kterému dochází, když jste přihlášení ke cloudové službě. KMSI rozšiřuje trvání tento rozdíl. Cloudová služba vyžaduje, abyste provést ověření znovu, budete muset zadat nové heslo.

### <a name="additional-advantages"></a>Další výhody

- Obecně platí je jednodušší než federační služby implementace synchronizace hesel. Nevyžaduje žádné další servery a eliminuje závislost na vysoce dostupný federační služby k ověřování uživatelů. 
- Kromě federačních může být rovněž povolena synchronizace hesel. Mohou být použity jako zálohu Pokud své služby FS dojde k výpadku.












## <a name="enable-password-synchronization"></a>Povolení synchronizace hesel
Při instalaci Azure AD Connect s použitím **Expresní nastavení** možnost synchronizace hesel je automaticky povolené. Další podrobnosti najdete v tématu [Začínáme s Azure AD Connect pomocí expresního nastavení](active-directory-aadconnect-get-started-express.md).

Pokud použijete vlastní nastavení při instalaci Azure AD Connect, synchronizace hesel je k dispozici na přihlašovací stránce uživatele. Další podrobnosti najdete v tématu [vlastní instalace Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Povolení synchronizace hesel](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Synchronizace hesel a FIPS
Pokud váš server byl uzamčen podle informace o zpracování FIPS (Federal Standard), je zakázané MD5.

**Povolit synchronizaci hesel MD5, proveďte následující kroky:**

1. Přejděte na Sync\Bin %programfiles%\Azure AD.
2. Otevřete miiserver.exe.config.
3. Přejděte do uzlu Konfigurace nebo modul runtime na konci souboru.
4. Přidejte následující uzly:`<enforceFIPSPolicy enabled="false"/>`
5. Uložte provedené změny.

Pro srovnání je tento fragment kódu, co by měl vypadat jako:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Informace o zabezpečení a FIPS najdete v tématu [AAD Sync heslo, šifrování a FIPS dodržování předpisů](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/).

## <a name="troubleshoot-password-synchronization"></a>Řešení potíží s synchronizace hesel
Pokud máte problémy s synchronizace hesel, najdete v části [řešení synchronizace hesel](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="next-steps"></a>Další kroky
* [Synchronizace Azure AD Connect: přizpůsobení možnosti synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
