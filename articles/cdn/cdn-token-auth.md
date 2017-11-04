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
ms.openlocfilehash: 88956b324a543c5347e16b1278f6b2179a3b9c24
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="securing-azure-content-delivery-network-assets-with-token-authentication"></a>Zabezpečení prostředků Azure Content Delivery Network pomocí tokenu ověřování

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Přehled

Ověření pomocí tokenu je mechanismus, který umožňuje zabránit obsluhující prostředky neoprávněným klientům Azure Content Delivery Network (CDN). Aby se zabránilo "hotlinking" obsahu, ve kterém používá jiný web, často zpráva Tabule, vaše prostředky bez oprávnění se obvykle provádí ověření tokenu. Hotlinking může mít vliv na vaše náklady na doručování obsahu. Povolením této funkce na CDN jsou požadavky ověřována edge CDN POP před CDN nabízí obsah. 

## <a name="how-it-works"></a>Jak to funguje

Ověření pomocí tokenu ověřuje, že požadavky generované jako důvěryhodný web tím, že tyto blokování kódovaný informace o žadatel požadavky tak, aby obsahovala hodnota tokenu. Obsahu obsluhovaného pro žadatele pouze v případě, že je zakódovaný informace splňuje požadavky; požadavky, jinak budou odepřeny. Požadavky můžete nastavit pomocí jednoho nebo více z následujících parametrů:

