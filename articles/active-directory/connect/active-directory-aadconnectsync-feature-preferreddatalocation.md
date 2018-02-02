---
title: "Synchronizace Azure AD Connect: konfigurujte upřednostňovaný data umístění pro geograficky více možností v Office 365 | Microsoft Docs"
description: "Popisuje, jak uvést vaše prostředky uživatele služeb Office 365 blízko uživatele s synchronizace Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: billmath
ms.openlocfilehash: 8a36fc45334a2f1d12e6eabbfb16731ccc9998bf
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="azure-ad-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Synchronizace Azure AD Connect: konfigurujte upřednostňovaný data umístění pro prostředky Office 365
Účelem tohoto tématu je pro vás provede procesem konfigurace PreferredDataLocation v Azure AD Connect Sync. Pokud zákazník používá více geograficky možnosti v Office 365, tento atribut slouží k určení geografického umístění dat uživatele v Office 365.

> [!IMPORTANT]
> Více geograficky je aktuálně ve verzi preview. Pokud chcete zapojit do programu preview, kontaktujte prosím zástupce společnosti Microsoft.
>
>

## <a name="enable-synchronization-of-preferreddatalocation"></a>Povolit synchronizaci PreferredDataLocation
Ve výchozím nastavení jsou Office 365 prostředky pro vaše uživatele nachází ve stejné oblasti jako váš klient Azure AD. Například pokud se váš klient nachází v Severní Americe pak poštovní schránky Exchange uživatele jsou taky umístěné v Severní Americe. Pro více national organizace nemusí být optimální. Nastavením preferredDataLocation atributu může být definováno oblast uživatele.

Nastavením tohoto atributu můžete mít prostředky Office 365 uživatele, například poštovní schránky a OneDrive, ve stejné oblasti jako uživatel a stále máte jednoho klienta pro celou organizaci.

> [!IMPORTANT]
> Způsobilé k více geograficky, musí mít alespoň 5000 licencí v rámci vašeho předplatného Office 365
>
>

Oblasti v Office 365, které jsou k dispozici pro více geograficky jsou:

| Oblast | Popis |
| --- | --- |
| OSOBY | Severní Amerika |
| EUR | Evropa |
| APC | Asie a Tichomoří |
| JPN | Japonsko |
| AUSTRÁLIE | Austrálie |
| CAN | Kanada |
| GBR | Velká Británie |
| LÁM | Latinská Amerika |

Ne všechny úlohami Office 365 podporuje použití nastavení oblasti uživatele.

Azure AD Connect podporuje synchronizaci **PreferredDataLocation** atribut pro **uživatele** objekty ve verzi 1.1.524.0 a po. Přesněji řečeno byly zavedeny následující změny:

* Schéma typu objektu **uživatele** v konektor služby Azure AD je rozšířen o PreferredDataLocation atribut, který je typu řetězec jednou hodnotou.
* Schéma typu objektu **osoba** v úložišti Metaverse je rozšířen o PreferredDataLocation atribut, který je typu řetězec a je jedinou hodnotu.

Ve výchozím nastavení není povoleno atribut PreferredDataLocation pro synchronizaci. Tato funkce je určena pro velké organizace a každý by těžit z něj. Musíte také určit atribut pro uložení oblasti Office 365 pro vaše uživatele vzhledem k tomu, že neexistuje žádný atribut PreferredDataLocation v místní službě Active Directory. To se bude lišit pro každou organizaci.

> [!IMPORTANT]
> V současné době Azure AD umožňuje že atribut PreferredDataLocation na synchronizované objekty uživatele a cloud uživatele objekty být přímo konfigurovat pomocí Azure AD PowerShell. Jakmile povolíte synchronizaci PreferredDataLocation atributu, je nutné zastavit pomocí Azure AD PowerShell ke konfiguraci atribut na **synchronizované uživatelské objekty** jako Azure AD Connect se přepíše je na základě Zdroj hodnot atributů v místní službě Active Directory.

> [!IMPORTANT]
> Od 1. září 2017, Azure AD už umožňuje atribut PreferredDataLocation na **synchronizované uživatelské objekty** přímo nakonfigurovat pomocí Azure AD PowerShell. Ke konfiguraci PreferredLocation atribut na synchronizované objekty uživatele, musíte použít Azure AD Connect.

Než povolíte synchronizaci atribut PreferredDataLocation, musíte:

* Nejdřív se rozhodněte, které místní služby Active Directory atribut má být použit jako zdrojový atribut. Musí být typu **jednohodnotové řetězec**. V těchto kroků mezi extensionAttributes se používá.
* Pokud jste dříve nakonfigurovali atribut PreferredDataLocation na existující synchronizované uživatelské objekty ve službě Azure AD pomocí Azure AD PowerShell, je nutné **backport** hodnoty atributů, které mají odpovídající uživatelské objekty v místní služby Active Directory.

    > [!IMPORTANT]
    > V takovém případě není backport hodnoty atributů, které mají odpovídající uživatelské objekty ve službě Active Directory v místě Azure AD Connect Odebere existující hodnoty atributu ve službě Azure AD, pokud je povolená synchronizace pro atribut PreferredDataLocation.

* Je doporučeno, nakonfigurujete zdrojový atribut na alespoň několik místní uživatele AD objekty nyní, který může být použit pro ověření později.

Postup povolení synchronizace atributu PreferredDataLocation jde vyhodnotit jako:

1. Zakažte plánovače synchronizace a ověřte, že neexistuje žádná synchronizace v průběhu
2. Přidejte zdrojový atribut do schématu místní přidá konektoru
3. Přidejte PreferredDataLocation do schématu konektor služby Azure AD
4. Vytvoření pravidla synchronizace příchozích dat tok hodnota atributu z místní služby Active Directory
5. Vytvořit pravidlo odchozí synchronizace pro tok hodnota atributu do služby Azure AD
6. Spustit úplnou synchronizaci cyklus
7. Povolit synchronizaci plánovače
8. Ověřte výsledek

> [!NOTE]
> Zbývající část tohoto oddílu popisuje tyto kroky v podrobnostech. Jsou popsány v rámci nasazení služby Azure AD s jednou doménovou strukturou topologií a bez vlastní synchronizační pravidla. Pokud máte topologie s více doménovými strukturami, vlastní synchronizační pravidla nakonfigurován nebo mít na testovacím serveru, bude nutné upravit kroky odpovídajícím způsobem.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Krok 1: Zakázat plánovače synchronizace a ověřte, že neexistuje žádná synchronizace v průběhu
Zajistěte, aby že žádná synchronizace bude probíhat, když jste právě aktualizují se pravidla synchronizace, aby se zabránilo neúmyslnému změny se exportují do služby Azure AD. Postup při zakázání plánovače předdefinované synchronizace:

1. Spusťte relaci prostředí PowerShell na server Azure AD Connect.
2. Zakázat plánované synchronizace spuštěním rutiny: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Spuštění **Synchronization Service Manager** přechodem na **spustit** > **synchronizační služba**.
4. Přejděte na **operace** kartě a potvrďte neprobíhá žádná operace se stavem *v průběhu*.

