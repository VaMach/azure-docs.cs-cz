<properties
    pageTitle="Konfigurace nastavení proxy serveru a brány firewall v Log Analytics | Microsoft Azure"
    description="Nakonfigurujte nastavení proxy serveru a brány firewall, pokud vaši agenti nebo služby OMS potřebují používat konkrétní porty."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>


# Konfigurace nastavení proxy serveru a brány firewall v Log Analytics

Akce potřebné ke konfiguraci nastavení proxy serveru a brány firewall pro Log Analytics v OMS se liší podle toho, zda používáte Operations Manager a jeho agenty, nebo agenty Microsoft Monitoring Agent, kteří se připojují přímo k serverům. Přečtěte si následující oddíly pro typ agenta, který používáte.

## Konfigurace nastavení proxy serveru a brány firewall pomocí Microsoft Monitoring Agent

Microsoft Monitoring Agent musí mít přístup k číslu portu vašich domén a k adresám URL, aby se mohl připojit a zaregistrovat do služby OMS. Pokud ke komunikaci mezi agentem a službou OMS používáte proxy server, budete se muset ujistit, že jsou dostupné příslušné prostředky. Používáte-li k omezení přístupu k internetu bránu firewall, je nutné ji nakonfigurovat tak, aby povolovala přístup k OMS. Porty, které OMS potřebuje, jsou uvedené v následujících tabulkách.

|**Prostředek agenta**|**Porty**|**Obejít kontrolu protokolu HTTPS**|
|--------------|-----|--------------|
|\*.ods.opinsights.azure.com|443|Ano|
|\*.oms.opinsights.azure.com|443|Ano|
|\*.blob.core.windows.net|443|Ano|
|ods.systemcenteradvisor.com|443| |