- Země: Povolit nebo odmítnout požadavky, které pocházely ze zadaného zemích.  [Seznam platných kódů.](https://msdn.microsoft.com/library/mt761717.aspx) 
- Adresa URL: Povolte jenom daný prostředek nebo cesta k požadavku.  
- Hostitele: Povolit nebo odepřít požadavky pomocí zadaní hostitelé v hlavičce žádosti.
- Odkazující server: Povolí nebo zakáže zadaný odkazující server můžete požádat.
- IP adresa: pouze povolit požadavky, které pochází z konkrétní IP adresu nebo podsíť protokolu IP.
- Protokol: Povolit nebo blokovat požadavky založené na protokolu slouží k vyžádání obsahu.
- Čas vypršení platnosti: přiřadit datum a čas období zajistit, že odkaz pouze zůstává platná po omezenou dobu.

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

    1. Zadejte jedinečný šifrovací klíč pro **primární klíč**.  Zadejte jinou pro **zálohování klíče**

        ![CDN tokenu ověřování Instalační klíč](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Nastavit parametry šifrování nástrojem šifrování (povolit nebo odepřít požadavky na základě čas vypršení platnosti, země, odkazující server, protokolu, IP adresa klienta. Můžete použít libovolnou kombinaci.)

        ![CDN šifrování nástroje](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - ES-vyprší: přiřadí čas vypršení platnosti tokenu po zadaném časovém období. Požadavky na odeslání po čas vypršení platnosti je odepřen. Tento parametr používá časové razítko systému Unix (podle počet sekund od standardní epoch 1/1/1970 00:00:00 GMT. Online nástroje můžete použít k poskytování převod mezi (běžný čas) a časem v systému Unix.) Například, pokud chcete nastavit tokenu vyprší v `12/31/2016 12:00:00 GMT`, použijte hodnotu časového razítka Unix, `1483185600`, a to takto.
    
        ![Příklad ec_expire CDN](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - Povolit adresu url ES: umožňuje přizpůsobit tokenů pro konkrétní prostředek nebo cestu. Omezuje přístup na požadavky, jejichž adresa URL začínat konkrétní relativní cesty. Můžete zadat více cest jednotlivé cesty oddělte čárkou. URL – adresy rozlišují velká a malá písmena. V závislosti na požadavek můžete nastavit jinou hodnotu poskytují různé úrovně přístupu. Níže je několik scénářů:
        
            Pokud máte adresu URL: http://www.mydomain.com/pictures/city/strasbourg.png. Vstupní hodnota najdete v části "" a jeho přístup na úrovni odpovídajícím způsobem

            1. Vstupní hodnota "/": všechny požadavky budou povoleny.
            2. Vstupní hodnota "/ obrázků": všechny tyto požadavky budou povolit
            
                - http://www.mydomain.com/Pictures.PNG
                - http://www.mydomain.com/Pictures/City/strasbourg.PNG
                - http://www.mydomain.com/picturesnew/City/strasbourgh.PNG
            3. Vstupní hodnota "/ obrázky /": pouze požadavky pro /pictures/ bude možné.
            4. Vstupní hodnota "/ pictures/city/strasbourg.png": pouze umožňuje požadavku pro tento prostředek
    
        ![Příklad ec_url_allow CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - Povolit země ES: umožňuje pouze požadavky, které pocházejí z jedné nebo více zadaný zemí. Požadavky, které pocházejí z jiných zemí budou odepřeny. Nastavit parametry pomocí kód země a každý kód země oddělíte čárkou. Například pokud chcete povolit přístup ze Spojených státech amerických a Francie, zadejte nám FR ve sloupci jako níže.  
        
        ![Příklad ec_country_allow CDN](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - Odepřít země ES: odmítne požadavky, které pochází z jedné nebo více zadaný zemí. Požadavky, které pocházejí z jiných zemí bude možné. Nastavit parametry pomocí kód země a každý kód země oddělíte čárkou. Například pokud chcete odepřít přístup ze Spojených státech amerických a Francie, zadejte nám FR ve sloupci.
    
        - ec_ref_allow: ze zadaného odkazující server jenom umožňuje požadavky. Odkazující server identifikuje adresu URL webové stránky, která je propojený s požadovaný prostředek. Odkazující server parametr hodnoty nebudou obsahovat protokol. Pro hodnotu parametru jsou povoleny následující typy vstup:
           - Název hostitele nebo název hostitele a cestu.
           - Chcete-li odkazující více servery. Chcete-li přidat více odkazujících serverů, oddělte čárkou odkazující na každý server. Pokud zadáte hodnotu odkazující server, ale odkazující server informace nebudou odeslány v požadavek z důvodu konfigurace prohlížeče, jsou ve výchozím nastavení zamítnutý tyto požadavky. 
           - Požadavky s chybějícími informacemi o odkazující server. Povolit tyto typy požadavků, zadejte text "chybí" nebo zadejte prázdnou hodnotu. 
           - Subdomény. Chcete-li povolit subdomény, zadejte hvězdičku (*). Chcete-li například povolit všechny subdomény z `consoto.com` zadejte `*.consoto.com`. 
           
          Následující příklad ukazuje vstup povolit přístup pro žádosti od `www.consoto.com`, všem dílčím doménám `consoto2.com`a požadavky s odkazující servery prázdné nebo chybí.
        
          ![Příklad ec_ref_allow CDN](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - Odepřít ref ES: odmítne požadavky z odkazující zadaný server. Viz podrobnosti a příklad v parametru "ES ref povolit".
         
        - umožňují proto – ES: pouze povoluje požadavky z zadaný protokol. Například http nebo https.
        
        ![Příklad ec_proto_allow CDN](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - ES proto – odepřít: odmítne požadavky z zadaný protokol. Například http nebo https.
    
        - Když ES: omezuje přístup k zadané žadatele IP adresu. Jsou podporovány adresy IPV4 a IPV6. Můžete zadat jednu žádost IP adresu nebo podsíť protokolu IP.
            
        ![Příklad ec_clientip CDN](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Můžete otestovat pomocí nástroje popis tokenu.

    4. Můžete také upravit typ odpovědi, který bude vrácen pro uživatele, když požadavek je odepřen. Ve výchozím nastavení používáme 403.

3. Klikněte na tlačítko **stroj pravidel** v části **HTTP velké**. Použijete na této kartě můžete definovat cesty použít funkci, povolte funkci ověření pomocí tokenu a povolit další token funkce souvisejících s ověřováním.

    - Použijte sloupec "Pokud" k definování asset nebo cestu, kterou chcete použít ověření tokenu. 
    - Kliknutím lze přidat "Tokenu ověřování" z rozevíracího seznamu funkce povolit ověření tokenu.
        
    ![Příklad ověření tokenu povolit modul CDN pravidla](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. V **stroj pravidel** se nachází několik dalších možností můžete povolit.
    
    - Token denial kód ověřování: Určuje typ odpovědi, který bude vrácen pro uživatele, když požadavek je odepřen. Pravidla nastavení tady potlačí nastavení kódu útok na kartě tokenu ověřování.
    - Ignorovat tokenu ověřování: Určuje, zda bude mít adresu URL, které slouží k ověření tokenu velká a malá písmena.
    - Parametr tokenu ověřování: Přejmenovat tokenu ověřování parametru řetězce dotazu zobrazující v požadovanou adresu URL. 
        
    ![Pravidla CDN modul příklad nastavení ověření pomocí tokenu](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Váš token zabezpečení, které je aplikace, která generuje token pro funkce na základě tokenu ověřování můžete přizpůsobit. Zdrojový kód je přístupná zde v [Githubu](https://github.com/VerizonDigital/ectoken).
Dostupné jazyky patří:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN funkce a zprostředkovatele ceny

Najdete v článku [přehled CDN](cdn-overview.md) tématu.
