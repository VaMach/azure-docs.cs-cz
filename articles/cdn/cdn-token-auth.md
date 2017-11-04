---
title: "Zabezpečení prostředků Azure CDN pomocí tokenu ověřování | Microsoft Docs"
description: "Zabezpečený přístup k prostředkům vaší Azure CDN pomocí ověření tokenu."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/11/2016
ms.author: mezha
ms.openlocfilehash: b1bcaf14e9805afa82c765092b62201f58c7cbc4
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Zabezpečení prostředků Azure Content Delivery Network pomocí tokenu ověřování

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Přehled

Ověření pomocí tokenu je mechanismus, který umožňuje zabránit obsluhující prostředky neoprávněným klientům Azure Content Delivery Network (CDN). Aby se zabránilo "hotlinking" obsahu, ve kterém používá jiný web, často zpráva Tabule, vaše prostředky bez oprávnění se obvykle provádí ověření tokenu. Hotlinking může mít vliv na vaše náklady na doručování obsahu. Povolením této funkce na CDN jsou požadavky ověřována edge CDN POP před CDN nabízí obsah. 

## <a name="how-it-works"></a>Jak to funguje

Ověření pomocí tokenu ověřuje, že požadavky generované jako důvěryhodný web tím, že tyto blokování kódovaný informace o žadatel požadavky tak, aby obsahovala hodnota tokenu. Obsahu obsluhovaného pro žadatele pouze v případě, že je zakódovaný informace splňuje požadavky; požadavky, jinak budou odepřeny. Požadavky můžete nastavit pomocí jednoho nebo více z následujících parametrů:

