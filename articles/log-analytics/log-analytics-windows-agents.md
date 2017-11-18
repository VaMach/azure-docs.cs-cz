---
title: "Připojení počítače se systémem Windows k analýze protokolů Azure | Microsoft Docs"
description: "Tento článek popisuje kroky pro počítače se systémem Windows ve vaší místní infrastruktuře připojení ke službě Analýza protokolů pomocí přizpůsobená verze z monitorování agenta MMA (Microsoft)."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aaf3e596f8c287c60531a6911c5797b3de26e570
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a>Připojení počítače se systémem Windows do služby analýzy protokolů Azure

Tento článek popisuje kroky pro připojení počítače se systémem Windows ve vaší místní infrastruktuře k pracovním prostorům, OMS pomocí přizpůsobená verze z monitorování agenta MMA (Microsoft). Musíte nainstalovat a připojit agentů pro všechny počítače, které chcete, aby pro ně k odesílání dat do služby analýzy protokolů a prohlížení a provádění akcí na těchto datech zařadit do provozu. Každý agent můžete zprávu několik pracovních prostorů.

Můžete nainstalovat agenty pomocí instalačního programu, příkazového řádku nebo pomocí požadovaného stavu konfigurace (DSC) ve službě Azure Automation.  

>[!NOTE]
Pro virtuální počítače běžící v Azure, můžete zjednodušit instalace pomocí [rozšíření virtuálního počítače](../virtual-machines/windows/extensions-oms.md).

Agent na počítačích s připojením k Internetu pomocí připojení k Internetu k odesílání dat do OMS. Pro počítače, které nemají připojení k Internetu, můžete použít proxy server nebo [OMS brány](log-analytics-oms-gateway.md).

Připojení počítače se systémem Windows k OMS je jednoduchý pomocí tří jednoduché kroky:

1. Stáhněte si instalační soubor agenta z portálu OMS
2. Instalace agenta pomocí metodu, kterou si zvolíte
3. Konfiguraci agenta nebo přidejte další pracovní prostory, v případě potřeby

Následující diagram znázorňuje vztah mezi OMS a počítačů s Windows po instalaci a nakonfigurovali agenty.

