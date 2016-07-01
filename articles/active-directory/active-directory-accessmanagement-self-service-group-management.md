<properties
    pageTitle="Nastavení služby Azure Active Directory pro správu přístupu k samoobslužným aplikacím | Microsoft Azure"
    description="Samoobslužná správa skupin umožňuje uživatelům vytvářet a spravovat skupiny zabezpečení nebo skupiny Office 365 ve službě Azure Active Directory a nabízí uživatelům možnost žádat o členství ve skupině zabezpečení nebo ve skupině Office 365."
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="stevenpo"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="curtand"/>

# Nastavení služby Azure Active Directory pro samoobslužnou správu skupin

Samoobslužná správa skupin umožňuje uživatelům vytvářet a spravovat skupiny zabezpečení nebo skupiny Office 365 ve službě Azure Active Directory (Azure AD) a nabízí uživatelům možnost žádat o členství ve skupině zabezpečení nebo ve skupině Office 365, což může být následně schváleno nebo odmítnuto vlastníkem skupiny. Díky používání funkce samoobslužné správy skupin můžete každodenní řízení členství ve skupině delegovat na uživatele, kteří chápou obchodní kontext takového členství. Funkce samoobslužné správy skupin jsou dostupné jenom pro skupiny zabezpečení a skupiny Office 365. Nejsou dostupné pro skupiny zabezpečení s povoleným e-mailem a pro distribuční seznamy.

Samoobslužná správa skupin v současné době obsahuje dva základní scénáře: delegovanou správu skupin a samoobslužnou správu skupin.

- **Delegovaná správa skupin** – příkladem je správce, který spravuje přístup k aplikaci SaaS, kterou používá jeho společnost. Správa těchto přístupových práv je čím dál náročnější, takže správce požádá majitele firmy, aby vytvořil novou skupinu. Správce nyní přiřadí přístup k aplikaci nové skupině, kterou majitel firmy právě vytvořil, a umístí všechny uživatele, kteří mají k této aplikaci aktuálně přístup, do této skupiny. Majitel firmy potom může přidat další uživatele a tito uživatelé budou o několik okamžiků později automaticky přiřazeni k aplikaci. Majitel firmy nemusí čekat na správce, ale může přístup svých uživatelů spravovat sám. Správce může stejnou věc udělat pro asistenta v případě jiné obchodní skupiny, takže majitel firmy i asistent teď můžou spravovat přístup svých uživatelů – a nebudou při tom mít možnost pracovat s uživateli nebo vidět uživatele toho druhého. Správce může stále vidět všechny uživatele, kteří mají přístup k aplikaci, a v případě potřeby je může zablokovat.

- **Samoobslužná správa skupiny** – příkladem tohoto scénáře jsou dva uživatelé, kteří oba mají nezávisle nastavené weby SharePoint Online, ale kteří chtějí poskytnout týmům druhé strany přístup na svůj web. Aby toho dosáhli, můžou vytvořit jednu skupinu v Azure AD a potom v SharePointu Online každý z nich vybere tu samou skupinu a poskytne jí přístup na svůj web. Pokud chce uživatel získat přístup, požádá o něj na přístupovém panelu a po schválení získá přístup k oběma webům SharePoint Online automaticky. Později se jeden z nich rozhodne, že všichni uživatelé s přístupem k jeho serveru by měli získat přístup i k určité aplikaci SaaS. Požádá správce takové aplikace SaaS, aby přidal přístupová práva k aplikaci na jeho web. Od toho okamžiku získají veškeré jím schválené žádosti přístup na oba weby SharePoint Online a také k aplikaci SaaS.

## Zpřístupnění skupiny pro samoobslužné funkce koncových uživatelů

Na [portálu Azure Classic](https://manage.windowsazure.com) na kartě **Konfigurovat** nastavte možnost **Delegovaná správa skupin** na Povoleno a potom nastavte možnost **Uživatelé můžou vytvářet skupiny zabezpečení** nebo **Uživatelé můžou vytvářet skupiny Office** na Povoleno.

Když je povolená možnost **Uživatelé můžou vytvářet skupiny zabezpečení**, mají všichni uživatelé v adresáři povoleno vytvářet nové skupiny zabezpečení a přidávat do těchto skupin členy. Tyto nové skupiny se na přístupovém panelu zobrazí také všem ostatním uživatelům a tito ostatní uživatelé budou moct žádat o připojení k těmto skupinám (pokud to nastavení zásad skupiny umožňuje). Pokud je možnost **Uživatelé můžou vytvářet skupiny zabezpečení** zakázaná, uživatelé nemůžou vytvářet skupiny ani měnit existující skupiny, které vlastní, ale můžou stále spravovat členství v těchto skupinách a můžou schvalovat žádosti jiných uživatelů o připojení ke skupinám.

Možnost **Uživatelé, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení ** můžete použít k zajištění podrobnějšího řízení přístupu, který vám samoobslužná správa skupin nenabídne. Když je povolená možnost **Uživatelé můžou vytvářet skupiny**, mají všichni uživatelé v adresáři povoleno vytváření nových skupin a přidávání členů do těchto skupin. Současným nastavením možnosti **Uživatelé, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** na Některé omezíte správu skupin jenom na omezenou skupinu uživatelů. Když tento přepínač nastavíte na Některé, vytvoří se v adresáři skupina s názvem SSGMSecurityGroupsUsers a jenom uživatelé, kterým jste udělili členství v této skupině, potom můžou ve vašem adresáři vytvářet nové skupiny zabezpečení a přidávat do těchto skupin členy. Nastavením možnosti **Uživatelé, kteří můžou využívat samoobslužné funkce pro skupiny zabezpečení** na Všichni povolíte všem uživatelům v adresáři vytváření nových skupin.

Můžete také použít pole **Skupina, která může používat samoobslužné funkce pro skupiny zabezpečení** (ve výchozím nastavení nastavené na „SSGMSecurityGroupsUsers“) a zadat vlastní název skupiny, která bude obsahovat všechny uživatele s možností používání samoobslužných funkcí a vytváření nových skupin v adresáři.

## Další informace

Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)

* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

* [Představení služby Azure Active Directory](active-directory-whatis.md)

* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)



<!--HONumber=Jun16_HO2-->


