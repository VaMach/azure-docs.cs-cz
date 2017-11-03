Můžete využít mnoho příležitosti k monitorování virtuálních počítačů shromažďování, zobrazení a analýza diagnostiky a protokolovat data. Uděláte to jednoduché [monitorování](../articles/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) virtuálního počítače, můžete použít na obrazovce Přehled pro virtuální počítač na portálu Azure. Můžete použít [rozšíření](../articles/virtual-machines/windows/extensions-features.md) ke konfiguraci diagnostiky na virtuální počítače shromažďovat další data metriky. Můžete také používat rozšířené možnosti monitorování, jako třeba [Application Insights](../articles/application-insights/app-insights-overview.md) a [analýzy protokolů](../articles/log-analytics/log-analytics-overview.md).

## <a name="diagnostics-and-metrics"></a>Diagnostika a metriky 

Můžete nastavit a monitorovat kolekce [diagnostická data](https://docs.microsoft.com/cli/azure/vm/diagnostics) pomocí [metriky](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) v portálu Azure, Azure CLI, Azure PowerShell a programovací rozhraní aplikace programovací (rozhraní API). Můžete například provést následující věci:

- **Sledujte základní metriky pro virtuální počítač.** Na obrazovce Přehled portálu Azure zahrnují základní metriky zobrazené využití procesoru, využití sítě, celkový počet bajtů disku a disku operací za sekundu.

- **Povolení shromažďování těchto Diagnostika spouštění a zobrazit ji pomocí portálu Azure.** Při zpětném přepnutí vlastní image Azure nebo ani jeden z Image platformy spouštění, může být mnoha důvodů, proč se virtuální počítač získá do jiných spouštěcího stavu. Při vytváření virtuálního počítače kliknutím můžete snadno povolit Diagnostika spouštění **povoleno** pro Diagnostika spouštění obrazovky nastavení v části monitorování.

    Jak spustit virtuální počítače, agenta pro diagnostiku spouštěcí zaznamená spouštěcí výstup a uloží je v úložišti Azure. Tato data můžete použít k odstraňování problémů spouštění virtuálních počítačů. Diagnostika spouštění nepovolí automaticky při vytvoření virtuálního počítače z nástroje pro příkazový řádek. Před povolením Diagnostika spouštění, musí být vytvořen pro ukládání protokolů spouštěcí účet úložiště. Pokud povolíte Diagnostika spouštění na portálu Azure, účet úložiště se automaticky vytvoří za vás.

    Pokud povolíte nebyla Diagnostika spouštění a při vytvoření virtuálního počítače, můžete vždy ji povolit později pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmbootdiagnostics), nebo [šablony Azure Resource Manageru](../articles/virtual-machines/windows/extensions-diagnostics-template.md).

- **Povolení shromažďování těchto dat diagnostiky hostovaného operačního systému.** Když vytvoříte virtuální počítač, máte možnost na obrazovce nastavení povolí se Diagnostika hostovaného operačního systému. Když povolíte shromažďování dat diagnostiky [IaaSDiagnostics rozšíření pro Linux](../articles/virtual-machines/linux/diagnostic-extension.md) nebo [IaaSDiagnostics rozšíření pro Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) se přidá do virtuálního počítače, které umožňuje shromažďovat další data na disku, využití procesoru a paměti.

    Shromažďovat diagnostická data, můžete nakonfigurovat automatické škálování pro virtuální počítače. Můžete také nakonfigurovat protokoly ukládání dat a nastavení výstrah, který vám oznamuje, kde výkon není tak.

## <a name="alerts"></a>Výstrahy

Můžete vytvořit [výstrahy](../articles/monitoring-and-diagnostics/monitoring-overview-alerts.md) na základě výkonu specifických metriky. Příklady problémy, které vám mohou být upozorňováni: když průměrné využití procesoru překročí určitou mez nebo volné místo na disku klesne pod určitou. Výstrahy se dá nakonfigurovat v [portál Azure](../articles/monitoring-and-diagnostics/insights-alerts-portal.md)pomocí [prostředí Azure PowerShell](../articles/monitoring-and-diagnostics/insights-alerts-powershell.md), nebo [rozhraní příkazového řádku Azure](../articles/monitoring-and-diagnostics/insights-alerts-command-line-interface.md).

## <a name="azure-service-health"></a>Azure Service Health

[Azure stavu služby](../articles/service-health/service-health-overview.md) poskytuje přizpůsobené pokyny a podpora, pokud problémy ve službách Azure můžete ovlivnit, a pomáhá připravíte na nadcházející plánované údržby. Azure stavu služby Výstrahy můžete a týmům pomocí cílové a flexibilní oznámení.

## <a name="azure-resource-health"></a>Azure Resource Health

