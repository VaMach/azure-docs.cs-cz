---
title: "Správa Azure Key Vault pomocí Azure Automation | Microsoft Docs"
description: "Další informace o používání služby Azure Automation pro správu Azure Key Vault."
services: Key-Vault, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
ms.openlocfilehash: dee39662472fe54776b591977f2b1ecb39d15b00
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a>Správa Azure Key Vault pomocí Azure Automation.
Tento průvodce vás seznámí s služba Azure Automation a jak může sloužit ke zjednodušení správy klíčů a tajných klíčů v Azure Key Vault.

## <a name="what-is-azure-automation"></a>Co je Azure Automation?
[Služby Azure Automation](../automation/automation-intro.md) je služba Azure pro zjednodušenou správu cloudu pomocí automatizace procesů a konfigurace požadovaného stavu. Pomocí Azure Automation, ruční, opakované, dlouhotrvajících a k chybám úloh je možné automatizovat zvýšit spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.

Azure Automation nabízí modul provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, který rozšiřuje podle vašich potřeb. Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.

Snížit provozní režie a uvolněte IT a zaměstnanci DevOps a zaměřit se na práci, kterou přidá obchodní hodnotu přesunutím vašeho cloudu spuštění úloh správy se automaticky automatizace Azure.

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Jak Azure Automation pomoci spravovat Azure Key Vault?
Key Vault můžete spravovat ve službě Azure Automation pomocí [rutin AzureRM Key Vault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) a [rutin Azure Classic Key Vault](https://msdn.microsoft.com/library/azure/dn868052.aspx). Modul Azure pro správu classic Key Vault je k dispozici automaticky ve službě Azure Automation a můžete importovat [AzureRM KeyVault modulu](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) do Azure Automation, tak, aby bylo možné provádět mnoho úloh správy vaší Key Vault v rámci služby. Může také párovat tyto rutiny ve službě Azure Automation pomocí rutin pro jinými službami Azure, na automatizují komplexní úlohy v služeb Azure a systémech 3. stran.

Pomocí rutiny Azure Key Vault můžete provádět tyto úlohy mimo jiné: 

* Vytvoření a konfigurace trezoru klíčů
* Vytvoření nebo import klíče
* Vytvořit nebo aktualizovat tajný klíč
* Aktualizovat atributy klíče
* Získat klíč nebo tajný klíč
* Odstranění klíče nebo tajného klíče

Tady jsou některé příklady použití Powershellu ke správě Key Vault:  

* [Azure Key Vault - krok za krokem](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Nastavení a konfiguraci Azure Key Vault](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy Azure Automation a jak může sloužit ke správě Azure Key Vault, postupujte podle následujících odkazech na další informace o Azure Automation.

* Najdete v části služby Azure Automation [kurz Začínáme](../automation/automation-first-runbook-graphical.md).
* Najdete v článku [Azure Key Vault PowerShell skripty](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

