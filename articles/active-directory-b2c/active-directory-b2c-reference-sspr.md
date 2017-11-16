---
title: "Azure Active Directory B2C: Resetování hesla pomocí samoobslužné služby | Microsoft Docs"
description: "Téma, který ukazuje, jak nastavit samoobslužné resetování hesla pro uživatele v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: beaf7dc6260db7509b2202c7801bcc0d2dd2c69e
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Nastavení samoobslužného resetování hesla pro uživatele
Funkce resetování hesla pomocí samoobslužné služby s uživatele (kteří zaregistrovali pro místní účty) mohou resetovat svá hesla na své vlastní. To významně snižuje zátěž vašim zaměstnancům technické podpory, zvlášť pokud vaše aplikace nemá milionům příjemcům na základě v pravidelných intervalech. V současné době podporujeme jenom pomocí ověřenou e-mailovou adresu jako metodu obnovení. Přidáme obnovení dodatečné metody (ověřené telefonní číslo, bezpečnostní otázky atd.) v budoucnu.

> [!NOTE]
> Tento článek se týká hesla pomocí samoobslužné služby resetování použít v kontextu zásady přihlášení. Pokud potřebujete zásady resetování plně přizpůsobitelná hesel volána z vaší aplikace, najdete v části [v tomto článku](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Ve výchozím nastavení, nebudou mít adresáře hesla pomocí samoobslužné služby resetování zapnutý. Chcete-li pomocí následujících kroků:

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) jako Správce předplatného. Toto je stejný pracovní nebo školní účet nebo stejný účet Microsoft, který jste použili k vytvoření adresáře.
2. Přejděte na rozšíření Active Directory v navigačním panelu na levé straně.
3. Najít váš adresář **Directory** kartě a klikněte na něj.
4. Klikněte na kartu **KONFIGUROVAT**.
5. Přejděte dolů k položce **zásady resetování hesel uživatelů** části a přepnutí **uživatele povolen pro resetování hesla** možnost k **Ano**. Všimněte si, že **alternativní e-mailovou adresu** zaškrtnutá možnost; necháte, protože se jedná.
   
    ![Samoobslužné resetování hesla](./media/active-directory-b2c-reference-sspr/sspr.png)
6. V dolní části stránky klikněte na **Uložit**. Hotovo!

K testování, pomocí funkce "Spustit nyní" na všechny zásady přihlášení, který má místní účty jako zprostředkovatele identity. Na přihlášení místní účet stránka (kde zadáte e-mailovou adresu a heslo, nebo uživatelské jméno a heslo), klikněte na tlačítko **nelze získat přístup k účtu?** ověření prostředí pro uživatele.

> [!NOTE]
> Na stránkách resetování hesla pomocí samoobslužné služby lze přizpůsobit pomocí [firemního brandingu funkce](../active-directory/customize-branding.md).
> 
> 

