<properties
   pageTitle="Správa adresáře pro předplatné služeb Office 365 ve službě Azure | Microsoft Azure"
   description="Správa adresáře pro předplatné služeb Office 365 pomocí služby Azure Active Directory a portálu Azure Classic"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/03/2016"
   ms.author="curtand"/>

# Správa adresáře pro předplatné služeb Office 365 ve službě Azure

Tento článek popisuje, jak spravovat adresář, který byl vytvořen pro předplatné služeb Office 365, pomocí portálu Azure Classic. Pokud se chcete přihlásit k portálu Azure Classic, musíte být správce nebo spolusprávce služeb v předplatném služby Azure. Pokud ještě nemáte předplatné Azure, můžete si pomocí tohoto odkazu zaregistrovat [bezplatnou 30denní zkušební verzi](https://azure.microsoft.com/trial/get-started-active-directory/) ještě dnes a nasadit první cloudové řešení za méně než 5 minut. Nezapomeňte použít pracovní nebo školní účet, jaký používáte k přihlášení do služeb Office 365.

Po dokončení registrace předplatného služby Azure se budete moct přihlásit k portálu Azure Classic a používat služby Azure. Pro správu stejného adresáře, který ověřuje uživatele služeb Office 365, klikněte na rozšíření Active Directory.

Pokud již máte předplatné služby Azure, proces správy dalšího adresáře je také snadný. Michael Smith má například předplatné služeb Office 365 pro doménu Contoso.com. Má také předplatné služby Azure, které si zaregistroval pomocí účtu Microsoft, msmith@hotmail.com. V tomto případě spravuje dva adresáře.

  Předplatné |  Office 365  |  Azure
  -------------- | ------------- | -------------------------------
  Zobrazované jméno |  Contoso  |     Výchozí adresář Azure Active Directory (Azure AD)
  Název domény  |  contoso.com  | msmithhotmail.onmicrosoft.com

Chce spravovat identity uživatelů v adresáři společnosti Contoso, když je přihlášený do služby Azure pomocí svého účtu Microsoft, aby mohl povolit funkce Azure AD, jako například vícefaktorové ověřování. Proces dobře znázorňuje následující diagram.

![Diagram ke správě dvou nezávislých adresářů](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

V tomto případě jsou na sobě tyto dva adresáře navzájem nezávislé.

## Správa dvou nezávislých adresářů
Aby mohl Michael Smith spravovat oba adresáře, když je přihlášený do služby Azure pomocí účtu msmith@hotmail.com, musí dokončit následující kroky:

> [AZURE.NOTE]
> Kroky je možné dokončit, jen když je uživatel přihlášený pomocí účtu Microsoft. Pokud je uživatel přihlášen pomocí pracovního nebo školního účtu, možnost **Použít existující adresář** není k dispozici. Pracovní nebo školní účet můžete ověřit pouze pomocí jeho domovského adresáře (to znamená adresáře, kde je pracovní nebo školní účet uložen a který společnost nebo škola vlastní).

1.  Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com) jako msmith@hotmail.com.
2.  Klikněte na tlačítko **Nový** > **App Services** > **Active Directory** > **Adresář** > **Vytvořit vlastní**.
3.  Klikněte na možnost Použít existující adresář a zaškrtněte možnost **Nyní mě můžete odhlásit**.
4.  Přihlaste se k portálu Azure Classic jako globální správce Contoso.onmicrosoft.com (například msmith@contoso.com).
5.  Po zobrazení výzvy **Používat adresář Contoso se službou Azure?** klikněte na tlačítko **Pokračovat**.
6.  Klikněte na tlačítko **Odhlásit**.
7.  Přihlaste se k portálu Azure Classic jako msmith@hotmail.com. Adresář společnosti Contoso a výchozí adresář se zobrazí v rozšíření služby Active Directory.

Po dokončení těchto kroků se účet msmith@hotmail.com stane globálním správcem v adresáři společnosti Contoso.

## Postup správy prostředků jako globální správce
Nyní předpokládejme, že Jane Doe potřebuje spravovat prostředky webových stránek a databází, které jsou spojené s předplatným služby Azure pro účet msmith@hotmail.com. Než to bude moct provést, musí Michael Smith dokončit tyto další kroky:

1.  Přihlásit se k [portálu Azure Classic](https://manage.windowsazure.com) pomocí účtu správce služeb pro předplatné Azure (v tomto příkladu msmith@hotmail.com).
2.  Převést předplatné do adresáře Contoso: kliknout na **Nastavení** > **Předplatné** > vybrat předplatné > **Upravit adresář** > a vybrat **Contoso (Contoso.com)**. V rámci převodu se odeberou veškeré pracovní nebo školní účty, které jsou spolusprávci předplatného.
3.  Přidat Jane Doe jako spolusprávce předplatného: kliknout na **Nastavení** > **Správci** > vybrat předplatné > **Přidat** > a zadat **JohnDoe@Contoso.com**.

## Další kroky
Další informace o vztahu mezi předplatnými a adresáři najdete v článku, [jak předplatné je spojeno s adresářem](active-directory-how-subscriptions-associated-directory.md).



<!---HONumber=Aug16_HO4-->