![OMS-direct agenta – diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

Pokud vaše zásady zabezpečení IT neumožňují počítače v síti pro připojení k Internetu, můžete konfigurovat počítače pro připojení k bráně OMS. Další informace a postup pro konfiguraci serverů pro komunikaci přes bránu OMS ke službě OMS najdete v tématu [počítače připojit k OMS pomocí brány OMS](log-analytics-oms-gateway.md).

## <a name="system-requirements-and-required-configuration"></a>Požadavky na systém a požadované konfiguraci
Před instalací nebo nasadit agenty, ujistěte se, že splňujete požadavky následující podrobnosti.

- OMS MMA můžete nainstalovat pouze na počítačích se systémem Windows Server 2008 SP 1 nebo novější nebo Windows 7 SP1 nebo novější.
- Budete potřebovat předplatné Azure.  Další informace najdete v tématu [začít pracovat s analýzy protokolů](log-analytics-get-started.md).
- Každý počítač se systémem Windows musí být schopni připojit k Internetu pomocí protokolu HTTPS nebo k bráně OMS. Toto připojení může být direct, prostřednictvím proxy serveru nebo přes bránu OMS.
- OMS MMA můžete nainstalovat na samostatných počítačů, serverů a virtuálních počítačů. Pokud chcete se připojit k OMS virtuálních počítačů hostovaných v Azure, najdete v části [virtuálních počítačích Azure připojit k analýze protokolů](log-analytics-azure-vm-extension.md).
- Agenta je potřeba použít port 443 protokolu TCP pro různé prostředky.

### <a name="network"></a>Síť

Pro agenty se systémem Windows pro připojení k a zaregistrovat službu OMS musí mít přístup k síťovým prostředkům, včetně čísla portů a adres URL domény.

- U proxy serverů musíte zajistit konfiguraci příslušných prostředků proxy serveru v nastavení agenta.
- Pro brány firewall, které omezují přístup k Internetu vám nebo vaší sítě technici nutné nakonfigurovat bránu firewall tak, aby povolovala přístup k OMS. V nastavení agenta nemusíte nic konfigurovat.

V následující tabulce najdete přehled prostředků potřebných pro komunikaci.

>[!NOTE]
>Některé z těchto prostředků zmínili Operational Insights, která byla předchozí název pro analýzu protokolu.

| Prostředek agenta | Porty | Obejít kontrolu protokolu HTTPS |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ano |
| *.oms.opinsights.azure.com | 443 | Ano |
| *.blob.core.windows.net | 443 | Ano |
| *.azure-automation.net | 443 | Ano |



## <a name="download-the-agent-setup-file-from-oms"></a>Stáhněte si instalační soubor agenta z OMS
1. V [portálu OMS](https://www.mms.microsoft.com)na **přehled** klikněte na tlačítko **nastavení** dlaždici.  Klikněte **připojené zdroje** v horní části.  
    ![Karta připojené zdroje](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Klikněte na tlačítko **servery Windows** a pak klikněte na **stáhnout agenta Windows** vztahuje na typ procesoru počítače stáhnout instalační soubor.
3. Na pravé straně **ID pracovního prostoru**, klikněte na ikonu kopírování a vložení ID do poznámkového bloku.
4. Na pravé straně **primární klíč**, klikněte na ikonu kopírování a vložení klíč do poznámkového bloku.     

## <a name="install-the-agent-using-setup"></a>Instalace agenta pomocí instalačního programu
1. Spusťte instalační program a nainstalujte agenta na počítači, který chcete spravovat.
2. Na úvodní stránce klikněte na tlačítko **Další**.
3. Na stránce Licenční podmínky, přečtěte si licenční a pak klikněte na tlačítko **souhlasím**.
4. Na stránce cílovou složku změnit nebo ponechat výchozí instalační složku a pak klikněte na tlačítko **Další**.
5. Na stránce Možnosti instalace agenta můžete připojit agenta k Azure Log Analytics (OMS), Operations Manager, nebo můžete nechat volby prázdné Pokud chcete provést konfiguraci agenta později. Klikněte na **Další**.   
    - Pokud jste zvolili pro připojení k Azure Log Analytics (OMS), vložte **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** který jste zkopírovali do poznámkového bloku v předchozím postupu a pak klikněte na tlačítko **další** .  
        ![Vložte ID pracovního prostoru a primární klíč](./media/log-analytics-windows-agents/connect-workspace.png)
    - Pokud jste vybrali možnost připojení k nástroji Operations Manager, zadejte **název skupiny pro správu**, **serveru pro správu** název, a **Port serveru pro správu**a potom klikněte na **Další**. Na stránce účet akce agenta zvolit účet místní systém nebo účet místní domény a pak klikněte na tlačítko **Další**.  
        ![Konfigurace skupiny pro správu](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![účet akce agenta](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Na stránce Připraveno k instalaci, zkontrolujte vybrané možnosti a pak klikněte na tlačítko **nainstalovat**.
7. Konfigurace byla úspěšně dokončena stránky, klikněte na tlačítko **Dokončit**.
8. Po dokončení **agenta Microsoft Monitoring Agent** se zobrazí v **ovládací panely**. Můžete zkontrolovat konfiguraci existuje a ověřte, zda agent je připojena k provozní přehledy (OMS). Při připojení k OMS, agent zobrazí zpráva s oznámením: **Microsoft Monitoring Agent úspěšně připojil ke službě Microsoft Operations Management Suite.**

## <a name="configure-proxy-settings"></a>Konfigurace nastavení proxy serveru

Následující postup slouží ke konfiguraci nastavení proxy serveru pro Microsoft Monitoring Agent pomocí Ovládacích panelů. Budete muset použít tento postup pro každý server. pokud máte mnoho serverů, které je nutné nakonfigurovat, může být jednodušší použít skript, který tento proces zautomatizuje. Je-li to váš případ, podívejte se na další postup [Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí skriptu](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí Ovládacích panelů
1. Otevřete **Ovládací panely**.
2. Otevřete **Microsoft Monitoring Agent**.
3. Klikněte na kartu **Nastavení proxy serveru**.  
    ![karta nastavení proxy serveru](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)
4. Zaškrtněte políčko **Použít proxy server** a zadejte adresu URL a v případě potřeby i číslo portu, podobně jako v příkladu výše. Pokud váš proxy server vyžaduje ověření, zadejte uživatelské jméno a heslo pro přístup k proxy serveru.


### <a name="verify-agent-connectivity-to-oms"></a>Ověření připojení agenta k OMS

Snadno můžete ověřit, zda jsou agenty komunikaci s OMS následujícím postupem:

1.  Na počítači s agentem Windows otevřete ovládací panely.
2.  Otevřete agenta Microsoft Monitoring Agent.
3.  Klikněte na kartu Azure Log Analytics (OMS).
4.  Ve sloupci Stav byste měli vidět, že agent úspěšně připojené ke službě Operations Management Suite.

![připojení agenta](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Konfigurace nastavení proxy serveru pro Microsoft Monitoring Agent pomocí skriptu
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



## <a name="install-the-agent-using-the-command-line"></a>Instalace agenta pomocí příkazového řádku
- Upravit a potom použijte následující příklad pro instalaci agenta pomocí příkazového řádku. V příkladu provede plně bezobslužnou instalaci.

    >[!NOTE]
    Pokud chcete provést upgrade agenta, budete muset použít analýzy protokolů skriptování rozhraní API. Najdete v další části Postup při upgradu agenta.

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

Agent používá IExpress jako jeho Self-Extractor pomocí `/c` příkaz. Zobrazí se přepínače příkazového řádku v [přepínače příkazového řádku pro IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) a aktualizujte příklad podle svých potřeb.

|MMA specifické možnosti                   |Poznámky         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = konfigurace agenta tak, aby odesílaly pracovního prostoru                |
|OPINSIGHTS_WORKSPACE_ID                | Id pracovního prostoru (guid) pro pracovní prostor pro přidání                    |
|OPINSIGHTS_WORKSPACE_KEY               | Klíč pracovního prostoru se používá k ověření původně s pracovním prostoru |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Zadejte cloudovém prostředí, kde se nachází v pracovním prostoru <br> 0 = komerční cloudu azure (výchozí) <br> 1 = azure Government |
|OPINSIGHTS_PROXY_URL               | Identifikátor URI pro proxy server používat |
|OPINSIGHTS_PROXY_USERNAME               | Uživatelské jméno pro přístup k ověřený server proxy |
|OPINSIGHTS_PROXY_PASSWORD               | Heslo pro přístup ověřený server proxy |

>[!NOTE]
Abyste se vyhnuli stiskne maximální délku příkazového řádku IExpress, nainstalujte agenta se žádný pracovní prostor nakonfigurované a potom pomocí skriptu pro nastavení konfigurace pro pracovní prostor.

>[!NOTE]
Pokud dojde `Command line option syntax error.` při použití `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parametr, můžete použít následující alternativní řešení:
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a>Přidat pracovní prostor pomocí skriptu
Přidáte pracovní prostor pomocí rozhraní API skriptování agenta analýzy protokolů v následujícím příkladu:

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

Pokud chcete přidat do pracovního prostoru v Azure pro US Government, použijte následující ukázka skriptu:
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
Pokud jste použili příkazového řádku nebo skriptu dříve pro instalaci nebo konfiguraci agenta, `EnableAzureOperationalInsights` byl nahrazen `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Instalace agenta pomocí DSC v Azure Automation.

Následující ukázkový skript můžete použít k instalaci agenta pomocí ve službě Azure Automation DSC. Tento příklad nainstaluje agenta nástroje 64-bit, identifikovaný `URI` hodnotu. Můžete taky 32bitová verze nahrazením hodnota identifikátoru URI. Identifikátory URI pro obě verze jsou:

- 64bitový agent Windows - https://go.microsoft.com/fwlink/?LinkId=828603
- 32bitový agent Windows - https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
Tento postup a skriptu příklad neprovede upgrade existujícího agenta.

1. Import xPSDesiredStateConfiguration DSC modulu z [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) do Azure Automation.  
2.  Vytvoření proměnných assetů Azure Automation pro *OPSINSIGHTS_WS_ID* a *OPSINSIGHTS_WS_KEY*. Nastavit *OPSINSIGHTS_WS_ID* ID pracovního prostoru analýzy protokolů OMS a sadu *OPSINSIGHTS_WS_KEY* na primární klíč pracovního prostoru.
3.  Pomocí následujícího skriptu a uložte ho jako MMAgent.ps1
4.  Upravit a potom použijte následující příklad pro instalaci agenta pomocí ve službě Azure Automation DSC. Importujte MMAgent.ps1 do Azure Automation pomocí rozhraní Azure Automation nebo rutinu.
5.  Přiřadíte konfigurace uzlu. Během 15 minut uzlu zkontroluje konfiguraci a MMA vložena do uzlu.

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-the-latest-productid-value"></a>Získat nejnovější hodnotu ProductId

`ProductId value` MMAgent.ps1 skript je jedinečné pro jednotlivé verze agenta. Při publikování aktualizovaná verze všech agentů, změní se hodnota ProductId. Takže když ProductId v budoucnu změní, můžete najít pomocí jednoduchého skriptu verze agenta. Až budete mít nejnovější verze agenta nainstalovat na testovací server, můžete použít následující skript k získání nainstalované ProductId hodnoty. Pomocí nejnovější ProductId hodnoty, můžete aktualizovat hodnotu ve skriptu MMAgent.ps1.

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Nakonfigurujte agenta ručně, nebo přidejte další pracovní prostory
Pokud jste nainstalovali agenty, ale nenakonfigurovali nebo pokud chcete agenta tak, aby odesílaly několik pracovních prostorů, můžete použít následující informace pro povolení agenta nebo překonfigurujte. Po dokončení konfigurace agenta, bude zaregistrovat službu agenta a získají potřebné informace o konfiguraci a sady management Pack, které obsahují informace o řešení.

1. Po instalaci agenta Microsoft Monitoring Agent, otevřete **ovládací panely**.
2. Otevřete **agenta Microsoft Monitoring Agent** a klikněte **Azure Log Analytics (OMS)** kartě.   
3. Klikněte na tlačítko **přidat** otevřete **přidat pracovní prostor Log Analytics** pole.
4. Vložení **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** kterou jste zkopírovali do poznámkového bloku v předchozím postupu pro pracovní prostor, který chcete přidat a pak klikněte na **OK**.  
    ![Konfigurace služby Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)

Po shromáždění dat z počítačů monitorovaných agentem, počet počítačů monitorovaných pomocí OMS se zobrazí na portálu OMS na **připojené zdroje** kartě v **nastavení** jako **servery Připojení**.


## <a name="to-disable-an-agent"></a>Chcete-li zakázat agenta
1. Po instalaci agenta, otevřete **ovládací panely**.
2. Otevřete Microsoft Monitoring Agent a klikněte **Azure Log Analytics (OMS)** kartě.
3. Vyberte pracovní prostor a pak klikněte na tlačítko **odebrat**. Tento krok opakujte pro všechny jiné pracovní prostory.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Volitelně nakonfigurujte agenty informuje o skupině pro správu nástroje Operations Manager

Pokud používáte nástroje Operations Manager v infrastruktuře IT, můžete také použít agenta MMA jako agenta nástroje Operations Manager.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>Konfigurace agentů MMA informuje o skupině pro správu nástroje Operations Manager
1.  Na počítači, kde je nainstalován agent, otevřete **ovládací panely**.  
2.  Otevřete **agenta Microsoft Monitoring Agent** a klikněte **nástroje Operations Manager** karta.  
    ![Karta Microsoft Monitoring Agent Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)
3.  Pokud vaše servery nástroje Operations Manager integrace se službou Active Directory, klikněte na tlačítko **automaticky aktualizovat přiřazení skupin pro správu ze služby AD DS**.
4.  Klikněte na tlačítko **přidat** otevřete **přidat skupinu pro správu** dialogové okno.  
    ![Přidat skupinu pro správu agenta Microsoft Monitoring Agent](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  V **název skupiny pro správu** zadejte název skupiny pro správu.
6.  V **primární server pro správu** zadejte název počítače primární server pro správu.
7.  V **port serveru pro správu** zadejte číslo portu TCP.
8.  V části **účet akce agenta**, zvolte účet místní systém nebo účet místní domény.
9.  Klikněte na tlačítko **OK** zavřete **přidat skupinu pro správu** dialogové okno a pak klikněte na tlačítko **OK** zavřete **vlastnosti agenta monitorování Microsoft** Dialogové okno.


## <a name="next-steps"></a>Další kroky

- Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.
