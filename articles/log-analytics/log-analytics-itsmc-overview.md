---
title: "IT služby konektoru Management v Azure Log Analytics | Microsoft Docs"
description: "Pomocí konektoru služby správy IT centrálně monitorovat a spravovat ITSM pracovní položky v Azure Log Analytics a rychle vyřešit všechny problémy."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: ba8542640fcec6e4bc63d8f0a41bf85b221d4c5e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Centrálně spravovat ITSM pracovních položek pomocí IT Service Management Connector (Preview)

![Symbol konektoru služby správy IT](./media/log-analytics-itsmc/itsmc-symbol.png)

Konektor pro správu služby IT (ITSMC) poskytuje obousměrnou integraci mezi podporované IT služby správy (ITSM) produktům a službám a analýzy protokolů.  Prostřednictvím tohoto připojení můžete vytvořit incidenty, výstrahy nebo události v produktu ITSM na základě analýzy protokolů výstrahy nebo záznamy protokolu. Konektor importuje také data, jako jsou incidenty a žádostí o změny z produktu ITSM do OMS Log Analytics.

ITSMC můžete:

  - Provozní výstrahy integrate vaše postupy správy incidentů v nástroji ITSM podle svého výběru.
    - Vytváření pracovních položek (například výstrahy, události, incident) v ITSM z OMS výstrah a prostřednictvím protokolu vyhledávání.
    - Vytváření pracovních položek na základě vaší protokol činnosti Azure výstrah pomocí akce ITSM ve skupinách akce.

  - Sjednocení monitorování, log a data správy služby používané v rámci celé organizace.
    - Korelovat incident a změňte data žádosti z vaší ITSM tooling s příslušnými daty protokolu v pracovní prostor analýzy protokolů.   
    - Zobrazit nejvyšší úrovně řídicí panely pro přehled na incidenty, žádostí o změnu a ovlivněné systémy.
    - Zápis dotazů analýzy protokolů a získáte přehled o data služby správy.

## <a name="adding-the-it-service-management-connector-solution"></a>Přidání IT řešení pro správu konektoru služby

Přidání konektoru služby správy IT řešení pro váš pracovní prostor analýzy protokolů, pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).

Tady je dlaždici ITSMC, jak vidíte v Galerii řešení:

