---
title: "Shromažďování dat z místních počítačů s Linuxem pomocí Azure Log Analytics | Dokumentace Microsoftu"
description: "Zjistěte, jak nasadit agenta Log Analytics pro Linux a povolit shromažďování dat z tohoto operačního systému pomocí Log Analytics."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/23/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 839fc3a326dca8b60c6750231b06d2369c3de2fc
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="collect-data-from-linux-computers-hosted-in-your-environment"></a>Shromažďování dat z počítačů s Linuxem hostovaných ve vašem prostředí
[Azure Log Analytics](log-analytics-overview.md) může shromažďovat data přímo z fyzických nebo virtuálních počítačů s Linuxem a dalších prostředků ve vašem prostředí do jednoho úložiště pro účely podrobných analýz a korelace.  Tento rychlý start ukazuje, jak v několik snadných krocích nakonfigurovat počítač s Linuxem a shromažďovat z něj data.  Informace týkající se virtuálních počítačů Azure s Linuxem najdete v tématu [Shromažďování dat o virtuálních počítačích Azure](log-analytics-quick-collect-azurevm.md).  

Pokud chcete porozumět síťovým a systémovým požadavkům pro nasazení agenta pro Linux, přečtěte si téma věnované [shromažďování dat z vašeho prostředí pomocí Azure Log Analytics](log-analytics-concept-hybrid.md#prerequisites).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="log-in-to-azure-portal"></a>Přihlášení k webu Azure Portal
Přihlaste se k webu Azure Portal na adrese [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Vytvoření pracovního prostoru
1. Na webu Azure Portal klikněte v levém dolním rohu na **Další služby**. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **Log Analytics**.<br><br> ![portál Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Klikněte na **Vytvořit** a podle potřeby změňte hodnoty následujících položek:

  * Zadejte název nového **pracovního prostoru OMS**, například *DefaultLAWorkspace*. 
  * Vyberte **předplatné**, které má být cílem propojení, výběrem z rozevíracího seznamu, pokud výchozí vybrané předplatné není vhodné.
  * Jako **skupinu prostředků** vyberte existující skupinu prostředků, která obsahuje jeden nebo několik virtuálních počítačů Azure.  
  * Vyberte **Umístění**, do kterého jsou vaše virtuální počítače nasazené.  Další informace najdete na stránce uvádějící [oblasti, ve kterých je dostupná služba Log Analytics](https://azure.microsoft.com/regions/services/).
  * V Log Analytics si můžete vybrat mezi třemi různými **cenovými úrovněmi**, ale pro účely tohoto rychlého startu vyberete úroveň **Free**.  Další informace o jednotlivých úrovních najdete v [podrobnostech o cenách Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Po zadání požadovaných informací v podokně **Pracovní prostor OMS** klikněte na **OK**.  

Během ověřování informací a vytváření pracovního prostoru můžete průběh zpracování sledovat prostřednictvím položky nabídky **Oznámení**. 

## <a name="obtain-workspace-id-and-key"></a>Získání ID a klíče pracovního prostoru
Před instalací agenta OMS pro Linux potřebujete ID a klíč vašeho pracovního prostoru Log Analytics.  Tyto informace vyžaduje skript obálky agenta ke správné konfiguraci agenta a zajištění, aby agent mohl úspěšně komunikovat s Log Analytics.  

1. Na webu Azure Portal klikněte v levém dolním rohu na **Další služby**. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **Log Analytics**.
2. V seznamu pracovních prostorů Log Analytics vyberte *DefaultLAWorkspace* vytvořený dříve.
3. Vyberte **Upřesňující nastavení**.<br><br> ![Upřesňující nastavení Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Vyberte **Připojené zdroje** a pak **Servery s Linuxem**.   
5. Napravo se zobrazí hodnoty **ID pracovního prostoru** a **Primární klíč**. Obě hodnoty zkopírujte a vložte do oblíbeného editoru.   

## <a name="install-the-agent-for-linux"></a>Instalace agenta pro Linux
V následujících krocích se nakonfiguruje instalace agenta pro Log Analytics v Azure a cloudu Azure Government.  

>[!NOTE]
>Agenta OMS pro Linux není možné nakonfigurovat tak, aby se hlásil více než jednomu pracovnímu prostoru Log Analytics.  

1. Pokud chcete nakonfigurovat počítač s Linuxem pro připojení k Log Analytics, spusťte následující příkaz a zadejte do něj ID a primární klíč pracovního prostoru, které jste zkopírovali dříve.  Tento příkaz stáhne agenta, ověří jeho kontrolní součet a nainstaluje ho. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Pokud chcete nakonfigurovat počítač s Linuxem pro připojení k Log Analytics v cloudu Azure Government, spusťte následující příkaz a zadejte do něj ID a primární klíč pracovního prostoru, které jste zkopírovali dříve.  Tento příkaz stáhne agenta, ověří jeho kontrolní součet a nainstaluje ho. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

## <a name="configure-agent-to-communicate-with-a-proxy-server"></a>Konfigurace komunikace agenta pomocí proxy serveru

Následující kroky proveďte, pokud vaše počítače s Linuxem potřebují komunikovat s Log Analytics prostřednictvím proxy serveru.  Hodnota konfigurace proxy má následující syntaxi: `[protocol://][user:password@]proxyhost[:port]`.  Vlastnost *proxyhost* přijímá plně kvalifikovaný název domény nebo IP adresu proxy serveru.    

1. Spuštěním následujících příkazů upravte soubor `/etc/opt/microsoft/omsagent/proxy.conf` a změňte hodnoty na vaše konkrétní nastavení.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Restartujte agenta spuštěním následujícího příkazu: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Shromažďování dat o událostech a výkonu
Log Analytics může shromažďovat události z protokolu Syslog v Linuxu a z čítačů výkonu, které určíte pro dlouhodobější analýzu a generování sestav, a provést akci při zjištění konkrétní podmínky.  Postupujte podle těchto kroků a pro začátek nakonfigurujte shromažďování událostí z protokolu Syslog v Linuxu a několika běžných čítačů výkonu.  

1. Vyberte **Syslog**.  
2. Protokol událostí přidáte zadáním názvu protokolu.  Zadejte **Syslog** a klikněte na symbol plus **+**.  
3. V tabulce zrušte zaškrtnutí závažností **Informace**, **Oznámení** a **Ladění**. 
4. Uložte konfiguraci kliknutím na **Uložit** v horní části stránky.
5. Výběrem **Data o výkonu systému Linux** povolte shromažďování čítačů výkonu na počítači s Linuxem. 
6. Při první konfiguraci linuxových čítačů výkonu pro nový pracovní prostor Log Analytics máte možnost rychle vytvořit několik běžných čítačů. Jsou zobrazené v seznamu a vedle každého je zaškrtávací políčko.<br><br> ![Vybrané výchozí čítače výkonu Windows](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png)<br><br> Klikněte na **Přidat vybrané čítače výkonu**.  Čítače se přidají a přednastaví s použitím ukázkového desetisekundového intervalu shromažďování.  
7. Uložte konfiguraci kliknutím na **Uložit** v horní části stránky.

## <a name="view-data-collected"></a>Zobrazení shromážděných dat
Teď, když jste povolili shromažďování dat, můžete spustit příklad jednoduchého prohledávání protokolu a zobrazit nějaká data z cílového počítače.  

1. Na webu Azure Portal přejděte do Log Analytics a vyberte pracovní prostor vytvořený dříve.
2. Klikněte na dlaždici **Prohledávání protokolu**, v podokně Prohledávání protokolu zadejte do pole dotazu `Perf` a stiskněte Enter nebo klikněte na tlačítko Vyhledat napravo od pole dotazu.<br><br> ![Příklad dotazu prohledávání protokolu v Log Analytics](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Například dotaz na následujícím obrázku vrátil 735 záznamů o výkonu.<br><br> ![Výsledek prohledávání protokolu v Log Analytics](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud je už nepotřebujete, můžete z počítače s Linuxem odebrat agenta a odstranit pracovní prostor Log Analytics.  

Pokud chcete odebrat agenta, proveďte následující kroky.

1. Stáhněte na počítač [univerzální skript](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) agenta pro Linux.
2. Spusťte na počítači soubor .sh sady s argumentem *--purge*. Tím se kompletně odebere agent a jeho konfigurace.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

Pokud chcete odstranit pracovní prostor, vyberte pracovní prostor Log Analytics, který jste vytvořili dříve, a na stránce prostředku klikněte na **Odstranit**.<br><br> ![Odstranění prostředku Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Další kroky
Teď, když shromažďujete data o provozu a výkonu z místního počítače s Linuxem, můžete jednoduše a *zdarma* začít zkoumat a analyzovat shromážděná data a provádět na jejich základě akce.  

Pokud chcete zjistit, jak zobrazit a analyzovat data, pokračujte k následujícímu kurzu.   

> [!div class="nextstepaction"]
> [Zobrazení nebo analýza dat v Log Analytics](log-analytics-tutorial-viewdata.md)
