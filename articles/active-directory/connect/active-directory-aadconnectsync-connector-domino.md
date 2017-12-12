---
title: Konektoru Lotus Domino | Microsoft Docs
description: "Tento článek popisuje postup při konfiguraci konektoru Domino Lotus společnosti Microsoft."
services: active-directory
documentationcenter: 
author: AndKjell
manager: mtillman
editor: 
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/119/2017
ms.author: barclayn
ms.openlocfilehash: 80151134821c6106382c58bf0ec68ea0f6d4646a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="lotus-domino-connector-technical-reference"></a>Technické reference Domino konektoru Lotus
Tento článek popisuje konektoru Lotus Domino. Se článek vztahuje následující produkty:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manageru 2010 R2 (FIM2010R2)
  * Musíte použít opravu hotfix 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

Pro MIM2016 a FIM2010R2, je k dispozici ke stažení z konektoru [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Přehled konektoru Lotus Domino
Konektoru Lotus Domino můžete integrovat server Lotus Domino společnosti IBM synchronizační služby.

Z hlediska vysoké úrovně služba aktuální verzi konektor podporuje následující funkce:

| Funkce | Podpora |
| --- | --- |
| Připojeného zdroje dat |Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Klienta:<li>Lotus Domino 8.5.x</li><li>Lotus Notes 9.x</li> |
| Scénáře |<li>Správa životního cyklu objektu</li><li>Správa skupin</li><li>Správa hesel</li> |
| Operace |<li>Úplné a rozdílový Import</li><li>Export</li><li>Nastavit a změnit heslo na heslo protokolu HTTP</li> |
| Schéma |<li>Uživatel (uživatel Roaming, obraťte se na (osoby s žádný certifikát))</li><li>Skupina</li><li>Prostředek (prostředků, místnosti, Online schůzky)</li><li>E-mailu v databázi</li><li>Dynamické zjišťování atributy pro podporované objekty</li><li>Podporovat až 250 vlastní udělení licence certifikátorům s organizace & organizační jednotky (OU)</li> |

Konektoru Lotus Domino používá ke komunikaci se serverem Domino Lotus Lotus Notes klienta. V důsledku této závislosti musí být nainstalován podporovaný klient Lotus Notes na serveru pro synchronizaci. Komunikace mezi klientem a serverem se implementuje přes rozhraní Lotus Notes .NET, zprostředkovatel komunikace s objekty (Interop.domino.dll). Toto rozhraní umožňuje komunikaci mezi Microsoft.NET platformy a Lotus Notes klienta a podporuje přístup k Lotus Domino dokumentů a zobrazení. Pro Rozdílový import je také možné, že se používá nativní rozhraní C++ (v závislosti na metodě import vybraných delta).

### <a name="prerequisites"></a>Požadavky
Než použijete tento konektor, zkontrolujte, zda že máte na serveru pro synchronizaci následující požadavky:

* 4.5.2 rozhraní Microsoft .NET Framework nebo novější
* Klient Lotus Notes musí být nainstalován na serveru pro synchronizaci
* Konektoru Lotus Domino vyžaduje výchozí Lotus Domino LDAP schématu databáze (schema.nsf) nacházet na serveru Domino adresáře. Pokud není přítomen, můžete ho nainstalovat spuštění nebo restartování služby LDAP na serveru Domino.

### <a name="connected-data-source-permissions"></a>Oprávnění připojených zdrojů dat
Chcete-li provést některou z podporovaných úlohy v konektoru Lotus Domino, musí být členem následujících skupin:

* Úplný přístup správce
* Správci
* Správce databáze

Následující tabulka uvádí oprávnění, které jsou požadovány pro každou operaci:

| Operace | Přístupová práva |
| --- | --- |
| Import |<li>Veřejné dokumenty pro čtení</li><li> Správce s úplnými oprávněními přístupu (Pokud jste členem skupiny administrators úplný přístup, automaticky máte platný přístup k v seznamu ACL.)</li> |
| Export a nastavit heslo. |Efektivní přístup: <li>Vytváření dokumentů</li><li>Odstranit dokumenty</li><li>Veřejné dokumenty pro čtení</li><li>Zápis veřejné dokumenty</li><li>Replikovat nebo kopírování dokumentů</li>Pro operace exportu musíte taky následující role: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Přímé operace a AdminP
Operace buď přejděte přímo k adresáři Domino nebo prostřednictvím AdminP procesu. Následující tabulky obsahují seznam všech podporovány objekty, operace a, pokud je k dispozici, metoda související implementace:

**Primární adresáře**

| Objekt | Vytvořit | Aktualizace | Odstranění |
| --- | --- | --- | --- |
| Osoba |AdminP |Přímé |AdminP |
| Skupina |AdminP |Přímé |AdminP |
| MailInDB |Přímé |Přímé |Přímé |
| Prostředek |AdminP |Přímé |AdminP |

**Sekundární adresáře**

| Objekt | Vytvořit | Aktualizace | Odstranění |
| --- | --- | --- | --- |
| Osoba |Není k dispozici |Přímé |Přímé |
| Skupina |Přímé |Přímé |Přímé |
| MailInDB |Přímé |Přímé |Přímé |
| Prostředek |Není k dispozici |Není dostupné. |Není k dispozici |

Když je vytvořen prostředek, vytvoří se poznámky k dokumentu. Podobně při odstranění prostředku je odstranit poznámky k dokumentu.

### <a name="ports-and-protocols"></a>Porty a protokoly
IBM Lotus Notes klienta a servery Domino komunikovat pomocí poznámky vzdálené procedury volání (NRPC) kde NRPC musí používat protokol TCP/IP. Výchozí číslo portu je 1352, ale může změnit správce Domino.

### <a name="not-supported"></a>Nepodporuje se
Aktuální verzi konektoru Lotus Domino nepodporuje následující operace:

* Přesuňte poštovní schránky mezi servery.

## <a name="create-a-new-connector"></a>Vytvořit nový konektor
### <a name="client-software-installation-and-configuration"></a>Instalace klientského softwaru a konfigurace
Na serveru musí být nainstalována aplikace Lotus Notes **před** je konektor nainstalovaný.

Když nainstalujete, ujistěte se, můžete udělat **jeden uživatel nainstalovat**. Výchozí hodnota **nainstalovat více uživateli** nefunguje.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Na stránce funkce nainstalovat požadované funkce Lotus Notes a **jeden pro přihlašování klientů**. Je požadováno pro konektor být schopni připojit k serveru Domino jednom přihlášení.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Poznámka:** účet služby spuštění Lotus Notes po s uživatelem, který se nachází na stejném serveru jako účet používáte jako spojnice. Také si nezapomeňte zavřít Lotus Notes klienta na serveru. Nemůže být spuštěna ve stejnou dobu, kdy konektor se pokusí připojit k serveru Domino.

### <a name="create-connector"></a>Vytvořit konektor
Vytvoření konektoru Lotus Domino, v **synchronizační služba** vyberte **agenta pro správu** a **vytvořit**. Vyberte **Lotus Domino (Microsoft)** konektor.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Pokud vaše verze služby synchronizace nabízí možnost konfigurace **architektura**, zajistěte, aby konektor je nastaven na výchozí hodnotu na spouštění **proces**.

### <a name="connectivity"></a>Připojení
Na stránce připojení musíte zadat název serveru Lotus Domino a zadejte přihlašovací údaje.  
![Připojení](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Vlastnost serveru Domino podporuje dva formáty pro název serveru:

* serverName
* ServerName/DirectoryName

**ServerName/DirectoryName** formát je upřednostňovaný formát pro tento atribut, protože když konektor kontaktuje Domino Server poskytuje rychlejší reakci.

Zadaný soubor ID uživatele je uložená v konfigurační databázi služby synchronizace.

Pro **rozdílový Import** máte tyto možnosti:

* **Žádný**. Konektor neprovádí žádné Rozdílový import.
* **Přidat nebo aktualizovat**. Rozdílový import konektor nemá přidejte a operace aktualizace. Pro odstranění **úplný Import** operace je požadováno. Tato operace je pomocí zprostředkovatele komunikace .net.
* **Přidání, aktualizace nebo odstranění**. Rozdílový import konektor nemá přidat, aktualizovat a odstranit operace. Tato operace je pomocí nativních rozhraní C++.

V **možnosti schématu** máte následující možnosti:

* **Výchozí schéma**. Konektor zjistí schématu ze serveru Domino. Tato volba je výchozí možností.
* **Schéma DSML**. Použít jen v případě serveru Domino nevystavuje schématu. Potom můžete vytvořit soubor DSML se schématem a importujte ho. Další informace o DSML najdete v tématu [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Po kliknutí na tlačítko Další, je ověřeno uživatelské ID a heslo parametry konfigurace.

### <a name="global-parameters"></a>Globální parametry
Na stránce globální parametry konfigurace časové pásmo a import a export – možnost operaci.  
![Globální parametry](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

**Časové pásmo serveru Domino** parametr definuje umístění Domino serveru.

Tato možnost konfigurace je nezbytná pro podporu **Rozdílový import** operations protože umožní synchronizační služba zjištění změn mezi posledních dvou importy.

>[!Note]
Počínaje aktualizace března 2017 obrazovce globální parametry nabízí možnost odstranění databáze e-mailu uživatele při odstranění uživatele.

![Odstranit poštovní schránky uživatele](./media/active-directory-aadconnectsync-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Import nastavení, – metoda
**Provést úplné Import pomocí** tyto možnosti:

* Search
* Zobrazení (doporučeno)

**Hledání** je pomocí indexování v Domino, ale je běžné, že indexy se neaktualizují v reálném čase a s daty vrácenými ze serveru vždy není správná. Tato možnost pro systém s mnoho změn, obvykle nebude fungovat správně a poskytuje false odstraní v některých situacích. Ale **vyhledávání** je rychlejší než **zobrazení**.

**Zobrazení** možnost se doporučuje, protože poskytuje správný stav data. Je něco pomalejší než vyhledávání.

#### <a name="creation-of-virtual-contact-objects"></a>Vytvoření virtuální kontaktní objektů
**Povolení vytvoření \_objekt kontakt** tyto možnosti:

* Žádný
* Hodnoty – referenční dokumentace
* Referenční dokumentace a – referenční dokumentace hodnoty

V Domino může obsahovat atributy typu odkaz mnoha různých formátech, chcete-li jiné objekty. Abyste mohli k reprezentaci různých změn, implementuje konektor \_obraťte se na objekty, také známé jako **virtuální kontakty** (VC). Tyto objekty jsou vytvořené tak bylo možné připojit k existující objekty MV nebo projekci jako nové objekty. Tímto způsobem je možné zachovat atribut odkazy.

Povolením tohoto nastavení a pokud není obsah atribut typu odkaz formátu DN \_je vytvořen objekt kontaktu. Atribut člen skupiny může například obsahovat adresy SMTP. Je také možné, že shortName a další atributy, které jsou součástí atributy typu odkaz. V tomto scénáři vyberte **Non-Reference hodnoty**. Tato konfigurace je nejběžnější nastavení pro implementace Domino.

Pokud Lotus Domino je nakonfigurován tak, aby měl samostatné adresáře s různými názvy rozlišující představující stejný objekt, je možné také vytvořit \_obraťte se na objekty pro všechny referenční hodnoty, které se nacházejí v adresáři. V tomto scénáři vyberte **odkaz a hodnoty Non-Reference** možnost.

Pokud máte více hodnot v atributu **FullName** v Domino, potom také chcete povolit vytváření virtuální kontakty, aby odkazy lze vyřešit. Tento atribut například může mít více hodnot po manželství nebo rozvodu. Zaškrtněte políčko **povolit... Úplný název má více hodnot** pro tento scénář.

Díky připojení ke službě na správné atributy, \_obraťte se na objekty by být připojen k objektu MV.

Tyto objekty mají VC =\_kontakt přidán do jejich rozlišující název.

#### <a name="import-settings-conflict-object"></a>Importovat nastavení, konfliktu objektu
**Vyloučit konflikt objektu**

Ve velkých Domino implementace je možné, že více objektů mají stejný název domény z důvodu problémů s replikací. V těchto případech by konektor zobrazí dva objekty s jinou UniversalIDs ale stejné rozlišující název. Tento konflikt by způsobilo objekt přechodný vytváří v prostoru konektoru. Konektor můžete ignorovat objekty, které byly vybrány v Domino jako obětí replikace. Doporučuje se zachovat toto políčko zaškrtnuto.

#### <a name="export-settings"></a>Export nastavení
Pokud možnost **použít AdminP aktualizace odkazů** nezaškrtnuté, pak export atributy typu odkaz, jako je například člena, je přímé volání a nepoužívá AdminP proces. Tuto možnost použijte pouze v případě AdminP nebyl nakonfigurován k udržení referenční integrity.

#### <a name="routing-information"></a>Informace o směrování
V Domino je možné, že atribut typu odkaz obsahuje informace o směrování vložených jako příponu k názvu domény. Například může obsahovat atribut členství ve skupině **CN =example/organization@ABC**. Přípona @ABC je informací o směrování. Informace o směrování Domino používané k odesílání e-mailů správné Domino systém, který může být systém v jiné organizaci. V poli směrování informace můžete zadat směrování přípon používaných v rámci organizace v oboru konektoru. Pokud jako příponu jednu z těchto hodnot naleznete v atribut typu odkaz, odeberou se z odkaz na informace o směrování. Pokud k příponě směrování na hodnotu odkazu na jednu z těchto hodnot zadán, \_je vytvořen objekt kontaktu. Tyto \_obraťte se na objekty jsou vytvořeny pomocí **RO = @<RoutingSuffix>**  vloženy do názvu domény. Pro tyto \_obraťte se na objekty, umožňující připojení k objektu skutečné v případě potřeby jsou rovněž přidány následující atributy: \_routingName, \_kontakt, \_displayName a UniversalID.

#### <a name="additional-address-books"></a>Další adresáře
Pokud nemáte **directory pomoc** nainstalovaná, která poskytuje název sekundární adresářů a pak tyto adresáře lze zadat ručně.

#### <a name="multivalued-transformation"></a>S více hodnotami transformace
Počet atributů v Lotus Domino jsou více hodnotami. Odpovídající atributy úložiště metaverse jsou obvykle jednu hodnot. Nakonfigurováním Import a Export operaci možnost, povolíte konektor usnadní požadované překlad ovlivněných atributů.

**Export**  
Operace možností exportu podporuje dva způsoby:

* Append – položky
* Nahraďte položku

**Nahraďte položku** – Pokud vyberete tuto možnost, konektor vždy odebrat aktuální hodnoty atributu v Domino a nahradíte je zadaných hodnot. Zadaný s hodnotou může být buď jednu nebo více hodnot.

Příklad: Atribut pomocníka osoba objektu má následující hodnoty:

* CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Pokud nového Pomocníka s názvem **David Alexander** je přiřazen k tomuto objektu osoba, výsledkem je:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Připojit položky** – Pokud vyberete tuto možnost, konektor uchovává existující hodnoty pro atribut v Domino a vložení nové hodnoty v horní části seznamu data.

Příklad: Atribut pomocníka osoba objektu má následující hodnoty:

* CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Pokud nového Pomocníka s názvem **David Alexander** je přiřazen k tomuto objektu osoba, výsledkem je:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Import**  
Možnost operace importu podporuje dva způsoby:

* Výchozí
* S více hodnotami pro jednu hodnotu

**Výchozí** – Pokud vyberete možnost výchozí, všechny hodnoty všech atributy jsou importovány.

**S více hodnotami pro jednu hodnotu** – Pokud vyberete tuto možnost, více hodnot atributů je převeden do atribut jednou hodnotou. Pokud existuje více než jednu hodnotu, použije se hodnota nahoře (Tato hodnota se obvykle taky nejnovější).

Příklad: Atribut pomocníka osoba objektu má následující hodnoty:

* CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Gregu Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN = Jan Smith/OU=Contoso/O=Americas,NAB=names.nsf

Nejnovější aktualizace pro tento atribut je **David Alexander**. Vzhledem k tomu, že je možnost operace importu nastavena na Multivalued pro jednu hodnotu, konektor importuje jenom **David Alexander** do prostoru konektoru.

Logika pro převést více hodnot atributů jednohodnotové atributy nevztahuje do atribut člena skupiny a do atribut fullname osoby.

Je také možné nakonfigurovat import a export pravidel transformace pro vícehodnotové atributy každý atribut, jako výjimku do globální pravidlo. Chcete-li konfigurovat tuto možnost, zadejte [objecttype]. [attributename] v **importovat seznam atributů vyloučení** a **exportovat seznam atributů vyloučení** textových polí. Například pokud zadáte Person.Assistant a je globální nastavený příznak k importu všech hodnot, je pro asistenta importován pouze první hodnota.

#### <a name="certifiers"></a>Udělení licence certifikátorům
Všechny organizace nebo organizační jednotky jsou uvedeny pomocí konektoru. Abyste mohli exportovat objekty osoba do primární adresáře, je vyžadován certifier s jeho heslo.

Pokud všechny udělení licence certifikátorům mají stejné heslo, **heslo pro všechny Certifers** lze použít. Pak můžete zadat heslo sem a určovat pouze soubor certifier.

Pokud jenom importujete, není nutné zadat všechny udělení licence certifikátorům.

### <a name="configure-provisioning-hierarchy"></a>Konfigurace hierarchie zřizování
Při konfiguraci konektoru Lotus Domino, přeskočte tuto stránku dialogové okno. Konektoru Lotus Domino nepodporuje hierarchie zřizování.  
![Zřizování hierarchie](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurace oddílů a hierarchií
Když konfigurujete oddílů a hierarchií, musíte vybrat primární adresář s názvem NAB=names.nsf. Kromě primární adresáře aplikace můžete vybrat sekundární adresářů, pokud existují.  
![Oddíly](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Vyberte atributy
Když konfigurujete vaší atributy, je nutné vybrat všechny atributy, které mají předponu  **\_MMS\_**. Tyto atributy jsou požadovaná při zřizování nové objekty, které se Lotus Domino

![Atributy](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Správa životního cyklu objektu
Tato část obsahuje přehled různých objektů v Domino.

### <a name="person-objects"></a>Objekty osoba
Objekt osoba představuje uživatele v organizaci a organizační jednotky. Kromě výchozí atributy Domino správce může přidat vlastní atributy objektu osoba. Minimálně musí obsahovat objekt osoba všechny povinné atributy. Úplný seznam povinné atributy, najdete v části [Lotus Notes vlastnosti](#lotus-notes-properties). Pokud chcete zaregistrovat objekt osoba, musí být splněny následující požadavky:

* Adresář (names.nsf) musí být definována a měla by být primární adresáře.
* Musíte mít O/OU certifier Id a heslo k registraci určitého uživatele v organizaci nebo organizační jednotka.
* Je nutné nastavit určitou sadu Lotus Notes vlastnosti pro objekt osoby. Tyto vlastnosti se používají pro zřizování objekt osoby. Další informace najdete v tématu části s názvem [Lotus Notes vlastnosti](#lotus-notes-properties) dále v tomto dokumentu.
* Počáteční heslo protokolu HTTP pro osoby je atribut a sadu během zřizování.
* Objekt osoba musí mít jednu z následujících tři podporované typy:
  1. Normální uživatele, který má soubor e-mailu a soubor id uživatele
  2. Roaming uživatele (obvykle uživatel, který zahrnuje všechny roamingu soubory databáze)
  3. Kontakty (uživatele s žádný soubor id)

Osob (s výjimkou kontakty) je možné seskupit do US uživatelům a uživatelům mezinárodní další, podle definice hodnotu \_MMS\_IDRegType vlastnost. Tyto osoby pomocí klienta poznámky pro přístup k serverům Lotus Domino mít Id poznámky a osoby dokumentu. Pokud používají poznámky k e-mailu, pak mají také soubor e-mailu. Uživatel musí být zaregistrován stane aktivním. Další informace naleznete v tématu:

* [Nastavení uživatelů poznámky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Registrace uživatele](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Správa uživatelů](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Přejmenování uživatele](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Všechny tyto operace jsou prováděny v Lotus Domino a pak importovat do synchronizační služby.

### <a name="resources-and-rooms"></a>Prostředky a místnosti
Prostředek je databáze v Lotus Domino jiného typu. Prostředky, může být konferenčních místností s různými typy zařízení, například projektory. Existují podtypů nepodporuje konektoru Lotus Domino prostředky, které jsou definovány atribut typ prostředku:

| Typ prostředku | Atribut typ prostředku |
| --- | --- |
| Místnosti |1 |
| Prostředek (Další) |2 |
| Online schůzky |3 |

Pro daný typ prostředku objekt pro práci je nutné provést následující:

* Rezervace databáze prostředků by již měla existovat na připojené Domino serveru
* Webu je již definována pro prostředek

Databáze prostředků rezervace obsahuje tři typy dokumentů:

* Profil společnosti
* Prostředek
* Rezervace

Další informace o nastavení databáze prostředků rezervace, najdete v části [nastavení databáze prostředků rezervace](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Vytvářet, aktualizovat a odstraňovat prostředky**  
Operace Create, Update a Delete jsou prováděny konektoru Lotus Domino v databázi prostředků rezervace. Prostředky jsou vytvořené jako dokumenty v Names.nsf (tedy primární adresáře). Další informace o úpravy a odstranění prostředků najdete v tématu [úpravy a odstraňování dokumentů prostředků](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Import a Export operace pro prostředky**  
Prostředky můžete importovat do a exportovat ze služby synchronizace, jako libovolný jiný typ objektu. Vyberte typ objektu jako prostředek během konfigurace. Pro úspěšné exportu měli byste mít podrobnosti pro typ prostředku, konferenční databáze a název lokality.

### <a name="mail-in-databases"></a>E-mailu v databázích
E-mailu v databáze je databáze, která je určená pro příjem e-mailů. Je Lotus poštovní schránky, není přidružen k žádné konkrétní uživatelský účet Lotus Domino (to znamená, nemá svůj vlastní soubor ID a heslo). E-mailu v databázi má jedinečný UserID (dále jen "krátkého názvu") přidružené k jeho a vlastní e-mailovou adresu.

Pokud je zapotřebí pro samostatné poštovní schránku s vlastní e-mailovou adresu, který může být sdílen mezi různými uživateli (například group@contoso.com), bude vytvořena databáze v e-mailu. Přístup k této poštovní schránky se řídí prostřednictvím jeho řízení přístupu seznamu (ACL), který obsahuje jména uživatelů poznámky, které jsou povoleny otevřete poštovní schránku.

Seznam povinné atributy, najdete v části [povinné atributy](#mandatory-attributes) dále v tomto článku.

Pokud databázi je určen pro příjem e-mailu, vytvoří se v Lotus Domino dokumentu e-mailu v databázi. Tento dokument musí existovat v adresáři Domino všech serverů, které je uložená kopie databáze. Podrobný popis vytvoření dokumentu v e-mailu v databázi, naleznete v části [vytváření e-mailu v databázi dokumentů](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Před vytvořením e-mailu v databázi, databázi by již měla existovat (měla být vytvořená Lotus správce) na serveru Domino.

### <a name="group-management"></a>Správa skupin
Podrobný přehled o správu skupiny Lotus Domino můžete získat z následujících zdrojů:

* [Použití skupin](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Vytvoření skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Vytvoření a úprava skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Správa skupin](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Přejmenování skupiny](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Správa hesel
Registrovaný uživatel Lotus Domino existují dva typy hesel:

1. Heslo uživatele (uložený v souboru User.id)
2. Internet nebo heslo protokolu HTTP

Konektoru Lotus Domino podporuje pouze operace s heslem HTTP.

K provedení Správa hesel, měli byste povolit správu hesel pro konektor v Návrháři správy agenta. Chcete-li povolit správu hesel, vyberte **povolit správu hesel** na **konfigurace rozšíření** dialogové okno stránky.  
![Konfigurace rozšíření](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Podpora konektoru Lotus Domino následující operace na Internetu heslo:

* Nastavit heslo: Nastavit heslo nastaví na uživatele v Domino nové heslo protokolu HTTP nebo Internetu. Ve výchozím nastavení je účet také odemknout. Příznak odemčení je vystaven na rozhraní WMI synchronizační modul.
* Změna hesla: V tomto scénáři, uživatel může být vhodné změnit heslo nebo je výzva ke změně hesla po určitou dobu. Tato operace trvat umístit, (starý i nové heslo) jsou povinné. Jakmile změníte, nové heslo se aktualizuje v Lotus Domino.

Další informace naleznete v tématu:

* [Pomocí funkce uzamčení Internetu](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Správa hesel Internetu](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Referenční informace
Tato část uvádí například popisy atribut a atribut požadavky pro konektoru Lotus Domino.

### <a name="lotus-notes-properties"></a>Vlastnosti aplikace Lotus Notes
Pokud zřizujete osoba objekty do vašeho adresáře Lotus Domino, vaše objekty musí mít konkrétní sadu vlastností s konkrétními hodnotami naplněno. Tyto hodnoty jsou pouze potřebné pro vytvoření operací.

Následující tabulka uvádí tyto vlastnosti a obsahuje popis z nich.

| Vlastnost | Popis |
| --- | --- |
| \_MMS_AltFullName |Alternativní celý název uživatele. |
| \_MMS_AltFullNameLanguage |Jazyka, který má být použit k určení alternativního úplný název tohoto uživatele. |
| \_MMS_CertDaysToExpire |Počet dní od dnešního data před certifikátu vyprší. Pokud není zadáno, je výchozí datum dva roky od dnešního data. |
| \_MMS_Certifier |Vlastnost, která obsahuje název organizační hierarchie certifier. Příklad: OU = OrganizationUnit, O = Org, C = země. |
| \_MMS_IDPath |Pokud vlastnost je prázdná, žádný soubor identifikace uživatele vytvoří místně na serveru pro synchronizaci. Pokud vlastnost obsahuje název souboru, soubor ID uživatele se vytvoří ve složce madata. Vlastnost může také obsahovat úplnou cestu. |
| \_MMS_IDRegType |Osob můžou být klasifikované jako kontakty, nám uživatelů a mezinárodní uživatele. Následující tabulka uvádí možné hodnoty: <li>0 – kontakt</li><li>1 - US uživatele</li><li>2 – mezinárodní uživatele</li> |
| \_MMS_IDStoreType |Požadovaná vlastnost pro USA a mezinárodní uživatele. Vlastnost obsahuje celočíselná hodnota, která určuje, zda je identifikace uživatele uložený jako přílohu v adresáři poznámky nebo v souboru e-mailu osoby. Pokud je soubor ID uživatele přílohy v adresáři, volitelně vytvořením jako soubor s \_MMS_IDPath. <li>Prázdný - ID úložiště souborů v ID trezoru, žádný soubor identifikace (používá se pro kontakty).</li><li> 1 - přílohy v adresáři poznámky. \_MMS_Password vlastnost musí být nastavena pro uživatele identifikace soubory, které jsou přílohy</li><li>2 - ID úložiště v souboru e-mailu osoby. \_MMS_UseAdminP musí být nastavena na hodnotu false, umožníte soubor e-mailu během registrace osoba nelze vytvořit. \_MMS_Password vlastnost musí být nastavena pro soubory identifikace uživatele.</li> |
| \_MMS_MailQuotaSizeLimit |Počet MB, které jsou povoleny pro databázový soubor e-mailu. |
| \_MMS_MailQuotaWarningThreshold |Počet MB, které jsou povoleny pro databázi soubor e-mailu před objeví se upozornění. |
| \_MMS_MailTemplateName |Soubor šablony e-mailu, který se používá k vytvoření souboru e-mailu uživatele. Pokud je zadat šablonu, vytvoří se soubor e-mailu použitím určené šablony. Pokud je zadána žádná šablona, výchozí šablony soubor se používá k vytvoření souboru. |
| \_MMS_OU |Volitelné vlastnosti, která je název organizační jednotky v rámci certifier. Tato vlastnost by měla být prázdná pro kontakty. |
| \_MMS_Password |Požadovaná vlastnost pro uživatele. Vlastnost obsahuje heslo pro soubor identifikace objektu. |
| \_MMS_UseAdminP |Vlastnost musí být nastavená na hodnotu true, pokud soubor e-mailu by měl vytvořit proces AdminP na serveru Domino (asynchronní do procesu exportu). Pokud je vlastnost nastavena na hodnotu false, e-mailu soubor je vytvořen s uživatelem Domino (synchronní v procesu exportu). |

Pro uživatele se souborem přidružené identifikace \_vlastnost MMS_Password musí obsahovat hodnotu. Pro přístup k e-mailu prostřednictvím klienta Lotus Notes vlastnosti MailServer a MailFile uživatele musí obsahovat hodnotu.

Pro přístup k e-mailu pomocí webového prohlížeče, musí obsahovat následující vlastnosti hodnoty:

* MailFile - povinnou vlastnost, která obsahuje cestu na serveru Lotus Domino, kde je uložen soubor e-mailu.
* MailServer - povinnou vlastnost, která obsahuje název serveru Lotus Domino. Tato hodnota je název, který má použít při vytváření souboru Lotus e-mailu na serveru Domino.
* HTTPPassword - volitelné vlastnosti, která obsahuje webové přístupové heslo pro objekt.

Pro přístup k serveru Domino bez schopnosti e-mailu, vlastnost HTTPPassword musí obsahovat hodnotu. Vlastnost MailFile a vlastnost MailServer nesmí být prázdné.

S \_MMS_ IDStoreType = 2 (id úložiště v souboru e-mailu), vlastnost MailSystem NotesRegistrationclass je nastavena na REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Povinné atributy
Konektoru Lotus Domino především podporuje tyto typy objektů (typů dokumentů):

* Skupina
* E-mailu v databázi
* Osoba
* Obraťte se na (osoba s žádné certifier)
* Prostředek

Tato část obsahuje seznam atributy, které jsou pro každý podporovaný objekt, který chcete exportovat do serveru Domino povinné.

| Typ objektu | Povinné atributy |
| --- | --- |
| Skupina |<li>ListName</li> |
| V hlavní databázi |<li>Úplný název</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Osoba |<li>Příjmení</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Obraťte se na (osoba s žádné certifier) |<li>\_MMS_IDRegType</li> |
| Prostředek |<li>Úplný název</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Lokality</li><li>displayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Běžné problémy a otázky
### <a name="schema-detection-does-not-work"></a>Schéma detekce nefunguje.
Abyste mohli zjistit schématu, je nutné na serveru Domino nachází soubor schema.nsf. Tento soubor se zobrazí, pouze pokud LDAP je nainstalován na serveru. Pokud není schéma rozpoznat, ověřte následující:

* Schema.nsf soubor se nachází v kořenové složce serveru Domino
* Uživatel má oprávnění k zobrazení souboru schema.nsf.
* Vynuťte restartování serveru LDAP. Otevřete **Lotus Domino konzoly** a používat **říct ReloadSchema LDAP** příkaz znovu načíst schéma.

### <a name="not-all-secondary-address-books-are-visible"></a>Ne všechny sekundární adresáře jsou viditelné
Konektor Domino spoléhá na funkce **Directory pomoc** být schopni najít sekundární adresáře. Pokud chybí sekundární adresářů, ověřte, zda [Directory pomoc](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) je povolená a nakonfigurovaná na serveru Domino.

### <a name="custom-attributes-in-domino"></a>Vlastní atributy v Domino
Existuje několik způsobů v Domino rozšíření schématu, takže se jeví jako vlastní atribut použití konektorem.

**Způsob 1: Rozšíření schématu Lotus Domino**

1. Vytvořit kopii šablony Directory Domino {PUBNAMES. NTF} podle [tyto kroky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nesmí přizpůsobit výchozí adresář IBM Lotus Domino šablony):
2. Otevřete šablonu directory kopie Domino {CONTOSO. Šablona NTF}, který byl vytvořen v Návrháři Domino a postupujte podle těchto kroků:
   * Klikněte na tlačítko sdílené elementy a rozbalte podformulářů
   * Klikněte dvakrát na ${ObjectName} InheritableSchema podformulář (kde {ObjectName} je název výchozí třídy strukturální objektu, například: osoba).
   * Název atributu, který chcete přidat do schématu {MyPersonAtrribute} a odpovídající pro tento atribut. Vytvořit pole vyberte **vytvořit** nabídce a potom vyberte **pole** z nabídky.
   * V poli přidané nastavte tak, že vyberete typ, styl, velikost, písma a dalších souvisejících parametrů na pole vlastnosti – okno jeho vlastnosti.
   * Zachovat atribut výchozí hodnota stejná jako název pro tuto vlastnost (například pokud je název atributu MyPersonAttribute, ponechte výchozí hodnotu se stejným názvem).
   * Uložte podformulář InheritableSchema ${ObjectName} s aktualizovanými hodnotami.
3. Nahrazení šablony Directory Domino {PUBNAMES. NTF} s novou vlastní šablonu {CONTOSO. NTF} podle [tyto kroky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zavřete Správce Domino a otevřete konzolu Domino restartování služby LDAP a nové načtení schématu LDAP:
   * V konzole Domino, vložte příkaz pod **příkaz Domino** text selhala se restartovat službu LDAP - [restartujte LDAP úloh](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
   * Znovu načíst LDAP schématu použijte příkaz říct LDAP - říct ReloadSchema LDAP
5. Otevřete správce Domino a vyberte kartu uživatelé a skupiny najdete v části Přidání atributu se odrazí v domino přidat osoby.
6. Otevřete Schema.nsf z **soubory** kartě a najdete v části Přidání atributu se projeví do třídy objektu dominoPerson LDAP.

**Způsob 2: Vytvoření auxClass pomocí vlastních atributů a přidružit třídu objektu**

1. Vytvořit kopii šablony Directory Domino {PUBNAMES. NTF} podle [tyto kroky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nikdy přizpůsobit výchozí adresář IBM Lotus Domino šablony):
2. Otevřete šablonu directory kopie Domino {CONTOSO. Šablona NTF}, který byl vytvořen v Návrháři Domino.
3. V levém podokně vyberte sdíleného kódu a následně podformulářů.
4. Klikněte na položku nový
5. Následujícím postupem zadejte vlastnosti pro nové odpovídající:
   * S novou podformulářem otevřete, zvolte návrh - podformulář vlastnosti
   * Vedle vlastnosti Name zadejte název pro třídu pomocného objektu – například TestSubform.
   * Zachovat vlastnost možnosti "Zahrnout podformulář Insert... dialogové okno" vybrali
   * Zrušte výběr možnosti vlastnost "vykreslení průchodu přes HTML v poznámkách."
   * Nechte ostatní vlastnosti stejné a zavřete okno podformulář vlastnosti.
   * Uložte a zavřete nový podformulář.
6. Následujícím postupem přidat pole, které chcete definovat třídu pomocného objektu:
   * Otevřete podformulář, kterou jste vytvořili.
   * Zvolte vytvořit - pole.
   * Vedle názvu na kartě Základní informace o dialogovém okně pole, zadejte jakýkoli název, například: {MyPersonTestAttribute}.
   * V poli přidané nastavte výběrem typu, stylu, velikost, písma a souvisejících vlastností jeho vlastnosti.
   * Zachovat atribut výchozí hodnota stejná jako název pro tuto vlastnost (například pokud je název atributu MyPersonTestAttribute, ponechte výchozí hodnotu se stejným názvem).
   * Uložit podformuláře s aktualizovanými hodnotami a postupujte takto:
     * V levém podokně vyberte sdíleného kódu a následně podformulářů
     * Vyberte nový podformulář a zvolte návrh - vlastnosti návrhu.
     * Klikněte na kartu třetí zleva a vyberte **rozšířit tento zákaz změnu návrhu**.
7. Otevřete podformulář ExtensibleSchema ${ObjectName}, (kde {ObjectName} je název výchozí třídy strukturální objektu, například – osoba).
8. Vložit prostředek a vyberte podformulář (který jste vytvořili, například – TestSubform) a uložte podformulář ExtensibleSchema ${ObjectName}.
9. Nahrazení šablony Directory Domino {PUBNAMES. NTF} s novou vlastní šablonu {CONTOSO. NTF} podle [tyto kroky](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zavřete Správce Domino a otevřete konzolu Domino restartování služby LDAP a nové načtení schématu LDAP:
    * V konzole Domino, vložte příkaz pod **příkaz Domino** text selhala se restartovat službu LDAP - [restartujte LDAP úloh](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html).
    * Znovu načíst LDAP schéma, použijte příkaz říct LDAP **říct ReloadSchema LDAP**.
11. Otevřete správce Domino a vyberte kartu lidé a skupiny zobrazíte přidané atribut se odrazí v domino přidat osoba (v rámci jiné kartě).
12. Otevřete Schema.nsf z **soubory** kartě a najdete v části Přidání atributu se projeví ve TestSubform LDAP pomocnou třídu objektu.

**Způsob 3: Přidejte do třídy ExtensibleObject vlastních atributů**

1. Otevření souboru {Schema.nsf} umístit do kořenového adresáře
2. Vyberte objekt třídy LDAP v levé nabídce v části **všechny dokumenty schématu** a klikněte na tlačítko **přidat objekt třídy** tlačítko:
3. Zadejte název protokolu LDAP ve formě {zzzExtensibleSchema} (kde zzz je název výchozí třídy strukturální objektu, například osoba). Například pro rozšíření schématu pro osobu třídu objektu, zadejte název protokolu LDAP {PersonExtensibleSchema}.
4. Zadejte název třídy pro objekt nadřízeného objektu, pro který chcete rozšířit schéma. Například rozšíření schématu pro třídu objektu osoba, zadejte název třídy pro objekt nadřízeného objektu {dominoPerson}:
5. Zadejte platný OID odpovídající třídu objektu.
6. Vyberte rozšířené nebo vlastních atributů v povinné nebo nepovinný atribut typy polí podle požadavek na:
7. Po přidání do ExtensibleObjectClass povinné atributy, klikněte na tlačítko **uložit a zavřít**.
8. ExtensibleObjectClass se vytvoří pro příslušné výchozí třídu objektů s rozšířenými atributy.

## <a name="troubleshooting"></a>Řešení potíží
* Informace o tom, jak povolit protokolování pro řešení potíží s konektoru najdete v tématu [postup povolení trasování ETW pro konektory](http://go.microsoft.com/fwlink/?LinkId=335731).
