---
title: Co je Azure Automation | Dokumentace Microsoftu
description: "Seznamte se s hodnotou, kterou Azure Automation nabízí, a získejte odpovědi na časté otázky, abyste mohli začít vytváření za pomoci runbooků a Azure Automation DSC."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "co je automatizace, azure automation, příklady azure automation"
ms.assetid: 0cf1f3e8-dd30-4f33-b52a-e148e97802a9
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2016
ms.author: magoedte;bwren
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 6b1fb9e7ae810df21cbcd592fef2b43309e2269c
ms.contentlocale: cs-cz
ms.lasthandoff: 07/10/2017

---
# <a name="azure-automation-overview"></a>Přehled Azure Automation
Microsoft Azure Automation nabízí uživatelům možnost automatizace ručních, dlouhotrvajících, problematických a často se opakujících úloh, které se běžně provádějí v cloudovém a podnikovém prostředí. Šetří čas a zvyšuje spolehlivost běžných administrativních úloh a umí je dokonce naplánovat, aby se v pravidelných intervalech prováděly automaticky. Procesy můžete automatizovat pomocí runbooků nebo můžete automatizovat správu konfigurace pomocí DSC (požadovaný stav konfigurace). Tento článek poskytuje stručný přehled Azure Automation a odpovědi na některé běžné dotazy. Podrobnější informace o dalších tématech najdete v jiných článcích z této knihovny.

## <a name="automating-processes-with-runbooks"></a>Automatizace procesů pomocí runbooků
Runbook představuje sadu úloh, které provádějí určité automatizované procesy ve službě Azure Automation. Může to být jednoduchý proces, například spuštění virtuálního počítače a vytvoření položky protokolu, nebo můžete mít složitý runbook, který kombinuje jiné menší runbooky kvůli provádění složitého procesu v několika prostředcích nebo dokonce v několika cloudech nebo místních prostředích.  

Můžete mít například existující ruční proces na zkracování databáze SQL (pokud se přiblíží maximální velikosti), který zahrnuje několik kroků, například připojení k serveru, připojení k databázi, získání aktuální velikost databáze, kontrolu překročení prahové hodnoty a její následné zkrácení a odeslání oznámení uživateli. Místo ručního provádění jednotlivých kroků můžete vytvořit runbook, který bude všechny tyto úlohy provádět jako jeden proces. Spustíte runbook, zadáte požadované informace (název serveru SQL, název databáze a e-mail příjemce) a potom si v klidu počkáte na dokončení procesu. 

## <a name="what-can-runbooks-automate"></a>Co může runbook zautomatizovat?
Runbooky v Azure Automation staví na prostředí Windows PowerShell nebo na pracovním postupu Windows PowerShell, aby mohly dělat všechno, co zvládne PowerShell. Pokud aplikace nebo služba používá rozhraní API, může s ním runbook pracovat. Pokud pro aplikaci používáte modul PowerShell, můžete modul načíst do Azure Automation a zahrnout tyto rutiny do vašeho runbooku. Runbooky Azure Automation běží v cloudu Azure a mají přístup ke všem cloudovým prostředkům nebo externím prostředkům, které jsou přístupné z cloudu. Pomocí [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) můžou runbooky běžet ve vašem místním datacentru a můžou spravovat místní prostředky. 

