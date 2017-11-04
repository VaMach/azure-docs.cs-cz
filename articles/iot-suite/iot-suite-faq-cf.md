---
title: "Připojen objekt pro vytváření řešení časté otázky – Azure | Microsoft Docs"
description: "Nejčastější dotazy pro připojené vytváření IoT Suite"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: corywink
ms.openlocfilehash: 1f0d6fb377c21f5ece77a878fd285b5dda2f4fbc
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="frequently-asked-questions-for-iot-suite-connected-factory-preconfigured-solution"></a>Nejčastější dotazy pro IoT Suite připojené vytváření předkonfigurovaného řešení

Viz také Obecné [– nejčastější dotazy](iot-suite-faq.md) pro IoT Suite.

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solution"></a>Kde najdu zdrojový kód pro předkonfigurované řešení?

Zdrojový kód je uložen v úložišti GitHub následující:

* [Připojené objekt pro vytváření předkonfigurovaného řešení](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>Co je OPC UA?

OPC Unified architektura (uživatelský Agent), vydané v 2008, je nezávislý na platformě, orientované na služby interoperabilita standardní. OPC UA používají různé průmyslových systémů a zařízení, jako jsou odvětví počítačů, plc a snímače. OPC UA funkce OPC Classic specifikace integruje do jednoho rozšiřitelná architektura integrované zabezpečení. Je standard, který doprovází OPC Foundation. [OPC Foundation](http://opcfoundation.org/) je pro neziskové organizace s víc než 440 členy. Cílem organizace je používat OPC specifikace usnadňuje interoperabilitu více dodavatele, více platformami, zabezpečený a spolehlivý prostřednictvím:

* Infrastruktura
* Specifikace
* Technologie
* Procesy

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-preconfigured-solution"></a>Proč Microsoft zvolit OPC UA pro připojené objekt pro vytváření předkonfigurovaného řešení?

Microsoft zvolili OPC UA, protože je otevřený, bez chráněné, platformu nezávislé, oborových rozpoznána a osvědčené standardem. Je požadavek na Industrie 4.0 (RAMI4.0) referenční Architektura řešení zajistit interoperabilitu mezi širokou škálu výrobní procesy a vybavení. Microsoft setkává vyžádání od našich zákazníků k sestavení řešení Industrie 4.0. Podpora OPC UA pomáhá snížit bariéry pro zákazníky, abyste dosáhli svých cílů a poskytuje okamžité obchodní hodnotu k nim.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>Jak přidat veřejnou IP adresu pro simulaci virtuálního počítače?

Máte dvě možnosti, které chcete přidat IP adresu:

* Pomocí skriptu prostředí PowerShell `Simulation/Factory/Add-SimulationPublicIp.ps1` v [úložiště](https://github.com/Azure/azure-iot-connected-factory). Název nasazení předat jako parametr. Pro místní nasazení použijte `<your username>ConnFactoryLocal`. Skript vypíše adresu IP virtuálního počítače.

* Na portálu Azure vyhledejte skupinu prostředků vašeho nasazení. S výjimkou místní nasazení skupiny prostředků má název, který jste zadali jako řešení nebo název nasazení. Pro místní nasazení pomocí skriptu buildu, název skupiny prostředků je `<your username>ConnFactoryLocal`. Nyní přidejte nový **veřejnou IP adresu** prostředků do skupiny prostředků.

> [!NOTE]
> V obou případech zajistěte instalaci nejnovějších oprav podle pokynů [Ubuntu webu](https://wiki.ubuntu.com/Security/Upgrades). Aktuálnost instalace pro tak dlouho, dokud virtuální počítač je přístupný prostřednictvím veřejnou IP adresu.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Odebrání veřejnou IP adresu pro simulaci virtuálních počítačů

Máte dvě možnosti pro IP adresu odeberte:

* Použití skriptu prostředí PowerShell Simulation/Factory/Remove-SimulationPublicIp.ps1 z [úložiště](https://github.com/Azure/azure-iot-connected-factory). Název nasazení předat jako parametr. Pro místní nasazení použijte `<your username>ConnFactoryLocal`. Skript vypíše adresu IP virtuálního počítače.

* Na portálu Azure vyhledejte skupinu prostředků vašeho nasazení. S výjimkou místní nasazení skupiny prostředků má název, který jste zadali jako řešení nebo název nasazení. Pro místní nasazení pomocí skriptu buildu, název skupiny prostředků je `<your username>ConnFactoryLocal`. Nyní odebrat **veřejnou IP adresu** prostředků ze skupiny prostředků.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>Jak se mohu přihlásit k simulaci virtuálního počítače?

Přihlášení k simulaci virtuálního počítače je podporována, pouze pokud jste nasadili řešení pomocí skriptu prostředí PowerShell `build.ps1` v [úložiště](https://github.com/Azure/azure-iot-connected-factory).

Pokud jste nasadili řešení od www.azureiotsuite.com, nemůžete se přihlásit k virtuálnímu počítači. Nemůžete se přihlásit, protože je náhodně vygenerované heslo a nelze ho obnovit.

1. Přidejte veřejnou IP adresu do virtuálního počítače. V tématu [jak přidám veřejnou IP adresu pro simulaci virtuálního počítače?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Vytvořte relace SSH do virtuálního počítače pomocí IP adresy virtuálního počítače.
1. Je uživatelské jméno používané k: `docker`.
1. K použití hesla, závisí na verzi, kterou jste použili k nasazení:
    * Pro řešení nasazuje pomocí skriptu build.ps1 před 1. června 2017, je heslo: `Passw0rd`.
    * Řešení nasazuje pomocí skriptu build.ps1 po 1. června 2017, můžete najít v heslo `<name of your deployment>.config.user` souboru. Heslo je uloženo v **VmAdminPassword** nastavení. Heslo je generována náhodně v době nasazení nezadáte pomocí `build.ps1` skript parametr`-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Jak zastavení a spuštění všech procesů docker v simulaci virtuálního počítače?

1. Přihlaste se k simulaci virtuálních počítačů. V tématu [jak se přihlásím k simulaci virtuálního počítače?](#how-do-i-sign-in-to-the-simulation-vm)
1. Chcete-li zkontrolovat, které kontejnery jsou aktivní, spusťte: `docker ps`.
1. Chcete-li ukončit všechny kontejnery simulace, spusťte: `./stopsimulation`.
1. Chcete-li spustit všechny kontejnery simulace:
    * Export proměnnou prostředí s názvem **IOTHUB_CONNECTIONSTRING**. Použijte hodnotu **IotHubOwnerConnectionString** nastavení v `<name of your deployment>.config.user` souboru. Například:

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Spusťte `./startsimulation`.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>Jak aktualizovat simulace ve virtuálním počítači?

Pokud jste udělali změny simulace, můžete použít skript prostředí PowerShell `build.ps1` v [úložiště](https://github.com/Azure/azure-iot-connected-factory) pomocí `updatedimulation` příkaz. Tento skript vytvoří všechny součásti simulace, zastaví simulace ve virtuálním počítači, odešle, nainstaluje a spustí je.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>Jak zjistím připojovací řetězec služby IoT hub používá mém řešení?

Pokud jste nasadili s řešením `build.ps1` skript v [úložiště](https://github.com/Azure/azure-iot-connected-factory), připojovací řetězec je hodnota **IotHubOwnerConnectionString** v `<name of your deployment>.config.user` souboru.

Můžete také získat připojovací řetězec pomocí portálu Azure. V rámci služby IoT Hub prostředku ve skupině prostředků vašeho nasazení vyhledejte nastavení připojovacího řetězce.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Zařízení IoT Hub, která používá simulace připojené vytváření?

Simulace vlastní zaregistruje následující zařízení:

* proxy.Beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.Mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.Rio.corp.contoso
* proxy.SEATTLE.corp.contoso
* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.SEATTLE.corp.contoso

Pomocí [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) nebo [iothub-explorer](https://github.com/azure/iothub-explorer) nástroj, můžete zkontrolovat zařízení, která jsou zaregistrovaná v centru IoT používá vaše řešení. Tyto nástroje používat, budete potřebovat připojovací řetězec pro službu IoT hub ve vašem nasazení.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Načtení dat protokolu z komponenty simulace?

Všechny součásti v simulaci přihlášení informace do souborů protokolu. Tyto soubory můžete najít ve virtuálním počítači ve složce `home/docker/Logs`. Pokud chcete načíst protokoly, můžete použít skript prostředí PowerShell `Simulation/Factory/Get-SimulationLogs.ps1` v [úložiště](https://github.com/Azure/azure-iot-connected-factory).

Tento skript, musí se přihlásit k virtuálnímu počítači. Musíte zadat přihlašovací údaje pro přihlášení. V tématu [jak se přihlásím k simulaci virtuální počítač?](#how-do-i-sign-in-to-the-simulation-vm) najít přihlašovací údaje.

Skript přidá nebo odebere veřejnou IP adresu pro virtuální počítač, pokud ho ještě nemá a odstraní ji. Skript vloží všech souborů protokolů v rámci archivu a stáhne archivu na stanici vývoje.

Případně Přihlaste se k virtuálnímu počítači pomocí protokolu SSH a zkontrolujte soubory protokolu v době běhu.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Jak můžete zkontrolovat, pokud simulaci odesílá data do cloudu?

S [DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) nebo [iothub-explorer](https://github.com/azure/iothub-explorer) nástroj, si můžete prohlédnout data odeslaná do služby IoT Hub z některých zařízení. Tyto nástroje používat, musíte znát připojovací řetězec pro službu IoT hub ve vašem nasazení. V tématu [Jak zjistím připojovací řetězec služby IoT hub používá mém řešení?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Zkontrolujte data odeslaná vydavatele zařízení:

* Publisher.Beijing.corp.contoso
* Publisher.capetown.corp.contoso
* Publisher.Mumbai.corp.contoso
* Publisher.munich0.corp.contoso
* Publisher.Rio.corp.contoso
* Publisher.SEATTLE.corp.contoso

Pokud se žádná data, odeslané do služby IoT Hub, nastane problém s simulace. Jako první krok analysis byste měli provést analýzu souborů protokolu součásti simulace. V tématu [načtení dat protokolu z komponenty simulace?](#how-can-i-get-log-data-from-the-simulation-components) V dalším kroku pokusí zastavit a spustit simulaci a v případě stále nejsou žádná data odesílá, aktualizujte simulaci úplně. V tématu [jak aktualizovat simulace ve virtuálním počítači?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="next-steps"></a>Další kroky

Můžete si taky prostudovat některé další funkce a možnosti předkonfigurovaných řešení sady IoT Suite:

* [Přehled řešení předkonfigurované prediktivní údržby](iot-suite-predictive-overview.md)
* [Přehled připojené objekt pro vytváření předkonfigurovaného řešení](iot-suite-connected-factory-overview.md)
* [Zabezpečení IoT od základů](securing-iot-ground-up.md)