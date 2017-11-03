---
title: "Monitorování a zpracování výstrah zabezpečení v Operations Management Suite zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument vám umožňuje používat intelligence možnost hrozeb, které jsou k dispozici v OMS zabezpečení a Audit k monitorování a reakce na výstrahy zabezpečení."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 7d45a32b-1341-4bb5-a436-1f42a8a2590a
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2017
ms.author: yurid
ms.openlocfilehash: df82afab2c38431e134146143524edc080ee38f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Monitorování a zpracování výstrah zabezpečení v Operations Management Suite zabezpečení a Audit řešení
Tento dokument vám pomůže používat intelligence možnost hrozeb, které jsou k dispozici v OMS zabezpečení a Audit k monitorování a reakce na výstrahy zabezpečení.

## <a name="what-is-oms"></a>Co je OMS?
Microsoft Operations Management Suite (OMS) je řešení pro správu IT, která pomáhá spravovat a chránit místní a cloudové infrastruktury založené na službě cloud společnosti Microsoft. Další informace o OMS najdete v článku [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Analýza hrozeb
V prostředí podniku, kde uživatelé mají široký přístup k síti a použít na široké škále zařízení pro připojení k firemní data je nutné, můžete aktivně sledovat vaše prostředky a rychle reagovat na incidenty zabezpečení. To je zvláště důležité z perspektivy životního cyklu zabezpečení, protože některé počítačové bezpečnosti, které hrozeb nemusí vyvolat výstrahy nebo podezřelé aktivity, které lze identifikovat podle tradiční technické ovládací prvky zabezpečení. 

Pomocí **analýzou hrozeb** možnost k dispozici v OMS zabezpečení a Audit, správci IT může identifikovat bezpečnostních hrozeb v prostředí, například, zjistit, zda je součástí určitého počítače [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Z počítačů se můžou stát uzly v botnetu, když útočníci neoprávněně nainstalují malware, který tajně připojí počítač k řídicímu serveru. Můžete také poznat potenciální hrozby pocházejících z podzemní komunikačních kanálů, jako například [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Chcete-li vytvořit tuto analýzou hrozeb, řešení OMS zabezpečení a Audit používá dat pocházejících z více zdrojů v rámci Microsoftu. OMS zabezpečení a Audit bude tyto údaje využijte k identifikaci potenciálních hrozeb pro vaše prostředí.

Podokno Analýza hrozeb se skládá ze tří hlavních možností:

* Servery s odchozím škodlivým provozem
* Typy zjištěnými hrozbami
* Mapa analýzy hrozeb

> [!NOTE]
> Přehled všechny tyto možnosti najdete v tématu [Začínáme s Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md).
> 
> 

### <a name="responding-to-security-alerts"></a>Zpracování výstrah zabezpečení
Mezi jednotlivými kroky [reakcí na incidenty zabezpečení](https://technet.microsoft.com/library/cc512623.aspx) proces je identifikace závažnost ohrožení systémy. V této fázi měli provádět následující úlohy:

* Určit povahu útoku.
* Určit, odkud útok přišel.
* Určit záměr útoku. Směřoval útok konkrétně na vaši organizaci za účelem získání určitých informací, nebo se jednalo o náhodný útok?
* Identifikovat napadené systémy.
* Označení souborů, které mají přístup k a zjistit, zda rozlišuje těchto souborů

Můžete využít **analýzou hrozeb** informace v OMS zabezpečení a Audit řešení usnadní tyto úlohy. Použijte následující postup k přístupu k těmto **analýzou hrozeb** možnosti:

1. Na hlavním řídicím panelu **Microsoft Operations Management Suite** klikněte na dlaždici **Zabezpečení a audit**.
   
    ![Zabezpečení a Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. V **zabezpečení a Audit** řídicího panelu, zobrazí se **analýzou hrozeb** možnosti v pravém, jak je uvedeno níže:
   
    ![Intel hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Tyto tři dlaždice získáte přehled o aktuální ohrožení. V **serveru s odchozím škodlivým provozem** bude možné zjistit, zda je jakýkoli počítač, který monitorujete (uvnitř nebo mimo síť), je odesílání škodlivý přenos k Internetu. 

**Zjistila hrozba typy** dlaždice zobrazuje souhrnné údaje o hrozeb, které jsou aktuální "v zástupné", pokud kliknete na tuto dlaždici se zobrazí další podrobnosti o těchto hrozbách jako zobrazit níže:

![Zjištěné typy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Další informace o jednotlivé hrozby můžete rozbalit kliknutím na. Následující příklad ukazuje další podrobnosti o Botnet:

![Další informace o ohrožení](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Jak je popsáno na začátku této části, tato informace může být velmi užitečná při s případem reakcí na incidenty. Může také být důležité během forenzního vyšetřování, potřebujete-li najít zdroj útoku, který systém došlo k ohrožení a časovou osu. V této sestavě mohli snadno identifikovat některé klíčové podrobnosti o útok, jako například: zdroji útoku, místní IP adresa, došlo k ohrožení a stavu aktuální relace připojení. 

**Threat intelligence mapy** vám pomůže identifikovat aktuální umístění po celém světě, které mají škodlivý přenos. Existují oranžová (příchozí) a červený (odchozí) šipky v této mapě identifikují směr provozu, pokud v jednom z těchto šipky kliknete, zobrazí typ hrozbě a směr provozu, jak je uvedeno níže:

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [!NOTE]
> Ukázka uvidíte o tom, jak použít tuto funkci během reakce na incidenty Výukové video prezentace [zmírnit hrozby zabezpečení datového centra s asistencí šetření pomocí služby Operations Management Suite](https://myignite.microsoft.com/videos/5000) doručit na Microsoft Ignite.
> 

### <a name="responding-to-distinct-malicious-ip-accessed"></a>Neodpovídá na požadavky odlišné škodlivé IP získat přístup
V některých scénářích si můžete všimnout potenciálně škodlivé IP adresy, ke které přistupoval jeden monitorovaný počítač:

![mapa analýzy hrozeb](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig6.PNG)

Tato výstraha a další v rámci stejné kategorie se generují prostřednictvím zabezpečení OMS s využitím [analýzy hrozeb Microsoftu](https://youtu.be/O4WtxgUrDc8). Data analýzy hrozeb shromažďuje Microsoft a také se kupují od vedoucích poskytovatelů analýzy hrozeb. Tato data se často aktualizují a přizpůsobují rychle se měnícím hrozbám. Díky jejich povaze je třeba při [vyšetřování](https://blogs.technet.microsoft.com/msoms/2016/12/08/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) výstrah zabezpečení kombinovat je s dalšími zdroji informací o zabezpečení. 

## <a name="customize-alerts-received-via-e-mail"></a>Přizpůsobení obdržel prostřednictvím e-mailových upozornění

Můžete přizpůsobit, kteří uživatelé ve vaší organizaci bude upozorněn výstrahy zabezpečení jsou aktivovány OMS zabezpečení. Tato možnost je k dispozici v části Přehled nebo nastavení na řídicím panelu OMS:

![E-mail](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig7.png)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak používat **analýzou hrozeb** možnost v OMS zabezpečení a Audit řešení reakce na výstrahy zabezpečení. Další informace o zabezpečení v OMS najdete v následujících článcích:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Začínáme se službou Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)

