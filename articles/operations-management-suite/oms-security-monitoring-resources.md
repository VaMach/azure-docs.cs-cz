---
title: "Sledování prostředků v Operations Management Suite zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument vám pomůže používat OMS zabezpečení a Audit umožňuje sledovat vaše prostředky a identifikovat problémy se zabezpečením."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: d6752120-821f-4aa7-a049-25bf5a653b95
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 2f73266b65a4eda6c8751a2d56bc3f11bf4e6a57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Sledování prostředků v Operations Management Suite zabezpečení a Audit řešení
Tento dokument vám pomůže používat OMS zabezpečení a možnosti auditu ke sledování vašich prostředků a identifikovat problémy se zabezpečením.

## <a name="what-is-oms"></a>Co je OMS?
Microsoft Operations Management Suite (OMS) je řešení pro správu IT, která pomáhá spravovat a chránit místní a cloudové infrastruktury založené na službě cloud společnosti Microsoft. Další informace o OMS najdete v článku [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Sledování prostředků
Vždy, když je možné, budete chtít zabránit bezpečnostní incidenty v oblasti děje na prvním místě. Je však možné zabránit všechny incidenty zabezpečení. Když dojít bezpečnostního incidentu, musíte zajistit, že je minimalizován jeho dopad.  Existují tři kritické doporučení, které lze použít, chcete-li minimalizovat dopad incidentů, zabezpečení a číslo:

* Pravidelně vyhodnoťte ohrožení zabezpečení ve vašem prostředí.
* Pravidelně zkontrolujte všechny počítačových systémů a síťová zařízení k zajištění, že mají všechny nainstalovány nejnovější opravy.
* Pravidelně zkontrolujte všechny protokoly a mechanismy protokolování, včetně protokolů událostí operačního systému, konkrétní protokoly aplikací a protokoly systému zjišťování neoprávněných vniknutí.

OMS zabezpečení a Audit řešení umožňuje IT aktivně sledovat všechny prostředky, které může pomoci minimalizovat dopad incidentů, zabezpečení. OMS zabezpečení a Audit má zabezpečení domény, které lze použít ke sledování prostředků. Zabezpečení domény poskytuje rychlý přístup k možnosti pro sledování zabezpečení, které z následujících domén se budeme v další podrobnosti:

* posouzením malwaru
* Posouzení aktualizací
* Identita a přístup

> [!NOTE]
> Přehled všechny tyto možnosti najdete v tématu [Začínáme s Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md).
> 
> 

### <a name="monitoring-system-protection"></a>Monitorování ochrany systému
V obrany ve hloubka přístup je důležité pro celkový stav zabezpečení asset každou úroveň ochrany. Počítače se zjištěnými hrozbami a počítače s nedostatečnou ochranou se zobrazují v dlaždici posouzením malwaru v části zabezpečení domény. Podle informací uvedených na posouzením malwaru můžete identifikovat plánu pro použití ochrany na servery, které je třeba ji. Pro přístup k této možnosti postupujte následujícím způsobem:

1. Na hlavním řídicím panelu **Microsoft Operations Management Suite** klikněte na dlaždici **Zabezpečení a audit**.
   
    ![Zabezpečení a Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. V **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **antimalwarových Assessment** pod **zabezpečení domény**. **Antimalwarových Assessment** řídicího panelu se zobrazí, jak je uvedeno níže:

![posouzením malwaru](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Můžete použít **posouzením malwaru** řídicí panel k identifikaci následující problémy se zabezpečením:

* **Aktivní hrozeb**: počítače, které byly ohrožené a mít aktivní hrozeb v systému.
* **Opravené hrozeb**: počítače, které byly ohrožení zabezpečení, ale hrozby provedla nápravu.
* **Zastaralý podpis**: počítače, které mají povolenou ochranu před škodlivým softwarem, ale podpis je zastaralý.
* **Žádná ochrana v reálném čase**: počítače, které nemají nainstalovaný Antimalwarový program.

### <a name="monitoring-updates"></a>monitorování aktualizací
Nejlepším postupem zabezpečení je použití nejnovější aktualizace zabezpečení a by měla být zahrnuta do strategie správy aktualizace. Služba Microsoft Monitoring Agent (HealthService.exe) čte informace o aktualizaci z monitorovaných počítačů a pak odešle tento aktualizované informace o službě OMS v cloudu pro zpracování. Služba Microsoft Monitoring Agent je nakonfigurovaný jako automatické služby a měla by být vždy spuštěna v cílovém počítači.

![monitorování aktualizací](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Aktualizace dat je použita logika a cloudové služby zaznamenává data. Pokud chybějící aktualizace, zobrazí se na **aktualizace** řídicího panelu. Můžete použít **aktualizace** řídicí panel k práci s chybějícími aktualizacemi a vytvořte plán je aplikovat na servery, které je potřebují. Použijte následující postup pro přístup **aktualizace** řídicí panel:

1. Na hlavním řídicím panelu **Microsoft Operations Management Suite** klikněte na dlaždici **Zabezpečení a audit**.
2. V **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **vyhodnocení aktualizací** pod **zabezpečení domény**. Aktualizace řídicího panelu se zobrazí, jak je uvedeno níže:

![Vyhodnocení aktualizací](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

V tomto řídicím panelu můžete provést vyhodnocení aktualizací pro informace o aktuálním stavu počítače a řešení nejdůležitější hrozeb. Pomocí **důležité aktualizace nebo aktualizace zabezpečení** dlaždice, správci IT bude moct přistupovat podrobné informace o aktualizacích, které chybí, jak je uvedeno níže:

![Výsledky vyhledávání](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Tato sestava zahrnují důležité informace, které lze použít k identifikaci typ hrozeb, které tento systém je snadno napadnutelný, což zahrnuje v článcích znalostní báze Microsoft KB přidružené aktualizace zabezpečení a Bulletin MS, který obsahuje další podrobnosti o chybě zabezpečení.

### <a name="monitoring-identity-and-access"></a>Sledování přístupu a identit
S uživatelů, kteří pracují odkudkoli a pomocí různých zařízení a přístupu k obrovské množství cloudové a místní aplikace je nutné ochrany svých přihlašovacích údajů. Útoky krádeže přihlašovacích údajů jsou ty, ve kterých útočník původně získá přístup k přihlašovacím údajům běžného uživatelského přístupu k systému v síti. Kolikrát tento počáteční útok je pouze způsob, jak získat přístup k síti, konečným cílem je ke zjišťování účtů oprávnění. 

Útočníci zůstanou v síti, pomocí volně dostupných nástrojů extrahovat pověření z relací jiné účty přihlášen. V závislosti na konfiguraci systému můžete extrahovat tyto přihlašovací údaje ve formě hodnoty hash, lístků nebo hesla i ve formátu prostého textu.  

> [!NOTE]
> počítače, které jsou přímo přístup k Internetu se zobrazí velký počet neúspěšných pokusů, které se pokusí o přihlášení pomocí všechny druh dobře známé uživatelských jmen (například správce). Ve většině případů je OK pokud dobře známé uživatelských jmen se nepoužívají, a pokud je dostatečně silné heslo.
> 
> 

Je možné identifikovat tyto případné útočníky před jejich ohrožení účet oprávnění. Můžete využít **OMS zabezpečení a Audit řešení** k monitorování identit a přístupu. Použijte následující postup pro přístup **identit a přístupu** řídicí panel:

1. V **Microsoft Operations Management Suite** zabezpečení a Audit, klikněte na hlavním řídicím panelu dlaždici.
2. V **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **identit a přístupu** pod **zabezpečení domény**. **Identit a přístupu** řídicího panelu se zobrazí, jak je uvedeno níže:

![Identita a přístup](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Jako součást strategie regulární monitorování je nutné zahrnout monitorování identity. Správce IT by měl vypadat, pokud je konkrétní platné uživatelské jméno, který má počtu pokusů o zadání. To může znamenat buď útočník, který získali skutečné uživatelské jméno a zkuste hrubou silou nebo automatické nástroj toto používá pevně zakódovaný heslo, jehož platnost vypršela.

Povolit tento řídicí panel tak rychle identifikovat potenciální hrozby související s identit a přístupu k prostředkům společnosti. Je konkrétní důležité také identifikovat potenciální trendy, například v dlaždici přihlášení v čase se zobrazí po dobu kolikrát byl proveden pokus o přihlášení se nezdařilo. V tomto případě počítač **souborovém serveru** přijatých 35 pokusů o přihlášení. Další informace o tomto počítači, můžete prozkoumat kliknutím na. 

![Další informace](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Sestava vygenerována pro tento počítač přináší cenné informace o tomto vzoru. Si všimli, který **účet** sloupci se zobrazuje uživatelský účet, který byl použit k pokusu o přístup k systému, **TIMEGENERATED** sloupci se zobrazuje časový interval, ve kterém byl proveden pokus a **LOGONTYPENAME** sloupci se zobrazuje umístění, kde bylo provedeno tento pokus. Pokud tyto pokusy nebyly provedeny místně v systému programem, **proces** sloupci se zobrazuje název procesu. Ve scénářích, kde se pokus o přihlášení pochází z programu máte k dispozici název procesu a teď můžete provádět další šetření v cílovém systému.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak používat OMS zabezpečení a Audit řešení ke sledování vašich prostředků. Další informace o zabezpečení v OMS najdete v následujících článcích:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Začínáme se službou Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md)
* [Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite](oms-security-responding-alerts.md)

