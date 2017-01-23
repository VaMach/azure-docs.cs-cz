---
title: "Zpracování chyb v grafických runboocích Azure Automation | Dokumentace Microsoftu"
description: "Tento článek popisuje, jak implementovat logiku zpracování chyb v grafických runboocích Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/26/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: f9b359691122da9e5d93e51f3085cad51e55d8f2
ms.openlocfilehash: 13c3e1693badcf4148738cb63666f34546d1696c

---

# <a name="error-handling-in-azure-automation-graphical-runbooks"></a>Zpracování chyb v grafických runboocích Azure Automation

Klíčovým aspektem návrhu runbooků, na který je třeba pamatovat, je identifikace různých problémů, se kterými se runbook může setkat – například úspěch, očekávané chybové stavy a neočekávané chybové podmínky.  Runbooky by měly zahrnovat zpracování chyb, aby je mohly odpovídajícím způsobem detekovat.  Kdybyste u grafických runbooků chtěli vhodným způsobem ověřit výstup aktivity nebo zpracovat chybu, možná byste pokračovali v psaní kódu PowerShellu, definovali podmíněnou logiku pro výstupní propojení aktivity nebo použili jinou metodu.          

Při provádění runbooků se často stává, že pokud v aktivitě dojde k chybě, která ale nezpůsobí ukončení, zpracují se i následující aktivity bez ohledu na chybu.  Samozřejmě nejspíše dojde k vyvolání výjimky, ale podstatné je, že je umožněno spuštění další aktivity. Důvodem takového chování je způsob, jakým je navrženo zpracování chyb v jazyce PowerShellu.    

Během provádění se mohou objevit dva typy chyb PowerShellu – ukončující nebo neukončující.  Jsou mezi nimi tyto rozdíly:

* Ukončující chyba: závažná chyba během provádění, která úplně zastaví příkaz (nebo provádění skriptu). Příkladem mohou být neexistující rutiny, chyby syntaxe znemožňující spuštění rutiny nebo jiné závažné chyby.

* Neukončující chyba: méně závažná chyba, která umožňuje pokračovat v provádění bez ohledu na selhání. Jedná se například o provozní chyby, jako je nenalezení souboru, problémy s oprávněním atd.

Grafické runbooky Azure Automation byly vylepšeny o schopnost zahrnovat zpracování chyb, takže je teď možné přepnout výjimky na neukončující chyby a vytvořit chybová propojení mezi aktivitami. Díky tomu může autor runbooku zachytávat chyby a má možnost obsloužit očekávané i neočekávané situace.  

## <a name="when-to-use"></a>Kdy je použít

Řízení provádění pracovního postupu je důležité pro zajištění, že kdykoli nějaká důležitá aktivita vyvolá chybu nebo výjimku, můžete zabránit pokračování na další aktivitu v runbooku a odpovídajícím způsobem chybu zpracovat.  To je obzvláště nutné v případě, kdy na vašich runboocích závisí podnikový proces nebo proces provozu služeb.

Pro každou aktivitu, která by mohla vést k chybě, může autor runbooku přidat chybové propojení odkazující na jinou aktivitu.  Cílová aktivita může být libovolného typu: aktivita s kódem, aktivita vyvolávající rutinu, aktivita vyvolávající jiný runbook nebo cokoli jiného. 

Kromě toho může cílová aktivita také obsahovat odchozí propojení (běžná nebo chybová), takže autor runbooku může implementovat komplexní logiku zpracování chyb bez nutnosti vkládat aktivitu s kódem.  Přestože doporučeným postupem je vytvoření vyhrazeného runbooku se společnými funkcemi pro zpracování chyb, není to nutné, a logika zpracování chyb v aktivitě s kódem PowerShellu není jedinou možností.  

Mějme například runbook, který se pokusí spustit virtuální počítač a nainstalovat v něm aplikaci, ale pokud se virtuální počítač nespustí správně, provede runbook dvě akce: 

1. Odešle upozornění na tento problém.
2. Spustí jiný runbook, který automaticky zřídí nový virtuální počítač. 

Jedním řešením může být vytvoření chybového propojení odkazujícího na aktivitu pro zpracování kroku 1 (například rutina **Write-Warning**), která je propojená s aktivitou pro provedení kroku 2 (například rutina **Start-AzureRmAutomationRunbook**). 

Toto chování můžete také zobecnit pro použití v mnoha runboocích tím, že obě aktivity umístíte do samostatného runbooku pro zpracování chyb podle výše navrhovaných pokynů.  Před voláním tohoto runbooku můžete z dat v původním runbooku vytvořit vlastní zprávu a předat ji jako parametr do runbooku pro zpracování chyb. 

## <a name="how-to-use"></a>Způsob použití

V konfiguraci každé aktivity je možnost přepnout výjimky na neukončující chyby. Ve výchozím nastavení je tato možnost zakázána.  Měli byste ji povolit u všech aktivit, pro které chcete zpracovávat chyby.  Povolením této možnosti zajistíte, že se s ukončujícími i neukončujícími chybami v aktivitě bude zacházet jako s neukončujícími chybami, a následně je můžete zpracovat pomocí chybového propojení.  Po konfiguraci tohoto nastavení vytvoříte aktivity, které budou zpracovávat chyby.  Pokud v aktivitě dojde k jakékoli chybě, použijí se odchozí chybová propojení a nikoli běžná propojení; běžná propojení se nepoužijí ani v případě, že aktivita vygenerovala kromě chyby i běžný výstup.<br><br> ![Příklad chybového propojení v runbooku Automation](media/automation-runbook-graphical-error-handling/error-link-example.png)

V následujícím jednoduchém příkladu máme runbook, který načte proměnnou obsahující název virtuálního počítače a pak se pokusí spustit virtuální počítač pomocí další aktivity.<br><br> ![Příklad zpracování chyb v runbooku Automation](media/automation-runbook-graphical-error-handling/runbook-example-error-handling.png)<br><br>      

Aktivity **Get-AutomationVariable** a **Start-AzureRmVm** jsou nakonfigurované, aby převáděly výjimky na chyby.  Pokud dojde k problémům se získáním proměnné nebo spuštěním virtuálního počítače, vygenerují se chyby.<br><br> ![Nastavení aktivity pro zpracování chyb v runbooku Automation](media/automation-runbook-graphical-error-handling/activity-blade-convertexception-option.png)

Chybová propojení vedou z těchto aktivit do jediné aktivity **Error Management** (aktivita s kódem). V té je nakonfigurovaný jednoduchý výraz PowerShellu, který pomocí klíčového slova *Throw* zastaví zpracování a prostřednictvím *$Error.Exception.Message* získá zprávu s popisem aktuální výjimky.<br><br> ![Příklad kódu pro zpracování chyb v runbooku Automation](media/automation-runbook-graphical-error-handling/runbook-example-error-handling-code.png)


## <a name="next-steps"></a>Další kroky

* Další informace o propojeních a vysvětlení typů propojení v grafických runboocích najdete v tématu [Vytváření grafického obsahu v Azure Automation](automation-graphical-authoring-intro.md#links-and-workflow).

* Další informace o spouštění runbooků, postupy při monitorování úloh runbooků a další technické podrobnosti najdete v článku [Sledování úlohy runbooku](automation-runbook-execution.md). 


<!--HONumber=Jan17_HO1-->


