---
title: "Azure Storage Explorer Průvodci odstraňováním potíží | Microsoft Docs"
description: "Přehled dvou ladění funkcí Azure"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: delhan
ms.openlocfilehash: e06c73c2c00b27178f8431b83b5c5a42110b6b1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Storage Explorer Průvodci odstraňováním potíží

Microsoft Azure Storage Explorer (Preview) je samostatná aplikace, která umožňuje snadno pracovat s daty Azure Storage ve Windows, systému macOS a Linux. Aplikace může připojit k účtům úložiště, které jsou hostované v Azure, suverénní Cloudy a zásobník Azure.

Tato příručka obsahuje souhrn řešení pro běžné problémy, které jsou vidět ve Storage Exploreru.

## <a name="sign-in-issues"></a>Přihlaste se problémy

Podporovány jsou pouze účty Azure Active Directory (AAD). Pokud používáte účet služby AD FS, očekává se, že přihlášení do služby Storage Explorer nebude fungovat. Než budete pokračovat, zkuste restartovat aplikaci a zjistit, zda lze dlouhodobý problémy.

### <a name="error-self-signed-certificate-in-certificate-chain"></a>Chyba: Certifikát podepsaný svým držitelem v řetězu certifikátů

Tady je několik důvodů, proč této chybě může dojít, a nejběžnější dva důvody jsou následující:

1. Aplikace je připojený prostřednictvím "transparentní proxy", což znamená, brání komunikaci přes protokol HTTPS, dešifrování ho a pak šifrování pomocí certifikát podepsaný svým držitelem serveru (například serveru vaší společnosti).

2. Běží aplikace, jako je antivirový software, který je podepsaný certifikát SSL vložení do zpráv protokolu HTTPS, které jste dostali.

Když Storage Explorer dojde jeden z problémů, můžete již nebude vědět, jestli je úmyslně přijatou zprávu protokolu HTTPS. Pokud máte kopie certifikátu podepsaného svým držitelem, můžete je nechat Storage Explorer důvěřujete mu. Pokud si nejste jisti kdo je vložení certifikát, použijte následující postup ji najít:

1. Instalace otevřete SSL

    - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (jakékoli světla verze by mělo být dostatečné)

    - Mac a Linux: by měly být zahrnuty s operačním systémem

