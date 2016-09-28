<properties
    pageTitle="Přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu"
    description="Postup přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu v Azure"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/13/2016"
    ms.author="MarkVi"/>


# Přidání firemního brandingu na přihlašovací stránku a na stránku přístupového panelu


Mnoho společností chce předcházet zmatení uživatele a upřednostňuje jednotný vzhled všech webů a služeb, které spravují. Azure Active Directory tuto možnost nabízí a umožňuje přizpůsobení vzhledu následujících webových stránek, na které můžete přidat svoje firemní logo a vlastní barevná schémata:

- **Přihlašovací stránka**: To je stránka, která se zobrazuje při přihlašování k Office 365 nebo k jiným webovým aplikacím, které používají Azure AD jako zprostředkovatele identity. Tuto stránku budete používat při vyhledávání domovské sféry nebo při zadávání přihlašovacích údajů. Vyhledávání domovské sféry umožňuje systému přesměrovat federované uživatele na jejich místní službu tokenů zabezpečení (například AD FS).

- **Stránka přístupového panelu**: Přístupový panel je webový portál, který vám umožňuje zobrazovat a spouštět cloudové aplikace, ke kterým vám správce Azure AD udělil přístup. Přístupový panel otevřete na následující adrese URL: [https://myapps.microsoft.com](https://myapps.microsoft.com).

Toto téma popisuje možnosti přizpůsobení přihlašovací stránky a stránky přístupového panelu.

> [AZURE.NOTE]
>
- Firemní branding je funkce, která je dostupná jenom v případě, pokud jste upgradovali na edici Premium nebo Basic služby Azure Active Directory. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).
- Edice Premium a Basic služby Azure Active Directory jsou zákazníkům v Číně dostupné prostřednictvím celosvětové instance služby Azure Active Directory. Edice Premium a Basic služby Azure Active Directory nejsou aktuálně podporované ve službě Microsoft Azure provozované v Číně společností 21Vianet. Další informace si vyžádejte na [fóru služby Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).



## Přizpůsobení přihlašovací stránky

Pokud potřebujete v prohlížeči otevřít cloudové aplikace a služby, které si vaše organizace předplatila, obvykle použijete přihlašovací stránku.

Pokud jste přihlašovací stránku změnili, může se taková změna projevit až za hodinu.

Přihlašovací stránka ve vaší firemní úpravě se zobrazí jenom tehdy, když službu navštívíte pomocí adresy URL konkrétního klienta, například https://outlook.com/**contoso**.com nebo https://mail.**contoso**.com.

