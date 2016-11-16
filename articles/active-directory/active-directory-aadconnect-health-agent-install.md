---
title: "Instalace agenta služby Azure AD Connect Health | Dokumentace Microsoftu"
description: "Toto je stránka služby Azure AD Connect Health, která popisuje instalaci agenta pro službu AD FS a synchronizaci."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0e30b59e618b7c01290002511367fa1e6dfc6eb9


---
# <a name="azure-ad-connect-health-agent-installation"></a>Instalace agenta služby Azure AD Connect Health
Tento dokument vás provede procesem instalace a konfigurace agentů služby Azure AD Connect Health. Agenty si můžete stáhnout [tady](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

## <a name="requirements"></a>Požadavky
Následující tabulka představuje seznam požadavků pro používání služby Azure AD Connect Health.

| Požadavek | Popis |
| --- | --- |
| Azure AD Premium |Azure AD Connect Health je funkcí služby Azure AD Premium a vyžaduje Azure AD Premium. </br></br>Další informace najdete v článku [Začínáme s Azure AD Premium](active-directory-get-started-premium.md). </br>Pokud chcete začít používat bezplatnou 30denní zkušební verzi, přečtěte si článek o tom, jak [začít se zkušební verzí](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Abyste mohli Azure AD Connect Health začít používat, musíte být globálním správcem služby Azure AD.  |Ve výchozím nastavení můžou agenty stavu instalovat a konfigurovat jenom globální správci. Bez nich agenty nespustíte, nebudete mít přístup na portál ani nebudete moct v rámci Azure AD Connect Health provádět jakékoli operace. Další informace najdete v článku o [správě adresáře Azure AD](active-directory-administer.md). <br><br> Pomocí řízení přístupu na základě rolí můžete povolit přístup k Azure AD Connect Health i ostatním uživatelům z vaší organizace. Další informace najdete v článku o [řízení přístupu k Azure AD Connect Health na základě rolí](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control). </br></br>**Důležité:** Účet, který jste použili při instalaci agentů, musí být pracovním nebo školním účtem. Nemůže to být účet Microsoft. Další informace najdete v článku o [registraci do Azure jako organizace](sign-up-organization.md). |
| Agent Azure AD Connect Health se instaluje na každý cílový server. |Azure AD Connect Health vyžaduje instalaci agentů na cílových serverech proto, aby dodávali data prohlížená na portálu. </br></br>Pokud například potřebujete získávat data z místní infrastruktury služby AD FS, musí být agent nainstalovaný na serverech služby AD FS, na proxy serverech služby AD FS a na proxy serverech webových aplikací. Podobně pro načtení dat ve vaší místní infrastruktuře služby AD DS musí být agent nainstalován na řadičích domény. </br></br>**Důležité:** Účet, který jste použili při instalaci agentů, musí být pracovním nebo školním účtem. Nemůže to být účet Microsoft. Další informace najdete v článku o [registraci do Azure jako organizace](sign-up-organization.md). |
| Odchozí připojení ke koncovým bodům služby Azure |Agent během instalace a za běhu vyžaduje připojení ke koncovým bodům služby Azure AD Connect Health. Pokud je odchozí připojení blokováno, nezapomeňte do seznamu povolených výjimek přidat následující koncové body: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.queue.core.windows.net</li><li>adhsprodwus.servicebus.windows.net - Port: 5671 </li><li>https://management.azure.com </li><li>https://s1.adhybridhealth.azure.com/</li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
| Porty brány firewall na serveru se spuštěným agentem. |Agent vyžaduje, aby následující porty brány firewall byly otevřené. Je to proto, aby agent mohl komunikovat s koncovými body služby Azure AD Health.</br></br><li>TCP/UDP port 443</li><li>TCP/UDP port 5671</li> |
| Pokud je povoleno rozšířené zabezpečení Internet Exploreru, povolte následující weby |Pokud je povoleno rozšířené zabezpečení Internet Exploreru, musí být na serveru, na který budete agenta instalovat, povoleny následující weby.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Federační server vaší organizace, který je pro službu Azure Active Directory důvěryhodný. Příklad: https://sts.contoso.com</li> |

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>Instalace agenta Azure AD Connect Health pro službu AD FS
Pokud chcete spustit instalaci agenta, poklikejte na stažený soubor .exe. Na první obrazovce klikněte na Instalovat.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install1.png)

Po dokončení instalace klikněte na Konfigurovat.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install2.png)

Spustí se příkazový řádek následovaný kódem PowerShellu, který provede Register-AzureADConnectHealthADFSAgent. Po zobrazení výzvy k přihlášení k Azure se přihlaste.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install3.png)

Po přihlášení bude PowerShell pokračovat. Po dokončení můžete PowerShell zavřít. Konfigurace je hotová.

