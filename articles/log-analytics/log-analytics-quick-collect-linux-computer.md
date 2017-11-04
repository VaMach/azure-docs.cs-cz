---
title: "Shromažďování dat z místního počítače se systémem Linux s Azure Log Analytics | Microsoft Docs"
description: "Zjistěte, jak nasadit agenta analýzy protokolů pro Linux a Povolit shromažďování dat z tohoto operačního systému s analýzy protokolů."
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
ms.date: 10/13/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: d22fe6456c3bd886f8f8863d362c0084fbe03da3
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="collect-data-from-linux-computers-hosted-in-your-environment"></a>Shromažďování dat z počítače se systémem Linux hostované ve vašem prostředí
[Azure Log Analytics](log-analytics-overview.md) shromažďovat data přímo z fyzických nebo virtuálních počítačů Linux a dalším prostředkům ve vašem prostředí do jednoho úložiště pro podrobné analýzy a korelace.  Tento rychlý start se dozvíte, jak nakonfigurovat a shromažďovat data z vašeho počítače Linux s několika jednoduchými kroky.  Pro virtuální počítače Linux Azure, naleznete v následujícím tématu [shromažďovat data o virtuálních počítačích Azure](log-analytics-quick-collect-azurevm.md).  
 
Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="log-in-to-azure-portal"></a>Přihlaste se k portálu Azure
Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Vytvoření pracovního prostoru
1. Na portálu Azure klikněte na tlačítko **další služby** v levém dolním nalezen. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **protokolu analýzy**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Klikněte na tlačítko **vytvořit**a potom vyberte možnosti pro následující položky:

  * Zadejte název nové **pracovním prostorem OMS**, jako například *DefaultLAWorkspace*. 
  * Vyberte **předplatné**, které má být cílem propojení, výběrem z rozevíracího seznamu, pokud výchozí vybrané předplatné není vhodné.
  * Pro **skupiny prostředků**, vyberte existující skupinu prostředků, který obsahuje jeden nebo více virtuálních počítačích Azure.  
  * Vyberte **umístění** nasazených virtuálních počítačů.  Další informace najdete v tématu který [analýzy protokolů je k dispozici v oblastech](https://azure.microsoft.com/regions/services/).
  * Můžete zvolit ze tří různých **cenové úrovně** v analýzy protokolů, ale pro tento rychlý start, chcete-li vybrat **volné** vrstvy.  Další informace o konkrétní úrovních najdete v tématu [podrobnosti o cenách na Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Po zadání požadovaných informací na **pracovním prostorem OMS** podokně klikněte na tlačítko **OK**.  

Během ověřování informací a vytváření pracovního prostoru můžete průběh zpracování sledovat prostřednictvím položky nabídky **Oznámení**. 

## <a name="obtain-workspace-id-and-key"></a>Získání ID a klíč
Před instalací agenta OMS pro Linux, je nutné ID a klíč vašeho pracovního prostoru analýzy protokolů.  Tyto informace se vyžadují agenta obálky skript tak, aby správně nakonfigurovat agenta a zajistěte, aby že mohla úspěšně komunikovat s analýzy protokolů.  

1. Na portálu Azure klikněte na tlačítko **další služby** v levém dolním nalezen. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **protokolu analýzy**.
2. V seznamu analýzy protokolů pracovních prostorů, vyberte *DefaultLAWorkspace* vytvořili dříve.
3. Vyberte **upřesňující nastavení**.<br><br> ![Nastavení zálohy analýzy protokolů](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Vyberte **připojené zdroje**a potom vyberte **servery se systémem Linux**.   
5. Hodnota napravo od **ID pracovního prostoru** a **primární klíč**. Zkopírujte a vložte do vašeho oblíbeného editoru.   

## <a name="install-the-agent-for-linux"></a>Nainstalujte agenta pro Linux
Následující kroky konfigurace instalace agenta pro analýzy protokolů v cloudu Azure a Azure Government.  

1. Chcete-li konfigurovat počítače se systémem Linux se připojit k analýze protokolů, spusťte následující příkaz poskytnutí ID pracovního prostoru a primární klíč zkopírovali dříve.  Tento příkaz stáhne agenta, ověří jeho kontrolního součtu a ho nainstaluje. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Chcete-li konfigurovat počítače se systémem Linux se připojit k analýze protokolů v cloudu Azure Government, spusťte následující příkaz poskytnutí ID pracovního prostoru a primární klíč zkopírovali dříve.  Tento příkaz stáhne agenta, ověří jeho kontrolního součtu a ho nainstaluje. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

## <a name="configure-agent-to-communicate-with-a-proxy-server"></a>Konfigurace agenta pro komunikaci se serverem proxy

Pokud vaše počítače Linux je nutné komunikovat prostřednictvím proxy serveru k analýze protokolů, proveďte následující kroky.  Hodnota konfigurace proxy serveru má následující syntaxi `[protocol://][user:password@]proxyhost[:port]`.

1. Upravte soubor `/etc/opt/microsoft/omsagent/proxy.conf` tak, že spustíte následující příkazy a změnit hodnoty na konkrétní nastavení.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Restartujte agenta spuštěním následujícího příkazu: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Shromáždit data události a výkonu
Analýzy protokolů můžete shromažďovat události z Linux Syslog a čítače výkonu, které zadáte pro delší období analýzu a vytváření sestav a provést akci, když je zjištěna určitá podmínka.  Postupujte podle těchto kroků nakonfigurujete začínat shromažďování událostí z Linux Syslog a několik běžných čítačů výkonu.  

1. Vyberte **Syslog**.  
2. Protokol událostí přidáte zadáním názvu do protokolu.  Typ **Syslog** a klikněte na symbol plus  **+** .  
3. V tabulce, zrušte zaškrtnutí políčka závažnosti **informace**, **oznámení** a **ladění**. 
4. Klikněte na tlačítko **Uložit** v horní části stránky a uložte konfiguraci.
5. Vyberte **údaje o výkonu Linux** jak povolit kolekce čítače výkonu v počítači se systémem Windows. 
6. Při první konfiguraci čítače výkonu Linux pro nový pracovní prostor analýzy protokolů, budete mít možnost rychle vytvořit několik běžných čítačů. Jsou uvedeny s zaškrtávací políčko vedle každého.<br><br> ![Čítače výkonu systému Windows výchozí vybraná](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br><br> Klikněte na tlačítko **přidat vybrané čítače výkonu**.  Jsou přidány a předvolby s ukázkového intervalu deset druhé kolekci.  
7. Klikněte na tlačítko **Uložit** v horní části stránky a uložte konfiguraci.

## <a name="view-data-collected"></a>Zobrazení data shromážděná
Teď, když povolíte shromažďování dat, umožňuje spustit v příkladu jednoduché protokolu hledání se zobrazí některá data z cílového počítače.  

1. Na portálu Azure přejděte k analýze protokolů a vyberte pracovní prostor vytvořili dříve.
2. Klikněte na tlačítko **hledání protokolů** dlaždici a v podokně hledání protokolů v dotazu pole typu `Perf` a potom stiskněte klávesu enter nebo klikněte na tlačítko vyhledávání napravo od pole dotazu.<br><br> ![Příklad dotazu vyhledávání protokolu analýzy protokolů](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Dotaz na následujícím obrázku například vrátil 735 záznamy výkonu.<br><br> ![Výsledek hledání protokolu analýzy protokolů](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud již nepotřebujete, můžete odebrat agenta z počítače se systémem Linux a odstranit pracovní prostor analýzy protokolů.  

Odebrat agenta, proveďte následující kroky.

1. Stažení agenta systému Linux [universal skriptu](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) do počítače.
2. Spustit w sady .sh souboru *– mazání* argument v počítači, který kompletně odebere agent a jeho konfigurace.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

Chcete-li odstranit pracovní prostor, vyberte pracovní prostor analýzy protokolů, které jste vytvořili dříve a na stránce prostředků, klikněte na tlačítko **odstranit**.<br><br> ![Odstranit prostředek analýzy protokolů](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Další kroky
Teď, když shromažďujete provozní a údaje o výkonu z vašeho počítače Linux místně, můžete snadno začít zkoumat, analýze a pořízení akce na data, která shromáždíte pro *volné*.  

Informace o tom můžete zobrazit a analyzovat data, i nadále tento kurz.   

> [!div class="nextstepaction"]
> [Zobrazit a analyzovat data v analýzy protokolů](log-analytics-tutorial-viewdata.md)
