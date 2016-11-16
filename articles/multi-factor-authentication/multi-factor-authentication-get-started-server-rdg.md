---
title: "Vzdálená plocha brány a server Azure Multi-Factor Authentication využívající protokol RADIUS"
description: "Toto je stránka ověřování Azure Multi-Factor Authentication, která vám pomůže při nasazení brány vzdálené plochy (RD) a serveru Azure Multi-Factor Authentication využívajícím protokol RADIUS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: f2354ac4-a3a7-48e5-a86d-84a9e5682b42
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: ee868c5ba1a8429a733633edbc7efaa74e512135


---
# <a name="remote-desktop-gateway-and-azure-multifactor-authentication-server-using-radius"></a>Brána vzdálené plochy Azure Multi-Factor Authentication Server pomocí protokolu RADIUS
V mnoha případech využívá brána vzdálené plochy používá NPS k ověřování uživatelů. Tento dokument popisuje požadavek směřování žádostí protokolu RADIUS ze služby brána vzdálené plochy (prostřednictvím místního serveru NPS) na server Multi-Factor Authentication.

Server Multi-Factor Authentication musí být nainstalován na samostatném serveru, který bude poté směrovat požadavky protokolu RADIUS přes proxy zpět na server NPS na serveru Brána vzdálené plochy. Když NPS ověří uživatelské jméno a heslo, vrátí reakci na server Multi-Factor Authentication, který provádí druhý faktor ověřování před vrácením výsledku brány.

## <a name="configure-the-rd-gateway"></a>Konfigurace brány VP
Brána VP musí být nakonfigurována na odeslání ověřování RADIUS na server Azure Multi-Factor Authentication. Po instalaci, konfiguraci a zprovoznění služby Brána VP přejděte do vlastností služby Brána VP. Přejděte na kartu Úložiště CAP k vzdálené ploše a změňte ji na použití centrálního serveru NPS namísto místního serveru NPS. Přidejte jeden nebo více serverů Azure Multi-Factor Authentication jako servery RADIUS a určete sdílený tajný klíč pro každý server.

## <a name="configure-nps"></a>Konfigurace NPS
Brána VP používá server NPS k odeslání požadavku protokolu RADIUS do Azure Multi-Factor Authentication. Vypršení časového limitu je nutné změnit, aby se zabránilo vypršení časového limitu služby Brána VP před dokončením ověřování Multi-Factor Authentication. Ke konfiguraci NPS použijte následující postup.

1. Na serveru NPS rozbalte nabídku klientů RADIUS a nabídku serveru v levém sloupci a klikněte na skupiny vzdálených serverů RADIUS. Přejděte do vlastností SKUPINY SERVERŮ BRÁNY TS. Upravte zobrazené servery RADIUS a přejděte na kartu Vyrovnávání zatížení. Změňte „Počet sekund bez odpovědi, než je žádost považována za zrušenou“ a „Počet sekund mezi požadavky, když je server identifikován jako nedostupný“ na 30–60 sekund. Klikněte na kartu Ověřování/účet a ověřte, zda určené porty protokolu RADIUS odpovídají portům, kterým bude server Multi-Factor Authentication naslouchat.
2. Server NPS musí být rovněž konfigurován pro příjem ověření protokolu RADIUS zpět ze serveru Azure Multi-Factor Authentication. Klikněte na klienty RADIUS v levé nabídce. Přidejte server Azure Multi-Factor Authentication jako klienta protokolu RADIUS. Zvolte popisný název a určete sdílený tajný klíč.
3. Rozbalte část zásady na levém navigačním panelu a klikněte na zásady vyžádání nového připojení. Musí obsahovat zásadu požadavku na připojení nazývanou ZÁSADA AUTORIZACE BRÁNY TS, která byla vytvořena při konfiguraci Brány VP. Tato zásada předá požadavky protokolu RADIUS serveru Multi-Factor Authentication.
4. Zkopírujte tuto zásadu a vytvořte novou. Do nové zásady přidejte podmínku, která odpovídá popisnému názvu klienta s popisným názvem nastavení v kroku 2 výše pro klienta RADIUS serveru Azure Multi-Factor Authentication. Změňte zprostředkovatele ověřování na místním počítači. Tato zásada zajistí, že při odeslání požadavku RADIUS ze serveru Azure Multi-Factor Authentication dojde k místnímu ověření místo odesílání požadavku protokolu RADIUS zpět do serveru Azure Multi-Factor Authentication, které by vytvořilo smyčku. Chcete-li zabránit vzniku smyčky, musí být tato nová zásada přikázána nad původní zásadou, které přeposílá požadavky do serveru Multi-Factor Authentication.

## <a name="configure-azure-multifactor-authentication"></a>Konfigurace Azure Multi-Factor Authentication
- - -
Server Azure Multi-Factor Authentication je nakonfigurován jako proxy server protokolu RADIUS mezi Bránou VP a serverem NPS.  Musí být nainstalován na serveru připojeném k doméně, který je oddělený od serveru Brána VP. Pomocí následujícího postupu nakonfigurujte server Azure Multi-Factor Authentication.

1. Otevřete server Azure Multi-Factor Authentication a klikněte na ikonu ověření služby RADIUS. Zaškrtněte políčko Povolit ověřování pomocí protokolu RADIUS.
2. Na kartě klienti ověřte, zda se porty shodují s tím, co je nakonfigurováno na serveru NPS a klikněte na tlačítko Přidat... . Přidejte IP adresu serveru Brány VP, název aplikace (volitelný) a sdílený tajný klíč. Sdílený tajný klíč bude muset být stejný jak na serveru Azure Multi-Factor Authentication, tak i v Bráně VP.
3. Klikněte na kartu Cíl a zvolte přepínač serveru RADIUS.
4. Klikněte na tlačítko Přidat... . Zadejte IP adresu, sdílený tajný klíč a porty serveru NPS. Pokud nepoužíváte centrální server NPS, klient protokolu RADIUS a cíl RADIUS budou stejné. Sdílený tajný klíč musí odpovídat jednomu nastavení v oddílu klienta protokolu RADIUS serveru NPS.

![Ověřování Radius](./media/multi-factor-authentication-get-started-server-rdg/radius.png)




<!--HONumber=Nov16_HO2-->