![dlaždice konektoru](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Po úspěšném přidání, zobrazí se konektoru Management Service IT v rámci **OMS** > **nastavení** > **připojené zdroje.**

![ITSMC připojení](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Ve výchozím nastavení ITSMC aktualizuje data připojení jednou za každých 24 hodin. Aktualizovat připojení k data okamžitě pro úpravy nebo aktualizace šablony, které provedete, klikněte na tlačítko "Aktualizace" zobrazí vedle připojení.

 ![Aktualizace ITSMC](./media/log-analytics-itsmc/itsmc-connection-refresh.png)


## <a name="configuring-the-connection-with-your-itsm-software"></a>Konfigurace připojení se softwarem společnosti ITSM

ITSMC podporuje připojení k **System Center Service Manager**, **ServiceNow**, **Provance**, a **Cherwell**.

Podle potřeby můžete pomocí následujících postupů:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Použití řešení

Po konfiguraci konektoru, začne shromažďování dat z připojených ITSM produktům a službám. V závislosti na počtu incidentů a žádostí o změnu v ITSM produktům a službám počáteční synchronizace by se měly dokončit několik minut.

> [!NOTE]
> - Data importovaná z produktu ITSM ITSMC řešení se zobrazí v analýzy protokolů jako záznamy protokolu typu **ServiceDesk_CL**.
> - Záznam protokolu obsahuje pole s názvem **ServiceDeskWorkItemType_s**, který je buď incidentů nebo žádostí o změnu, dva druhy data importovaná z ITSM produktu.

## <a name="data-synced-from-itsm-product"></a>Synchronizované z produktu ITSM dat
Incidenty a žádosti o změnu jsou synchronizované z vašeho produktu ITSM do pracovního prostoru analýzy protokolů.
Tyto informace jsou uvedeny příklady podle dat shromažďovaných funkcí ITSMC:

> [!NOTE]

> V závislosti na typu pracovní položky naimportovat do analýzy protokolů **ServiceDesk_CL** obsahuje následující pole:

**Pracovní položka:** **incidenty**  
ServiceDeskWorkItemType_s = "Incidentem"

**Pole**

- ServiceDeskConnectionName
- ID služby podpory
- Stav
- Naléhavosti
- Dopad
- Priorita
- Eskalaci
- Autor
- Vyřešil
- Uzavřené
- Zdroj
- Přiřazené
- Kategorie
- Název
- Popis
- Datum vytvoření
- Datum uzavření
- Datum vyřešení
- Datum poslední změny
- Počítač


**Pracovní položka:** **žádosti o změnu**

ServiceDeskWorkItemType_s = "ChangeRequest"

**Pole**
- ServiceDeskConnectionName
- ID služby podpory
- Autor
- Uzavřené
- Zdroj
- Přiřazené
- Název
- Typ
- Kategorie
- Stav
- Eskalaci
- Konflikt stavu
- Naléhavosti
- Priorita
- Riziko
- Dopad
- Přiřazené
- Datum vytvoření
- Datum uzavření
- Datum poslední změny
- Požadovaná data
- Plánované počáteční datum
- Naplánované koncové datum
- Pracovní počáteční datum
- Pracovní koncové datum
- Popis
- Počítač

## <a name="output-data-for-a-servicenow-incident"></a>Výstupní data pro ServiceNow incident

| Pole OMS | ITSM pole |
|:--- |:--- |
| ServiceDeskId_s| Číslo |
| IncidentState_s | Stav |
| Urgency_s |Naléhavosti |
| Impact_s |Dopad|
| Priority_s | Priorita |
| CreatedBy_s | Otevřít |
| ResolvedBy_s | Vyřešil|
| ClosedBy_s  | Uzavřené |
| Source_s| Obraťte se na typ |
| AssignedTo_s | Přiřazené  |
| Category_s | Kategorie |
| Title_s|  Krátký popis |
| Description_s|  Poznámky |
| CreatedDate_t|  Otevřít |
| ClosedDate_t| uzavřený|
| ResolvedDate_t|Vyřešil|
| Počítač  | položky konfigurace |

## <a name="output-data-for-a-servicenow-change-request"></a>Výstupní data pro ServiceNow žádost o změnu

| Pole OMS | ITSM pole |
|:--- |:--- |
| ServiceDeskId_s| Číslo |
| CreatedBy_s | Požadoval |
| ClosedBy_s | Uzavřené |
| AssignedTo_s | Přiřazené  |
| Title_s|  Krátký popis |
| Type_s|  Typ |
| Category_s|  Kategorie |
| CRState_s|  Stav|
| Urgency_s|  Naléhavosti |
| Priority_s| Priorita|
| Risk_s| Riziko|
| Impact_s| Dopad|
| RequestedDate_t  | Požadovaná data |
| ClosedDate_t | Datum uzavření |
| PlannedStartDate_t  |     Plánované počáteční datum |
| PlannedEndDate_t  |   Plánované koncové datum |
| WorkStartDate_t  | Skutečné počáteční datum |
| WorkEndDate_t | Skutečné koncové datum|
| Description_s | Popis |
| Počítač  | Položky konfigurace |

**Ukázka obrazovky analýzy protokolů pro ITSM data:**

![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="itsmc-integration-with-other-oms-solutions"></a>ITSMC integrace s jinými řešeními OMS

Konektor ITSM aktuálně podporuje integraci s řešením mapy služeb.

Mapa služeb automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapuje komunikace mezi službami. Umožňuje zobrazit vaše servery co možná z nich – jako vzájemně propojena systémy, které doručují důležité služby. Mapy služeb zobrazí připojení mezi servery, procesy, a vyžaduje porty mezi žádné připojení TCP architektura žádnou konfiguraci, jiné než instalace agenta.

Další informace: [mapy služeb](../operations-management-suite/operations-management-suite-service-map.md).

Pokud používáte také řešení mapy služeb, můžete zobrazit položky služby podpory, které jsou vytvořené v ITSM řešení, jak je znázorněno v následujícím příkladu:

![Integrace ServiceMap](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Vytváření pracovních položek ITSM OMS výstrahy

S řešením ITSMC v místě můžete nakonfigurovat výstrahy OMS k aktivaci vytváření pracovních položek v vaše připojené ITSM nástroje. Použijte následující postup:

1. Z **hledání protokolů** okně Spustit protokolu vyhledávací dotaz. Chcete-li zobrazit data. Výsledky dotazu jsou zdroj pro pracovní položky.
2. V **hledání protokolů**, klikněte na tlačítko **výstrahy** otevřete **přidat pravidlo výstrahy** stránky.

    ![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Na **přidat pravidlo výstrahy** okno, zadejte požadované podrobnosti pro **název**, **závažnost**, **vyhledávací dotaz**, a **výstrah kritéria** (časová okna/metrika měří období).
4. Vyberte **Ano** pro **ITSM akce**.
5. Vyberte ITSM připojení z **vyberte připojení** seznamu.
6. Zadejte podrobnosti podle potřeby.
7. Vytvořit samostatný pracovní položku pro každé položky protokolu této výstrahy, vyberte **vytvořit jednotlivé pracovní položky pro každý záznam protokolu** zaškrtávací políčko.

    Nebo

    nechte toto políčko Zrušit vytvořit pouze jeden pracovní položku pro libovolný počet záznamů protokolu v rámci této výstrahy.

7. Klikněte na **Uložit**.

Můžete zobrazit výstrahy OMS jste vytvořili v části **nastavení**>**výstrahy**. Pracovní položky odpovídající ITSM připojení vytvářejí, když je splněna podmínka zadaný výstrahy.

## <a name="create-itsm-work-items-from-oms-logs"></a>Vytváření pracovních položek ITSM z protokolů OMS

Můžete také vytvořit pracovní položky v připojených zdrojů ITSM přímo z záznam protokolu. Použijte následující postup:

1. Z **hledání protokolů**hledání požadovaných dat, vyberte podrobností a klikněte na tlačítko **vytvořit pracovní položka**.

    **Vytvoření pracovní položky ITSM** zobrazí se okno:

    ![Obrazovka analýzy protokolů](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Přidejte následující podrobnosti:

  - **Název pracovní položky**: název pro pracovní položku.
  - **Pracovní položky Popis**: popis novou pracovní položku.
  - **Vliv na počítače**: název počítače, kde tato data protokolu byla nalezena.
  - **Vyberte připojení**: ITSM připojení, ve kterém chcete vytvořit tuto pracovní položku.
  - **Pracovní položka**: typ pracovní položky.

3. Chcete-li použít existující šablonu pracovní položky pro incidentu, klikněte na tlačítko **Ano** pod **generovat pracovní položka založený na šabloně** a potom klikněte na **vytvořit**.

    Nebo:

    Klikněte na tlačítko **ne** Pokud chcete zadat vlastní hodnoty.

4. Zadejte odpovídající hodnoty v **typu Kontakt**, **dopad**, **naléhavost**, **kategorie**, a **dílčí kategorie** textová pole a pak klikněte na tlačítko **vytvořit**.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Vytváření pracovních položek ITSM z Azure výstrah
ITSMC je integrovaná do skupiny akcí.

[Akce skupiny](../monitoring-and-diagnostics/monitoring-action-groups.md) poskytují modulární a opakovaně použitelné způsob spouštění akcí pro upozornění Azure. Pomocí akce ITSM ve skupinách akce, můžete vytvořit pracovní položky v ITSM produktu, který má stávající připojení k ITSM konektor řešení.

Použijte následující postup:

1. Na portálu Azure, klikněte na tlačítko **monitorování**.
2. V levém podokně klikněte na **skupiny akcí**. **Přidat skupinu akce** se zobrazí v okně.

    ![Skupiny akcí](media/log-analytics-itsmc/action-groups.png)

3. Zadejte **název** a **ShortName** pro skupinu pro akce. Vyberte **skupiny prostředků** a **předplatné** kde chcete vytvořit skupině Akce.

    ![Podrobnosti skupiny akce](media/log-analytics-itsmc/action-groups-details.png)

4. Vyberte v seznamu akcí **ITSM** z rozevírací nabídky pro **typ akce**. Zadejte **název** pro akci a klikněte na tlačítko **upravit podrobnosti**.
5. Vyberte **předplatné** kde je umístěn pracovní prostor analýzy protokolů. Vyberte **připojení** name (název vašeho konektoru ITSM), za nímž následuje název pracovního prostoru. Například "MyITSMMConnector(MyWorkspace)".

    ![Podrobnosti o ITSM akce](./media/log-analytics-itsmc/itsm-action-details.png)

6. Vyberte **pracovní položka** typu z rozevírací nabídky.
   Vyberte existující šablony nebo vyplnit pole, která vyžaduje vaše ITSM produktu.
7. Klikněte na **OK**.

Při vytváření nebo úpravách Azure pravidla výstrahy, použijte skupinu akce, který má ITSM akce. Když se aktivuje výstraha, je vytvořena pracovní položka v nástroji ITSM.

>[!NOTE]

> Pouze aktivity protokolu výstrahy v současné době podporují ITSM akce. ITSM akce není podporována pro jiné Azure výstrahy.


## <a name="troubleshoot-itsm-connections-in-oms"></a>Řešení potíží s ITSM připojení v OMS
1.  Pokud připojení selže z uživatelského rozhraní připojené zdroje s **Chyba při ukládání připojení** zpráva, proveďte následující kroky:
 - Pro připojení ServiceNow, Cherwell a Provance-zkontrolujte správně zadali uživatelské jméno, heslo, ID klienta a tajný klíč klienta pro jednotlivá připojení.
        -Zkontrolujte, pokud máte dostatečná oprávnění v rámci odpovídající ITSM produktu pro připojení.
 - U připojení k portálu Service Manager-zajistěte, aby webová aplikace je úspěšně nasazen a hybridní připojení se vytvoří. Ověřte připojení se úspěšně naváže na místní počítač portálu Service Manager, najdete na adresu URL webové aplikace podle popisu v dokumentaci k provádění [hybridní připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  Pokud není získávání synchronizovat data z ServiceNow k analýze protokolů, zajistěte, aby ServiceNow instance není pozastaveno. Instance ServiceNow Dev někdy přejděte do režimu spánku při nečinnosti, po dlouhou dobu. Jinak ohlaste daný problém.
3.  Pokud OMS výstrahy fire, ale pracovní položky nejsou vytvořeny v produktu ITSM nebo položek konfigurace nejsou vytvořen nebo propojené pracovní položky nebo další obecné informace, podívejte se na těchto místech:
 -  ITSMC: Řešení zobrazuje souhrnné údaje o připojení nebo pracovní položky nebo počítače atd. Klikněte na dlaždici zobrazující **stav konektoru**, což trvá, abyste **hledání protokolů** s relevantní dotazu. Podívejte se na záznamy protokolu s LogType_S jako chyba. Další informace.
 - **Protokolu vyhledávání** stránky: zobrazení chyb/souvisejících informací o přímo pomocí dotazu *typ = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Řešení potíží s nasazení aplikace webového portálu Service Manager
1.  V případě jakékoliv problémy s nasazení webové aplikace zkontrolujte, zda že máte dostatečná oprávnění v rámci předplatného uvedených vytvořit nebo nasadit prostředky.
2.  Pokud dojde **"Objektu odkaz není nastaven na instanci objektu"** Chyba při spuštění [skriptu](log-analytics-itsmc-service-manager-script.md), zkontrolujte, zda jste zadali platné hodnoty v části **konfigurace uživatele** části .
3.  Pokud se vám nepodaří vytvoření oboru názvů service bus relay, ujistěte se, že zprostředkovatel požadovaný prostředek je zaregistrován v rámci předplatného. Pokud není zaregistrovaný, ručně vytvořte obor názvů předávání sběrnice z portálu Azure. Můžete také vytvořit ji při [vytvoření hybridního připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) z portálu Azure.


## <a name="contact-us"></a>Kontaktujte nás

Pro dotazy nebo zpětnou vazbu na správu konektoru služby IT, kontaktujte nás na adrese [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Další kroky
[Přidání ITSM produktů nebo služeb do konektoru služby správy IT](log-analytics-itsmc-connections.md).
