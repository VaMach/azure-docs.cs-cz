---
title: "Přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu v Azure Active Directory"
description: "Postup přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu v Azure"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f74621b4-4ef0-4899-8c0e-0c20347a8c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/07/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 6d4fbfe97288fcb76628b45649b8b678152198a9
ms.lasthandoff: 04/07/2017


---
# <a name="add-company-branding-to-sign-in-and-access-panel-pages"></a>Přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu
Mnoho společností upřednostňuje jednotný vzhled a chování všech webů a služeb, které spravují. Azure Active Directory tuto možnost nabízí a umožňuje odborníkům na IT pomocí firemních log a obrázků přizpůsobit vzhled následujících webových stránek:

* **Přihlašovací stránka:** To je stránka, která se zobrazí, když se zaměstnanci nebo hosté přihlásí k Office 365 nebo k jiným aplikacím, které používají Azure AD.
* **Stránka přístupového panelu:** Přístupový panel je webový portál, který vám umožňuje zobrazovat a spouštět cloudové aplikace, ke kterým vám správce Azure AD udělil přístup. Přístupový panel najdete na adrese [https://myapps.microsoft.com](https://myapps.microsoft.com).

Toto téma popisuje možnosti přizpůsobení přihlašovací stránky a stránky přístupového panelu.

> [!NOTE]
> * Firemní branding je dostupný jenom v případě, pokud jste upgradovali na edici Premium nebo Basic služby Azure Active Directory nebo pokud máte licenci Office 365. Další informace najdete v článku Edice služby Azure Active Directory.
> 
> * Edice Premium a Basic služby Azure Active Directory jsou zákazníkům v Číně dostupné prostřednictvím celosvětové instance služby Azure Active Directory. Edice Premium a Basic služby Azure Active Directory nejsou aktuálně podporované ve službě Microsoft Azure provozované v Číně společností 21Vianet. Další informace si vyžádejte na fóru služby Azure Active Directory.


## <a name="customizing-the-sign-in-page"></a>Přizpůsobení přihlašovací stránky
Uživatelé obvykle na přihlašovací stránku Azure AD narazí při pokusu o přístup ke cloudovým aplikacím a službám, které si vaše organizace předplatila.

Pokud na přihlašovací stránce měníte branding, může trvat až hodinu, než se taková změna projeví koncovým uživatelům.

Prvky firemního brandingu se na přihlašovací stránce služby Azure AD zobrazí, když uživatel přistupuje k adrese URL konkrétního tenanta, třeba https://outlook.com/contoso.com.

Když uživatel navštíví službu na obecné adrese URL, třeba www.office.com, přihlašovací stránka ještě nebude obsahovat firemní branding, protože systém nebude vědět, kdo tento uživatel je. Jakmile ale uživatel zadá svoje ID uživatele nebo vybere dlaždici uživatele, firemní branding se zobrazí.

> [!NOTE]
> * Název domény se musí v sekci portálu Azure Classic **Active Directory** > **Adresář** > **Domény**, kde jste branding nakonfigurovali, zobrazovat jako „Aktivní“.
> * Branding přihlašovací stránky se nepřenáší na přihlašovací stránku pro osobní účty Microsoftu. Pokud se zaměstnanci nebo obchodní hosté přihlásí pomocí osobního účtu Microsoft, na jejich přihlašovací stránce se branding vaší organizace nezobrazí.
>

Na následujících snímcích obrazovky je vysvětlené, jak se přihlašovací stránky přizpůsobují.

### <a name="scenario-1-contoso-employee-goes-to-a-generic-app-url-for-example-wwwofficecom"></a>Scénář 1: Zaměstnanec společnosti Contoso přejde na obecnou adresu URL aplikace (třeba www.office.com)

V tomto příkladu se uživatel Contoso přihlásí k mobilní nebo webové aplikaci pomocí obecné adresy URL. Obrázek nalevo vždycky reprezentuje aplikaci. Podokno interakce napravo se aktualizuje, aby se případně zobrazily prvky značky Contoso.

![Přihlašovací stránka Office 365 před přizpůsobením a po něm][1]

### <a name="scenario-2-contoso-employee-goes-to-contoso-app-thats-restricted-to-internal-users"></a>Scénář 2: Zaměstnanec společnosti Contoso přejde na aplikaci Contoso, která je omezená jenom na interní uživatele

V tomto příkladu se uživatel Contoso přihlašuje k interní aplikaci pomocí konkrétní firemní adresy URL. Ilustrace nalevo představuje firemní značku (Contoso). Podokno interakce na pravé straně je pevně spojené se společností Contoso a pomáhá zaměstnancům při přihlašování.

![Přihlašovací stránka aplikace s omezeným přístupem][2]

### <a name="scenario-3-contoso-employee-goes-to-a-contoso-app-thats-open-to-external-users"></a>Scénář 3: Zaměstnanec společnosti Contoso přejde na aplikaci Contoso, která je otevřená i pro externí uživatele

V tomto příkladu se uživatel přihlašuje k obchodní aplikaci ze společnosti Contoso, přitom ale nemusí být jejím zaměstnancem. Obrázek na levé straně představuje vlastníka prostředku (Contoso), stejně jako ve scénáři \#2. Ale tentokrát podokno interakce na pravé straně není vázané na Contoso, aby se externí uživatele neodrazovalo od přihlášení.

![Přihlášení s otevřeným přístupem][3]

### <a name="scenario-4-fabrikam-business-guest-goes-to-contoso-app-thats-open-to-external-users"></a>Scénář 4: Obchodní host Fabrikam přejde k aplikaci Contoso, která je otevřená pro externí uživatele

V tomto příkladu se uživatel Contoso přihlašuje k interní aplikaci pomocí konkrétní firemní adresy URL. Ilustrace nalevo představuje firemní značku (Contoso). Podokno interakce na pravé straně je pevně spojené se společností Contoso a pomáhá zaměstnancům při přihlašování.

![Přihlášení jako externí uživatel][4]


## <a name="what-elements-on-the-page-can-i-customize"></a>Přizpůsobitelné prvky na stránce

Na přihlašovací stránce může přizpůsobit následující prvky:

![][5]

| Prvek stránky | Umístění na stránce |
|:--- | --- |
| Banner s logem | Zobrazuje se v pravé horní části stránky. Nahrazuje logo aplikace, jakmile se určí organizace uživatele (obvykle po zadání uživatelského jména). |
| Obrázek na pozadí | Zobrazuje se jako celoplošný obrázek na levé straně přihlašovací stránky. Nahrazuje obrázek aplikace v případě přihlašování s použitím tenantů (když uživatelé přistupují k aplikaci, kterou publikovala jejich vlastní organizace nebo organizace, ve které jsou obchodním hostem).<br>U připojení s nízkou šířkou pásma se obrázek na pozadí nahrazuje barvou pozadí. Na úzkých obrazovkách (třeba na telefonech) se tento obrázek nezobrazuje.<br>Když uživatelé změní velikost okna prohlížeče, obrázek na pozadí se ořízne. Při návrhu obrázku proto důležité vizuální prvky umístěte do levého horního rohu, aby nedošlo k jejich oříznutí. | 
| Zaškrtávací pole Zůstat přihlášeni | Zobrazuje se pod polem **Heslo**. |
| Text na přihlašovací stránce | Často používaný text, který se zobrazí nad zápatím stránky. Dá se využít k předání informací užitečných pro vaše uživatele, jako je třeba telefonní číslo technické podpory nebo právní prohlášení. |

> [!NOTE]
> Všechny prvky jsou volitelné. Pokud zadáte banner s logem, ale ne obrázek na pozadí, na přihlašovací stránce se zobrazí vaše logo a obrázek z cílového webu (v tomto případě obrázek kalifornské dálnice z Office 365).
>

Zaškrtávací políčko **Zůstat přihlášeni** na vaší přihlašovací stránce umožňuje, aby příslušný uživatel zůstal přihlášený i po zavření a dalším spuštění prohlížeče, a na životnost relace nemá vliv.

Zobrazení zaškrtávacího políčka závisí na nastavení **Skrýt políčko Zůstat přihlášeni**.

![Nastavení Skrýt políčko Zůstat přihlášeni][6]

Pokud chcete toto zaškrtávací políčko skrýt, použijte pro toto nastavení hodnotu **Skryté**.

> [!NOTE]
> Některé funkce služeb SharePoint Online a Office 2010 závisí na tom, že uživatelé mohou toto políčko zaškrtnout. Pokud ho nastavíte jako skryté, mohou se vašim uživatelům zobrazovat další (neočekávané) výzvy k přihlášení.
>
>

Všechny prvky na této stránce můžete lokalizovat. Po konfiguraci „výchozí“ sady prvků přizpůsobení můžete nakonfigurovat další verze pro různá národní prostředí. Různé prvky mezi sebou můžete kombinovat. Můžete například provést následující věci:

* Vytvořte „výchozí“ obrázek, který je použitelný pro všechny jazykové verze, a potom vytvořte specifické verze pro angličtinu a francouzštinu. Když svoje prohlížeče nastavíte na jeden z těchto dvou jazyků, zobrazí se tento konkrétní obrázek. U všech ostatních jazyků se zobrazí výchozí obrázek.
* Nakonfigurujte různá loga vaší organizace (třeba verze pro japonštinu nebo hebrejštinu).

## <a name="access-panel-page-customization"></a>Přizpůsobení stránky přístupového panelu
Stránka přístupového panelu je v podstatě stránkou portálu, která umožňuje rychlý přístup ke cloudové aplikaci, ke které vám správce udělil přístup. Na této stránce se vaše aplikace zobrazují jako dlaždice aplikací, na které můžete kliknout.

Následující snímek obrazovky ukazuje příklad stránky přístupového panelu po přizpůsobení.

![][8]

## <a name="configure-your-directory-with-company-branding"></a>Nastavení konfigurace adresáře pomocí firemního brandingu
Na portálu Azure Classic můžete každému adresáři nakonfigurovat jednu výchozí sadu přizpůsobitelných prvků. Po uložení výchozích hodnot může správce přidat lokalizované verze jednotlivých prvků pro různé jazyky a národní prostředí. Všechny přizpůsobitelné prvky jsou volitelné.

Pokud například nakonfigurujete výchozí banner s logem, ale žádný velký obrázek, přihlašovací stránka zobrazí vaše logo v pravém horním rohu. Zobrazí se nicméně výchozí obrázek webu.

Představte si následující konfiguraci:

* Výchozí banner s logem a text přihlašovací stránky v angličtině
* Text přihlašovací stránky přizpůsobený pro němčinu

Pokud je vaším preferovaným jazykem němčina, zobrazí se vám výchozí banner s logem, ale s německým textem.

Technicky je možné nakonfigurovat sadu pro každý jazyk podporovaný službou Azure AD, přesto doporučujeme raději menší počet variant – z důvodu snazší údržby a lepšího výkonu.
 
**Pokud chcete do adresáře přidat vlastní firemní branding, postupujte takto:**

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com) jako správce adresáře, který chcete přizpůsobit.
2. Vyberte adresář, který chcete přizpůsobit.
3. Na panelu nástrojů v horní části stránky klikněte na **Konfigurovat**.
4. Klikněte na **Přizpůsobit branding**.
5. Upravte prvky, které chcete přizpůsobit. Všechna pole jsou volitelná.
6. Klikněte na **Uložit**.

