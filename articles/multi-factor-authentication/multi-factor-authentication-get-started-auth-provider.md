<properties 
    pageTitle="Začínáme s poskytovatelem ověřování Microsoft Azure Multi-Factor Auth" 
    description="Naučte se vytvářet poskytovatele ověřování Azure Multi-Factor Auth." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>



# Začínáme s poskytovatelem ověřování Azure Multi-Factor Auth
Ověřování Multi-Factor authentication je k dispozici ve výchozím nastavení pro globální správce, kteří mají uživatele služeb Azure Active Directory a Office 365. Pokud však budete chtít využívat výhod [pokročilých funkcí](multi-factor-authentication-whats-next.md), pak je nutné zakoupit plnou verzi Azure MFA. 

> [AZURE.NOTE]  Poskytovatel ověřování Azure Multi-Factor Auth umožňuje využít výhod funkcí poskytovaných plnou verzí Azure MFA. Je určena pro uživatele, kteří **nemají licence prostřednictvím Azure MFA, Azure AD Premium nebo EMS**.  Azure MFA, Azure AD Premium a EMS zahrnují plnou verzi Azure MFA ve výchozím nastavení.  Pokud máte licence, pak nepotřebujete poskytovatele Azure Multi-Factor Auth. 

Poskytovatel vícefaktorového ověřování Azure je vyžadován, chcete-li stáhnout sadu SDK.

> [AZURE.IMPORTANT]  Chcete-li stáhnout sadu SDK, je třeba vytvořit poskytovatele vícefaktorového ověřování Azure i v případě, že máte licence Azure MFA, AAD Premium nebo EMS.  Pokud vytvoříte vícefaktorového poskytovatele ověřování Azure pro tento účel a již máte licence, je nutné vytvořit zprostředkovatele s modelem **Podle povoleného uživatele** a propojit zprostředkovatele do adresáře, který obsahuje licence Azure MFA, Azure AD Premium nebo EMS.  Tím bude zajištěno, že nebudete dostávat faktury, pokud nemáte více jedinečných uživatelů využívajících sadu SDK než počet vlastněných licencí.
 
K vytvoření poskytovatele ověřování Azure Multi-Factor Auth použijte následující postup.

## Vytvoření poskytovatele Multi-Factor Auth
--------------------------------------------------------------------------------

1. Přihlaste se jako správce do **portálu Azure Classic**.
2. Vlevo vyberte možnost **Active Directory**.
3. Na stránce služby Active Directory nahoře vyberte možnost **Poskytovatelé Multi-Factor Authentication**.
![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. V dolní části, klikněte na tlačítko **Nový**.
![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. V části **aplikační služby** vyberte možnost **Poskytovatel Multi-Factor Auth**
![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Vyberte možnost **Rychle vytvořit**.
![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Vyplňte následující pole a vyberte možnost **Vytvořit**.
    1. **Název** – název poskytovatele Multi-Factor Auth.
    2. **Použití modelu** – model využití poskytovatele Multi-Factor Authentication.
        - Za ověření – nákupní model, který účtuje za ověření. Obvykle se používá pro scénáře, které používají Azure Multi-Factor Authentication v aplikaci zaměřené na spotřebitele.
        - Pro povolené uživatele – nákupní model, který účtuje za povoleného uživatele. Obvykle se používá pro přístup zaměstnanců k aplikacím, například Office 365.
    2. **Adresář** – klient Azure Active Directory, ke kterému je přidružen poskytovatel Multi-Factor Authentication. Uvědomte si následující skutečnosti:
        - Pro vytvoření poskytovatele Multi-Factor Auth nepotřebujete adresář Azure AD.  Pokud plánujete používat pouze server Azure Multi-Factor Authentication nebo SDK, jednoduše pole ponechte prázdné.
        - Poskytovatel Multi-Factor Auth musí být přidružen k adresáři služby Azure AD, aby mohl využívat výhod pokročilých funkcí.
        - Azure AD Connect, AAD Sync nebo DirSync jsou požadavek pouze v případě, že synchronizujete prostředí služby Active Directory v místě s adresářem služby Azure AD.  Pokud používáte pouze adresář služby Azure AD, který není synchronizován, pak tento postup není třeba. 
![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)    
5. Po kliknutí na tlačítko vytvořit se vytvoří poskytovatel MFA a měla by se zobrazit zpráva s oznámením: **Poskytovatel Multi-Factor Authentication byl úspěšně vytvořen**. Klikněte na tlačítko **OK**.
![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)    



<!---HONumber=Aug16_HO4-->


