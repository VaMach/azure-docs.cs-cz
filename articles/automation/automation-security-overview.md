<properties
   pageTitle="Zabezpečení služby Azure Automation"
   description="Tento článek nabízí přehled zabezpečení automatizace a různé metody ověřování, které jsou dostupné pro účty Automation ve službě Azure Automation."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automation security, secure automation" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/10/2016"
   ms.author="magoedte" />

# Zabezpečení služby Azure Automation
Azure Automation umožňuje automatizovat úlohy s prostředky v Azure, místně a u jiných poskytovatelů cloudu, například Amazon Web Services (AWS).  Aby mohl runbook provádět požadované akce, musí mít oprávnění pro bezpečný přístup k prostředkům s minimálními požadovanými právy v rámci předplatného.  
Tento článek se bude zabývat různými scénáři ověřování, které Azure Automation podporuje, a ukáže vám, jak začít pracovat s ohledem na prostředí, které nebo která potřebujete spravovat.  

## Přehled účtu Automation
Při prvním spuštění služby Azure Automation vytvořte alespoň jeden účet Automation. Účty Automation umožňují izolovat vaše prostředky Automation (runbooky, assety, konfigurace) od prostředků, které jsou obsažené v jiných účtech Automation. Účty Automation můžete použít k oddělení prostředků do samostatných logických prostředí. Jeden účet můžete například použít pro vývoj, druhý k produkci a další pro svoje místní prostředí.  Účet Azure Automation se liší od účtu Microsoft a účtů vytvořených v rámci vašeho předplatného Azure.

Prostředky Automation jednotlivých účtů Automation jsou přidružené k jedné oblasti Azure, ale účty Automation můžou spravovat prostředky v libovolné oblasti. Hlavním důvodem k vytvoření účtů Automation v různých oblastech by byla situace, kdy máte zásady, které vyžadují izolaci dat a prostředků v určité oblasti.

>[AZURE.NOTE]Účty Automation a v nich obsažené prostředky, které jsou vytvořené pomocí portálu Azure, nemůžete otevírat pomocí portálu Azure Classic. Pokud chcete tyto účty nebo jejich prostředky spravovat pomocí rozhraní Windows PowerShell, použijte moduly Azure Resource Manageru.

Všechny úlohy, které s prostředky provádíte pomocí správce Azure Resource Manager (ARM) a rutin Azure ve službě Azure Automation, se musí ověřovat pro službu Azure pomocí ověření na základě přihlašovacích údajů organizační identity v Azure Active Directory.  Ověřování pomocí certifikátů bylo původní metodou ověřování v režimu Azure Service Management (ASM), ale jeho instalace byla složitá.  Ověřování v Azure pomocí uživatele Azure AD bylo znovu zavedeno v roce 2014, nejen kvůli zjednodušení procesu konfigurace ověřovacího účtu, ale také kvůli podpoře možnosti neinteraktivního ověření pro Azure pomocí jediného uživatelského účtu, který pracoval s režimem ASM i ARM.   

Nedávno jsme vydali další aktualizaci, ve které teď automaticky vytváříme objekt zabezpečení služby Azure AD při vytvoření účtu Automation. Toto ověření se označuje jako účet Spustit v Azure jako a je výchozí metodou ověřování pro automatizaci runbooků pomocí správce Azure Resource Manager.     

Řízení přístupu na základě role v režimu ARM zajišťuje udělování povolených akcí na uživatelském účtu a objektu služby Azure AD a ověřování takového objektu služby.  Přečtěte si článek [Řízení přístupu na základě role ve službě Azure Automation](../automation/automation-role-based-access-control.md), kde najdete další informace, které vám pomůžou s vývojem vašeho modelu pro správu oprávnění ve službě Automation.  

Runbooky, které běží v procesu Hybrid Runbook Worker ve vašem datovém centru nebo s výpočetními službami v AWS, nemůžou používat stejné metody, které se obvykle používají pro runbooky ověřované pro prostředky Azure.  Důvodem je to, že tyto prostředky jsou spuštěné mimo Azure a budou proto vyžadovat svoje vlastní zabezpečovací přihlašovací údaje definované ve službě Automation, aby se ověřovaly v prostředcích, ke kterým budou přistupovat místně.  

## Metody ověřování

Následující tabulka shrnuje různé metody ověřování pro jednotlivá prostředí podporovaná službou Azure Automation a články popisující, jak nastavit ověřování runbooků.

Metoda  |  Prostředí  | Článek
----------|----------|----------
Uživatelský účet Azure AD | Azure Resource Manager a správa služby Azure | [Ověření runbooků pomocí uživatelského účtu Azure AD](../automation/automation-sec-configure-aduser-account.md)
Objekt zabezpečení služby Azure AD | Azure Resource Manager | [Ověření runbooků pomocí účtu Spustit v Azure jako](../automation/automation-sec-configure-azure-runas-account.md)
Ověřování systému Windows | Místní datové centrum | [Ověření runbooků pro proces Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md)
Přihlašovací údaje služby Amazon Web Services | Amazon Web Services | [Ověření runbooků pomocí Amazon Web Services (AWS)](../automation/automation-sec-configure-aws-account.md)



<!--HONumber=Jun16_HO2-->