2. Spustit otevřete SSL

    - Windows: otevřete instalační adresář, klikněte na **/bin/**a potom dvakrát klikněte na **openssl.exe**.
    - Mac a Linux: Spusťte **openssl** z terminálu.

3. Spuštění s_client - showcerts-připojení microsoft.com:443

4. Hledejte certifikáty podepsané svým držitelem. Pokud si nejste jistí, které jsou podepsané svým držitelem, vyhledejte kdekoli subjektu ("s:") a vystavitele ("i") jsou stejné.

5. Po nalezení všechny certifikáty podepsané svým držitelem pro každé z nich, zkopírujte a vložte všechno z a to včetně **---BEGIN CERTIFICATE---** k **---END CERTIFICATE---** do nového souboru .cer.

6. Otevřete Storage Explorer, klikněte na **upravit** > **certifikáty SSL** > **importu certifikátů**a potom pomocí nástroje pro výběr souborů najít, vyberte a otevřete .cer soubory, které jste vytvořili.

Pokud nenajdete žádné certifikáty podepsané svým držitelem pomocí výše uvedené kroky, kontaktujte nás pomocí nástroje zpětnou vazbu o další pomoc.

### <a name="unable-to-retrieve-subscriptions"></a>Nelze načíst předplatná

Pokud jste se nepodařilo načíst vašich předplatných, po úspěšném přihlášení, použijte následující postup řešení tohoto problému:

- Ověřte, že má váš účet přístup k předplatným po přihlášení k portálu Azure.

- Ujistěte se, že jste se zaregistrovali pomocí správné prostředí (Azure, Azure China, Azure v Německu, Azure US Government nebo vlastní prostředí nebo Azure zásobníku).

- Pokud se nacházíte za proxy, ujistěte se, že jste nakonfigurovali proxy Storage Explorer správně.

- Zkuste odebrat a nové přidání účtu.

- Pokuste se odstranit následující soubory z kořenového adresáře (tedy C:\Users\ContosoUser) a potom znovu přidat účet:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Kukátko nástrojů pro vývojáře, (podle stisknutím klávesy F12), pokud se přihlašujete všechny chybové zprávy:

![Nástroje pro vývojáře](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a>Nelze zobrazit stránku ověřování

Pokud nelze zobrazit stránku ověřování, použijte následující postup řešení tohoto problému:

- V závislosti na rychlosti připojení může trvat nějakou dobu stránku přihlášení a načíst, počkejte před jeho zavřením dialogové okno ověřování alespoň jednu minutu.

- Pokud se nacházíte za proxy, ujistěte se, že jste nakonfigurovali proxy Storage Explorer správně.

- Zobrazení konzole pro vývojáře stisknutím klávesy F12. Podívejte se na odpovědi z konzole pro vývojáře a zobrazit, zda můžete najít všechny potvrzením pro důvod, proč ověřování nepracuje.

### <a name="cannot-remove-account"></a>Nelze odebrat účet

Pokud se nepodařilo odebrat účet, nebo pokud znovu ověřit propojení nemá žádný, použijte následující postup řešení tohoto problému:

- Pokuste se odstranit následující soubory z kořenového adresáře a pak nové přidání účtu:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Pokud chcete odebrat SAS připojená prostředky úložiště, odstraňte následující soubory:

    - %AppData%/StorageExplorer složky pro Windows

    - /Users/ < vaše_jméno >/knihovny/Express podporu nebo StorageExplorer pro Mac

    - ~/.config/StorageExplorer pro Linux

> [!NOTE]
>  Budete muset znovu zadat všechny přihlašovací údaje, pokud odstraníte tyto soubory.

## <a name="proxy-issues"></a>Problémy s proxy

První zajistěte, aby byla následující informace, které jste zadali správně:

- Adresa URL proxy serveru a číslo portu

- Uživatelské jméno a heslo, pokud to vyžaduje proxy server

### <a name="common-solutions"></a>Běžná řešení

Pokud stále dochází k problémům, použijte následující postup řešení potíží s je:

- Pokud se můžete připojit k Internetu bez použití proxy, ověřte, že Storage Explorer funguje bez povolené nastavení proxy serveru. Pokud je to tento případ, může být problém se vaše nastavení proxy serveru. Práce se na správce serveru proxy a identifikovat problémy.

- Ověřte, že jiných aplikací pomocí proxy serveru fungovat podle očekávání.

- Ověřte, zda se můžete připojit k portálu Microsoft Azure pomocí webového prohlížeče

- Ověřte, zda se zobrazila odpovědí z koncových bodů služby. Zadejte jednu z váš koncový bod adresy URL do prohlížeče. Pokud se můžete připojit, měli byste obdržet InvalidQueryParameterValue nebo podobné odpovědi ve formátu XML.

- Pokud někdo jiný používá také Storage Explorer proxy serveru, ověřte, zda se můžete připojit. Pokud se můžete připojit, možná budete muset obrátit na správce serveru proxy.

### <a name="tools-for-diagnosing-issues"></a>Nástroje pro diagnostiku problémů

Pokud máte síťové nástroje, například aplikaci Fiddler pro Windows, bude pravděpodobně možné diagnostikovat problémy následujícím způsobem:

- Pokud máte fungovat prostřednictvím proxy, možná budete muset nakonfigurovat vaše síťové nástroje pro připojení prostřednictvím proxy serveru.

- Zkontrolujte číslo portu používané nástrojem pro vaší sítě.

- Zadejte adresu URL místního hostitele a číslo portu sítě nástroj jako nastavení proxy serveru v Storage Explorer. Když toto dokončíte správně, vaše síťové nástroje spustí se protokolování síťové požadavky provedené Průzkumník úložišť pro koncové body služby a správu. Zadejte například https://cawablobgrs.blob.core.windows.net/ pro koncový bod služby objektů blob v prohlížeči a zobrazí se odpověď se podobá následující text, který naznačuje prostředek existuje, i když nelze k němu přístup.

![Ukázka kódu](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Obraťte se na správce serveru proxy

Pokud vaše nastavení proxy serveru jsou správné, možná budete muset obrátit na správce serveru proxy a

- Ujistěte se, že proxy neblokovala přenosy na koncové body Azure pro správu nebo prostředků.

- Zkontrolujte protokol ověřování používá proxy server. Storage Explorer v současné době nepodporuje proxy protokolu NTLM.

## <a name="unable-to-retrieve-children-error-message"></a>Chybová zpráva "Nelze načíst, děti"

Pokud jste připojeni k Azure prostřednictvím proxy serveru, ověřte správnost nastavení proxy serveru. Pokud byly udělen přístup k prostředku z vlastník předplatného nebo účet, ověřte, zda přečetli nebo seznamu oprávnění pro tento prostředek.

### <a name="issues-with-sas-url"></a>Problémy s adresou URL SAS
Pokud se připojujete ke službě pomocí adresy URL SAS a hlásí tuto chybu:

- Ověřte, že adresa URL poskytuje potřebná oprávnění ke čtení nebo seznamu prostředků.

- Ověřte, zda nevypršela platnost adresu URL.

- Pokud SAS adresa URL je založená na zásadách přístupu, ověřte, že nebyl odvolaný zásady přístupu.

## <a name="next-steps"></a>Další kroky

Pokud žádná z řešení fungovat pro vás, odešlete svůj problém prostřednictvím nástroje zpětnou vazbu s e-mailu a tolik podrobnosti o problému zahrnuty jako je možné, tak, aby budeme vás moc kontaktovat o nápravě problému.

Chcete-li to provést, klikněte na tlačítko **pomoci** nabídce a pak klikněte na tlačítko **odeslat zpětnou vazbu**.

![Váš názor](./media/storage-explorer-troubleshooting/4022503_en_1.png)
