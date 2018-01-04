
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Virtuální počítače zobrazení naplánována údržba na portálu

Jakmile je naplánováno wave plánované údržby a oznámení se odesílají, můžete sledovat seznam virtuálních počítačů, které jsou ovlivněny wave nadcházející údržbě. 

Můžete použít portál Azure a vyhledejte naplánována údržba virtuálních počítačů.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. V levém navigačním panelu, klikněte na tlačítko **virtuální počítače**.

3. V podokně virtuální počítače, klikněte **sloupce** tlačítko otevřete seznam dostupných sloupců.

4. Vyberte a přidejte následující sloupce:

   **Údržby** -zobrazuje stav údržby pro virtuální počítač. Toto jsou možné hodnoty:
      
      | Hodnota | Popis |
      |-------|-------------|
      | Začít teď | Virtuální počítač je v okně údržby samoobslužné služby, který umožňuje zahájení údržby sami. Níže najdete postup při spuštění údržby na vašem virtuálním počítači | 
      | Podle plánu | Virtuální počítač je naplánována údržba bez možnosti pro vás k zahájení údržby. Další nástroje pro správu a údržbu výběrem okna automaticky naplánované v tomto zobrazení, nebo kliknutím na virtuálním počítači | 
      | Dokončeno | Máte úspěšně spustil a dokončení údržby na vašem virtuálním počítači. | 
      | Vynecháno| Rozhodli jste se spustit údržby úspěšná. Nebudete moci používat možnost samoobslužné služby údržby. Virtuální počítač bude muset restartovat Azure během fáze plánované údržby. | 

   **Údržby aktivní** -zobrazuje časový interval, když údržby můžete svým spustit virtuální počítače.
   
   **Naplánovaná údržba** -zobrazuje časový interval, když Azure se restartuje virtuální počítač, aby bylo možné dokončit údržbu. 




## <a name="notification-and-alerts-in-the-portal"></a>Oznámení a výstrahy na portálu

Azure komunikuje zasláním e-mailu ke skupině předplatné vlastník a spoluvlastníci plán pro plánované údržby. Vytvořením aktivita Azure protokolu výstrahy, můžete přidat další příjemce a kanály pro tuto komunikaci. Další informace najdete v tématu [aktivity monitorování předplatné s protokol činnosti Azure] (.. / articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce na levé straně vyberte **monitorování**. 
3. V **monitorování – protokol aktivit** podokně, vyberte **výstrahy**.
4. V **monitorování – výstrahy** podokně klikněte na tlačítko **+ přidat aktivitu protokolu upozornění**.
5. Zadejte informace do **přidat aktivitu protokolu upozornění** stránky a nezapomeňte nastavit následující **kritéria**: **typ**: údržby **stav**: Všechny (nenastavujte stav na aktivní nebo vyřešený) **úroveň**: všechny
    
Další informace o tom, jak konfigurovat výstrahy protokolu aktivit najdete v tématu [vytvořit aktivitu protokolu výstrahy](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Zahájení údržby na vašem virtuálním počítači z portálu.

Při hledání na podrobnosti virtuálního počítače, bude moci zobrazit další podrobnosti týkající se údržby.  
V horní části zobrazení podrobností virtuálního počítače bude přidán nový pásu karet oznámení Pokud virtuální počítač je součástí wave plánované údržby. Kromě toho je přidána nová možnost spuštění údržby, pokud je to možné. 


Kliknutím na oznámení údržby naleznete na stránce údržby s dalšími podrobnostmi na plánované údržby. Odtud můžete budou moci **spustit údržby** na vašem virtuálním počítači.

Jakmile začnete údržby, virtuální počítač bude restartován a údržby stav bude aktualizován, aby odrážela výsledek během několika minut.

Pokud provedena okno, kde můžete spustit údržby můžete stále bude moci zobrazit okno, pokud virtuální počítač bude restartován v Azure. 