Nové změny brandingu přihlašovací stránky se můžou projevit třeba až za hodinu.

**Pokud chcete přidat firemní branding pro konkrétní jazyk, postupujte takto:**

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com) jako správce adresáře, který chcete přizpůsobit.
2. Vyberte adresář, který chcete přizpůsobit.
fs3. Na panelu nástrojů v horní části stránky klikněte na **Konfigurovat**.
4. Klikněte na **Přizpůsobit branding**.
5. Klikněte na **Přidat branding pro konkrétní jazyk**.
6. Vyberte jazyk, pro který chcete logo přizpůsobit, a potom klikněte na **Další**.
7. Upravte jenom prvky, které chcete nakonfigurovat, aby pro konkrétní jazyk přepisovaly výchozí nastavení . Všechna pole jsou volitelná. Pokud necháte pole prázdné, použije se vlastní výchozí hodnota (nebo výchozí hodnota Microsoftu, pokud vlastní výchozí hodnotu nemáte nakonfigurovanou).
8. Klikněte na **Uložit**.

**Pokud chcete z adresáře odstranit vlastní firemní branding, postupujte takto:**

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com) jako správce adresáře, který chcete přizpůsobit.
2. Vyberte adresář, který chcete přizpůsobit.
3. Na panelu nástrojů v horní části stránky klikněte na **Konfigurovat**.
4. Klikněte na **Přizpůsobit branding**.
5. Na stránce Přizpůsobit branding vyberte **Upravit nastavení existujícího brandingu** a potom přejděte na další stránku.
6. Podle toho, jaký prvek chcete odebrat, proveďte jeden nebo několik z následujících kroků:

    a. V části **Banner s logem** vyberte možnost **Odebrat nahrané logo**.

    b. V části **Dlaždice s logem** vyberte možnost **Odebrat nahrané logo**.

    c. Odeberte text ze všech textových polí.

    d. Klikněte na **Další**.

    e. Odeberte text ze všech textových polí.