Když službu navštívíte pomocí adresy URL, která se neváže ke konkrétnímu klientu (např: https://mail.office365.com), zobrazí se přihlašovací stránka bez firemní úpravy. V tomto případě se branding zobrazí až potom, co zadáte ID uživatele nebo vyberete dlaždici uživatele.

> [AZURE.NOTE]
>
- Název domény se musí v sekci portálu Azure Classic **Active Directory** > **Adresář** > **Domény**, kde jste branding nakonfigurovali, zobrazovat jako „Aktivní“.
- Branding přihlašovací stránky se nepřenáší na spotřebitelskou přihlašovací stránku Microsoftu. Pokud se přihlásíte pomocí osobního účtu Microsoft, může se vám zobrazit seznam uživatelských dlaždic s brandingem, které vykreslí Azure AD, ale branding vaší organizace se na přihlašovací stránku účtu Microsoft nepoužije.


Pokud chcete na této stránce zobrazit značku, barvy a další přizpůsobitelné prvky vaší společnosti, prohlédněte si následující obrázky, abyste pochopili rozdíl mezi oběma prostředími.

Následující snímek obrazovky ukazuje příklad přihlašovací stránky Office 365 na stolním počítači **před** přizpůsobením:

![Přihlašovací stránka Office 365 před přizpůsobením][1]

Následující snímek obrazovky ukazuje příklad přihlašovací stránky Office 365 na stolním počítači **po** přizpůsobení:

![Přihlašovací stránka Office 365 po přizpůsobení][2]

Následující snímek obrazovky ukazuje příklad přihlašovací stránky Office 365 na mobilním zařízení **před** přizpůsobením:

![Přihlašovací stránka Office 365 před přizpůsobením][3]


Následující snímek obrazovky ukazuje příklad přihlašovací stránky Office 365 na mobilním zařízení **po** přizpůsobení:

![Přihlašovací stránka Office 365 po přizpůsobení][4]


Když změníte velikost okna prohlížeče, velký obrázek (podobný jako výše uvedený) se často bude muset oříznout, aby vyhovoval poměrům stran různých obrazovek. Myslete na to a snažte se zachovat klíčové vizuální prvky obrázku tím, že se budou vždy zobrazovat v levém horním rohu (v pravém horním rohu v případě jazyků psaných zprava doleva). Je to důležité kvůli tomu, že změna velikosti obvykle začíná v pravém dolním rohu a postupuje směrem nahoru a doleva nebo zezdola směrem nahoru.

Následující obrázek znázorňuje oříznutí obrázku při změně velikosti prohlížeče doleva:

![][6]

Zde je zobrazení po změně velikosti prohlížeče směrem nahoru:

![][7]

## Přizpůsobitelné prvky na stránce

Na přihlašovací stránce může přizpůsobit následující prvky:

![][5]

 Prvek stránky  | Umístění na stránce
    ------------- | -------------
Banner s logem | Zobrazuje se v pravé horní části stránky. Nahrazuje logo, které zobrazuje cílový web, ke kterému se přihlašujete (například Office 365 nebo Azure).
Velký obrázek / barva pozadí | Zobrazuje se v levé části stránky. Nahrazuje obrázek, který zobrazuje cílový web, ke kterému se přihlašujete. Barva pozadí se může zobrazit místo velkého obrázku v případě připojení s malou šířkou pásma nebo na úzkých obrazovkách.
Text na přihlašovací stránce | Zobrazuje se nad zápatím stránky, když potřebujete sdělit užitečné informace před samotným přihlášením pomocí pracovního nebo školního účtu. Můžete chtít například zobrazit telefonní číslo na oddělení technické podpory nebo právní prohlášení.

> [AZURE.NOTE]
Všechny prvky jsou volitelné. Pokud například určíte banner s logem, ale neurčíte žádný velký obrázek, přihlašovací stránka zobrazí vaše logo a obrázek z cílového webu (v tomto případě obrázek kalifornské dálnice z Office 365).

Všechny prvky na této stránce můžete lokalizovat. Po konfiguraci „výchozí“ sady prvků přizpůsobení můžete nakonfigurovat další verze pro různá národní prostředí. Různé prvky mezi sebou můžete kombinovat. Můžete například provést následující věci:

- Vytvořte „výchozí“ velký obrázek, který je použitelný pro všechny kultury, a potom vytvořte specifické verze pro angličtinu a francouzštinu. Když svoje prohlížeče nastavíte na jeden z těchto dvou jazyků, zobrazí se tento konkrétní obrázek. U všech ostatních jazyků se zobrazí výchozí obrázek.

- Nakonfigurujte různá loga vaší organizace (například verze pro japonštinu nebo hebrejštinu).



## Přizpůsobení stránky přístupového panelu

Stránka přístupového panelu je v podstatě stránkou portálu, která umožňuje rychlý přístup ke cloudové aplikaci, ke které vám správce udělil přístup. Na této stránce se vaše aplikace zobrazují jako dlaždice aplikací, na které můžete kliknout.


Následující snímek obrazovky ukazuje příklad stránky přístupového panelu po přizpůsobení.

![][8]

## Nastavení konfigurace adresáře pomocí firemního brandingu

Na portálu Azure Classic můžete každému adresáři nakonfigurovat jednu výchozí sadu přizpůsobitelných prvků. Po uložení výchozích hodnot může správce přidat lokalizované verze jednotlivých prvků pro různé jazyky a národní prostředí. Všechny přizpůsobitelné prvky jsou volitelné.

Pokud například nakonfigurujete výchozí banner s logem, ale žádný velký obrázek, přihlašovací stránka zobrazí vaše logo v pravém horním rohu. Zobrazí se nicméně výchozí obrázek webu.

Představte si následující konfiguraci:

- Výchozí banner s logem a text přihlašovací stránky v angličtině
- Text přihlašovací stránky přizpůsobený pro němčinu

Pokud je vaším preferovaným jazykem němčina, zobrazí se vám výchozí banner s logem, ale s německým textem.

Technicky je možné nakonfigurovat sadu pro každý jazyk podporovaný službou Azure AD, přesto doporučujeme raději menší počet variant – z důvodu snazší údržby a lepšího výkonu.

**Pokud chcete do adresáře přidat vlastní firemní branding, proveďte následující kroky:**

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com) jako správce adresáře, který chcete přizpůsobit.
2. Vyberte adresář, který chcete přizpůsobit.
3. Na panelu nástrojů v horní části stránky klikněte na **Konfigurovat**.
4. Klikněte na **Přizpůsobit branding**.
4. Upravte prvky, které chcete přizpůsobit. Všechna pole jsou volitelná.
5. Klikněte na **Uložit**.

