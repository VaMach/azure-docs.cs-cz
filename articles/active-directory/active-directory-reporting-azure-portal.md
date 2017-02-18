---
title: "Generování sestav v Azure Active Directory – verze Preview | Dokumentace Microsoftu"
description: "Obsahuje seznam různých dostupných sestav pro Azure Active Directory ve verzi Preview."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/19/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: be986fd7bb1745dcf43a1066dfabc1e1c699ab4c
ms.openlocfilehash: b9cd11954a52600c1cd50155cb7ce9b7d2355cd3


---
# <a name="azure-active-directory-reporting---preview"></a>Generování sestav v Azure Active Directory – verze Preview


*Tato dokumentace je součástí [Příručky generování sestav v Azure Active Directory](active-directory-reporting-guide.md).*

Díky možnosti vytváření sestav v Azure Active Directory ve verzi Preview získáte všechny informace, které potřebujete při určování stavu prostředí. [Co je součástí verze Preview?](active-directory-preview-explainer.md)

Existují dvě hlavní oblasti vytváření sestav:

* **Aktivity přihlašování** – informace o použití spravovaných aplikací a aktivitách přihlašování uživatelů
* **Protokoly auditu** – informace aktivit systému o správě uživatelů a skupin, spravovaných aplikacích a aktivitách adresářů