## <a name="getting-runbooks-from-the-community"></a>Získávání runbooků od komunity
[Galerie runbooků](automation-runbook-gallery.md#runbooks-in-runbook-gallery) obsahuje runbooky od Microsoftu a od komunity. Můžete je ve vašem prostředí použít beze změny, nebo si je můžete přizpůsobit pro vlastní účely. Jsou užitečné také jako reference, které vás seznámí se způsoby, jak vytvářet vlastní runbooky. Pokud si myslíte, že vaše runbooky můžou být ostatním uživatelům užitečné, můžete je přidat do galerie. 

## <a name="creating-runbooks-with-azure-automation"></a>Vytvoření runbooků pomocí Azure Automation
Můžete [vytvářet vlastní runbooky](automation-creating-importing-runbook.md) od nuly nebo můžete podle svých potřeb upravovat runbooky z [galerie runbooků](http://msdn.microsoft.com/library/azure/dn781422.aspx). Existují čtyři různé [typy runbooků](automation-runbook-types.md), ze kterých si můžete vybrat podle svých požadavků a podle prostředí PowerShell. Pokud chcete raději pracovat přímo s kódem PowerShellu, můžete použít [runbook PowerShellu](automation-runbook-types.md#powershell-runbooks) nebo [runbook pracovního postupu PowerShellu](automation-runbook-types.md#powershell-workflow-runbooks), který upravíte v režimu offline nebo pomocí [textového editoru](http://msdn.microsoft.com/library/azure/dn879137.aspx) na webu Azure Portal. Pokud dáváte přednost úpravě runbooku bez zasahování přímo do kódu, můžete pomocí [grafického editoru](automation-graphical-authoring-intro.md) na webu Azure Portal vytvořit [grafický runbook](automation-runbook-types.md#graphical-runbooks). 

Raději se díváte, než čtete? Podívejte se na níže uvedené video z relace Microsoft Ignite v květnu 2015. Poznámka: Koncepty a funkce popsané v tomto videu jsou sice správné, ale služba Azure Automation se od okamžiku záznamu tohoto videa posunula hodně dopředu a obsahuje teď rozsáhlejší uživatelské rozhraní na webu Azure Portal a podporuje další funkce.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3451/player]
> 
> 

## <a name="automating-configuration-management-with-desired-state-configuration"></a>Automatizace správy konfigurací pomocí DSC (konfigurace požadovaného stavu)
[DSC prostředí PowerShell](https://technet.microsoft.com/library/dn249912.aspx) je platforma pro správu, která umožňuje spravovat, nasazovat a vynucovat konfiguraci pro fyzické hostitele a virtuální počítače pomocí deklarativní syntaxe PowerShellu. Konfigurace můžete definovat na centrálním serveru vyžádané replikace s DSC, kterou můžou cílové počítače automaticky načíst a použít. DSC poskytuje sadu rutin PowerShellu, které můžete použít ke správě konfigurací a prostředků.  

[Azure Automation DSC](automation-dsc-overview.md) je cloudové řešení pro DSC PowerShellu, které poskytuje služby nezbytné pro podniková prostředí.  Svoje prostředky DSC můžete spravovat v Azure Automation a konfigurace můžete používat na virtuálních nebo fyzických počítačích, které je načtou ze serveru vyžádaných replikací s DSC v cloudu Azure.  Poskytuje také služby generování sestav, které informují o důležitých událostech, například když se uzly odchylují od svých přiřazených konfigurací a když se používá nová konfigurace. 

## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>Vytvoření vlastních konfigurací DSC pomocí Azure Automation
[Konfigurace DSC](automation-dsc-overview.md) určují požadovaný stav uzlu.  Stejnou konfiguraci můžete použít na několik uzlů, abyste zajistili, že si všechny udrží identický stav.  Konfiguraci můžete vytvořit pomocí libovolného textového editoru na místním počítači a následně ji naimportovat do Azure Automation, kde ji můžete zkompilovat a použít na uzly.

## <a name="getting-modules-and-configurations"></a>Získávání modulů a konfigurací
[Moduly PowerShellu](automation-runbook-gallery.md#modules-in-powershell-gallery) obsahující rutiny, které můžete použít v runboocích a konfiguracích DSC, můžete získat z [galerie prostředí PowerShell](http://www.powershellgallery.com/). Tuto galerii můžete spustit na webu Azure Portal a moduly můžete importovat přímo do Azure Automation nebo si je můžete stáhnout a naimportovat je ručně. Moduly nemůžete instalovat přímo z webu Azure Portal, ale můžete si je stáhnout a nainstalovat je stejně jako ostatní moduly. 

## <a name="example-practical-applications-of-azure-automation"></a>Příklad použití Azure Automation v praxi
Následující řádky představují jenom několik příkladů různých druhů scénářů pro použití Azure Automation. 

* Vytvoření a zkopírování virtuálních počítačů v různých předplatných Azure. 
* Naplánovaný soubor se zkopíruje z místního počítače do kontejneru služby Azure Blob Storage. 
* Automatizace funkcí zabezpečení, například odepření požadavků od klienta při zjištění útoku DoS. 
* Zajištění, aby počítače neustále dodržovaly nakonfigurované zásady zabezpečení.
* Správa průběžného nasazování kódu aplikace v cloudové a místní infrastruktuře. 
* Sestavení doménové struktury služby Active Directory v Azure pro prostředí laboratoře. 
* Zkrácení tabulky v databázi SQL, pokud se databáze blíží k dosažení maximální velikosti. 
* Vzdálená aktualizace nastavení prostředí pro web Azure. 

## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Vztah Azure Automation k ostatním nástrojům pro automatizaci
[Service Management Automation (SMA)](http://technet.microsoft.com/library/dn469260.aspx) slouží k automatizaci úloh správy v privátním cloudu. Instaluje se místně v datovém centru jako součást sady [Microsoft Azure Pack](https://www.microsoft.com/en-us/server-cloud/). SMA a Azure Automation používají stejný formát runbooků založený na prostředí Windows PowerShell a pracovním postupu prostředí Windows PowerShell, ale SMA nepodporuje [grafický runbook](automation-graphical-authoring-intro.md).  

[System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) slouží k automatizaci místních prostředků. Používá jiný formát runbooků než Azure Automation a Service Management Automation a má grafické rozhraní pro vytváření runbooků bez nutnosti jakéhokoli skriptování. Jeho runbooky se skládají z aktivit z integračních balíčků, které jsou napsané konkrétně pro orchestrátor. 

## <a name="where-can-i-get-more-information"></a>Zdroje dalších informací
K dispozici jsou různé druhy zdrojů, ze kterých můžete získat další informace o Azure Automation a vytváření vlastních runbooků. 

* **Knihovna Azure Automation** je místem, kde se právě teď nacházíte. Články v této knihovně nabízejí ucelenou dokumentaci o konfiguraci a správě Azure Automation a o vytváření vlastních runbooků. 
* [Rutiny Azure PowerShell](http://msdn.microsoft.com/library/jj156055.aspx) obsahují informace pro automatizaci operace v Azure pomocí Windows PowerShellu. Runbooky tyto rutiny používají k práci s prostředky Azure. 
* [Blog o správě](https://azure.microsoft.com/blog/tag/azure-automation/) obsahuje nejnovější informace o Azure Automation a dalších technologiích pro správu od Microsoftu. Pokud chcete dostávat nejnovější informace od týmu Azure Automation, přihlaste se k odběru tohoto blogu. 
* [Fórum Automation](http://go.microsoft.com/fwlink/p/?LinkId=390561) vám umožňuje pokládat Microsoftu a komunitě dotazy týkající se Azure Automation. 
* [Rutiny Azure Automation](https://msdn.microsoft.com/library/mt244122.aspx) poskytují informace pro automatizaci úloh správy. Obsahují rutiny pro správu účtů, prostředků, runbooků a DSC služby Automation.

## <a name="can-i-provide-feedback"></a>Posílání připomínek
**Budeme velmi rádi za vaše připomínky.** Pokud hledáte řešení runbooku služby Azure Automation nebo hledáte modul integrace, zadejte v Centru skriptů požadavek na skript. Pokud máte požadavky k připomínkám nebo funkcím Azure Automation, odešlete je na [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback). Děkujeme! 