Nové změny brandingu přihlašovací stránky se můžou projevit třeba až za hodinu.

**Pokud chcete přidat firemní branding pro konkrétní jazyk, proveďte následující kroky:**

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com) jako správce adresáře, který chcete přizpůsobit.
2. Vyberte adresář, který chcete přizpůsobit.
3. Na panelu nástrojů v horní části stránky klikněte na **Konfigurovat**.
4. Klikněte na **Přizpůsobit branding**.
2. Klikněte na **Přidat branding pro konkrétní jazyk**.
3. Vyberte jazyk, pro který chcete logo přizpůsobit, a potom klikněte na **Další**.
3. Upravte jenom prvky, které chcete nakonfigurovat, aby pro konkrétní jazyk přepisovaly výchozí nastavení . Všechna pole jsou volitelná. Pokud necháte pole prázdné, použije se vlastní výchozí hodnota (nebo výchozí hodnota Microsoftu, pokud vlastní výchozí hodnotu nemáte nakonfigurovanou).
4. Klikněte na **Uložit**.

**Pokud chcete z adresáře odstranit vlastní firemní branding, proveďte následující kroky:**

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com) jako správce adresáře, který chcete přizpůsobit.
2. Vyberte adresář, který chcete přizpůsobit.
3. Na panelu nástrojů v horní části stránky klikněte na **Konfigurovat**.
4. Klikněte na **Přizpůsobit branding**.
5. Na stránce Přizpůsobit branding vyberte **Upravit nastavení existujícího brandingu** a potom přejděte na další stránku.
3. Podle toho, jaký prvek chcete odebrat, proveďte jeden nebo několik z následujících kroků:

    a. V části **Banner s logem** vyberte možnost **Odebrat nahrané logo**.

    b. V části **Dlaždice s logem** vyberte možnost **Odebrat nahrané logo**.

    c. Odeberte text ze všech textových polí.

    d. Klikněte na **Další**.

    e. Odeberte text ze všech textových polí.

4. Kliknutím na **Uložit** prvky odeberete.
5. V případě potřeby znovu klikněte na **Přizpůsobit branding** a opakujte tyto kroky pro každý branding pro konkrétní jazyk, který chcete odebrat.
    Že jsou všechna nastavení brandingu odebraná poznáte tak, že když kliknete na **Přizpůsobit branding**, uvidíte formulář **Přizpůsobit výchozí branding**, který neobsahuje žádnou konfiguraci nastavení.

## Testování a příklady

Před provedením změn v produkčním prostředí doporučujeme nejprve experimenty s testovacím klientem.

**Pokud chcete ověřit, jestli se váš branding používá, proveďte následující kroky:**

1. Otevřete relaci prohlížeče InPrivate nebo Incognito.
2. Navštivte stránku https://outlook.com/contoso.com a text „contoso.com“ nahraďte doménou, kterou jste přizpůsobili.

Tento postup funguje i s doménami, které mají tvar contoso.onmicrosoft.com.

Abychom vám pomohli s vytvářením efektivních sad přizpůsobení, přizpůsobili jsme dvě fiktivní přihlašovací stránky:

- [http://aka.ms/aaddemo001](http://aka.ms/aaddemo001)
- [http://aka.ms/aaddemo002](http://aka.ms/aaddemo002)

Abyste mohli otestovat nastavení pro konkrétní jazyk, změňte výchozí jazykové předvolby webového prohlížeče na jazyk, který jste nastavili ve vašem vlastním přizpůsobení. V Internet Exploreru můžete konfiguraci provést v nabídce **Možnosti Internetu**.

## Přizpůsobitelné prvky

Některé přizpůsobitelné prvky v Azure AD mají více možností použití. Firemní logo můžete v adresáři nakonfigurovat jen jednou a používat ho na přihlašovací stránce i na stránce přístupového panelu. Některé přizpůsobitelné prvky jsou určené jenom pro přihlašovací stránku. Následující tabulka obsahuje podrobnosti o různých přizpůsobitelných prvcích.

Name (Název) | Popis | Omezení | Doporučení
    ------------- | ------------- | ------------- | -------------
Banner s logem | Banner s logem se zobrazuje na přihlašovací stránce a na přístupovém panelu. | <p>JPG nebo PNG</p><p>60 x 280 pixelů</p><p>10 kB</p> | <p>Použijte celé logo vaší organizace (včetně piktogramu a logotypu).</p><p>Dodržte maximální výšku 30 pixelů, aby se na mobilních zařízeních nezobrazovaly posuvníky.</p><p>Dodržte maximální velikost 4 kB.</p><p>Použijte průhledný obrázek PNG (nepředpokládejte, že přihlašovací stránka bude mít vždy bílé pozadí).</p>
Dlaždice s logem | (Aktuálně se na přihlašovací stránce nepoužívá.) V budoucnu může tento text na různých místech prostředí nahradit obecný piktogram „pracovní nebo školní účet“. | <p>JPG nebo PNG</p><p>120 x 120 pixelů</p><p>10 kB</p> | <p>Udržte to jednoduché (žádný drobný text), protože velikost obrázku může být změněna na 50 %.
</p> |
Popisek uživatelského jména na přihlašovací stránce | (Aktuálně se na přihlašovací stránce nepoužívá.) V budoucnu může tento text na různých místech prostředí nahradit obecný řetězec „pracovní nebo školní účet“. Můžete ho nastavit třeba na „Účet Contoso“ nebo „Contoso ID“. | <p>Text v kódu Unicode, až 50 znaků.</p><p>Jenom prostý text (žádné odkazy nebo značky jazyka HTML).</p> | <p>Pište krátce a jednoduše.</p><p>Zeptejte se uživatelů, jak obvykle nazývají pracovní nebo školní účet, který jim poskytujete.</p>
Text na přihlašovací stránce | Tento často používaný text se zobrazuje pod formulářem přihlašovací stránky a vy ho můžete použít ke sdělení dalších pokynů nebo k nasměrování na nápovědu a podporu. | <p>Text v kódu Unicode, až 256 znaků.</p><p>Jenom prostý text (žádné odkazy nebo značky jazyka HTML).</p> | Dodržte maximální délku textu 250 znaků (přibližně tři řádky textu).
Obrázek na přihlašovací stránce | Jedná se o velký obrázek, který se zobrazí na přihlašovací stránce nalevo od formuláře. | <p>JPG nebo PNG</p><p>1420 × 1200</p><p>500 kB</p> | <p>1420 × 1200 pixelů</p><p>Důležité: Pokuste se udržet co nejmenší, ideálně do 200 kB. Pokud je obrázek příliš velký, ovlivní to výkon přihlašovací stránky v případě, kdy obrázek není načtený v mezipaměti.</p><p>Obrázek se často ořezává, aby vyhovoval poměrům stran různých obrazovek. Klíčové vizuální prvky umístěte do levého horního rohu (do pravého horního rohu v případě jazyků psaných zprava doleva), protože při zmenšení okna prohlížeče začíná změna velikosti obvykle v pravém dolním rohu a postupuje směrem nahoru a doleva.</p>
Barva pozadí na přihlašovací stránce | Barva pozadí na přihlašovací stránce se používá v oblasti, která je nalevo od formuláře. | Musí to být barva RGB v šestnáctkovém formátu (příklad: #FFFFFF). | <p>Barva pozadí se může zobrazit místo velkého obrázku v případě připojení s malou šířkou pásma.</p><p>Doporučujeme vybrat primární barvu banneru s logem.</p>


## Další kroky

- [Začínáme se službou Azure Active Directory Premium](active-directory-get-started-premium.md)
- [Zobrazení sestav přístupů a používání](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-add-company-branding/SignInPage_beforecustomization.png
[2]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization.png
[3]: ./media/active-directory-add-company-branding/SignInPage_mobile_beforecustomization.png
[4]: ./media/active-directory-add-company-branding/SignInPage_mobile_aftercustomization.png
[5]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_elements.png
[6]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedleft.png
[7]: ./media/active-directory-add-company-branding/SignInPage_aftercustomization_croppedtop.png
[8]: ./media/active-directory-add-company-branding/APBranding.png



<!--HONumber=Sep16_HO3-->


