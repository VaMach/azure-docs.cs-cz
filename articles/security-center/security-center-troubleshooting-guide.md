---
title: "Průvodce odstraňováním potíží se službou Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže při odstraňování potíží se službou Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: fa70dffc2a4bade44e1dec583bdfcf7b5dae6801
ms.contentlocale: cs-cz
ms.lasthandoff: 06/17/2017


---
# Průvodce odstraňováním potíží pro službu Azure Security Center
<a id="azure-security-center-troubleshooting-guide" class="xliff"></a>
Tento průvodce je určený odborníkům na informační technologie (IT), analytikům zabezpečení informací a správcům cloudů, jejichž společnosti používají službu Azure Security Center a potřebují odstraňovat potíže týkající se služby Security Center.

>[!NOTE] 
>Od začátku června 2017 bude Security Center používat ke shromažďování a ukládání dat agenta Microsoft Monitoring Agent. Další informace najdete v článku o [migraci platformy pro Azure Security Center](security-center-platform-migration.md). Informace v tomto článku představují funkce služby Security Center po přechodu na agenta Microsoft Monitoring Agent.
>

## Průvodce odstraňováním potíží
<a id="troubleshooting-guide" class="xliff"></a>
Tento průvodce vysvětluje, jak odstraňovat potíže související se službou Security Center. U většiny postupů odstraňování potíží prováděných v rámci služby Security Center je třeba nejprve si prohlédnout záznamy [protokolu auditu](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) pro komponentu, u které se potíže vyskytly. Na základě protokolů auditu můžete zjistit:

* Které operace proběhly
* Kdo operaci zahájil
* Kdy k operaci došlo
* Stav operace
* Hodnoty dalších vlastností, které vám mohou pomoci při zkoumání operace

Protokol auditu obsahuje všechny operace zápisu (PUT, POST, DELETE) prováděné s vašimi prostředky, ale neobsahuje operace čtení (GET).

## Microsoft Monitoring Agent
<a id="microsoft-monitoring-agent" class="xliff"></a>
Security Center používá agenta Microsoft Monitoring Agent – to je stejný agent, kterého používá Operations Management Suite a služba Log Analytics – ke shromažďování dat zabezpečení z virtuálních počítačů Azure. Po povolení shromažďování dat a řádné instalaci agenta v cílovém počítači by se měl provádět tento proces:

* HealthService.exe

Pokud otevřete konzolu pro správu služeb (services.msc), uvidíte také spuštěnou službu Microsoft Monitoring Agent, jak je znázorněno na následujícím obrázku:

![Služby](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

Pokud chcete zjistit, kterou verzi agenta máte, otevřete **Správce úloh**, na kartě **Procesy** vyhledejte **Služba Microsoft Monitoring Agent**, klikněte na ni pravým tlačítkem myši a klikněte na **Vlastnosti**. Na kartě **Podrobnosti** vyhledejte verzi souboru, jak je znázorněno níže:

![File](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)
   

## Scénáře instalace služby Microsoft Monitoring Agent
<a id="microsoft-monitoring-agent-installation-scenarios" class="xliff"></a>
Existují dva scénáře instalace služby Microsoft Monitoring Agent na počítač, které mohou přinést různé výsledky. Podporované scénáře:

* **Agent nainstalovaný automaticky službou Security Center**: V tomto scénáři se budete moci podívat na výstrahy v obou umístěních – ve službě Security Center i v Hledání v protokolu. E-mailová oznámení budete dostávat na e-mailovou adresu, kterou jste si nakonfigurovali v zásadách zabezpečení pro předplatné, do něhož prostředek patří.
.
* **Agent ručně nainstalovaný na virtuálním počítači umístěném v Azure**: V tomto scénáři – pokud používáte agenty, které jste stáhli a nainstalovali ručně před únorem 2017 – se budete moci podívat na výstrahy na portálu Security Center jenom v případě, že si vyfiltrujete předplatné, do kterého pracovní prostor patří. Pokud si vyfiltrujete předplatné, do kterého prostředek patří, neuvidíte žádné jiné výstrahy. E-mailová oznámení budete dostávat na e-mailovou adresu, kterou jste si nakonfigurovali v zásadách zabezpečení pro předplatné, do něhož pracovní prostor patří.

>[!NOTE]
> Pokud se chcete vyhnout chování uvedenému v druhém scénáři, stáhněte si nejnovější verzi agenta.
> 

## Odstraňování potíží se síťovými požadavky na agenta monitorování
<a id="troubleshooting-monitoring-agent-network-requirements" class="xliff"></a>
Agenti, kteří se připojují ke službě Security Center a registrují se v ní, musí mít přístup k síťovým prostředkům, včetně čísel portů a doménových adres URL.

- U proxy serverů musíte zajistit konfiguraci příslušných prostředků proxy serveru v nastavení agenta. Přečtěte si tento článek, ve kterém se dozvíte další informace o [tom, jak změnit nastavení proxy](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings).
- Pokud používáte k omezení přístupu k internetu bránu firewall, je nutné ji nakonfigurovat tak, aby povolovala přístup k OMS. V nastavení agenta nemusíte nic konfigurovat.

V následující tabulce najdete přehled prostředků potřebných pro komunikaci.

| Prostředek agenta | Porty | Obejít kontrolu protokolu HTTPS |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ano |
| *.oms.opinsights.azure.com | 443 | Ano |
| *.blob.core.windows.net | 443 | Ano |
| *.azure-automation.net | 443 | Ano |

Pokud narazíte na problémy s registrací agenta, přečtěte si článek [Řešení potíží s registrací v Operations Management Suite](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).


## Řešení potíží s tím, že ochrana koncových bodů nefunguje správně
<a id="troubleshooting-endpoint-protection-not-working-properly" class="xliff"></a>

Agent hosta je nadřazený proces pro všechno, co dělá rozšíření [Microsoft Antimalware](../security/azure-security-antimalware.md). Pokud proces agenta hosta selže, může se stát, že selže i Microsoft Antimalware, který je spuštěný jako jeho podřízený proces.  V podobných situacích se doporučuje ověřit následující možnosti:

- Pokud je cílový virtuální počítač založený na vlastní imagi a tvůrce virtuálního počítače nikdy nenainstaloval agent hosta.
- Pokud cílový virtuální počítač místo Windows používá Linux, instalace verze antimalwarového rozšíření pro Windows na linuxovém virtuálním počítači selže. Linuxový agent hosta má specifické požadavky na verzi operačního systému a požadované balíčky. Pokud tyto požadavky nejsou splněny, agent virtuálního počítače také nebude fungovat. 
- Pokud se při vytvoření virtuálního počítače použila stará verze agenta hosta. Pokud ano, měli byste vědět, že u některých starých agentů nefunguje automatická aktualizace na novější verzi a to by mohlo vést k tomuto problému. Pokud vytváříte vlastní image, vždy používejte nejnovější verzi agenta hosta.
- Některé softwary pro správu třetích stran mohou zakázat agenta hosta nebo blokovat přístup k určitým umístěním souborů. Pokud máte ve virtuálním počítači nainstalovaný software třetích stran, ujistěte se, že agent je uvedený v seznamu vyloučení.
- Určitá nastavení brány firewall nebo skupiny zabezpečení sítě (NSG) mohou zablokovat síťový provoz do a z agenta hosta.
- Některé seznamy řízení přístupu (ACL) mohou bránit v přístupu k disku.
- Nedostatek místa na disku může blokovat agent hosta a ten nebude fungovat správně. 

Ve výchozím nastavení je uživatelské rozhraní rozšíření Microsoft Antimalware zablokované. Další informace o tom, jak ho v případě potřeby povolit, najdete v tématu věnovaném [povolení uživatelského rozhraní rozšíření Microsoft Antimalware ve virtuálních počítačích s Azure Resource Managerem po nasazení](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/).

## Odstraňování potíží s načtením řídicího panelu
<a id="troubleshooting-problems-loading-the-dashboard" class="xliff"></a>

Pokud dochází k problémům s načtením řídicího panelu služby Security Center, ujistěte se, že uživatel, který předplatné ke službě Security Center registruje (tj. uživatel, který s tímto předplatným otevřel službu Security Center) a uživatel, který chce zapnout shromažďování dat, mají u daného předplatného roli *Vlastník* nebo *Přispěvatel*. Od této chvíle budou moci i uživatelé, kteří u předplatného mají roli *Čtenář*, zobrazovat řídicí panel, upozornění, doporučení a zásady.

## Kontaktování oddělení podpory společnosti Microsoft
<a id="contacting-microsoft-support" class="xliff"></a>
Některé potíže lze identifikovat podle pokynů v tomto článku, některé další jsou také dokumentovány ve veřejném [fóru](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter) služby Security Center. Pokud však potřebujete odstraňovat potíže mimo tento rámec, můžete vytvořit novou žádost o podporu prostřednictvím webu **Azure Portal**, jak je znázorněno níže: 

![Podpora společnosti Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

## Viz také
<a id="see-also" class="xliff"></a>
V tomto dokumentu jste zjistili, jak ve službě Azure Security Center konfigurovat zásady zabezpečení. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md) – Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů


