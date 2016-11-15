---
title: SQL Azure s Azure RemoteAppem | Dokumentace Microsoftu
description: "Naučte se používat SQL Azure s Azure RemoteAppem."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
editor: 
ms.assetid: 35f81d75-bfd7-4980-807e-00339f2cb2a4
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 08/15/2016
ms.author: elizapo
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: cfd3da08a8c8674e686ae2933db331809fb0e34d


---
# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure s Azure RemoteAppem
> [!IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Když se zákazníci rozhodnou, že budou své aplikace hostovat v cloudu pomocí Azure RemoteAppu, často chtějí do cloudu migrovat také svá data, například na serverech SQL, aby byl přesun nasazení do cloudu úplný. Tímto způsobem lze vytvářet řešení hostovaná čistě v cloudu, ke kterým lze kdykoliv a kdekoliv přistupovat pomocí Azure RemoteAppu. Níže naleznete odkazy na zdroje a reference spolu s pokyny, které vás tímto procesem provedou.  

## <a name="migrate-your-sql-data"></a>Migrace dat SQL
Začněte [migrací databáze SQL Server do databáze Azure SQL](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfigurace Azure RemoteAppu
Aplikaci pro Windows hostujte v Azure RemoteAppu. Zde jsou základní kroky:

1. Vytvořte [virtuální počítač podle šablony Azure RemoteAppu](remoteapp-imageoptions.md). 
2. Do virtuálního počítače nainstalujte požadovanou aplikaci.
3. Nakonfigurujte aplikaci tak, aby se připojovala k databázi SQL a ověřte, že připojení funguje.
4. Zpracujte virtuální počítač pomocí nástroje Sysprep a vypněte jej. Výsledek zaznamenejte jako image k použití s Azurem. **Poznámka:** Pomocí procesu nástroje Sysprep je nutné zajistit, aby si aplikace uchovala informace o připojení k databázi. Pokud si aplikace neuchová informace o připojení k databázi, může být vhodné obrátit se na výrobce aplikace a zjistit, jak lze zadat připojovací řetězec.
5. Importujte vlastní image do knihovny Azure RemoteAppu a vyberte správné zeměpisné umístění svého nasazení SQL Azure. 
6. Pomocí výše uvedené šablony nasaďte kolekci RemoteAppu ve stejném datovém centru jako své nasazení SQL Azure a aplikaci publikujte. Nasazení Azure RemoteAppu ve stejném datovém centru jako SQL Azure pomáhá zajistit nejrychlejší připojení a snižovat latenci. 

## <a name="app-and-sql-configuration-considerations"></a>Požadavky na konfiguraci aplikace a SQL:
Při používání Azure SQL s RemoteAppem je nutné zvážit několik věcí:

Přečtěte si [jak konfigurovat bránu firewall databáze Azure SQL](../sql-database/sql-database-firewall-configure.md). Výňatek ze článku uvádí: „Zpočátku je veškerý přístup k vaší službě Azure SQL Database blokován branou firewall. Pokud chcete používat svůj server služby Azure SQL Database, musíte přejít na portál Azure Classic a zadat jedno nebo více pravidel brány firewall na úrovni serveru, která umožní přístup na server služby Azure SQL Database. Pomocí pravidel brány firewall určete, které rozsahy IP adres z internetu jsou povolené a zda se mohou aplikace Azure pokoušet připojovat k vašemu serveru služby Azure SQL Database.“

Když se počítač pokusí z internetu připojit k databázovému serveru, brána firewall pomocí úplné sady pravidel brány firewall na úrovni serveru a (pokud je to nutné) na úrovni databáze zkontroluje zdrojovou IP adresu požadavku. „Pokud je IP adresa požadavku v jednom z rozsahů určených v pravidlech brány firewall na úrovni serveru, připojení k vašemu serveru služby Azure SQL Database je povoleno.“ Lze tedy používat rozsahy IP adres, a není nutné používat pouze jednotlivé zdrojové IP adresy.

Rozsah IP adres určete pomocí podrobných pokynů v tématu [Postup: Konfigurace nastavení brány firewall pro službu SQL Database pomocí webu Azure Portal](../sql-database/sql-database-configure-firewall-settings.md). Při konfiguraci pravidel brány firewall pro SQL zadejte rozsah IP adres podsítě, která je určena pro kolekci Azure RemoteAppu. To by mělo serverům ARA umožňovat připojení k databázi SQL, i když budou mít dynamicky přidělované IP adresy.

## <a name="troubleshooting"></a>Řešení potíží
Pokud má klientská aplikace hostovaná v Azure RemoteAppu používající databázi SQL, která je také hostovaná v Azure, nebo místní databázi nízký výkon, může to být z několika důvodů.  

* Síť používaná k připojení vašeho zařízení k Azure má vysokou latenci. K dosažení nejlepšího výkonu přejděte na nejlepší a nejrychlejší možné síťové připojení. Jako obecný nástroj k testování latence připojení svých zařízení k datovému centru Azure používejte web[azurespeed.com](http://azurespeed.com/).  
* Klientská aplikace hostovaná v Azure RemoteAppu je vytížená. Vyberte jiný fakturační plán, například Premium, který umožní zvýšení výkonu. Další možností je monitorování prostředků, které aplikace používá: v aktivní relaci stiskněte kombinaci kláves Ctrl+Alt+End, která spustí obrazovku SAS, vyberte Správce úloh a sledujte, jak vaše aplikace používá prostředky.
* Server SQL je vytížený nebo není optimalizovaný. Postupujte pokynů k odstraňování potíží se serverem SQL. 




<!--HONumber=Nov16_HO2-->


