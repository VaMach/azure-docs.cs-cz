---
title: "Řešení potíží s Azure Backup Server | Microsoft Docs"
description: "Řešení potíží s instalací, registraci Azure Backup Server a zálohování a obnovení úloh aplikace"
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;
ms.openlocfilehash: 696f86f616575364bb65021260daf0c8458fc4e9
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="troubleshoot-azure-backup-server"></a>Odstraňování potíží Azure Backup Serveru

Řešení potíží s chyb zjištěných při pomocí serveru Azure Backup informace uvedené v následující tabulce.

## <a name="invalid-vault-credentials-provided"></a>Neplatné přihlašovací údaje úložiště 

Postupujte [] (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues) Chcete-li vyřešit tento problém.

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-server"></a>Operace agenta se nezdařila kvůli chybě komunikace se službou DPM Agent Coordinator na serveru 

Postupujte [] (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues) Chcete-li vyřešit tento problém.

## <a name="setup-could-not-update-registry-metadata"></a>Instalační program nemohl aktualizovat metadata registru

Postupujte [] (https://docs.microsoft.com/en-us/azure/backup/backup-azure-mabs-troubleshoot#installation-issues) Chcete-li vyřešit tento problém.


## <a name="installation-issues"></a>Problémy instalace

| Operace | Podrobnosti o chybě | Alternativní řešení |
|-----------|---------------|------------|
|Instalace | Instalační program nemohl aktualizovat metadata registru. Toto selhání aktualizace může vést k přes využití spotřebu úložiště. Chcete-li se vyhnout této aktualizujte položku registru ořezávání odolný systém souborů. | Upravte klíč registru, SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim. Nastavte hodnotu Dword na 1. |
|Instalace | Instalační program nemohl aktualizovat metadata registru. Toto selhání aktualizace může vést k přes využití spotřebu úložiště. Chcete-li se vyhnout této aktualizujte položku registru SnapOptimization svazku. | Vytvořte klíč registru, SOFTWARE\Microsoft Manager\Configuration\VolSnapOptimization\WriteIds ochrany dat, s hodnotou prázdný řetězec. |

## <a name="registration-and-agent-related-issues"></a>Problémy související s registrací a Agent
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Registrace do trezoru | Neplatné přihlašovací údaje úložiště. Soubor je buď poškozený nebo nemá mít poslední přihlašovací údaje přísluší službě obnovení | Chcete-li vyřešit tuto chybu: <ol><li> Stáhnout nejnovější soubor s přihlašovacími údaji trezoru a zkuste to znovu <br>(NEBO)</li> <li> Pokud výše uvedené akce nefunguje, zkuste stáhnout přihlašovací údaje jiného místního adresáře nebo vytvořit nový trezor <br>(NEBO)</li> <li> Zkuste aktualizovat datum a čas nastavení, jak je uvedeno v [tomto blogu](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/) <br>(NEBO)</li> <li> Zkontrolujte, zda c:\windows\temp má více než 65000 soubory. Přesunutí zastaralých souborů do jiného umístění nebo odstranění položek ve složce Temp <br>(NEBO)</li> <li> Zkontrolujte stav certifikátů <br> a. Otevřete "Správa certifikátů počítače" (v Ovládacích panelech) <br> b. Rozbalte uzel "Osobní" a jeho podřízený uzel "Certifikáty" <br> c.  Odebrat certifikát "Nástroje systému Windows Azure" <br> d. Opakujte registraci v klientovi Azure Backup <br> (NEBO) </li> <li> Zkontrolujte, zda všechna zásad skupiny, je na místě </li></ol> |
| Nabízení agentů do chráněných serverů | Operace agenta se nezdařila kvůli chybě komunikace se službou koordinátora agenta DPM na \<ServerName > | **Pokud nefunguje Doporučená akce uvedené v produktu**, <ol><li> Pokud se připojujete počítači z nedůvěryhodné domény, postupujte podle [tyto](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx) kroky <br> (NEBO) </li><li> Pokud se připojujete počítače z důvěryhodné domény, řešení problémů pomocí kroků uvedených v [tomto blogu](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/) <br>(NEBO)</li><li> Zkuste zakázat antivirový v rámci řešení potíží. Pokud tento problém řeší, upravte nastavení antivirový dle pokynů [v tomto článku] (https://technet.microsoft.com/library/hh757911.aspx)</li></ol> |
| Nabízení agentů do chráněných serverů | Přihlašovací údaje pro server jsou neplatné | **Pokud nefunguje Doporučená akce uvedené v produktu**, <br> Pokuste se ručně nainstalovat agenta ochrany na provozním serveru zadané v [v tomto článku](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual)|
| Azure Backup Agent se nemohl připojit ke službě Azure Backup (ID: 100050) | Azure Backup Agent se nemohl připojit ke službě Azure Backup. | **Pokud nefunguje Doporučená akce uvedené v produktu**, <br>1. Spusťte následující příkaz z řádku, zvýšené nástroje psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe" otevře se okno internet Exploreru. <br/> 2. Přejděte na Nástroje -> Možnosti Internetu -> připojení -> Nastavení místní sítě. <br/> 3. Ověřte nastavení proxy serveru pro účet System. Nastavení proxy serveru IP adresy a portu. <br/> 4. Zavřete Internet Explorer.|
| Instalace služby Azure Backup Agent se nezdařila | Instalace služeb zotavení Microsoft Azure se nezdařila. Všechny změny provedené při instalaci služeb zotavení Microsoft Azure v systému byly vráceny zpět. (ID: 4024) | Ručně nainstalovat agenta Azure


## <a name="configuring-protection-group"></a>Konfigurace skupiny ochrany
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Konfigurace skupin ochrany | Aplikace DPM se nepodařil výčet součásti aplikace v chráněném počítači (chráněný počítač název) | Na obrazovce konfigurace skupiny ochrany uživatelského rozhraní na příslušné úrovni zdroj dat nebo součásti klikněte na tlačítko 'aktualizovat. |
| Konfigurace skupin ochrany | Ochranu nelze nakonfigurovat | Pokud na chráněném serveru SQL server, zkontrolujte, zda oprávnění role správce systému pro systémový účet (NTAuthority\System) v chráněném počítači byly zadány jak je uvedeno v [v tomto článku](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx)
| Konfigurace skupin ochrany | Není dostatek volného místa ve fondu úložiště pro tuto skupinu ochrany | Disky, které jsou přidány do fondu úložiště [nesmí obsahovat oddíl](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Odstraňte všechny existující svazky na discích a poté jej přidejte do fondu úložiště|
| Změny zásad |Zásady zálohování se nepodařilo upravit. Chyba: Aktuální operace selhala kvůli vnitřní chybě služby [0x29834]. Zkuste operaci po nějaké době zopakovat. Pokud se problém opakuje, obraťte se prosím na podporu Microsoftu. |**Příčina:**<br/>Tato chyba se dodává při nastavení zabezpečení povoleno, pokusíte snížit rozsah uchování nižší než minimální výše uvedené hodnoty a jsou v nepodporované verzi (nižší než verze MAB 2.0.9052 a Azure Backup server aktualizace 1). <br/>**Doporučená akce:**<br/> V takovém případě byste měli nastavit dobu uchování výše zadaná minimální doba uchovávání období (sedm dní denně, čtyři týdny týdně, tři týdny, měsíčně nebo jeden rok pro roční) pokračujte aktualizace související se zásadami. Volitelně žádoucí bude aktualizovat agenta zálohování a Azure Backup Server můžete využít všech aktualizací zabezpečení. |

## <a name="backup"></a>Zálohování
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Zálohování | Replika je nekonzistentní | Zkontrolujte, zda tento kontrola ve skupině ochrany Průvodce je zapnutá možnost automatické konzistence. Najít další podrobnosti o příčinách nekonzistence repliky a relevantní návrhy [sem](https://technet.microsoft.com/library/cc161593.aspx) <br> <ol><li> V případě zálohování stavu systému nebo BMR zkontrolujte, zda je nainstalována zálohování serveru Windows či není na chráněný Server </li><li> Zkontrolujte místo související problémy na úložiště aplikace DPM na serveru aplikace DPM nebo MABS fond a přidělit úložiště podle potřeby </li><li> Zkontrolujte stav služby Stínová kopie svazku na chráněném serveru. Pokud je v zakázaném stavu nastavte ji chcete spustit ručně a spusťte službu na serveru. Poté přejděte zpět do konzoly aplikace DPM nebo MABS a spustí synchronizaci s kontrolou konzistence.</li></ol>|
| Zálohování | Došlo k neočekávané chybě při provádění úlohy, zařízení není připraveno | **Pokud nefunguje Doporučená akce uvedené v produktu**, <br> <ol><li>Nastavit prostor úložiště stínové kopie na neomezený na položky ve skupině ochrany a proveďte kontrolu konzistence <br></li> (NEBO) <li>Odstraňte existující skupiny ochrany a vytvořit více nové – jednu s jednotlivé položky v něm</li></ol> |
| Zálohování | Pokud zálohujete pouze stav systému, ověřte, pokud není dostatek volného místa v chráněném počítači k uložení zálohy stav systému | <ol><li>Ověřte, zda je nainstalován WSB na chráněném počítači</li><li>Ověřte, zda je dostatek místa pro stav systému k dispozici v chráněném počítači: nejjednodušší způsob je přejděte do chráněného počítače, otevřete WSB a klikněte na tlačítko prostřednictvím výběr a vyberte úplné obnovení systému. Uživatelské rozhraní se zprávou, kolik místa je požadované pro tento. Otevřete WSB -> Místní zálohování-zálohování > plán -> Vybrat konfiguraci zálohování -> celého serveru (se zobrazí velikost). Použijte pro ověření této velikosti.</li></ol>
| Zálohování | Vytvoření bodu obnovení online se nezdařilo | Pokud je zobrazeno chybová zpráva "Windows Azure Backup Agent nemohl vytvořit snímek vybraného svazku", zkuste to prosím zvětšení místa ve svazku bodu obnovení a repliky.
| Zálohování | Vytvoření bodu obnovení online se nezdařilo | Pokud chybová zpráva říká "Windows Azure Backup Agent se nemůže připojit ke službě obengine", ověřte, zda obengine uvedena v seznamu služeb spuštěných v počítači. Pokud není spuštěná služba obengine uvedena příkazem "net start obengine uvedena" zahájíte službě obengine.
| Zálohování | Vytvoření bodu obnovení online se nezdařilo | Pokud chybová zpráva říká "šifrovací přístupové heslo pro tento server není nastaven. Konfigurujte šifrovací přístupové heslo", zkuste konfigurace šifrovací přístupové heslo. Pokud se nezdaří, <br> <ol><li>Zkontrolujte, zda pomocné umístění existuje, nebo ne. Umístění uvedený v registru HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config s názvem "ScratchLocation" by měla existovat.</li><li> Pokud pomocné umístění existuje, pokuste se znovu registraci pomocí staré heslo. **Vždy, když konfigurujete šifrovací přístupové heslo, uložte jej na bezpečném místě**</li><ol>
| Zálohování | Selhání zálohování pro úplné obnovení systému | Pokud je velikost BMR velký, opakujte po přesunutí některých aplikací souborů na jednotce operačního systému |
| Zálohování | Znovu ochranu virtuálního počítače VMWare na nový server MABS nezobrazuje jako dostupné pro přidání. | Na staré vyřazeno MABS serveru jsou odkazoval vlastnosti VMWare. K vyřešení tohoto problému: VCenter (SC-VMM ekvivalentní), přejděte na kartu "souhrnné informace a potom, vlastní atributy'.  Odstraňte starý název serveru MABS z hodnoty 'DPMServer'.  Přejděte zpět na nový server MABS a upravit stránky  Po použití tlačítko 'Aktualizovat', zobrazí se zaškrtávací políčko jako dostupný pro přidání do ochrany virtuálního počítače. |
| Zálohování | Chyba při přístupu k soubory nebo sdílené složky | Zkuste upravit nastavení antivirové ochrany jako navrhované [sem](https://technet.microsoft.com/library/hh757911.aspx)|
| Zálohování | Úlohu vytvoření bodu obnovení online pro virtuální počítač VMware selže. Aplikaci DPM došlo při pokusu o získání informací o vlastnost ChangeTracking chyba z VMware. ErrorCode – FileFaultFault (ID 33621) |  1. Resetování ctk VMware, ovlivněných virtuální počítače <br/> 2. Zkontrolujte, zda nezávislé disk není v místě VMWare <br/> 3. Ukončete ochranu pro ovlivněné virtuální počítače a znovu ochranu pomocí tlačítka Aktualizovat <br/> 4. Spuštění kopie pro ovlivněné virtuální počítače|


## <a name="change-passphrase"></a>Změnit heslo
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Změnit heslo |Zabezpečení zadán kód PIN není správný. Zadejte správný kód zabezpečení PIN k dokončení této operace. |**Příčina:**<br/> Tato chyba se dodává při zadání kódu PIN zabezpečení neplatná nebo vypršela její platnost při provádění kritické operace (jako jsou změnit heslo). <br/>**Doporučená akce:**<br/> K dokončení operace, je nutné zadat platný kód PIN zabezpečení. Pokud chcete získat kód PIN, přihlaste se k portálu Azure a přejděte do trezoru služeb zotavení > Nastavení > Vlastnosti > generování kódu PIN zabezpečení. Chcete-li změnit heslo, použijte tento PIN kód. |
| Změnit heslo |Operace se nezdařila. ID: 120002 |**Příčina:**<br/>Tato chyba se dodává při nastavení zabezpečení povoleno, pokusíte změnit heslo a na nepodporovanou verzi.<br/>**Doporučená akce:**<br/> Chcete-li změnit heslo, musíte nejdřív aktualizovat agenta zálohování na minimální verzi minimální 2.0.9052 a server Azure Backup ke minimální update 1, zadejte platný kód PIN zabezpečení. Pokud chcete získat kód PIN, přihlaste se k portálu Azure a přejděte do trezoru služeb zotavení > Nastavení > Vlastnosti > generování kódu PIN zabezpečení. Chcete-li změnit heslo, použijte tento PIN kód. |


## <a name="configure-email-notifications"></a>Konfigurace e-mailových oznámení
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Došlo k pokusu o nastavení e-mailová oznámení pomocí účtu Office 365. | získání ID chyby: 2013| **Příčina:**<br/> Při pokusu o použití účtu Office 365 <br/> **Doporučená akce:**<br/> 1. První věc, aby je, že "Povolit anonymní předávání na přijímat konektor" pro váš server aplikace DPM je instalace na serveru Exchange. Tady je [odkaz](http://technet.microsoft.com/en-us/library/bb232021.aspx) o tom, jak nakonfigurovat tuto funkci.  <br/> 2. Pokud nemůžete použít interní předávací doména SMTP a muset nastavit pomocí serveru Office 365, můžete nastavit IIS jako předávání pro tento. Musíte nakonfigurovat server DPM za účelem být [moct předávání SMTP k O365 pomocí služby IIS](https://technet.microsoft.com/en-us/library/aa995718(v=exchg.65).aspx) <br/><br/> Důležité upozornění: V kroku 3 -> g -> ii, je nutné použít user@domain.com formátu a není doména\uživatel <br/> Bod použít místní servername jako SMTP serveru, port 587 a pak e-mailu uživatel, který e-maily musí pocházet z aplikace DPM. <br/> Uživatelské jméno a heslo na stránce instalačního programu aplikace DPM SMTP musí být účet domény v doméně, ve které aplikace DPM. <br/><br/> Poznámka: Při změně adresy serveru SMTP, proveďte požadovanou změnu nové nastavení, zavřete pole nastavení a znovu otevřete jistotu odráží novou hodnotu.  Jednoduše změna a testování nemusí vždy provést nové nastavení, testování tímto způsobem je osvědčeným postupem. <br/><br/> Kdykoli během tohoto procesu můžete vymazat toto nastavení zavření konzoly aplikace DPM a úpravou těchto klíčů registru:<br/> HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> Odstraňte SMTPPassword a SMTPUserName klíče. <br/> Můžete je přidat zpět v uživatelském rozhraní při znovu spusťte.
