---
title: "Cílení aktualizací s využitím kolekcí SCCM v OMS Update Managementu | Dokumentace Microsoftu"
description: "Tento článek vám objasní, jak pomocí tohoto řešení nakonfigurovat System Center Configuration Manager a spravovat aktualizace počítačů spravovaných pomocí SCCM."
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.openlocfilehash: ec97432cd14c6289928f0419c242e1ccc2c8d876
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="integrate-system-center-configuration-manager-with-oms-update-management"></a>Integrace System Center Configuration Manageru s OMS Update Managementem

Zákazníci, kteří investovali do System Center Configuration Manageru pro správu počítačů, serverů a mobilních zařízení využívají jeho odolnost a další přednosti také při správě aktualizací softwaru jako součást cyklu správy softwarových aktualizací (SUM).  

Na základě stávající integrace mezi OMS a Configuration Managerem můžete vytvářet sestavy a aktualizovat spravované servery Windows tak, že vytvoříte a předem připravíte nasazení aktualizací softwaru v Configuration Manageru, a získat podrobný stav dokončených nasazení aktualizaci pomocí [řešení Update Management](../operations-management-suite/oms-solution-update-management.md). Pokud Configuration Manager používáte k vytváření sestav dodržování předpisů aktualizací, ale ne ke správě nasazení aktualizací pro servery Windows, můžete pokračovat ve vytváření sestav s využitím Configuration Manageru a ke správě aktualizací zabezpečení používat řešení OMS Update Management.

## <a name="prerequisites"></a>Požadavky

* V pracovním prostoru Log Analytics musíte mít přidané [řešení Update Management](../operations-management-suite/oms-solution-update-management.md), které je propojené s účtem Automation ve stejné oblasti a skupině prostředků.   
* Servery Windows aktuálně spravované pomocí prostředí System Center Configuration Manageru také musí do pracovního prostoru Log Analytics nahlásit, že mají povolené řešení Update Management.  
* Tato funkce podporuje System Center Configuration Manager aktuální verze 1606 a vyšší.  Pokud chcete integrovat samostatný primární web nebo centrální web správy Configuration Manageru s Log Analytics, přečtěte si téma věnované [propojení Configuration Manageru k Log Analytics](../log-analytics/log-analytics-sccm.md).  
* Pokud agenti Windows nedostávají aktualizace zabezpečení z Configuration Manageru, musí mít nakonfigurovanou komunikaci se serverem služeb Windows Server Update Services (WSUS) nebo musí mít přístup ke službě Microsoft Update.   

To, jak spravujete klienty hostované v Azure IaaS s využitím stávajícího prostředí Configuration Manageru, závisí primárně na připojení, které máte mezi datovými centry Azure a vaší infrastrukturou. Toto připojení ovlivňuje případné změny v návrhu, které je třeba provést v infrastruktuře Configuration Manageru, a náklady související s provedením těchto nezbytných změn.  Pokud chcete lépe pochopit, jaké aspekty plánování byste měli vyhodnotit, než budete pokračovat dál, přečtěte si [nejčastější dotazy ke Configuration Manageru v Azure](https://docs.microsoft.com/sccm/core/understand/configuration-manager-on-azure#networking).    

## <a name="configuration"></a>Konfigurace

### <a name="manage-software-updates-from-configuration-manager"></a>Správa aktualizací softwaru s využitím Configuration Manageru 

Pokud budete nadále spravovat nasazení aktualizace z Configuration Manageru, proveďte následující kroky.  OMS se připojí ke Configuration Manageru a nainstaluje aktualizace do klientských počítačů připojených k pracovnímu prostoru Log Analytics. Obsah aktualizace obsah je dostupný z mezipaměti klientského počítače, stejně jako v případě, kdy nasazení spravoval Configuration Manager.  

1. Na webu nejvyšší úrovně v hierarchii Configuration Manager vytvořte nasazení aktualizace softwaru, a to pomocí postupu popsaného v [procesu nasazení softwarových aktualizací](https://docs.microsoft.com/sccm/sum/deploy-use/deploy-software-updates).  Jediným nastavením, které se musí nakonfigurovat jinak než u standardního nasazení, je možnost **Neinstalovat softwarové aktualizace**, která řídí řízení chování balíčku pro nasazení při stahování. Toto chování spravuje řešení OMS Update Management vytvořením plánovaného nasazení aktualizace v dalším kroku.  

1. Na portálu OMS otevřete řídicí panel Update Managementu.  Pomocí kroků popsaných v tématu [Vytvoření nasazení aktualizace](../operations-management-suite/oms-solution-update-management.md#creating-an-update-deployment) vytvořte nové nasazení a v rozevíracím seznamu vyberte příslušnou kolekci Configuration Manageru reprezentovanou jako skupina počítačů OMS.  Mějte na paměti následující důležité skutečnosti:
    1. Pokud je u vybrané kolekce zařízení Configuration Manageru definované časové období údržby, členové této kolekce se ho budou držet (a nevyužijí nastavení **Doba trvání** definované pro naplánované nasazení v OMS.
    1. Členové cílové kolekce musí mít připojení k internetu (přímo, přes proxy server nebo přes bránu OMS).  

Po dokončení nasazení aktualizace s využitím řešení OMS cílové počítače, které jsou členy vybrané skupiny počítačů, v naplánovaném čase nainstalují aktualizace z místní mezipaměti klienta.  Můžete si [zobrazit stav nasazení aktualizací](../operations-management-suite/oms-solution-update-management.md#viewing-update-deployments) a monitorovat výsledky nasazení.  


### <a name="manage-software-updates-from-oms"></a>Správa aktualizací softwaru s využitím OMS

Pokud chcete spravovat aktualizace pro virtuální počítače s Windows Serverem, které jsou klienty Configuration Manageru, musíte nakonfigurovat zásady klienta tak, aby funkce správy softwarových aktualizací byla zakázaná pro všechny klienty spravované tímto řešením.  Nastavení klienta standardně cílí na všechna zařízení v hierarchii.  Další informace o tomto nastavení zásad a jeho konfiguraci najdete v tématu věnovaném [postupu při konfigurace nastavení klienta v System Center Configuration Manageru](https://docs.microsoft.com/sccm/core/clients/deploy/configure-client-settings).  

Po provedení této změny konfigurace pomocí kroků popsaných v tématu [Vytvoření nasazení aktualizace](../operations-management-suite/oms-solution-update-management.md#creating-an-update-deployment) vytvořte nové nasazení a v rozevíracím seznamu vyberte příslušnou kolekci Configuration Manageru reprezentovanou jako skupina počítačů OMS. 

