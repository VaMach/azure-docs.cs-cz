<properties 
    pageTitle="Zabezpečení cloudových prostředků s Azure Multi-Factor Authentication a AD FS" 
    description="Toto je stránka vícefaktorového ověřování Azure, která popisuje, jak začít s vícefaktorovým ověřováním Azure a službami AD FS v cloudu." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>

# Zabezpečení cloudových prostředků s Azure Multi-Factor Authentication a AD FS

Pokud je vaše organizace federovaná pomocí služby Azure Active Directory a vy máte prostředky, které jsou dostupné přes Azure AD, můžete použít k zabezpečení těchto prostředků ověřování Azure Multi-Factor Authentication nebo Active Directory Federation Services. Následující postupy použijte k zabezpečení prostředků Azure Active Directory s ověřováním Azure Multi-Factor Authentication nebo Active Directory Federation Services.

## Zabezpečení prostředků Azure AD pomocí služby AD FS proveďte následující: 



1. Postupujte podle kroků uvedených v [zapnutí ověřování multi-factor authentication](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) pro uživatele pro povolení účtu.
2. Chcete-li nastavit pravidlo deklarací identity, použijte následující postup:

![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)

-   Spusťte konzolu pro správu služby AD FS.
-   Přejděte do trustů přejímající strany a klikněte pravým tlačítkem na trust přejímající  strany. Vyberte možnost upravit pravidla deklarace identity...
-   Klikněte na možnost Přidat pravidlo...
-   Z rozevírací nabídky vyberte možnost odeslat nároky pomocí vlastního pravidla a klikněte na tlačítko Další.
-   Zadejte název pravidla deklarace identity.
-   V části Vlastní pravidlo: přidejte následující položky:


        => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");

    Odpovídající deklarace identity:

        <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
        <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
        </saml:Attribute>
- Klikněte na tlačítko OK. Klikněte na tlačítko Dokončit. Uzavřete konzolu pro správu služby AD FS.

Uživatelé pak mohou dokončit přihlášení pomocí místních metod (například pomocí karty smartcard).

## Důvěryhodné IP adresy pro federované uživatele
Důvěryhodné IP adresy umožňují správcům obejít ověřování multi-factor authentication pro konkrétní IP adresu nebo pro federované uživatele, kteří mají požadavky pocházejících z vlastního intranetu. V následujících částech popíšeme, jak nakonfigurovat důvěryhodné IP adresy ověřování Azure multi-factor authentication s federovanými uživateli a obejít ověřování multi-factor authentication, když požadavek pochází z intranetu federovaného uživatele.  Toho dosáhnete pomocí konfigurace služby AD FS pro používání průchodu nebo filtru příchozí šablony deklarace identity pomocí typu deklarace identity uvnitř podnikové sítě.  Tento příklad používá Office 365 pro naše trusty přijímající strany.

### Konfigurace pravidel deklarací identity služby AD FS

První věc, kterou je potřeba udělat, je konfigurace deklarací identity služby AD FS. Budeme vytvářet dvě pravidla deklarace identity, jedno pro typ deklarace identity uvnitř podnikové sítě a další pro zachování přihlášení uživatelů.

1. Otevřete správu služby AD FS.
2. Na levé straně vyberte trusty přijímající strany.
3. Uprostřed klikněte pravým tlačítkem myši na aplikaci Microsoft Office 365 Identity Platform a vyberte možnost **Upravit pravidla deklarace identity...**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. V pravidlech transformace vystavení klikněte na tlačítko **Přidat pravidlo.**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. V průvodci Přidat transformované pravidlo deklarace identity vyberte možnost Předat prostřednictvím nebo Filtrovat příchozí deklarace identity z rozevíracího seznamu a klikněte na tlačítko Další.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Do pole vedle názvu pravidla deklarace identity zadejte název pravidla. Příklad: InsideCorpNet.
7. Z rozevíracího seznamu vedle možnosti Typ příchozí deklarace identity vyberte položku V podnikové síti.
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klikněte na tlačítko Dokončit.
9. V pravidlech transformace vystavení klikněte na tlačítko **Přidat pravidlo**.
10. V průvodci Přidat transformované pravidlo deklarace identity vyberte možnost Odeslat deklarace identity pomocí vlastního pravidla z rozevíracího seznamu a klikněte na tlačítko Další.
11. V rozevíracím seznamu pod položkou Název pravidla deklarace identity: zadejte Zachovat přihlášené uživatele.
12. Do pole Vlastní pravidlo zadejte:
        
        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klikněte na **Dokončit**.
14. Klikněte na tlačítko **Použít**.
15. Klikněte na tlačítko **OK**.
16. Zavřete správu služby AD FS.



### Konfigurovat důvěryhodné IP adresy ověřování Azure Multi-Factor Authentication s federovanými uživateli
Teď, když jsou deklarace identity na místě můžeme konfigurovat důvěryhodné IP adresy.

1. Přihlaste se do portálu pro správu Azure.
2. Na levé straně klikněte na službu Active Directory.
3. Pod oblastí adresáře klikněte na adresář, pro který chcete nastavit důvěryhodné adresy IP.
4. Ve vybraném adresáři klikněte na tlačítko Konfigurovat.
5. V části ověřování multi-factor authentication klikněte na možnost nastavení správy služeb.
6. Na stránce Nastavení služby v rámci důvěryhodných adres IP vyberte možnost **Pro žádosti od federovaných uživatelů pocházející z mého intranetu.**
![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Klikněte na Uložit.
8. Poté použití aktualizací klikněte na tlačítko Zavřít.


A to je vše! Od tohoto okamžiku musí federovaní uživatelé služeb Office 365 využívat MFA pouze pokud deklarace identity pochází z oblasti mimo podnikového intranetu.









<!--HONumber=Aug16_HO4-->