7. Kliknutím na **Uložit** prvky odeberete.
8. V případě potřeby znovu klikněte na **Přizpůsobit branding** a opakujte tyto kroky pro každý branding pro konkrétní jazyk, který chcete odebrat.
    Že jsou všechna nastavení brandingu odebraná poznáte tak, že když kliknete na **Přizpůsobit branding**, uvidíte formulář **Přizpůsobit výchozí branding**, který neobsahuje žádnou konfiguraci nastavení.


## <a name="customizable-elements"></a>Přizpůsobitelné prvky
Firemní loga se používají pro přihlašovací stránku i pro stránku přístupového panelu, zatímco ostatní prvky se používají jenom na přihlašovací stránce. Následující tabulka obsahuje podrobnosti o různých přizpůsobitelných prvcích.

| Name (Název) | Popis | Omezení | Doporučení |
| --- | --- | --- | --- |
| Banner s logem |Banner s logem se zobrazuje na přihlašovací stránce a na přístupovém panelu. |<p>JPG nebo PNG</p><p>60 x 280 pixelů</p><p>10 kB</p> |<p>Použijte celé logo vaší organizace (včetně piktogramu a logotypu).</p><p>Dodržte maximální výšku 30 pixelů, aby se na mobilních zařízeních nezobrazovaly posuvníky.</p><p>Dodržte maximální velikost 4 kB.</p><p>Použijte průhledný obrázek PNG (nepředpokládejte, že přihlašovací stránka bude mít vždy bílé pozadí).</p> |
| Dlaždice s logem | Momentálně se nepoužívá |<p>JPG nebo PNG</p><p>120 x 120 pixelů</p><p>10 kB</p> |<p>Udržte to jednoduché (žádný drobný text), protože velikost obrázku může být změněna na 50 %. |
| </p> | | | |
| Štítek s přihlašovacím uživatelským jménem | Momentálně se nepoužívá |<p>Text v kódu Unicode, až 50 znaků.</p><p>Jenom prostý text (žádné odkazy nebo značky jazyka HTML).</p> |<p>Pište krátce a jednoduše.</p><p>Zeptejte se uživatelů, jak obvykle nazývají pracovní nebo školní účet, který jim poskytujete.</p> |
| Často používaný text přihlašovací stránky |Tento často používaný text se zobrazuje pod formulářem přihlašovací stránky a vy ho můžete použít k uvedení dalších pokynů nebo k nasměrování na nápovědu a podporu. |<p>Text v kódu Unicode, až 256 znaků.</p><p>Jenom prostý text (žádné odkazy nebo značky jazyka HTML).</p> |Dodržte maximální délku textu 250 znaků (přibližně tři řádky textu). |
| Obrázek na pozadí na přihlašovací stránce | Velký obrázek, který se zobrazuje na levé straně přihlašovací stránky (pro jazyky psané zprava doleva na pravé straně), když uživatel přistupuje k adrese URL konkrétního klienta. |<p>JPG nebo PNG</p><p>1420 × 1200</p><p>500 kB</p> |<p>1420 × 1200 pixelů</p><p>Důležité: Pokuste se udržet co nejmenší, ideálně do 200 kB. Pokud je obrázek příliš velký, ovlivní to výkon přihlašovací stránky v případě, kdy obrázek není načtený v mezipaměti.</p><p>Tento obrázek bude skoro vždycky oříznutý, aby se zohlednily poměry stran různých obrazovek. Primární vizuální prvky proto umístěte do levého horního rohu.</p> |
| Barva pozadí na přihlašovací stránce | U připojení s nízkou šířkou pásma tato plná barva nahrazuje obrázek na pozadí. | Musí to být barva RGB v šestnáctkovém formátu (příklad: \#FFFFFF). | Doporučujeme vybrat primární barvu banneru s logem. |

## <a name="next-steps"></a>Další kroky
* [Začínáme se službou Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Zobrazení sestav přístupů a používání](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/signin-page_before-customization.png
[2]: ./media/active-directory-add-company-branding/signin-page-restricted-app.png
[3]: ./media/active-directory-add-company-branding/signin-page-open-access.png
[4]: ./media/active-directory-add-company-branding/signin-page-external-guest.png
[5]: ./media/active-directory-add-company-branding/which-elements-can-i-customize.png
[6]: ./media/active-directory-add-company-branding/hide-kmsi.png
[8]: ./media/active-directory-add-company-branding/APBranding.png
[9]: ./media/active-directory-add-company-branding/hidekmsi.png

