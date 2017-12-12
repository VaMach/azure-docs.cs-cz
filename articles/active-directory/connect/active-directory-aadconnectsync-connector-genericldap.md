---
title: "Generický konektor LDAP | Microsoft Docs"
description: "Tento článek popisuje, jak nakonfigurovat obecná konektor LDAP společnosti Microsoft."
services: active-directory
documentationcenter: 
author: AndKjell
manager: mtillman
editor: 
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: fe8db8f2a2412a3dfdf31201678c51e4fa0cee30
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="generic-ldap-connector-technical-reference"></a>Technické informace o obecné konektor LDAP
Tento článek popisuje obecný konektor LDAP. Se článek vztahuje následující produkty:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manageru 2010 R2 (FIM2010R2)
  * Musíte použít opravu hotfix 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

Pro MIM2016 a FIM2010R2, je k dispozici ke stažení z konektoru [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Při odkazování na dokumenty IETF RFC, tento dokument je ve formátu (RFC [RFC číslo] nebo [oddílu v dokumentu RFC]), například (RFC 4512/4.3).
Další informace najdete v http://tools.ietf.org/html/rfc4500 (budete muset nahraďte správné číslo RFC 4500).

## <a name="overview-of-the-generic-ldap-connector"></a>Přehled generický konektor LDAP
Obecné konektor LDAP umožňuje integraci se serverem LDAP v3 synchronizační služby.

Některé operace a prvky schématu, jako jsou ty, které vyžaduje k provedení Rozdílový import, nebyly zadány v dokumentech IETF RFC. Pro tyto operace jsou podporovány pouze adresáře LDAP explicitně určena.

Z hlediska vysoké úrovně služba aktuální verzi konektor podporuje následující funkce:

| Funkce | Podpora |
| --- | --- |
| Připojeného zdroje dat |Konektor je podporována u všech serverů LDAP v3 (specifikaci RFC 4510). Byl testován s následujícími službami: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directory globální katalog (AD GC)</li><li>389 adresářový Server</li><li>Apache adresářový Server</li><li>IBM Tivoli DS</li><li>Isode adresáře</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Otevřete DJ</li><li>Otevřete DS</li><li>Otevřete LDAP (openldap.org)</li><li>Oracle (dříve Sun) Directory Server Enterprise Edition</li><li>Virtuální adresář serveru RadiantOne (VDS)</li><li>Jeden Sun Directory Server</li>**Významné adresářů není podporována:** <li>Microsoft Active Directory Domain Services (AD DS) [integrovaného konektoru služby Active Directory použijte místo toho]</li><li>Oracle Internet adresáře (OID)</li> |
| Scénáře |<li>Správa životního cyklu objektu</li><li>Správa skupin</li><li>Správa hesel</li> |
| Operace |Podporovány jsou následující operace pro všechny adresáře LDAP: <li>Úplný Import</li><li>Export</li>Tyto operace jsou podporovány pouze na zadaných adresářích:<li>Rozdílový import</li><li>Nastavení hesla, změnit heslo</li> |
| Schéma |<li>Schéma je zjištěna ze schématu protokolu LDAP (RFC3673 a RFC4512 nebo 4.2)</li><li>Podporuje strukturální třídy, aux třídy a třída objektu extensibleObject (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Podpora správy Rozdílový import a heslo
Rozdílový import a Správa hesel podporováno adresáře:

* Microsoft Active Directory Lightweight Directory Services (AD LDS)
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavení hesla
* Microsoft Active Directory globální katalog (AD GC)
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavení hesla
* 389 adresářový Server
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Apache adresářový Server
  * Vzhledem k tomu, že tento adresář nemá trvalé změny protokolu nepodporuje Rozdílový import
  * Podporuje nastavení hesla
* IBM Tivoli DS
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Isode adresáře
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Novell eDirectory a NetIQ eDirectory
  * Podporuje operace přidat, aktualizace a přejmenování pro Rozdílový import
  * Nepodporuje operace Delete pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Otevřete DJ
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Otevřete DS
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Otevřete LDAP (openldap.org)
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavení hesla
  * Nepodporuje změnu hesla
* Oracle (dříve Sun) Directory Server Enterprise Edition
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Virtuální adresář serveru RadiantOne (VDS)
  * Musí používat verzi 7.1.1 nebo vyšší
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo
* Jeden Sun Directory Server
  * Podporuje všechny operace pro Rozdílový import
  * Podporuje nastavit heslo a změnit heslo

### <a name="prerequisites"></a>Požadavky
Než použijete tento konektor, ujistěte se, že máte k dispozici následující na serveru synchronizace:

* 4.5.2 rozhraní Microsoft .NET Framework nebo novější

### <a name="detecting-the-ldap-server"></a>Zjišťování serveru LDAP
Konektor závisí na různých technik ke zjišťování a identifikaci serveru LDAP. Konektor používá DSE kořenové, dodavatele název, verzi a ho kontroluje schéma najít jedinečný objektů a atributů ví, že existují v určitých serverů LDAP. Tato data, pokud nalezen, bude použita k předběžnému naplnění možnosti konfigurace v konektoru.

### <a name="connected-data-source-permissions"></a>Oprávnění připojených zdrojů dat
Chcete-li provést import a export operací na objektech v adresáři připojené, musí mít účet konektoru dostatečná oprávnění. Konektor musí být schopni export oprávnění k zápisu a oprávnění ke čtení pro moci importovat. Konfigurace oprávnění se provádí v rámci správy zkušenosti cílový adresář sám sebe.

### <a name="ports-and-protocols"></a>Porty a protokoly
Konektor používá číslo portu, který je zadaný v konfiguraci, který ve výchozím nastavení je 389 pro LDAP a 636 pro LDAPS.

Pro LDAPS musí používat protokol SSL 3.0 nebo TLS. Protokol SSL 2.0 není podporována a nelze aktivovat.

### <a name="required-controls-and-features"></a>Požadovaný ovládací prvky a funkce
Tyto ovládací prvky LDAP nebo funkce musí být k dispozici na serveru LDAP pro konektor správně fungovat:  
`1.3.6.1.4.1.4203.1.5.3`Filtry true nebo False

Filtr hodnotu True nebo False není hlášena často podporuje adresáře LDAP a může zobrazovat ve **globální stránky** pod **povinné funkce nebyla nalezena**. Se používá k vytvoření **nebo** filtry v dotazech LDAP, například při importu více typů objektu. Pokud importujete více než jeden typ objektu, serveru LDAP podporuje tuto funkci.

Pokud používáte adresář, kde je jedinečný identifikátor ukotvení následující musí být také k dispozici (Další informace najdete v tématu [konfigurace kotvy](#configure-anchors) část):  
`1.3.6.1.4.1.4203.1.5.1`Všechny provozní atributy

Pokud adresář má více objektů, než co vejde na jedno volání do adresáře, pak se doporučuje použít stránkování. Pro stránkování pracovat, je třeba jeden z následujících možností:

**Možnost 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Možnost 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Pokud obě možnosti jsou povolené v konfiguraci konektoru, použije se pagedResultsControl.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl používá pouze pro metodu USNChanged Rozdílový import do Zobrazit odstraněné objekty.

Konektor se pokusí zjistit možnosti nachází na serveru. Když možnosti nelze rozpoznat, upozornění se nachází na stránce globální ve vlastnostech konektoru. Ne všechny servery LDAP nachází všechny ovládací prvky nebo funkce podporují a i v případě, že toto upozornění je k dispozici, konektor může fungovat bez problémů.

### <a name="delta-import"></a>Rozdílový import
Rozdílový import je k dispozici, pouze pokud byla zjištěna podpora adresáře. Aktuálně se používají následující metody:

* LDAP Accesslog. V tématu [http://www.openldap.org/doc/admin24/overlays.html#Access protokolování](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
* Protokol LDAP změn. V tématu [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Časové razítko. Pro Novell/NetIQ eDirectory konektor používá datum a čas poslední potřebujete vytvořit a aktualizovat objekty. Novell/NetIQ eDirectory neposkytuje ekvivalentní prostředky k načtení odstraněných objektů. Tuto možnost lze také Pokud žádnou jinou metodu Rozdílový import je aktivní na serveru LDAP. Tato možnost není možné odstranit import objektů.
* USNChanged. Přejděte na téma: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nepodporuje se
Nejsou podporovány následující funkce LDAP:

* Referenční seznamy LDAP mezi servery (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Vytvořit nový konektor
Chcete-li vytvořit generický konektor LDAP, v **synchronizační služba** vyberte **agenta pro správu** a **vytvořit**. Vyberte **obecné LDAP (Microsoft)** konektor.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Připojení
Na stránce připojení musíte určit informace o hostiteli, Port a vazby. V závislosti na tom, které je vazba vybrané, další mohou být dodány informace v následujících částech.

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

* Časový limit připojení nastavení slouží pouze pro první připojení k serveru při zjišťování schématu.
* Pokud vazba je anonymní, pak ani uživatelského jména a hesla ani certifikát se používají.
* Pro další vazby, zadejte informace o buď v uživatelské jméno / heslo nebo vyberte certifikát.
* Pokud používáte protokol Kerberos k ověření, zadejte také sféry nebo domény uživatele.

**Atribut aliasy** textového pole se používá pro atributy definované ve schématu se syntaxí RFC4522. Tyto atributy nebyl nalezen při rozpoznávání schématu a konektor musí nápovědy k identifikaci těchto atributů. V poli atributů aliasy správně identifikovat atribut userCertificate jako binární atribut je třeba zadat například následující:

`userCertificate;binary`

Následuje příklad, jak může vypadat tuto konfiguraci:

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Vyberte **zahrnout provozní atributy do schématu** políčko zahrnout také atributy vytvořená serverem. Mezi ně patří atributy, jako je například když objekt byl vytvořen a čas poslední aktualizace.

Vyberte **zahrnout extensible atributy do schématu** budou použita rozšiřitelné objekty (RFC4512/4.3) a povolením této možnosti umožníte každý atribut, který se má použít na všechny objektu. Tato možnost vám schéma velké, pokud je připojený adresář pomocí této funkce doporučujeme ponechat možnost zrušit.

### <a name="global-parameters"></a>Globální parametry
Na stránce globální parametry konfigurace DN rozdílů protokol změn a další funkce LDAP. Stránce je již nastavena informace poskytované serverem LDAP.

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

V horní části zobrazí informace poskytované serverem samostatně, jako je název serveru. Konektor také ověří, zda povinné ovládací prvky jsou k dispozici v DSE kořenové. Pokud tyto ovládací prvky nejsou na seznamu, se zobrazí upozornění. Některé adresáře LDAP, neuvádějte všem funkcím v DSE kořenové a je možné, že konektor funguje bez problémů, i když upozornění je k dispozici.

**Podporované ovládací prvky** zaškrtávací políčka řídí chování pro určité operace:

* Pomocí stromové struktury odstranit vybrané, se odstraní hierarchie s jedno volání LDAP. S stromu odstranit zrušit konektor nebude odstranění rekurzivní v případě potřeby.
* Konektor s stránkových výsledků vybraného, nemá stránkové import s zadaná velikost na spuštění kroky.
* VLVControl a SortControl je alternativa k pagedResultsControl číst data z adresáře LDAP.
* Pokud jsou všechny tři možnosti (pagedResultsControl, VLVControl a SortControl) nezaškrtnuté konektor importuje všech objektů v rámci jedné operace, které může selhat, pokud je to velké adresář.
* ShowDeletedControl se používá pouze v případě metody import Delta USNChanged.

Protokol změn rozlišující název je názvový kontext, který používá protokol změn rozdílů, například **cn = protokol změn**. Abyste mohli provést Rozdílový import je třeba zadat tuto hodnotu.

Následuje seznam protokol změn výchozí DNs:

| Adresář | Protokol změn rozdílů |
| --- | --- |
| Uvolňování paměti Microsoft AD LDS a AD |Automaticky zjištěna. USNChanged. |
| Apache adresářový Server |Není k dispozici. |
| Directory 389 |Protokol změn. Výchozí hodnota použití: **cn = protokol změn** |
| IBM Tivoli DS |Protokol změn. Výchozí hodnota použití: **cn = protokol změn** |
| Isode adresáře |Protokol změn. Výchozí hodnota použití: **cn = protokol změn** |
| Novell/NetIQ eDirectory |Není k dispozici. Časové razítko. Konektor používá poslední datum a čas získat aktualizace a aktualizace záznamů. |
| Otevřete DJ/DS |Protokol změn.  Výchozí hodnota použití: **cn = protokol změn** |
| Otevřete LDAP |Přístup k protokolu. Výchozí hodnota použití: **cn = accesslog** |
| Oracle DSEE |Protokol změn. Výchozí hodnota použití: **cn = protokol změn** |
| RadiantOne VDS |Virtuální adresář. Závisí na adresář, připojený k VDS. |
| Jeden Sun Directory Server |Protokol změn. Výchozí hodnota použití: **cn = protokol změn** |

Atribut password je název atributu, který konektor by měl použít k nastavení hesla v změny hesla a operace nastavené heslo.
Tato hodnota je výchozím nastavením **userPassword** však lze změnit v případě potřeby pro konkrétní systém LDAP.

V seznamu další oddíly je možné přidat další obory názvů automaticky zjištěna. Toto nastavení můžete například použijí, pokud několik serverů, které tvoří logickou cluster, který by měl být importovány všechny ve stejnou dobu. Stejně jako všechny domény sdílet jedno schéma služby Active Directory může mít v jedné doménové struktuře víc domén však můžete zadáním další obory názvů v tomto poli simulated stejné. Každý obor názvů můžete importovat z různých serverů a je nakonfigurované na stránce konfigurace oddílů a hierarchií. Ctrl + Enter použijte k získání nového řádku.

### <a name="configure-provisioning-hierarchy"></a>Konfigurace hierarchie zřizování
Tato stránka slouží k mapování komponentu rozlišující název, například organizační jednotku, na typu objektu, který by měl být zřízený, například organizationalUnit.

![Zřizování hierarchie](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Konfigurace hierarchie zřizování, můžete konfigurovat konektor pro automatické vytvoření struktury v případě potřeby. Například pokud je obor názvů dc = contoso, řadič domény com a nový objekt cn = Jan, ou = = Seattle, c = US, řadič domény = contoso, řadič domény = com je zřízený a pak tento konektor můžete vytvořit objekt typu země pro USA a organizationalUnit pro Seattle, pokud těch, které se již nenacházejí v adresáři.

### <a name="configure-partitions-and-hierarchies"></a>Konfigurace oddílů a hierarchií
Na stránce oddílů a hierarchií vyberte všechny obory názvů s objekty, které chcete importovat a exportovat.

![Oddíly](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Pro každý obor názvů je také možné nakonfigurovat nastavení připojení, které by se mělo přepsat hodnoty zadané na obrazovce připojení. Pokud tyto hodnoty jsou ponechána na jejich výchozí prázdné hodnoty, použije se informace na obrazovce připojení.

Je také možné vybrat, které kontejnery a organizační jednotky by měl import a export na konektoru.

Při prohledávání probíhá přes všechny kontejnery v oddílu. V případech, kde je velké množství kontejnerů toto chování způsobí snížení výkonu.

>[!NOTE]
Počínaje března 2017 aktualizace obecné LDAP konektor hledání omezené v oboru pouze vybrané kontejnery. Tento krok můžete provést zaškrtnutím políčka 'vyhledávání pouze ve vybrané kontejnery, jak je znázorněno na obrázku níže.

![Vyhledávat pouze vybrané kontejnerů](./media/active-directory-aadconnectsync-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Konfigurace ukotvení
Tato stránka nemá vždy hodnotu předkonfigurované a nedá se změnit. Pokud byla zjištěna dodavatele serveru, může se nedá změnit atribut, například identifikátor GUID pro objekt naplněno ukotvení. Pokud nebyl nalezen nebo je zřejmé, nemá atribut neměnné, pak tento konektor využívá jako kotvu rozlišující název (rozlišující název).

![ukotvení](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)


Následuje seznam serverů LDAP a ukotvení používá:

| Adresář | Atribut kotvy |
| --- | --- |
| Uvolňování paměti Microsoft AD LDS a AD |objectGUID |
| 389 adresářový Server |rozlišující název |
| Apache adresáře |rozlišující název |
| IBM Tivoli DS |rozlišující název |
| Isode adresáře |rozlišující název |
| Novell/NetIQ eDirectory |IDENTIFIKÁTOR GUID |
| Otevřete DJ/DS |rozlišující název |
| Otevřete LDAP |rozlišující název |
| Oracle ODSEE |rozlišující název |
| RadiantOne VDS |rozlišující název |
| Jeden Sun Directory Server |rozlišující název |

## <a name="other-notes"></a>Další poznámky
Tato část obsahuje informace o aspektů, které jsou specifické pro tento konektor nebo z jiných důvodů jsou důležité vědět.

### <a name="delta-import"></a>Rozdílový import
Vodoznak rozdílů v otevřené LDAP je datum a čas UTC. Z tohoto důvodu musí být synchronizovány hodiny mezi synchronizační služba FIM a otevřete LDAP. Pokud ne, může tento parametr vynechán některé položky v protokolu rozdílové změny.

Pro Novell eDirectory není rozdílový import zjišťování žádné odstranění objektu. Z tohoto důvodu je nutné spustit úplný import pravidelně chcete najít všechny odstraněné objekty.

Pro adresáře s protokol změn rozdílů, který je založen na datum a čas důrazně doporučujeme spustit úplný import v pravidelných časech. Tento proces umožňuje synchronizační modul najít a rozdíly mezi serverem LDAP a co je aktuálně v prostoru konektoru.

## <a name="troubleshooting"></a>Řešení potíží
* Informace o tom, jak povolit protokolování pro řešení potíží s konektoru najdete v tématu [postup povolení trasování ETW pro konektory](http://go.microsoft.com/fwlink/?LinkId=335731).