[Azure Resource health](../articles/service-health/resource-health-overview.md) vám pomůže diagnostikovat a získat podporu, když Azure problém ovlivní vaše prostředky. Informuje o aktuálním a dřívějším stavu prostředků a pomáhá zmírnit problémy. Resource Health poskytuje technickou podporu, když potřebujete pomoc při potížích se službami Azure.

## <a name="logs"></a>Logs

[Protokol činnosti Azure](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md) je protokol odběru, který poskytuje přehled o události na úrovni předplatného, k nimž došlo v Azure. V protokolu obsahuje řadu dat z provozních dat Azure Resource Manager aktualizací na události stavu služby. Můžete kliknout na protokol aktivit na portálu Azure k zobrazení protokolu pro virtuální počítač.

Mezi věcí, které můžete provést pomocí protokolu aktivit, patří:

- Vytvoření [upozornění na aktivitu protokolu události](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).
- [Stream do centra událostí](../articles/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) pro přijímání službu třetí strany nebo řešení vlastní analýzy, jako je například PowerBI.
- Analyzovat v Power BI pomocí [balíček obsahu Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Uložit na účet úložiště](../articles/monitoring-and-diagnostics/monitoring-archive-activity-log.md) pro archivaci nebo ruční kontroly. Můžete zadat dobu uchování (ve dnech) pomocí profilu protokolu.

Můžete také přístup k datům aktivity protokolu pomocí [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/monitor), nebo [monitorování REST API](https://docs.microsoft.com/rest/api/monitor/).

[Azure diagnostické protokoly](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) protokoly vysílaných virtuálního počítače, které poskytují bohatou a často data o své činnosti. Diagnostické protokoly se liší od protokolu aktivit tím, že poskytuje přehled o operacích, které byly provedeny v rámci virtuálního počítače.

Mezi věcí, které můžete provést pomocí protokolů diagnostiky, patří:

- [Uložit je do účtu úložiště](../articles/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) pro auditování nebo ruční kontroly. Můžete zadat dobu uchování (ve dnech), používá nastavení diagnostiky pro prostředek.
- [Stream je do centra událostí](../articles/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) pro přijímání službu třetí strany nebo řešení vlastní analýzy, jako je například PowerBI.
- Analyzovat, s [analýzy protokolů OMS](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Pokročilé sledování

- [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/) poskytuje možnosti nápravy monitorování, výstrahy a výstrahy v cloudové a místní prostředky. Rozšíření můžete nainstalovat [virtuálního počítače s Linuxem](../articles/virtual-machines/linux/extensions-oms.md) nebo [virtuální počítač s Windows](../articles/virtual-machines/windows/extensions-oms.md) , nainstaluje se OMS agent a zaregistruje virtuální počítač do existující pracovní prostor OMS.

- [Analýza protokolu](../articles/log-analytics/log-analytics-overview.md) je služba v OMS, který monitoruje své cloudové a místní prostředí k udržování své dostupnosti a výkonu. Shromažďuje data generovaná prostředky ve vašem cloudovém a místním prostředí a také data z dalších nástrojů pro monitorování a poskytuje analýzy napříč zdroji.

    Doporučené metody pro shromažďování protokolů a metriky pro systém Windows a virtuální počítače s Linuxem, je instalace agenta analýzy protokolů. Nejjednodušší způsob, jak nainstalovat agenta analýzy protokolů na virtuální počítač je prostřednictvím [rozšíření virtuálního počítače Log Analytics](../articles/log-analytics/log-analytics-azure-vm-extension.md). Pomocí rozšíření zjednodušuje proces instalace a automaticky nakonfiguruje agenta k odesílání dat do pracovního prostoru analýzy protokolů, který určíte. Agent je také automaticky aktualizovány, zajistíte, že máte nejnovější funkce a opravy.

- [Sledovací proces sítě](../articles/network-watcher/network-watcher-monitoring-overview.md) umožňuje monitorovat virtuální počítač a jeho přidružené prostředky, které se vztahují k síti, která jsou v. Rozšíření sítě sledovacích procesů agenta můžete nainstalovat [virtuálního počítače s Linuxem](../articles/virtual-machines/linux/extensions-nwa.md) nebo [virtuální počítač s Windows](../articles/virtual-machines/windows/extensions-nwa.md).

## <a name="next-steps"></a>Další kroky
- Provede jednotlivými kroky v [monitorování virtuálního počítače s Windows v prostředí Azure PowerShell](../articles/virtual-machines/windows/tutorial-monitoring.md) nebo [monitorovat virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure](../articles/virtual-machines/linux/tutorial-monitoring.md).
- Další informace o osvědčených postupech kolem [monitorovací a diagnostické](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