- Země: Povolit nebo odmítnout požadavky, které pocházejí z uvedených zemích jejich [kód země](https://msdn.microsoft.com/library/mt761717.aspx).
- Adresa URL: Povolit pouze požadavky, které odpovídají daný prostředek nebo cestu.
- Hostitel: Povolit nebo odmítnout požadavky, které používají zadaní hostitelé v hlavičce žádosti.
- Odkazující server: Povolit nebo odepřít žádost ze zadaného odkazující server.
- IP adresa: Povolit pouze požadavky, které pochází z konkrétní IP adresu nebo podsíť protokolu IP.
- Protokol: Povolit nebo odepřít požadavky založené na protokolu slouží k vyžádání obsahu.
- Čas vypršení platnosti: přiřadit datum a čas období zajistit, aby zůstala odkaz platný pouze po omezenou dobu.

Další informace najdete v tématu příklady podrobnou konfiguraci pro každý parametr [nastavení ověření pomocí tokenu](#setting-up-token-authentication).

Po vytvoření zašifrovaného tokenu, připojte ji jako řetězec dotazu na konec adresy URL cesta k souboru. Například, `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.

## <a name="reference-architecture"></a>Referenční architektura

Následující diagram pracovní postup popisuje, jak CDN používá ověření pomocí tokenu pro práci s webovou aplikací.

![Pracovní postup ověření pomocí tokenu CDN](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Ověření tokenu logiky na koncový bod CDN
    
Následující vývojový diagram popisuje, jak Azure CDN ověří požadavek klienta po ověření tokenu je nakonfigurovaná na koncový bod CDN.

![CDN token ověřovací logiku](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Nastavení ověření pomocí tokenu

1. Z [portál Azure](https://portal.azure.com), přejděte na svůj profil CDN a pak klikněte na tlačítko **spravovat** ke spuštění na doplňkovém portálu.

    ![Tlačítko Spravovat profil CDN](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Pozastavte ukazatel myši nad **HTTP velké**a potom klikněte na **tokenu ověřování** v plovoucím panelem. Nastavíte šifrovací klíč a parametry šifrování na této kartě.

    1. Zadejte jedinečný šifrovací klíč pro **primární klíč** a druhý pro zadejte **zálohování klíče**.

        ![CDN tokenu ověřování Instalační klíč](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Nastavte parametry šifrování pomocí nástroje šifrovat. Pomocí nástroje šifrování můžete povolit nebo odepřít požadavků podle čas vypršení platnosti, země, odkazující server, protokol a IP adresa klienta (v libovolné kombinace).

        ![CDN šifrování nástroje](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

       - ec_expire: přiřadí čas vypršení platnosti tokenu, po jejímž uplynutí vyprší platnost tokenu. Požadavky na odeslání po čas vypršení platnosti je odepřen. Tento parametr používá časové razítko systému Unix, která je založena na počtu sekund od standardní epocha `1/1/1970 00:00:00 GMT`. (Online nástroje můžete pro převod mezi (běžný čas) a Unix času.) Například, pokud chcete nastavit tokenu vyprší v `12/31/2016 12:00:00 GMT`, použijte hodnotu časového razítka Unix, `1483185600`, a to takto. 
    
         ![Příklad ec_expire CDN](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
       - ec_url_allow: umožňuje přizpůsobit tokenů pro konkrétní prostředek nebo cestu. Omezuje přístup na požadavky, jejichž adresa URL začínat konkrétní relativní cesty. URL – adresy rozlišují velká a malá písmena. Zadejte více cest a oddělit každou z cest oddělujte čárkami. V závislosti na požadavcích můžete nastavit různé hodnoty poskytují různé úrovně přístupu. 
        
          Například pro adresu URL `http://www.mydomain.com/pictures/city/strasbourg.png`, nastavte vstupní hodnoty a její úroveň přístupu takto:

           - Vstupní hodnota `"/"`: jsou povoleny všechny požadavky
           - Vstupní hodnota `"/pictures`. Jsou povoleny následující požadavky:
              - `http://www.mydomain.com/pictures.png`
              - `http://www.mydomain.com/pictures/city/strasbourg.png`
              - `http://www.mydomain.com/picturesnew/city/strasbourgh.png`
            - Vstupní hodnota `/pictures/`: pouze požadavky pro `/pictures/` jsou povoleny. Například, `http://www.mydomain.com/pictures/city/strasbourg.png`.
            - Vstupní hodnota `/pictures/city/strasbourg.png`: jsou povoleny pouze požadavky pro tento konkrétní prostředek.
    
          ![Příklad ec_url_allow CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
       - ec_country_allow: umožňuje pouze požadavky, které pocházejí z jedné nebo více zadaný zemí. Požadavky, které pocházejí z jiných zemí odmítnuty. Kódy zemí a jednotlivé každé z nich oddělujte čárkami. Například pokud chcete povolit přístup ze Spojených státech amerických a Francie, zadejte nám FR v poli následujícím způsobem.  
        
           ![Příklad ec_country_allow CDN](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

       - ec_country_deny: odmítne požadavky, které pocházejí z jedné nebo více zadaný zemí. Jsou povoleny požadavky, které pocházejí z dalších zemích. Kódy zemí a jednotlivé každé z nich oddělujte čárkami. Například pokud chcete odepřít přístup z USA a Francie, zadejte nám FR v poli.
    
       - ec_ref_allow: umožňuje pouze požadavky z odkazující zadaný server. Odkazující server identifikuje adresu URL webové stránky, která je propojený s požadovaný prostředek. Odkazující server parametr hodnoty nebudou obsahovat protokol. Pro hodnotu parametru jsou povoleny následující typy vstup:
           - Název hostitele nebo název hostitele a cestu.
           - Chcete-li odkazující více servery. Chcete-li přidat více odkazujících serverů, oddělte čárkou odkazující na každý server. Pokud zadáte hodnotu odkazující server, ale odkazující server informace nebudou odeslány v požadavek z důvodu konfigurace prohlížeče, jsou ve výchozím nastavení zamítnutý tyto požadavky. 
           - Požadavky s chybějícími informacemi o odkazující server. Povolit tyto typy požadavků, zadejte text "chybí" nebo zadejte prázdnou hodnotu. 
           - Subdomény. Chcete-li povolit subdomény, zadejte hvězdičku (*). Chcete-li například povolit všechny subdomény z `consoto.com` zadejte `*.consoto.com`. 
           
          Následující příklad ukazuje vstup povolit přístup pro žádosti od `www.consoto.com`, všem dílčím doménám `consoto2.com`a požadavky s odkazující servery prázdné nebo chybí.
        
          ![Příklad ec_ref_allow CDN](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
       - ec_ref_deny: odmítne požadavky z odkazující zadaný server. Implementace je stejný jako parametr ec_ref_allow.
         
       - ec_proto_allow: pouze povoluje požadavky z zadaný protokol. Například HTTP nebo HTTPS.
        
         ![Příklad ec_proto_allow CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
       - ec_proto_deny: odmítne požadavky z zadaný protokol. Například HTTP nebo HTTPS.
    
       - ec_clientip: omezuje přístup k zadané žadatele IP adresu. Jsou podporovány adresy IPV4 a IPV6. Můžete zadat na jednu žádost IP adresu nebo podsíť protokolu IP.
            
         ![Příklad ec_clientip CDN](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Volitelně můžete otestujte pomocí nástroje popis tokenu.

    4. Volitelně můžete upravte typ odpovědi, která je vrácena, pokud požadavek je odepřen. Ve výchozím nastavení je kód odpovědi 403 použít.

3. V části **HTTP velké**, klikněte na tlačítko **stroj pravidel**. Stroj pravidel používáte k definování cesty použít funkci, povolte funkci ověření pomocí tokenu a povolit další token funkce souvisejících s ověřováním.

    1. Použití **Pokud** sloupec k definování asset nebo cestu, pro který chcete použít ověření pomocí tokenu. 
    2. Pokud chcete povolit ověřování tokenem, vyberte **tokenu ověřování** z **funkce** rozevíracího seznamu.
        
    ![Příklad ověření tokenu povolit modul CDN pravidla](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. V modulu pravidla jsou několik dalších možností, které můžete povolit:
    
    - **Token Auth Denial kód**: Určuje typ odpovědi, která se vrátí pro uživatele, když požadavek je odepřen. Pravidla nastavená přepsání nastavení denial kódu v **tokenu ověřování** kartě.
    - **Token Auth Ignorovat adresy URL případ**: Určuje, zda je adresa URL použitá k ověření tokenu malá a velká písmena.
    - **Token Auth parametr**: přejmenuje tokenu ověřování parametru řetězce dotazu, který se zobrazí v požadovanou adresu URL. 
        
    ![Pravidla CDN modul příklad nastavení ověření pomocí tokenu](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Váš token, který je aplikace, která generuje token pro funkce na základě tokenu ověřování můžete přizpůsobit. Zdrojový kód je přístupná v [Githubu](https://github.com/VerizonDigital/ectoken).
Dostupné jazyky patří:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN funkce a zprostředkovatele ceny

Informace najdete v tématu [přehled CDN](cdn-overview.md).