V tomto okamžiku by služby měly být automaticky spuštěny, což agentovi umožní sledovat a shromažďovat data. Pokud jste nesplnili všechny předpoklady popsané v předchozích částech, budou se v okně PowerShellu zobrazovat upozornění. Nezapomeňte proto [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) splnit ještě před samotnou instalací agenta. Následující snímek obrazovky obsahuje příklad těchto chyb.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install4.png)

Pokud chcete ověřit úspěšnou instalaci agenta, vyhledejte na serveru následující služby. Pokud jste konfiguraci dokončili, měly by již být spuštěné. V opačném případě jsou zastavené, dokud se konfigurace nedokončí.

* Diagnostické služby AD FS pro Azure AD Connect Health
* Služba analýz AD FS pro Azure AD Connect Health
* Služba monitorování AD FS pro Azure AD Connect Health

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>Instalace agenta na servery se systémem Windows Server 2008 R2
Kroky pro servery se systémem Windows Server 2008 R2:

1. Zkontrolujte, jestli server používá aktualizaci Service Pack 1 nebo novější.
2. Pro instalaci agenta vypněte rozšířené zabezpečení IE:
3. Před instalací agenta AD Health nainstalujte na jednotlivé servery Windows PowerShell 4.0. Pokud chcete nainstalovat Windows PowerShell 4.0, postupujte následovně:
   * Pomocí následujícího odkazu na stažení offline instalačního programu nainstalujte [rozhraní Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779).
   * Instalace PowerShellu ISE (z funkcí systému Windows)
   * Nainstalujte [Windows Management Framework 4.0.](https://www.microsoft.com/download/details.aspx?id=40855)
   * Nainstalujte na server Internet Explorer verze 10 nebo novější. (Tento krok je požadován službou Health, aby bylo možné provádět ověřování s použitím vašich přihlašovacích údajů správce Azure.)
4. Další informace o instalaci Windows PowerShell 4.0 na server se systémem Windows Server 2008 R2 najdete v článku na wikiwebu [tady](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### <a name="enable-auditing-for-ad-fs"></a>Povolení auditování služby AD FS
> [!NOTE]
> Tento oddíl se vztahuje pouze na federační servery služby AD FS.
> 
> 

Aby mohla funkce analýzy využití shromažďovat a analyzovat data, potřebuje agent Azure AD Connect Health informace z protokolů auditu služby AD FS. Tyto protokoly nejsou ve výchozím nastavení povolené. Pokud chcete na svých serverech služby AD FS povolit auditování služby AD FS a najít protokoly auditu služby AD FS, postupujte následovně.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Povolení auditování služby AD FS 2.0
1. Klikněte na **Start**, najeďte myší na **Programy**, potom na **Nástroje pro správu** a potom klikněte na **Místní zásady zabezpečení**.
2. Přejděte do složky **Security Settings\Local Policies\User Rights Management** a poklikejte na položku Generovat audity zabezpečení.
3. Na kartě **Místní nastavení zabezpečení** zkontrolujte, jestli je tam uvedený účet služby AD FS 2.0. Pokud tam není, klikněte na **Přidat uživatele nebo skupinu** a přidejte ho do seznamu. Potom klikněte na **OK**.
4. Pokud chcete povolit auditování, otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz: <code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Zavřete místní zásady zabezpečení a potom otevřete snap-in Správa. Pokud chcete otevřít snap-in Správa, klikněte na tlačítko **Start**, najeďte myší na **Programy**, potom na **Nástroje pro správu** a potom klikněte na Správa služby AD FS 2.0.
6. V podokně Akce klikněte na tlačítko Upravit vlastnosti služby FS (Federation Service).
7. V dialogovém okně **Vlastnosti služby FS (Federation Service)** klikněte na kartu **Události**.
8. Zaškrtněte políčka **Úspěšné audity** a **Neúspěšné audity**.
9. Klikněte na tlačítko **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Povolení auditování služby AD FS v systému Windows Server 2012 R2
1. Otevřením **Správce serveru** na úvodní obrazovce nebo Správce serveru na hlavním panelu na ploše otevřete **Místní zásady zabezpečení** a klikněte na **Nástroje/Místní zásady zabezpečení**.
2. Přejděte do složky **Security Settings\Local Policies\User Rights Management** a poklikejte na položku **Generovat audity zabezpečení**.
3. Na kartě **Místní nastavení zabezpečení** zkontrolujte, jestli je tam uvedený účet služby AD FS. Pokud tam není, klikněte na **Přidat uživatele nebo skupinu** a přidejte ho do seznamu. Potom klikněte na **OK**.
4. Pokud chcete povolit auditování, otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz: <code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Zavřete **Místní zásady zabezpečení** a potom otevřete snap-in **Správa služby AD FS** (ve Správci serveru klikněte na Nástroje a potom vyberte správu služby AD FS).
6. V podokně Akce klikněte na tlačítko **Upravit vlastnosti služby FS (Federation Service)**.
7. V dialogovém okně Vlastnosti služby FS (Federation Service) klikněte na kartu **Události**.
8. Zaškrtněte políčka **Úspěšné audity a Neúspěšné audity** a klikněte na tlačítko **OK**.

#### <a name="to-locate-the-ad-fs-audit-logs"></a>Nalezení protokolů auditu služby AD FS
1. Otevřete **Prohlížeč událostí**.
2. Přejděte na Protokoly systému Windows a vyberte **Zabezpečení**.
3. Na pravé straně klikněte na **Filtrovat aktuální protokoly**.
4. V části Zdroj události vyberte **Auditování služby AD FS**.

![Protokoly auditu služby AD FS](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [!WARNING]
> Pokud existují zásady skupiny, které auditování služby AD FS zakazují, agent Azure AD Connect Health nemůže shromažďovat informace. Zkontrolujte, jestli nepoužíváte zásady skupiny, které auditování zakazují.
> 
> 

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>Instalace agenta Azure AD Connect Health pro synchronizaci
Agent Azure AD Connect Health pro synchronizaci se v posledním sestavení Azure AD Connect instaluje automaticky. Pokud chcete používat Azure AD Connect pro synchronizaci, stáhněte si nejnovější verzi Azure AD Connect a nainstalujte ji. Nejnovější verzi můžete stáhnout [tady](http://www.microsoft.com/download/details.aspx?id=47594).

Pokud chcete ověřit úspěšnou instalaci agenta, vyhledejte na serveru následující služby. Pokud jste konfiguraci dokončili, měly by již být spuštěné. V opačném případě jsou zastavené, dokud se konfigurace nedokončí.

* Služba analýz synchronizace služby Azure AD Connect Health
* Služba monitorování synchronizace služby Azure AD Connect Health

![Ověření Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/services.png)

> [!NOTE]
> Nezapomeňte, že používání Azure AD Connect Health vyžaduje službu Azure AD Premium. Pokud nemáte Azure AD Premium, nemůžete dokončit konfiguraci na webu Azure Portal. Další informace najdete na [stránce s požadavky](active-directory-aadconnect-health-agent-install.md#requirements).
> 
> 

## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Ruční registrace Azure AD Connect Health pro synchronizaci
Pokud se registrace agenta Azure AD Connect Health pro synchronizaci po úspěšné instalaci Azure AD Connect nepodaří, můžete k ruční registraci agenta použít následující příkaz PowerShellu.

> [!IMPORTANT]
> Použití tohoto příkazu PowerShellu je nutné jenom při neúspěšné registraci agenta po instalaci Azure AD Connect.
> 
> 

Následující příkaz PowerShellu je nutný jenom v případě neúspěšné registrace agenta stavu po úspěšné instalaci a konfiguraci Azure AD Connect. Služby Azure AD Connect Health se spustí po úspěšné registraci agenta.

Ruční registraci agenta Azure AD Connect Health pro synchronizaci můžete provést pomocí následujícího příkazu PowerShellu:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Příkaz přijímá následující parametry:

* AttributeFiltering: $true (výchozí nastavení) – pokud Azure AD Connect nesynchronizuje výchozí sadu atributů a byl přizpůsoben, aby používal filtrovanou sadu atributů. V opačném případě $false.
* StagingMode: $false (výchozí nastavení) – pokud server Azure AD Connect NENÍ v pracovním režimu, $true, pokud je server nakonfigurovaný, aby byl v pracovní režimu.

Po zobrazení výzvy k ověření použijte stejný účet globálního správce (například admin@domain.onmicrosoft.com), který jste použili ke konfiguraci Azure AD Connect.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>Instalace agenta Azure AD Connect Health pro službu AD DS
Pokud chcete spustit instalaci agenta, poklikejte na stažený soubor .exe. Na první obrazovce klikněte na Instalovat.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Po dokončení instalace klikněte na Konfigurovat.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Spustí se příkazový řádek následovaný kódem PowerShellu, který provede Register-AzureADConnectHealthADDSAgent. Po zobrazení výzvy k přihlášení k Azure se přihlaste.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Po přihlášení bude PowerShell pokračovat. Po dokončení můžete PowerShell zavřít. Konfigurace je hotová.

V tomto okamžiku by služby měly být automaticky spuštěny, což agentovi umožní sledovat a shromažďovat data. Pokud jste nesplnili všechny předpoklady popsané v předchozích částech, budou se v okně PowerShellu zobrazovat upozornění. Nezapomeňte proto [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) splnit ještě před samotnou instalací agenta. Následující snímek obrazovky obsahuje příklad těchto chyb.

![Ověření Azure AD Connect Health pro AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Pokud chcete ověřit úspěšnou instalaci agenta, vyhledejte na řadiči domény následující služby.

* Služba analýz AD DS pro Azure AD Connect Health
* Služba monitorování AD DS pro Azure AD Connect Health

Pokud jste konfiguraci dokončili, tyto služby by již měly být spuštěny. V opačném případě jsou zastavené, dokud se konfigurace nedokončí.

![Ověření Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>Instalace agenta Azure AD Connect Health pro službu AD DS na jádro serveru.
Po instalaci souboru .exe můžete dokončit proces registrace pomocí následujícího příkazu prostředí PowerShell:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Konfigurace agentů Azure AD Connect Health k používání proxy serveru HTTP
Agenty Azure AD Connect Health můžete nakonfigurovat, aby používaly proxy server HTTP.

> [!NOTE]
> * Použití příkazu „Netsh WinHttp set ProxyServerAddress“ není podporováno, protože agent používá k vytvoření webových požadavků System.Net místo služby Microsoft Windows HTTP Services.
> * Nakonfigurovaná adresa proxy serveru Http se používá k předávání šifrovaných zpráv protokolu Https.
> * Ověřené servery proxy (použití HTTPBasic) nejsou podporované.
> 
> 

### <a name="change-health-agent-proxy-configuration"></a>Změna konfigurace proxy serveru agenta stavu
Při konfiguraci agenta Azure AD Connect Health k používání proxy serveru HTTP máte následující možnosti.

> [!NOTE]
> Chcete-li aktualizovat nastavení proxy serveru, je třeba restartovat všechny služby agenta Azure AD Connect Health. Spusťte následující příkaz:<br>
> Restart-Service AdHealth*
> 
> 

#### <a name="import-existing-proxy-settings"></a>Import existujících nastavení proxy serveru
##### <a name="import-from-internet-explorer"></a>Import z Internet Exploreru
Nastavení proxy serveru HTTP z Internet Exploreru je možné importovat, aby je agenti Azure AD Connect Health mohli používat. Na všech serverech se spuštěným agentem Health spusťte následující příkaz prostředí PowerShell:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Import ze služby WinHTTP
Nastavení proxy serveru ze služby WinHTTP je možné importovat, aby je agenti Azure AD Connect Health mohli používat. Na všech serverech se spuštěným agentem Health spusťte následující příkaz prostředí PowerShell:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Ruční zadání adres proxy serverů
Na všech serverech se spuštěným agentem Health můžete ručně určit proxy server spuštěním následujícího příkazu prostředí PowerShell:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Příklad: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress myproxyserver: 443*

* Adresa může být název serveru DNS, který lze přeložit, nebo adresa IPv4.
* Port můžete vynechat. Pokud tento parametr vynecháte, bude jako výchozí vybrán port 443.

#### <a name="clear-existing-proxy-configuration"></a>Vymazání existující konfigurace proxy serveru
Existující konfiguraci proxy serveru můžete vymazat spuštěním následujícího příkazu:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>Čtení aktuálního nastavení proxy serveru
Nastavení aktuálně nakonfigurovaného proxy serveru můžete přečíst spuštěním následujícího příkazu:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>Test připojení ke službě Azure AD Connect Health
Při provozu může dojít k potížím, které způsobí, že agent Azure AD Connect Health ztratí připojení ke službě Azure AD Connect Health. Patří sem problémy se sítí, problémy s oprávněním a další příčiny.

Pokud agent nemůže odesílat data do služby Azure AD Connect Health po dobu delší než dvě hodiny, zobrazí se na portálu následující výstraha: „Data služby Health Service nejsou aktuální.“ Ověřit, zda ovlivněný agent Azure AD Connect Health může odesílat data do služby Azure AD Connect Health, můžete spuštěním následujícího příkazu prostředí PowerShell:

    Test-AzureADConnectHealthConnectivity -Role ADFS

Parametr „role“ v současnosti přijímá následující hodnoty:

* ADFS
* Sync
* PŘIDÁ

Pokud chcete zobrazit podrobné protokoly, použijte v příkazu příznak - ShowResults. Použijte následující příklad:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

> [!NOTE]
> Pokud chcete použít nástroj pro připojení, musíte nejdřív dokončit registraci agenta. Pokud se vám nedaří dokončit registraci agenta, zkontrolujte, jestli jste splnili všechny [požadavky](active-directory-aadconnect-health-agent-install.md#requirements) služby Azure AD Connect Health. Tento test připojení se ve výchozím nastavení provádí během registrace agenta.
> 
> 

## <a name="related-links"></a>Související odkazy
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)




<!--HONumber=Nov16_HO2-->


