---
title: "Průvodce odstraňováním potíží se službou Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže při odstraňování potíží se službou Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: b9f4a8b185f9fb06f8991b6da35a5d8c94689367
ms.openlocfilehash: dbbec729c14d0d9dc5781e7a88a1db3f66f7df97


---
# <a name="azure-security-center-troubleshooting-guide"></a>Průvodce odstraňováním potíží pro službu Azure Security Center
Tento průvodce je určený odborníkům na informační technologie (IT), analytikům zabezpečení informací a správcům cloudů, jejichž společnosti používají službu Azure Security Center a potřebují odstraňovat potíže týkající se služby Security Center.

## <a name="troubleshooting-guide"></a>Průvodce odstraňováním potíží
Tento průvodce vysvětluje, jak odstraňovat potíže související se službou Security Center. U většiny postupů odstraňování potíží prováděných v rámci služby Security Center je třeba nejprve si prohlédnout záznamy [protokolu auditu](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) pro komponentu, u které se potíže vyskytly. Na základě protokolů auditu můžete zjistit:

* Které operace proběhly
* Kdo operaci zahájil
* Kdy k operaci došlo
* Stav operace
* Hodnoty dalších vlastností, které vám mohou pomoci při zkoumání operace

Protokol auditu obsahuje všechny operace zápisu (PUT, POST, DELETE) prováděné s vašimi prostředky, ale neobsahuje operace čtení (GET).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Odstraňování potíží s instalací agenta sledování ve Windows
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


Pokud stále máte problémy se shromažďováním dat, můžete agenta odinstalovat pomocí následujících kroků:

1. Na webu **Azure Portal** vyberte virtuální počítač, u kterého dochází k problémům se shromažďováním dat, a klikněte na **Rozšíření**.
2. Klikněte pravým tlačítkem myši na **Microsoft.Azure.Security.Monitoring** a klikněte na **Odinstalovat**.

![Odebrání agenta](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig4.png)

Rozšíření Azure Security Monitoring by se mělo během několika minut samo přeinstalovat.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Odstraňování potíží s instalací agenta sledování v systému Linux
Při odstraňování potíží s instalací agenta virtuálního počítače v systému Linux je třeba zajistit, aby se rozšíření stáhlo do umístění /var/lib/waagent /. Spuštěním následujícího příkazu můžete ověřit, zda instalace proběhla:

`cat /var/log/waagent.log` 

Další soubory protokolů obsahující informace užitečné při odstraňování potíží: 

* /var/log/mdsd.err
* /var/log/azure/

Ve funkčním systému by mělo být vidět připojení k procesu mdsd na portu TCP 29130. Jedná se o komunikaci programu syslog s procesem mdsd. Toto chování lze ověřit spuštěním následujícího příkazu:

`netstat -plantu | grep 29130`

## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Řešení potíží s tím, že ochrana koncových bodů nefunguje správně

Agent hosta je nadřazený proces pro všechno, co dělá rozšíření [Microsoft Antimalware](../security/azure-security-antimalware.md). Pokud proces agenta hosta selže, může se stát, že selže i Microsoft Antimalware, který je spuštěný jako jeho podřízený proces.  V podobných situacích se doporučuje ověřit následující možnosti:

- Pokud je cílový virtuální počítač založený na vlastní imagi a tvůrce virtuálního počítače nikdy nenainstaloval agent hosta.
- Pokud cílový virtuální počítač místo Windows používá Linux, instalace verze antimalwarového rozšíření pro Windows na linuxovém virtuálním počítači selže. Linuxový agent hosta má specifické požadavky na verzi operačního systému a požadované balíčky. Pokud tyto požadavky nejsou splněny, agent virtuálního počítače také nebude fungovat. 
- Pokud se při vytvoření virtuálního počítače použila stará verze agenta hosta. Pokud ano, měli byste vědět, že u některých starých agentů nefunguje automatická aktualizace na novější verzi a to by mohlo vést k tomuto problému. Pokud vytváříte vlastní image, vždy používejte nejnovější verzi agenta hosta.
- Některé softwary pro správu třetích stran mohou zakázat agenta hosta nebo blokovat přístup k určitým umístěním souborů. Pokud máte ve virtuálním počítači nainstalovaný software třetích stran, ujistěte se, že agent je uvedený v seznamu vyloučení.
- Určitá nastavení brány firewall nebo skupiny zabezpečení sítě (NSG) mohou zablokovat síťový provoz do a z agenta hosta.
- Některé seznamy řízení přístupu (ACL) mohou bránit v přístupu k disku.
- Nedostatek místa na disku může blokovat agent hosta a ten nebude fungovat správně. 

Ve výchozím nastavení je uživatelské rozhraní rozšíření Microsoft Antimalware zablokované. Další informace o tom, jak ho v případě potřeby povolit, najdete v tématu věnovaném [povolení uživatelského rozhraní rozšíření Microsoft Antimalware ve virtuálních počítačích s Azure Resource Managerem po nasazení](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/).

## <a name="troubleshooting-problems-loading-the-dashboard"></a>Odstraňování potíží s načtením řídicího panelu

Pokud dochází k problémům s načtením řídicího panelu služby Security Center, ujistěte se, že uživatel, který předplatné ke službě Security Center registruje (tj. uživatel, který s tímto předplatným otevřel službu Security Center) a uživatel, který chce zapnout shromažďování dat, mají u daného předplatného roli *Vlastník* nebo *Přispěvatel*. Od této chvíle budou moci i uživatelé, kteří u předplatného mají roli *Čtenář*, zobrazovat řídicí panel, upozornění, doporučení a zásady.

## <a name="contacting-microsoft-support"></a>Kontaktování oddělení podpory společnosti Microsoft
Některé potíže lze identifikovat podle pokynů v tomto článku, některé další jsou také dokumentovány ve veřejném [fóru](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter) služby Security Center. Pokud však potřebujete odstraňovat potíže mimo tento rámec, můžete vytvořit novou žádost o podporu prostřednictvím webu **Azure Portal**, jak je znázorněno níže: 

![Podpora společnosti Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak ve službě Azure Security Center konfigurovat zásady zabezpečení. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md) – Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů




<!--HONumber=Feb17_HO3-->