V závislosti na rozsahu hledaných dat můžete k těmto sestavám přistupovat po kliknutí buď na možnost **Uživatelé a skupiny**, nebo na možnost **Podnikové aplikace** v seznamu služeb na webu [Azure Portal](https://portal.azure.com).

## <a name="sign-in-activities"></a>Aktivity přihlašování
### <a name="user-sign-in-activities"></a>Aktivity přihlašování uživatelů
Na základě informací poskytnutých sestavou přihlašování uživatelů najdete odpovědi na otázky tohoto typu:

* Jaký je vzorec přihlašování uživatele?
* Kolik uživatelů se přihlásilo za týden?
* Jaký je stav těchto přihlášení?

Vaším vstupním bodem pro tato data je graf přihlašování uživatelů v oddílu **Přehled** v části **Uživatelé a skupiny**.

 ![Vytváření sestav](./media/active-directory-reporting-azure-portal/05.png "Vytváření sestav")

V grafu přihlašování uživatelů jsou znázorněny týdenní agregace přihlášení všech uživatelů za dané časové období. Výchozí časové období je 30 dnů.

![Vytváření sestav](./media/active-directory-reporting-azure-portal/02.png "Vytváření sestav")

Když v grafu přihlašování kliknete na konkrétní den, zobrazí se podrobný seznam aktivit přihlašování.

![Vytváření sestav](./media/active-directory-reporting-azure-portal/03.png "Vytváření sestav")

Každý řádek v seznamu aktivit přihlašování obsahuje podrobné informace o vybraném přihlášení. Příklad:

* Kdo se přihlásil?
* Jaký byl související hlavní název uživatele (UPN)?
* Která aplikace byla cílem přihlášení?
* Jaká je IP adresa přihlášení?
* Jaký byl stav přihlášení?

### <a name="usage-of-managed-applications"></a>Použití spravovaných aplikací
S použitím zobrazení dat přihlašování zaměřeného na aplikace můžete odpovídat na otázky tohoto typu:

* Kdo používá mé aplikace?
* Které jsou 3 nejpoužívanější aplikace v organizaci?
* Nedávno jsem zpřístupnil aplikaci. Jak to s ní vypadá?

Vaším vstupním bodem k těmto datům jsou 3 nejpoužívanější aplikace v organizaci v rámci sestavy za posledních 30 dnů v oddílu **Přehled** v části **Podnikové aplikace**.

 ![Vytváření sestav](./media/active-directory-reporting-azure-portal/06.png "Vytváření sestav")

Graf využívání aplikací s týdenními agregacemi přihlašování pro 3 nejpoužívanější aplikace v daném časovém období. Výchozí časové období je 30 dnů.

![Vytváření sestav](./media/active-directory-reporting-azure-portal/78.png "Vytváření sestav")

Pokud chcete, můžete se zaměřit na konkrétní aplikaci.

![Vytváření sestav](./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Vytváření sestav")

Když v grafu využívání aplikací kliknete na konkrétní den, zobrazí se podrobný seznam aktivit přihlašování.

![Vytváření sestav](./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Vytváření sestav")

Možnost **Přihlášení** poskytuje úplný přehled o všech událostech přihlašování pro vaše aplikace.

![Vytváření sestav](./media/active-directory-reporting-azure-portal/85.png "Vytváření sestav")

Pomocí voliče sloupců můžete vybrat datová pole, která chcete zobrazit.

![Vytváření sestav](./media/active-directory-reporting-azure-portal/column_chooser.png "Vytváření sestav")

### <a name="filtering-sign-ins"></a>Filtrování přihlášení
Přihlášení můžete filtrovat pomocí následujících polí a omezit tak množství zobrazených dat:

* Datum a čas 
* Název objektu zabezpečení User
* Název aplikace
* Název klienta
* Stav přihlášení

![Vytváření sestav](./media/active-directory-reporting-azure-portal/293.png "Vytváření sestav")

Jinou metodou filtrování záznamů aktivit přihlašování je vyhledání konkrétních položek.
Metoda hledání umožňuje omezit obor přihlášení na konkrétní **uživatele**, **skupiny** nebo **aplikace**.

![Vytváření sestav](./media/active-directory-reporting-azure-portal/84.png "Vytváření sestav")

## <a name="audit-logs"></a>Protokoly auditu
Protokoly auditování v Azure Active Directory obsahují záznamy aktivit systému kvůli dodržování předpisů.

Na webu Azure Portal jsou k dispozici tři hlavní kategorie aktivit souvisejících s auditováním:

* Uživatelé a skupiny   
* Aplikace
* Adresář   

Úplný seznam aktivit sestavy auditování najdete v [seznamu událostí sestavy auditu](active-directory-reporting-audit-events.md#list-of-audit-report-events).

Vaším vstupním bodem k veškerým datům auditování je možnost **Protokoly auditu** v oddílu **Aktivita** v **Azure Active Directory**.

![Auditování](./media/active-directory-reporting-azure-portal/61.png "Auditování")

Protokolu auditu nabízí zobrazení seznamu s účastníky (kdo), aktivitami (co) a cíli.

![Auditování](./media/active-directory-reporting-azure-portal/345.png "Auditování")

Kliknutím na položku v zobrazení seznamu zobrazíte další podrobnosti.

![Auditování](./media/active-directory-reporting-azure-portal/873.png "Auditování")

### <a name="users-and-groups-audit-logs"></a>Protokoly auditu uživatelů a skupin
S použitím sestav auditu orientovaných na uživatele a skupiny můžete najít odpovědi na otázky tohoto typu:

* Jaké typy aktualizací uživatelé použili?
* Kolik uživatelů bylo změněno?
* Kolik hesel bylo změněno?
* Co provedl správce v adresáři?
* Které skupiny byly přidány?
* Došlo u některých skupin ke změnám členství?
* Došlo ke změnám vlastníků skupiny?
* Jaké licence byly přiřazeny skupině nebo uživateli?

Pokud chcete jen zkontrolovat data auditování týkající se uživatelů a skupin, najdete filtrované zobrazení v sekci **Protokoly auditu** v oddílu **Aktivity** v části **Uživatelé a skupiny**.

![Auditování](./media/active-directory-reporting-azure-portal/93.png "Auditování")

### <a name="application-audit-logs"></a>Protokoly auditu aplikací
S použitím sestav auditu orientovaných na aplikace můžete najít odpovědi na otázky tohoto typu:

* Které aplikace byly přidány nebo aktualizovány?
* Které aplikace byly odebrány?
* Změnil se instanční objekt pro aplikaci?
* Změnily se názvy aplikací?
* Kdo udělil souhlas pro aplikaci?

Pokud chcete jen zkontrolovat data auditování týkající se aplikací, najdete filtrované zobrazení v sekci **Protokoly auditu** v oddílu **Aktivity** v části **Podnikové aplikace**.

![Auditování](./media/active-directory-reporting-azure-portal/134.png "Auditování")

### <a name="filtering-audit-logs"></a>Filtrování protokolů auditu
Přihlášení můžete filtrovat pomocí následujících polí a omezit tak množství zobrazených dat:

* Datum a čas
* Název objektu zabezpečení Actor
* Typ aktivity
* Aktivita

![Auditování](./media/active-directory-reporting-azure-portal/356.png "Auditování")

Obsah seznamu **Typ aktivity** závisí na tom, kudy do tohoto okna vstoupíte.  
Pokud je vstupním bodem služba Azure Active Directory, bude tento seznam obsahovat všechny možné typy aktivit:

* Aplikace 
* Skupina 
* Uživatel
* Zařízení
* Adresář
* Zásada
* Ostatní

![Auditování](./media/active-directory-reporting-azure-portal/825.png "Auditování")

Obor aktivit uvedených v seznamu je nastaven podle typu aktivity.
Pokud jste například jako **Typ aktivity** vybrali **Skupina**, bude seznam **Aktivita** obsahovat pouze aktivit související se skupinami.   

![Auditování](./media/active-directory-reporting-azure-portal/654.png "Auditování")

Jinou metodou filtrování záznamů protokolu auditu je vyhledání konkrétních položek.

![Auditování](./media/active-directory-reporting-azure-portal/237.png "Auditování")

## <a name="next-steps"></a>Další kroky
Přečtěte si článek [Příručka generování sestav v Azure Active Directory](active-directory-reporting-guide.md).




<!--HONumber=Jan17_HO3-->


