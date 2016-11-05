---
title: Průvodce odstraňováním potíží se službou Azure Security Center | Microsoft Docs
description: Tento dokument vám pomůže při odstraňování potíží se službou Azure Security Center.
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: yurid

---
# Průvodce odstraňováním potíží pro službu Azure Security Center
Tento průvodce je určený odborníkům na informační technologie (IT), analytikům zabezpečení informací a správcům cloudů, jejichž společnosti používají službu Azure Security Center a potřebují odstraňovat potíže týkající se služby Security Center.

## Průvodce odstraňováním potíží
Tento průvodce vysvětluje, jak odstraňovat potíže související se službou Security Center. U většiny postupů odstraňování potíží prováděných v rámci služby Security Center je třeba nejprve si prohlédnout záznamy [protokolu auditu](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) pro komponentu, u které se potíže vyskytly. Na základě protokolů auditu můžete zjistit:

* Které operace proběhly
* Kdo operaci zahájil
* Kdy k operaci došlo
* Stav operace
* Hodnoty dalších vlastností, které vám mohou pomoci při zkoumání operace

Protokol auditu obsahuje všechny operace zápisu (PUT, POST, DELETE) prováděné s vašimi prostředky, ale neobsahuje operace čtení (GET).

## Odstraňování potíží s instalací agenta sledování ve Windows
Agent sledování služby Security Center slouží ke shromažďování dat. Po povolení shromažďování dat a řádné instalaci agenta v cílovém počítači by se měly provádět tyto procesy:

* ASMAgentLauncher.exe – agent sledování Azure 
* ASMMonitoringAgent.exe – rozšíření Azure Security Monitoring
* ASMSoftwareScanner.exe – Azure Scan Manager

Rozšíření Azure Security Monitoring prochází různé položky konfigurace v oblasti zabezpečení a shromažďuje protokoly zabezpečení z daného virtuálního počítače. Služba Scan Manager se používá k procházení oprav.

Pokud je instalace provedena úspěšně, měla by se v protokolech auditu pro cílový virtuální počítač zobrazit položka podobná následující:

![Protokoly auditu](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Další informace o procesu instalace můžete získat také přečtením protokolů agenta, které jsou k dispozici v umístění *%systemdrive%\windowsazure\logs* (příklad: C:\WindowsAzure\Logs).

> [!NOTE]
> Pokud se agent služby Azure Security Center nechová podle očekávání, je třeba restartovat cílový virtuální počítač, protože pro zastavení a spuštění agenta není k dispozici žádný příkaz.
> 
> 

## Odstraňování potíží s instalací agenta sledování v systému Linux
Při odstraňování potíží s instalací agenta virtuálního počítače v systému Linux je třeba zajistit, aby se rozšíření stáhlo do umístění /var/lib/waagent /. Spuštěním následujícího příkazu můžete ověřit, zda instalace proběhla:

`cat /var/log/waagent.log` 

Další soubory protokolů obsahující informace užitečné při odstraňování potíží: 

* /var/log/mdsd.err
* /var/log/azure/

Ve funkčním systému by mělo být vidět připojení k procesu mdsd na portu TCP 29130. Jedná se o komunikaci programu syslog s procesem mdsd. Toto chování lze ověřit spuštěním následujícího příkazu:

`netstat -plantu | grep 29130`

## Kontaktování oddělení podpory společnosti Microsoft
Některé potíže lze identifikovat podle pokynů v tomto článku, některé další jsou také dokumentovány ve veřejném [fóru](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter) služby Security Center. Pokud však potřebujete odstraňovat potíže mimo tento rámec, můžete otevřít nový požadavek na podporu prostřednictvím služby Azure Portal, jak je uvedeno níže: 

![Podpora společnosti Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

## Viz také
V tomto dokumentu jste zjistili, jak ve službě Azure Security Center konfigurovat zásady zabezpečení. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md) – Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů

<!--HONumber=Sep16_HO3-->


