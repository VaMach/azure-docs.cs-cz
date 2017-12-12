---
title: "Přizpůsobit přihlašovací stránka ve službě Azure Active Directory | Microsoft Docs"
description: "Informace o postupu přidání firemního brandingu na stránky Azure přihlášení"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: curtand
ms.reviewer: kexia
custom: it-pro
ms.openlocfilehash: 0855129c35c0c3d0f1814e8d29b6f3ae7d950db7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="quickstart-add-company-branding-to-your-sign-in-page-in-azure-ad"></a>Rychlé spuštění: Přidání firemního brandingu na přihlašovací stránku ve službě Azure AD
Mnoho společností chce předcházet zmatení uživatele a upřednostňuje jednotný vzhled všech webů a služeb, které spravují. Azure Active Directory (Azure AD) tuto možnost nabízí a umožňuje přizpůsobení vzhledu stránku přihlášení s svoje firemní logo a vlastní barevná schémata. Přihlašovací stránka je stránka, která se zobrazí při přihlášení k Office 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity. Budete používat tuto stránku k zadání pověření.

> [!NOTE]
> * Firemní branding je k dispozici pouze v případě, že jste upgradovali na edici Premium nebo Basic služby Azure AD, nebo mít licenci pro Office 365. Další informace, pokud je funkce nepodporuje váš typ licence, zkontrolujte [stránce s cenami. informace o službě Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
> 
> * Edice Premium a Basic služby Azure Active Directory jsou zákazníkům v Číně dostupné prostřednictvím celosvětové instance služby Azure Active Directory. Edice Premium a Basic služby Azure Active Directory nejsou aktuálně podporované ve službě Microsoft Azure provozované v Číně společností 21Vianet. Další informace si vyžádejte na [fóru služby Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="customizing-the-sign-in-page"></a>Přizpůsobení přihlašovací stránky

<!--You can customize the following elements on the sign-in page: <attach image>-->

Firemního brandingu přizpůsobení zobrazí na přihlašovací stránce služby Azure AD, když uživatelé přistupují k adresy URL konkrétního klienta, jako [ *https://outlook.com/contoso.com*](https://outlook.com/contoso.com).

Když uživatelé navštíví obecné adresy URL, například www.office.com, neobsahuje firemního brandingu přizpůsobení, protože systém nebude vědět, kdo je uživatel na přihlašovací stránku. Firemní branding zobrazí po uživatele zadejte své ID uživatele nebo vyberte dlaždici uživatele.

> [!NOTE]
> * Název domény musí zobrazovat jako "Aktivní" v **domén** část portálu Azure, ve kterém jste branding nakonfigurovali. Další informace najdete v tématu [přidání vlastního názvu domény](add-custom-domain.md).
> * Branding přihlašovací stránky se nepřenáší na přihlašovací stránku pro osobní účty Microsoftu. Pokud zaměstnancům nebo obchodní hosté, přihlaste se pomocí osobního účtu Microsoft, jejich přihlašovací stránce nemusí odpovídat branding vaší organizace.


### <a name="banner-logo"></a>Banner s logem 

Popis | Omezení | Doporučení
------- | ------- | ----------
Banner s logem se zobrazí na přihlášení a stránkách panelu.<br>Na stránce přihlášení zobrazí po uživatele organizaci je určen, obvykle po zadejte uživatelské jméno.  | Transparentní JPG nebo PNG<br>Maximální výška: 36 px<br>Maximální šířka: 245 px | Tady použijte logo vaší organizace.<br>Použijte průhledný obrázek. Nepředpokládejte, že bude bílé pozadí.<br>Nepřidávejte výplně kolem vaše logo v bitové kopii nebo vaše logo bude vypadat nepřiměřeně malé.

### <a name="username-hint"></a>Pomocný parametr uživatelského jména   
Popis | Omezení | Doporučení
------- | ------- | ----------
To umožní vlastní nastavení pomocný parametr text do pole uživatelské jméno. | Text v kódu Unicode až 64 znaků.<br>Jenom prostý text | Doporučujeme vám, že není nastaveno to pokud očekáváte mimo vaši organizaci k přihlášení do aplikace pro uživatele typu Host.
            
### <a name="sign-in-page-text"></a>Text na přihlašovací stránce   
Popis | Omezení | Doporučení
------- | ------- | ----------
Zobrazuje se v dolní části formuláře přihlášení a slouží ke komunikaci Další informace, jako je telefonní číslo, které má vaše oddělení technické podpory nebo právní prohlášení. | Text v kódu Unicode až 256 znaků.<br>Jenom prostý text (žádné odkazy nebo značky jazyka HTML).   

### <a name="sign-in-page-image"></a>Obrázek přihlašovací stránky  
Popis | Omezení | Doporučení
------- | ------- | ----------
To se zobrazí na pozadí na přihlašovací stránku, je ukotven k centru zobrazitelné místa a bude škálování a oříznout okna prohlížeče.    <br>Na úzkých obrazovkách například mobilních telefonů se nezobrazí tuto bitovou kopii.<br>Černé maska s 0.55 krytí uplatní přes tuto bitovou kopii kódem našeho při načtení stránky. | JPG nebo PNG<br>Obrázek dimenze: 1920 × 1080 pixelů<br>Velikost souboru: &lt; 300 KB | <br>Používat Image místě, kde není silné subjektu fokus. Formulář neprůhledného přihlášení se zobrazí na střed tuto bitovou kopii a může zahrnovat všechny součástí bitové kopie v závislosti na velikosti okna prohlížeče.<br>Udržet co nejmenší zajistit časů rychlé načítání velikost souboru. 

### <a name="background-color"></a>Barva pozadí
Popis | Omezení | Doporučení
------- | ------- | ----------
Tato barva použijí se místo obrázek pozadí na připojení s malou šířkou pásma. | Barva RGB v šestnáctkové soustavě (Příklad: #FFFFFF | Doporučujeme pomocí primární barvu banneru s logem nebo barva vaší organizace.

### <a name="show-option-to-remain-signed-in"></a>Zobrazit možnost zůstat přihlášeni
Popis | Omezení | Doporučení
------- | ------- | ----------
Přihlášení Azure AD poskytuje možnost při jejich zavřete a znovu ho otevřete svého prohlížeče zůstat přihlášeného uživatele. Použijte tuto možnost skrýt.<br>Tuto možnost nastavíte na hodnotu "Ne" Skrýt tuto možnost od uživatelů. | &nbsp; | Toto nastavení neovlivní dobu platnosti relace.<br>Některé funkce služby SharePoint Online a Office 2010, závisí na možnosti výběru zůstat přihlášeného uživatele. Pokud nastavíte jako skryté, může se uživatelům zobrazí další a neočekávané výzev k přihlášení.

> [!NOTE]
> Všechny prvky jsou volitelné. Například pokud určíte banner s logem se žádný obrázek na pozadí, na přihlašovací stránce se zobrazí vaše logo a obrázek pozadí pro cílové lokalitě (například Office 365).

## <a name="add-company-branding-to-your-directory"></a>Přidání firemního brandingu do vašeho adresáře

1. Přihlaste se k [portálu Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/customize-branding/user-management.png)
3. Na **uživatelů a skupin** vyberte **firemní branding**.
4. Na **uživatelé a skupiny - firemní branding** okně, vyberte **upravit** příkaz.

    ![Upravit vlastní branding](./media/customize-branding/edit-branding.png)
5. Upravte prvky, které chcete přizpůsobit. Všechny prvky jsou volitelné.
6. Klikněte na **Uložit**.

Může trvat až jednu hodinu pro všechny změny, které jste udělali na přihlašovací stránku branding zobrazí.

## <a name="add-language-specific-company-branding-to-your-directory"></a>Přidání konkrétní jazyk firemního brandingu do vašeho adresáře

1. Přihlaste se k [centru pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který má k adresáři oprávnění globálního správce.
2. Vyberte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/customize-branding/user-management.png)
3. Na **uživatelů a skupin** vyberte **firemní branding**.
4. Na **uživatelé a skupiny - firemní branding** okně, vyberte **přidat jazyk** příkaz.

    ![Přidání brandingu elementy jazyka](./media/customize-branding/add-language.png)
5. Upravte prvky, které chcete přizpůsobit. Všechny prvky jsou volitelné.
6. Klikněte na **Uložit**.

Může trvat až jednu hodinu pro všechny změny, které jste udělali na přihlašovací stránku branding zobrazí.

## <a name="next-steps"></a>Další kroky
V tento rychlý start když jste se naučili postup přidání firemního brandingu na adresář Azure AD. 

Na následující odkaz můžete použít ke konfiguraci vašeho firemního brandingu ve službě Azure AD na portálu Azure.

> [!div class="nextstepaction"]
> [Konfigurace značky společnosti](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LoginTenantBrandingBlade) 