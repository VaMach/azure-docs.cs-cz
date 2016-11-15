---
title: "Začínáme s poskytovatelem ověřování Azure Multi-Factor Auth | Dokumentace Microsoftu"
description: "Naučte se vytvářet poskytovatele ověřování Azure Multi-Factor Auth."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 5fa7882653d6a41376cffb0eaca78c8d05044dc2


---
# <a name="getting-started-with-an-azure-multifactor-auth-provider"></a>Začínáme s poskytovatelem ověřování Azure Multi-Factor Auth
Dvoustupňové ověřování je k dispozici ve výchozím nastavení pro globální správce, kteří mají uživatele služeb Azure Active Directory a Office 365. Pokud ale budete chtít využívat výhod [pokročilých funkcí](multi-factor-authentication-whats-next.md) měli byste si zakoupit plnou verzi Azure Multi-Factor Authentication (MFA).

> [!NOTE]
> Poskytovatel ověřování Azure Multi-Factor Auth umožňuje využít výhod funkcí poskytovaných plnou verzí Azure MFA. Je určena pro uživatele, kteří **nemají licence prostřednictvím Azure MFA, Azure AD Premium nebo EMS**.  Azure MFA, Azure AD Premium a EMS zahrnují plnou verzi Azure MFA ve výchozím nastavení.  Pokud máte licence, nepotřebujete poskytovatele Azure Multi-Factor Auth.
> 
> 

Pokud si chcete stáhnout sadu SDK, budete potřebovat poskytovatele Azure Multi-Factor Auth.

> [!IMPORTANT]
> Pokud chcete stáhnout sadu SDK, vytvořte poskytovatele Azure Multi-Factor Auth i v případě, že máte licence Azure MFA, AAD Premium nebo EMS.  Pokud vytvoříte poskytovatele Azure Multi-Factor Auth pro tento účel a máte už licence, nezapomeňte poskytovatele vytvořit podle modelu **Na povoleného uživatele**. Potom propojte poskytovatele s adresářem, ve kterém jsou uložené licence Azure MFA, Azure AD Premium nebo EMS.  Tím zajistíte, že nebudete dostávat faktury, pokud nemáte více jedinečných uživatelů využívajících sadu SDK než počet vlastněných licencí.
> 
> 

## <a name="to-create-a-multifactor-auth-provider"></a>Vytvoření poskytovatele Multi-Factor Auth
K vytvoření poskytovatele ověřování Azure Multi-Factor Auth použijte následující postup.

1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost **Active Directory**.
3. Na stránce služby Active Directory nahoře vyberte možnost **Poskytovatelé Multi-Factor Authentication**.
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. V dolní části, klikněte na tlačítko **Nový**.
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. V části Aplikační služby vyberte **Poskytovatel Multi-Factor Auth**
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Vyberte možnost **Rychle vytvořit**.
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
7. Vyplňte následující pole a vyberte možnost **Vytvořit**.
   1. **Název** – název poskytovatele Multi-Factor Auth.
   2. **Model použití** – jestli chcete povolit jednotlivé uživatele nebo platit za jednotlivá ověření. Zvolte jednu ze dvou možností:
      * Za ověření – nákupní model, který účtuje za ověření. Obvykle se používá pro scénáře, které používají Azure Multi-Factor Authentication v aplikaci zaměřené na spotřebitele.
      * Pro povolené uživatele – nákupní model, který účtuje za povoleného uživatele. Obvykle se používá pro přístup zaměstnanců k aplikacím, například Office 365. Tuto možnost vyberte, pokud máte uživatele, kteří už mají licence na Azure MFA.
   3. **Adresář** – klient Azure Active Directory, ke kterému je přidružen poskytovatel Multi-Factor Authentication. Uvědomte si následující skutečnosti:
      * Pro vytvoření poskytovatele Multi-Factor Auth nepotřebujete adresář Azure AD. Pokud plánujete používat pouze server Azure Multi-Factor Authentication nebo SDK, ponechte pole prázdné.
      * Poskytovatel Multi-Factor Auth musí být přidružen k adresáři služby Azure AD, aby mohl využívat výhod pokročilých funkcí.
      * Azure AD Connect, AAD Sync nebo DirSync jsou požadavek pouze v případě, že synchronizujete prostředí služby Active Directory v místě s adresářem služby Azure AD.  Pokud používáte pouze adresář služby Azure AD, který není synchronizován, pak tento postup není třeba.
        ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
8. Po kliknutí na tlačítko vytvořit se vytvoří poskytovatel MFA a měla by se zobrazit zpráva s oznámením: **Poskytovatel Multi-Factor Authentication byl úspěšně vytvořen**. Klikněte na tlačítko **OK**.
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)




<!--HONumber=Nov16_HO2-->


