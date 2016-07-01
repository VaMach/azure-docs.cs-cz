<properties
   pageTitle="Generování sestav Azure Active Directory: Začínáme | Microsoft Azure"
   description="Obsahuje seznam různých dostupných sestav generovaných v Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# Začínáme s generováním sestav v Azure Active Directory

## Co to je

Azure Active Directory (Azure AD) obsahuje různé sestavy zabezpečení, aktivit a auditu pro váš adresář. Zde je seznam zahrnutých sestav:

### Sestavy zabezpečení

- Přihlášení z neznámých zdrojů
- Přihlášení po několika neúspěších
- Přihlášení z více geografických poloh
- Přihlášení z IP adres s podezřelou aktivitou
- Nestandardní přihlašovací aktivita
- Přihlášení z možných nakažených zařízení
- Uživatelé s neobvyklou přihlašovací aktivitou

### Sestavy aktivit

- Využití aplikací: souhrn
- Využití aplikací: podrobnosti
- Řídicí panel aplikací
- Chyby zřizování účtů
- Zařízení jednotlivých uživatelů
- Aktivity jednotlivých uživatelů
- Sestava aktivit skupin
- Sestava aktivit registrace resetování hesla
- Aktivity resetování hesla

### Sestavy auditu

- Sestava auditu adresáře

> [AZURE.TIP] Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).



## Jak to funguje


### Kanál generování sestav

Kanál generování sestav zahrnuje tři hlavní kroky. Pokaždé, když se uživatel přihlásí nebo je provedeno ověření, stane se toto:

- Nejprve je uživatel ověřen (úspěšně nebo neúspěšně) a výsledek je uložen do databází služby Azure Active Directory.
- V pravidelných intervalech jsou zpracovávána všechna poslední přihlášení. V tom okamžiku naše algoritmy zabezpečení a neobvyklých aktivit vyhledávají a zkouší identifikovat podezřelé aktivity ve všech posledních přihlášeních.
- Po zpracování jsou vytvořeny sestavy, které jsou uloženy a poskytovány na portálu Azure Classic.

### Časy generování sestav

Vzhledem k velkému objemu ověření a přihlášení zpracovávaných platformou Azure AD jsou nejnověji zpracovaná přihlášení v průměru hodinu stará. Ve výjimečných případech může zpracování nejnovějších přihlášení trvat až 8 hodin.

Nejnovější zpracované přihlášení zjistíte v pomocném textu v horní části každé sestavy.

![Pomocný text v horní části každé sestavy](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).



## Začínáme


### Přihlášení k portálu Azure Classic

Nejprve se budete muset přihlásit k [portálu Azure Classic](https://manage.windowsazure.com) jako globální správce nebo správce dodržování předpisů. Musíte také být správce či spolusprávce služby předplatného Azure nebo používat předplatné Azure „Přístup k Azure AD“ .

### Přechod na Sestavy

Chcete-li zobrazit sestavy, přejděte na kartu Sestavy v horní části adresáře.

Pokud si sestavy zobrazujete poprvé, musíte před jejich zobrazením potvrdit souhlas v dialogovém okně. Je to z toho důvodu, abyste potvrdili, že je ve vaší organizaci přijatelné, aby správci zobrazovali tato data. Ta mohou být v některých zemích považována za osobní údaje.

![Dialogové okno](./media/active-directory-reporting-getting-started/dialogBox.png)

### Zkoumání jednotlivých sestav

Přejděte na sestavu, která vás zajímá, a uvidíte shromážděná data a zpracovaná přihlášení. Zde můžete najít [seznam všech sestav](active-directory-reporting-guide.md).

![Všechny sestavy](./media/active-directory-reporting-getting-started/reportsMain.png)

### Stažení sestavy ve formátu souboru CSV

Každou sestavu lze stáhnout jako soubor CSV (hodnoty oddělené čárkami). Tyto soubory můžete použít v aplikacích Excel, PowerBI nebo analytických programech třetích stran k další analýze dat.

Pokud chcete některou sestavu stáhnout jako soubor CSV, přejděte na sestavu a klikněte na tlačítko „Stáhnout“ v dolní části.

![Tlačítko Stáhnout](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).





## Další kroky

### Přizpůsobení upozornění na neobvyklé přihlašovací aktivity

Přejděte na kartu „Konfigurace“ adresáře.

Přejděte do části „Oznámení“.

Povolte nebo zakažte sekci „E-mailová oznámení neobvyklých přihlášení“.

![Sekce Oznámení](./media/active-directory-reporting-getting-started/notificationsSection.png)

### Integrace s rozhraním API pro generování sestav Azure AD

Viz [Začínáme s rozhraním API pro generování sestav](active-directory-reporting-api-getting-started.md).

### Povolení služby Multi-Factor Authentication pro uživatele

Vyberte uživatele v sestavě.

Klikněte na tlačítko „Povolit MFA“ v dolní části obrazovky.

![Tlačítko služby Multi-Factor Authentication v dolní části obrazovky](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).




## Další informace


### Auditování událostí

Informace o událostech, které jsou v adresáři auditovány, získáte v článku o [auditovaných událostech pro generování sestav Azure Active Directory](active-directory-reporting-audit-events.md).

### Integrace rozhraní API

Viz [Začínáme s rozhraním API pro generování sestav](active-directory-reporting-api-getting-started.md) a [referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### Kontakt

Pro zpětnou vazbu, pomoc a případné dotazy slouží e-mailová adresa [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com).

> [AZURE.TIP] Další dokumentaci k sestavám Azure AD najdete v článku o [zobrazení sestav přístupu a využití](active-directory-view-access-usage-reports.md).



<!--HONumber=Jun16_HO2-->