![Zkontrolujte Synchronization Service Manager - žádné operace v průběhu](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema"></a>Krok 2: Přidání zdrojového atributu schématu místní přidá konektoru
Všechny atributy AD importují do místní AD prostoru konektoru. Pokud jste vybrali do atribut nejsou synchronizovány ve výchozím nastavení použít, budete muset importovat. Zdrojový atribut přidat do seznamu importované atributy:

1. Přejděte na **konektory** kartě Synchronization Service Manager.
2. Klikněte pravým tlačítkem myši **místní AD Connector** a vyberte **vlastnosti**.
3. V dialogovém okně automaticky otevírané okno, přejděte na **vybrat atributy** kartě.
4. Zkontrolujte, že je zaškrtnuté zdrojový atribut, který Pokud chcete použít v seznamu atributů. Pokud se vaše atribut nezobrazí, klikněte na tlačítko "Zobrazit vše" zaškrtávací políčko.
5. Klikněte na tlačítko **OK** uložit.

![Přidejte zdrojový atribut místně schéma AD Connector.](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Krok 3: Přidání PreferredDataLocation schématu konektor služby Azure AD
Ve výchozím nastavení není atribut PreferredDataLocation importován do prostoru konektoru služby Azure AD. Přidat atribut PreferredDataLocation do seznamu importované atributy:

1. Přejděte na **konektory** kartě Synchronization Service Manager.
2. Klikněte pravým tlačítkem myši **konektoru Azure AD** a vyberte **vlastnosti**.
3. V dialogovém okně automaticky otevírané okno, přejděte na **vybrat atributy** kartě.
4. Vyberte atribut preferredDataLocation v seznamu atributů.
5. Klikněte na tlačítko **OK** uložit.

![Přidejte zdrojový atribut do schématu Azure AD Connector.](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Krok 4: Vytvoření pravidla synchronizace příchozích dat tok hodnota atributu z místní služby Active Directory
Pravidla synchronizace příchozích dat umožňuje hodnota atributu, které jsou předávány z atributu zdroje z místní služby Active Directory do úložiště Metaverse:

1. Spuštění **editoru pravidel synchronizace** přechodem na **spustit** > **editoru pravidel synchronizace**.
2. Nastavit filtr hledání **směr** být **příchozí**.
3. Klikněte na tlačítko **přidat nové pravidlo** tlačítko pro vytvoření nového příchozího pravidla.
4. V části **popis** kartě, zadejte následující konfiguraci:

    | Atribut | Hodnota | Podrobnosti |
    | --- | --- | --- |
    | Název | *Zadejte název* | Například *"v ze služby Active Directory – uživatel PreferredDataLocation"* |
    | Popis | *Zadejte vlastní popis* |  |
    | Připojený systém | *Vyberte místní AD connector.* |  |
    | Typ objektu systému připojené | **Uživatel** |  |
    | Typ objektu úložiště Metaverse | **Osoba** |  |
    | Typ propojení | **Spojení** |  |
    | Priorita | *Vyberte číslo mezi 1 – 99* | 1 – 99 je vyhrazený pro vlastní synchronizačního pravidla. Není vyberte hodnotu, která je používána jinou synchronizační pravidlo. |

5. Zachovat **Scoping filtru** prázdný zahrnovalo všechny objekty. Budete muset upravit oboru filtru, podle vašeho nasazení Azure AD Connect.
6. Přejděte na **transformace karta** a implementovat následující pravidla transformace:

    | Typ toku | Cílový atribut | Zdroj | Použít jednou | Merge – typ |
    | --- | --- | --- | --- | --- |
    |Přímý | PreferredDataLocation | Vyberte zdrojový atribut | Nezaškrtnuto | Aktualizace |

7. Klikněte na tlačítko **přidat** k vytvoření příchozího pravidla.

![Vytvoření pravidla synchronizace příchozích dat](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Krok 5: Vytvoření pravidlo odchozí synchronizace pro tok hodnota atributu do služby Azure AD
Pravidlo odchozí synchronizace umožňuje hodnota atributu, které jsou předávány z úložiště Metaverse do atribut PreferredDataLocation ve službě Azure AD:

1. Přejděte na **synchronizační pravidla** Editor.
2. Nastavit filtr hledání **směr** být **odchozí**.
3. Klikněte na tlačítko **přidat nové pravidlo** tlačítko.
4. V části **popis** kartě, zadejte následující konfiguraci:

    | Atribut | Hodnota | Podrobnosti |
    | ----- | ------ | --- |
    | Název | *Zadejte název* | Například "se do AAD – uživatel PreferredDataLocation" |
    | Popis | *Zadejte popis* ||
    | Připojený systém | *Vyberte konektor AAD* ||
    | Typ objektu systému připojené | Uživatel ||
    | Typ objektu úložiště Metaverse | **Osoba** ||
    | Typ propojení | **Spojení** ||
    | Priorita | *Vyberte číslo mezi 1 – 99* | 1 – 99 je vyhrazený pro vlastní synchronizačního pravidla. Není vyberte hodnotu, která je používána jinou synchronizační pravidlo. |

5. Přejděte na **Scoping filtru** kartě a přidejte **jednu skupinu oboru filtru se dvěma klauzule**:

    | Atribut | Operátor | Hodnota |
    | --- | --- | --- |
    | sourceObjectType | ROVNÁ | Uživatel |
    | cloudMastered | NOTEQUAL | True |

    Určuje oboru filtru, které objekty Azure AD, že je toto pravidlo odchozí synchronizace použito pro. V tomto příkladu používáme stejné oboru filtru z "Na více systémů do AD – identitu uživatele" OOB synchronizační pravidlo. Pravidlo synchronizace zabrání bylo použito pro uživatelské objekty, které nejsou synchronizovány z místní služby Active Directory. Budete muset upravit oboru filtru, podle vašeho nasazení Azure AD Connect.

6. Přejděte na **transformace** kartě a implementovat následující pravidla transformace:

    | Typ toku | Cílový atribut | Zdroj | Použít jednou | Merge – typ |
    | --- | --- | --- | --- | --- |
    | Přímý | PreferredDataLocation | PreferredDataLocation | Nezaškrtnuto | Aktualizace |

7. Zavřít **přidat** vytvoření odchozí pravidla.

![Vytvořit pravidlo odchozí synchronizace](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>Krok 6: Spusťte úplnou synchronizaci cyklu
Obecně je úplná synchronizace cyklus požadované vzhledem k tomu, že jsme přidali nové atributy pro obě služby AD a Azure AD Connector schématu a přináší vlastní synchronizační pravidla. Doporučujeme ověřit změny před exportováním je do Azure AD. Chcete-li ověřit změny při spuštění ručně kroky, které tvoří cyklus úplné synchronizace můžete použít následující kroky.

1. Spustit **úplný import** krok na **místní AD Connector**:

   1. Přejděte na **Operations** kartě Synchronization Service Manager.
   2. Klikněte pravým tlačítkem myši **místní AD Connector** a vyberte **spustit...** .
   3. V dialogovém okně automaticky otevírané okno Vyberte **úplný Import** a klikněte na tlačítko **OK**.
   4. Počkejte na dokončení operace.

    > [!NOTE]
    > Úplný Import, můžete přeskočit na místní AD Connector. Pokud zdrojový atribut je již zahrnut v seznamu importu atributy. Jinými slovy, můžete neměl k provedení jakékoli změny během [krok 2: přidání atributu zdroje do místní AD Connector. schéma](#step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema).

2. Spustit **úplný import** krok na **konektoru služby Azure AD**:

   1. Klikněte pravým tlačítkem myši **konektoru služby Azure AD** a vyberte **spustit...**
   2. V dialogovém okně automaticky otevírané okno Vyberte **úplný Import** a klikněte na tlačítko **OK**.
   3. Počkejte na dokončení operace.

3. Ověření změny pravidel synchronizace na existující objekt uživatele:

Zdrojový atribut z místního služby Active Directory a PreferredDataLocation z Azure AD byly naimportovány do prostoru příslušného konektoru. Než budete pokračovat v kroku úplnou synchronizaci, je doporučeno, abyste provedli **Preview** na stávajícího uživatele objekt v místní AD prostoru konektoru. Objekt, který jste vybrali měli zdrojový atribut naplněno. Úspěšně **Preview** s PreferredDataLocation vložené do úložiště Metaverse je dobré indikátoru, které jste nakonfigurovali synchronizaci pravidla správně. Informace o tom, jak provést **Preview**, informace naleznete v sekci [změnu ověřit](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Spustit **úplná synchronizace** krok na **místní AD Connector**:

   1. Klikněte pravým tlačítkem myši **místní AD Connector** a vyberte **spustit...** .
   2. V dialogovém okně automaticky otevírané okno Vyberte **úplná synchronizace** a klikněte na tlačítko **OK**.
   3. Počkejte na dokončení operace.

5. Ověřte **čekajících exportů** do služby Azure AD:

   1. Klikněte pravým tlačítkem myši **konektoru služby Azure AD** a vyberte **hledání prostoru konektoru**.
   2. V dialogovém okně hledání prostoru konektoru automaticky otevírané okno:

      1. Nastavit **oboru** k **čekající na vyřízení Export**.
      2. Zkontrolujte všechny tři políčka, včetně **přidat, upravit a odstranit**.
      3. Klikněte **vyhledávání** tlačítko získat seznam objektů se změnami pro export. Chcete-li zkontrolovat změny pro daný objekt, dvakrát klikněte na objekt.
      4. Ověřte, že se očekává změny.

6. Spustit **exportovat** krok na **Azure AD Connector.**

   1. Klikněte pravým tlačítkem myši **konektoru služby Azure AD** a vyberte **spustit...** .
   2. V dialogovém okně Spustit konektor automaticky otevírané okno, vyberte **exportovat** a klikněte na tlačítko **OK**.
   3. Počkejte, než pro Export do Azure AD pro dokončení.

> [!NOTE]
> Můžete si všimnout, že kroky nezahrnují kroku úplná synchronizace v konektoru služby Azure AD a Export na konektoru služby AD. Vzhledem k tomu, že jsou hodnoty atributu odesílaných z místní služby Active Directory do Azure AD pouze, nejsou nutné kroky.

## <a name="step-7-re-enable-sync-scheduler"></a>Krok 7: Opětovné povolení plánovače synchronizace
Znovu povolte plánovače předdefinované synchronizace:

1. Spusťte relaci prostředí PowerShell.
2. Plánované synchronizace znovu povolte spuštěním rutiny:`Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>Krok 8: Ověřit výsledek
Nyní je čas ověřte konfiguraci a povolení pro vaše uživatele.

1. Oblast přidáte do vybraného atributu na uživatele. Seznam dostupných oblastí najdete v [tuto tabulku](#enable-synchronization-of-preferreddatalocation).  
![Atribut AD přidat na uživatele](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Počkejte, než pro atribut synchronizovány se službou Azure AD.
3. Pomocí Exchange Online Powershellu ověřte správně nastavené oblasti poštovní schránky.  
![Poštovní schránky oblast nastavit na uživatele v systému Exchange Online](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Za předpokladu, že váš klient je označená mohli tuto funkci používat, je poštovní schránka přesunuta do oblasti správné. To můžete ověřit pohledem na název serveru, kde se nachází poštovní schránku.
4. Pokud chcete ověřit, že toto nastavení bylo efektivní přes kolik poštovních schránek, použijte skript v [galerii Technet](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Tento skript také obsahuje seznam všech předpon serveru datových centrech Office 365 a který je umístěný v oblasti. Můžete použít jako referenci v předchozím kroku ověření umístění poštovní schránku.

## <a name="next-steps"></a>Další postup

**Další informace o více geograficky v Office 365:**

* Geograficky více relací na Ignite: https://aka.ms/MultiGeoIgnite
* Více geograficky ve Onedrivu: https://aka.ms/OneDriveMultiGeo
* Více geograficky ve službě SharePoint Online: https://aka.ms/SharePointMultiGeo

**Další informace o konfiguraci modelu v synchronizační modul:**

* Další informace o konfiguraci modelu v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Další informace o jazyk výrazů v [Principy deklarativní zřizování výrazy](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