Následující postup slouží ke konfiguraci nastavení proxy serveru pro Microsoft Monitoring Agent pomocí Ovládacích panelů. Tento postup bude nutné použít pro každý server. pokud máte mnoho serverů, které je nutné nakonfigurovat, může být jednodušší použít skript, který tento proces zautomatizuje. Je-li to váš případ, podívejte se na další postup [Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí skriptu](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí Ovládacích panelů

1. Otevřete **Ovládací panely**.

2. Otevřete **Microsoft Monitoring Agent**.

3. Klikněte na kartu **Nastavení proxy serveru**.<br>  
  ![karta nastavení proxy serveru](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Zaškrtněte políčko **Použít proxy server** a zadejte adresu URL a v případě potřeby i číslo portu, podobně jako v příkladu výše. Pokud váš proxy server vyžaduje ověření, zadejte uživatelské jméno a heslo pro přístup k proxy serveru.

Pomocí následujícího postupu vytvoříte skript prostředí PowerShell, jehož spuštěním můžete nastavit proxy server pro každého agenta, který se připojuje přímo k serverům.

### Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí skriptu

Zkopírujte následující ukázku kódu, aktualizujte ji pomocí informací specifických pro vaše prostředí, uložte ji s příponou názvu souboru PS1 a následně tento skript spusťte na každém počítači, který se připojuje přímo ke službě OMS.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## Konfigurace nastavení proxy serveru a brány firewall pomocí nástroje Operations Manager

Skupina pro správu nástroje Operations Manager musí mít přístup k číslu portu vašich domén a k adresám URL, aby se mohla připojit a zaregistrovat do služby OMS. Pokud ke komunikaci mezi serverem pro správu nástroje Operations Manager a službou OMS používáte proxy server, budete se muset ujistit, že jsou dostupné příslušné prostředky. Používáte-li k omezení přístupu k internetu bránu firewall, je nutné ji nakonfigurovat tak, aby povolovala přístup k OMS. I v případě, že server pro správu nástroje Operations Manager není za proxy server, mohou tam být jeho agenti. V tom případě by měl být proxy server nakonfigurován stejným způsobem jako agenti, aby bylo možné zapnout a povolit odesílání dat z řešení Zabezpečení a správa protokolů do webové služby OMS.

Pro zajištění komunikace mezi agenty nástroje Operations Manager a službou OMS by vaše infrastruktura nástroje Operations Manager (včetně agentů) měla mít správné nastavení proxy serveru a správnou verzi. Nastavení proxy serveru pro agenty se zadává v konzole nástroje Operations Manager. Vaše verze by měla být jedna z těchto:

- Operations Manager 2012 SP1 s kumulativní aktualizací 7 nebo novější
- Operations Manager 2012 R2 s kumulativní aktualizací 3 nebo novější


Porty související s těmito úkoly jsou uvedené v následujících tabulkách.

>[AZURE.NOTE] Některé z následujících prostředků zmiňují služby Advisor a Operational Insights – obě jsou předchozí verze OMS. Uvedené prostředky se ale v budoucnu změní.

Zde je seznam prostředků agenta a portů:<br>

|**Prostředek agenta**|**Porty**|
|--------------|-----|
|\*.ods.opinsights.azure.com|443|
|\*.oms.opinsights.azure.com|443|
|\*.blob.core.windows.net/\*|443|
|ods.systemcenteradvisor.com|443|
<br>
Zde je seznam prostředků serveru pro správu a porty:<br>

|**Prostředek serveru pro správu**|**Porty**|**Obejít kontrolu protokolu HTTPS**|
|--------------|-----|--------------|
|service.systemcenteradvisor.com|443| |
|\*.service.opinsights.azure.com|443| |
|\*.blob.core.windows.net|443|Ano| 
|data.systemcenteradvisor.com|443| | 
|ods.systemcenteradvisor.com|443| | 
|\*.ods.opinsights.azure.com|443|Ano| 
<br>
Zde je seznam prostředků OMS a konzoly nástroje Operations Manager a porty.<br>

|**Prostředek OMS a konzoly nástroje Operations Manager**|**Porty**|
|----|----|
|service.systemcenteradvisor.com|443|
|\*.service.opinsights.azure.com|443|
|\*.live.com|Port 80 a 443|
|\*.microsoft.com|Port 80 a 443|
|\*.microsoftonline.com|Port 80 a 443|
|\*.mms.microsoft.com|Port 80 a 443|
|login.windows.net|Port 80 a 443|
<br>

Pomocí následujících postupů zaregistrujete skupinu pro správu nástroje Operations Manager do služby OMS. Máte-li problémy s komunikací mezi skupinou pro správu a službou OMS, použijte postupy ověření k řešení potíží při přenosu dat do služby OMS.

### Žádost o výjimky pro koncové body služby OMS

1. Pomocí informací z první tabulky zobrazené výše zajistěte, aby prostředky, které potřebuje server pro správu nástroje Operations Manager, byly dostupné přes všechny brány firewall, které máte.
2. Pomocí informací z druhé tabulky zobrazené výše zajistěte, aby prostředky, které potřebuje OMS a konzola Operations Console v nástroji Operations Manager byly dostupné přes všechny brány firewall, které máte.
3. Pokud používáte proxy server s aplikací Internet Explorer, ujistěte se, že je nakonfigurována a funguje správně. Ověřit to můžete tak, že otevřete zabezpečené webové připojení (HTTPS), například [https://bing.com](https://bing.com). Pokud zabezpečené webové připojení nefunguje v prohlížeči, pravděpodobně nebude fungovat ani v konzole pro správu nástroje Operations Manager s webovými službami v cloudu.

### Konfigurace proxy serveru v konzole nástroje Operations Manager

1. Otevřete konzolu nástroje Operations Manager a vyberte pracovní prostor **Správa**.

2. Rozbalte **Operational Insights** a poté vyberte **Připojení služby Operational Insights**.<br>  
    ![Operations Manager Připojení OMS](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. V zobrazení Připojení OMS klikněte na **Konfigurovat proxy server**.<br>  
    ![Operations Manager Připojení OMS Konfigurace proxy serveru](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. V Průvodci nastavením služby Operational Insights: Proxy server zaškrtněte políčko **Použít proxy server pro přístup k webovým službám Operational Insights** a poté zadejte adresu URL s číslem portu, například **http://mojeproxy:80**.<br>  
    ![Operations Manager Adresa proxy serveru OMS](./media/log-analytics-proxy-firewall/proxy-om03.png)


### Zadání přihlašovacích údajů, pokud proxy server vyžaduje ověření
 Přihlašovací údaje proxy serveru a nastavení je nutné rozšířit i na spravované počítače, které se budou hlásit k OMS. Tyto servery by měly být ve *skupině monitorovacích serverů služby Microsoft System Center Advisor*. Přihlašovací údaje jsou šifrované v registru každého serveru ve skupině.

1. Otevřete konzolu nástroje Operations Manager a vyberte pracovní prostor **Správa**.
2. V části **Konfigurace RunAs** vyberte **Profily**.
3. Otevřete profil **Proxy server profilu Spustit jako služby System Center Advisor**.  
    ![obrázek profilu Proxy server profilu Spustit jako služby System Center Advisor](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Chcete-li použít účet Spustit jako, v Průvodci profilem Spustit jako klikněte na **Přidat**. Můžete vytvořit nový účet Spustit jako nebo použít existující účet. Tento účet musí mít dostatečná oprávnění k průchodu proxy serverem.  
    ![obrázek Průvodce profilem Spustit jako](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Chcete-li nastavit účet, který chcete spravovat, zvolením možnosti **Vybranou třídu, skupinu nebo objekt** otevřete dialogové okno Vyhledat objekty.  
    ![obrázek Průvodce profilem Spustit jako](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Vyhledejte a následně vyberte **Skupina monitorovacích serverů služby Microsoft System Center Advisor**.  
    ![obrázek dialogového okna Vyhledat objekty](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Kliknutím na **OK** zavřete dialogové okno Přidat účet Spustit jako.  
    ![obrázek Průvodce profilem Spustit jako](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Dokončete průvodce a uložte změny.  
    ![obrázek Průvodce profilem Spustit jako](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### Ověření, že se stáhly sady Management Pack pro OMS

Pokud jste do OMS přidali řešení, můžete je zobrazit v konzole nástroje Operations Manager jako sady Management Pack v části **Správa**. Vyhledejte *System Center Advisor*, tak je najdete rychleji.  
    ![stažené sady Management Pack](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) Nebo můžete zkontrolovat sady Management Pack pro OMS pomocí následujícího příkazu prostředí Windows PowerShell na serveru pro správu nástroje Operations Manager:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### Ověření, že nástroj Operations Manager odesílá data do služby OMS

1. Na serveru pro správu nástroje Operations Manager otevřete Sledování výkonu (perfmon.exe) a vyberte **Sledování výkonu**.
2. Klikněte na **Přidat** a poté vyberte **Skupiny pro správu služby Health Service**.
3. Přidejte všechny čítače, které začínají na **HTTP**.  
    ![přidání čítačů](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Pokud je vaše konfigurace nástroje Operations Manager v pořádku, uvidíte u čítačů správy služby Health Service aktivitu událostí a dalších datových položek v závislosti na sadách Management Pack, které jste přidali v OMS, a na konfigurovaných zásadách shromažďování protokolů.  
    ![Sledování výkonu ukazující aktivitu](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## Další kroky

- Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.
- Chcete-li zobrazit podrobné informace shromážděné řešeními, seznamte se s [vyhledáváním protokolů](log-analytics-log-searches.md).



<!--HONumber=Sep16_HO3-->


