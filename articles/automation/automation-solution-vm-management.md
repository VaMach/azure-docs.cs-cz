---
title: Spouštění/zastavování virtuálních počítačů v době mimo špičku [Preview] | Microsoft Docs
description: Řešení pro správu virtuálních počítačů spouštějí a zastavují vaše virtuální počítače s Azure Resource Managerem podle časového plánu a proaktivně provádějí monitorování ze služby Log Analytics.
services: automation
documentationcenter: ''
author: MGoedtel
manager: jwhit
editor: ''

ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/07/2016
ms.author: magoedte

---
# Řešení pro spouštění/zastavování virtuálních počítačů v době mimo špičku [Preview] ve službě Automation
Řešení pro spouštění virtuálních počítačů v době mimo špičku (Preview) spouští a zastavuje vaše virtuální počítače s Azure Resource Managerem podle časového plánu definovaného uživatelem a poskytuje podrobné informace o úspěchu úloh služby Automation spouštějících a zastavujících vaše virtuální počítače s použitím služby OMS Log Analytics.  

## Požadavky
* Runbooky pracují s [účtem Spustit jako Azure](automation-sec-configure-azure-runas-account.md).  Účet Spustit jako je upřednostňovanou metodou ověřování, protože namísto hesla, jehož platnost může vypršet nebo které se může často měnit, používá ověřování certifikátu.  
* Toto řešení může spravovat pouze virtuální počítače v rámci stejného předplatného a skupiny prostředků jako účet služby Automation.  
* Toto řešení se nasazuje pouze v následujících oblastech Azure: Austrálie – jihovýchod, Východní USA, Jihovýchodní Asie a Západní Evropa.  Cílem runbooků, které spravují plán virtuálních počítačů, mohou být virtuální počítače ve kterékoli oblasti.  
* Pokud chcete odeslat e-mailová oznámení po dokončení runbooků, které spouštějí a zastavují virtuální počítače, vyžaduje se předplatné Office 365 obchodní třídy.  

## Součásti řešení
Toto řešení se skládá z následujících prostředků, které budou importovány a přidány k vašemu účtu služby Automation.

### Runbooky
| Runbook | Popis |
| --- | --- |
| CleanSolution-MS-Mgmt-VM |Tento runbook odebere veškeré obsažené prostředky a plány, když přejdete k odstranění řešení z předplatného. |
| SendMailO365-MS-Mgmt |Tento runbook odešle e-mail prostřednictvím Office 365 Exchange. |
| StartByResourceGroup-MS-Mgmt-VM |Tento runbook slouží ke spouštění virtuálních počítačů (klasických i s Azure Resource Managerem) uvedených v daném seznamu skupin prostředků Azure. |
| StopByResourceGroup-MS-Mgmt-VM |Tento runbook slouží k zastavování virtuálních počítačů (klasických i s Azure Resource Managerem) uvedených v daném seznamu skupin prostředků Azure. |

<br>

### Proměnné
| Proměnná | Popis |
| --- | --- |
| Runbook **SendMailO365-MS-Mgmt** | |
| SendMailO365-IsSendEmail-MS-Mgmt |Určuje, zda runbooky StartByResourceGroup-MS-Mgmt-VM a StopByResourceGroup-MS-Mgmt-VM mohou po dokončení odeslat e-mailové oznámení.  Vyberte hodnotu **True**, pokud chcete e-mailová upozornění povolit, nebo hodnotu **False**, chcete-li je zakázat. Výchozí je hodnota **False**. |
| Runbook **StartByResourceGroup-MS-Mgmt-VM** | |
| StartByResourceGroup-ExcludeList-MS-Mgmt-VM |Zadejte názvy virtuálních počítačů, které mají být z operace správy vyloučeny; názvy oddělujte středníkem (;). V hodnotách se rozlišují malá a velká písmena a je podporován zástupný znak (hvězdička). |
| StartByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt |Text, který lze připojit na začátek obsahu e-mailové zprávy. |
| StartByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt |Určuje název účtu služby Automation, který obsahuje runbook Email.  **Neprovádějte žádné změny této proměnné.** |
| StartByResourceGroup-SendMailO365-EmailRunbookName-MS-Mgmt |Určuje název runbooku pro e-mail.  Slouží k odesílání e-mailů prostřednictvím runbooků StartByResourceGroup-MS-Mgmt-VM a StopByResourceGroup-MS-Mgmt-VM.  **Neprovádějte žádné změny této proměnné.** |
| StartByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt |Určuje název skupiny prostředků, která obsahuje runbook Email.  **Neprovádějte žádné změny této proměnné.** |
| StartByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt |Určuje text pro řádek předmětu e-mailu. |
| StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt |Určuje příjemce e-mailu.  Jména oddělujte středníkem (;). |
| StartByResourceGroup-TargetResourceGroups-MS-Mgmt-VM |Zadejte názvy virtuálních počítačů, které mají být z operace správy vyloučeny; názvy oddělujte středníkem (;). V hodnotách se rozlišují malá a velká písmena a je podporován zástupný znak (hvězdička).  Výchozí hodnota (hvězdička) bude obsahovat všechny skupiny prostředků v rámci předplatného. |
| StartByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM |Určuje předplatné obsahující virtuální počítače, které mají být spravovány tímto řešením.  Musí se jednat o předplatné, ve kterém je obsažen účet služby Automation tohoto řešení. |
| Runbook **StopByResourceGroup-MS-Mgmt-VM** | |
| StopByResourceGroup-ExcludeList-MS-Mgmt-VM |Zadejte názvy virtuálních počítačů, které mají být z operace správy vyloučeny; názvy oddělujte středníkem (;). V hodnotách se rozlišují malá a velká písmena a je podporován zástupný znak (hvězdička). |
| StopByResourceGroup-SendMailO365-EmailBodyPreFix-MS-Mgmt |Text, který lze připojit na začátek obsahu e-mailové zprávy. |
| StopByResourceGroup-SendMailO365-EmailRunBookAccount-MS-Mgmt |Určuje název účtu služby Automation, který obsahuje runbook Email.  **Neprovádějte žádné změny této proměnné.** |
| StopByResourceGroup-SendMailO365-EmailRunbookResourceGroup-MS-Mgmt |Určuje název skupiny prostředků, která obsahuje runbook Email.  **Neprovádějte žádné změny této proměnné.** |
| StopByResourceGroup-SendMailO365-EmailSubject-MS-Mgmt |Určuje text pro řádek předmětu e-mailu. |
| StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt |Určuje příjemce e-mailu.  Jména oddělujte středníkem (;). |
| StopByResourceGroup-TargetResourceGroups-MS-Mgmt-VM |Zadejte názvy virtuálních počítačů, které mají být z operace správy vyloučeny; názvy oddělujte středníkem (;). V hodnotách se rozlišují malá a velká písmena a je podporován zástupný znak (hvězdička).  Výchozí hodnota (hvězdička) bude obsahovat všechny skupiny prostředků v rámci předplatného. |
| StopByResourceGroup-TargetSubscriptionID-MS-Mgmt-VM |Určuje předplatné obsahující virtuální počítače, které mají být spravovány tímto řešením.  Musí se jednat o předplatné, ve kterém je obsažen účet služby Automation tohoto řešení. |

<br>

### Plány
| Plán | Popis |
| --- | --- |
| StartByResourceGroup-Schedule-MS-Mgmt |Plán pro runbook StartByResourceGroup, který provádí spouštění virtuálních počítačů spravovaných tímto řešením. |
| StopByResourceGroup-Schedule-MS-Mgmt |Plán pro runbook StopByResourceGroup, který provádí vypínání virtuálních počítačů spravovaných tímto řešením. |

### Přihlašovací údaje
| Přihlašovací údaj | Popis |
| --- | --- |
| O365Credential |Určuje platný uživatelský účet Office 365 pro odesílání e-mailů.  Je vyžadován pouze v případě, že je proměnná SendMailO365-IsSendEmail-MS-Mgmt nastavena na hodnotu **True**. |

## Konfigurace
Provedením následujících kroků přidejte řešení pro spouštění/zastavování virtuálních počítačů v době mimo špičku [Preview] do svého účtu služby Automation a poté řešení upravte prostřednictvím konfigurace proměnných.

1. Na domovské obrazovce na webu Azure Portal vyberte dlaždici **Marketplace**.  Pokud již dlaždice není připnutá k vaší domovské obrazovce, vyberte v navigačním podokně vlevo možnost **Nový**.  
2. V okně Marketplace zadejte do pole hledání text **pustit virtuální počítač** ve vyhledávacím poli a pak ve výsledcích hledání vyberte řešení **Spouštění /zastavování virtuálních počítačů v době mimo špičku [Preview]**.  
3. V okně **Spouštění/zastavování virtuálních počítačů v době mimo špičku [Preview]** pro vybrané řešení zkontrolujte souhrnné informace a pak klikněte na **Vytvořit**.  
4. Zobrazí se okno **Přidat řešení**, ve kterém se zobrazí výzva ke konfigurování řešení, než je budete moci naimportovat do svého předplatného služby Automation.<br><br> ![Okno Přidat řešení správy virtuálních počítačů](media/automation-solution-vm-management/vm-management-solution-add-solution-blade.png)<br><br>
5. V okně **Přidat řešení** vyberte možnost **Pracovní prostor**. Můžete zde vybrat pracovní prostor OMS propojený s předplatným Azure, které obsahuje příslušný účet služby Automation, nebo vytvořit nový pracovní prostor OMS.  Pokud nemáte pracovní prostor OMS, můžete vybrat možnost **Vytvořit nový pracovní prostor** a v okně **Pracovní prostor OMS** provést následující kroky: 
   
   * Zadejte název pro nový **pracovní prostor OMS**.
   * Vyberte **předplatné**, které má být cílem propojení, výběrem z rozevíracího seznamu, pokud výchozí vybrané předplatné není vhodné.
   * Pro možnost **Skupina prostředků** můžete vytvořit novou skupinu prostředků nebo vybrat existující skupinu prostředků.  
   * Vyberte **Umístění**.  V současnosti jsou pro výběr k dispozici pouze umístění **Austrálie – jihovýchod**, **Východní USA**, **Jihovýchodní Asie** a **Západní Evropa**.
   * Vyberte možnost u položky **Cenová úroveň**.  Řešení je k dispozici ve dvou úrovních: úroveň Free a placená úroveň OMS.  V úrovni Free je omezen objem dat shromážděných za den, doba uchovávání a počet minut běhu úloh runbooku.  V placené úrovni OMS není objem dat shromážděných za den omezen.  
     
     > [!NOTE]
     > Ačkoli je jako volitelná zobrazena placená úroveň Standalone, nelze ji použít.  Pokud ji vyberete a budete pokračovat ve vytváření tohoto řešení v rámci předplatného, dojde k selhání.  Problém bude odstraněn po oficiálním vydání tohoto řešení.<br>Pokud použijete toto řešení, bude využívat pouze příjem protokolů a minuty úloh služby Automation.  Řešení nepřidá do vašeho prostředí další uzly OMS.  
     > 
     > 
6. Po zadání požadovaných informací v okně **Pracovní prostor OMS** klikněte na **Vytvořit**.  Během ověřování informací a vytváření pracovního prostoru můžete průběh zpracování sledovat prostřednictvím položky nabídky **Oznámení**.  Budete přesměrováni zpět do okna **Přidat řešení**.  
7. V okně **Přidat řešení** vyberte možnost **Účet služby Automation**.  Pokud vytváříte nový pracovní prostor OMS, bude třeba, abyste vytvořili i nový účet služby Automation, který bude přidružen k novému pracovnímu prostoru OMS určenému dříve, a to včetně vašeho předplatného Azure, skupiny prostředků a oblasti.  Můžete vybrat možnost **Vytvořit účet Automation** a v okně **Přidat účet Automation** zadat následující údaje: 
   
   * Do pole **Název** zadejte název účtu služby Automation.
     
     Všechny ostatní možnosti se vyplní automaticky na základě vybraného pracovního prostoru OMS. Tyto možnosti nelze upravovat.  Účet Spustit v Azure jako představuje výchozí metodu ověřování pro runbooky obsažené v tomto řešení.  Po kliknutí na **OK** se ověří možnosti konfigurace a vytvoří se účet služby Automation.  Průběh zpracování můžete sledovat prostřednictvím možnosti nabídky **Oznámení**. 
     
     Můžete také vybrat existující účet služby Automation Spustit jako.  Všimněte si, že účet, který vyberete, již nelze propojit s jiným pracovním prostorem OMS. V opačném případě se v okně zobrazí zpráva s informací.  Pokud je propojení již vytvořeno, je třeba vybrat jiný účet služby Automation Spustit jako nebo vytvořit nový.<br><br> ![Účet služby Automation, který je již propojen s pracovním prostorem OMS](media/automation-solution-vm-management/vm-management-solution-add-solution-blade-autoacct-warning.png)<br>
8. Nakonec v okně **Přidat řešení** vyberte možnost **Konfigurace**. Zobrazí se okno **Parametry**.  V okně **Parametry** se zobrazí výzva k provedení následujících akcí:  
   
   * Zadejte hodnotu **Názvy cílové skupiny prostředků**. Jedná se o název skupiny prostředků obsahující virtuální počítače, které mají být spravovány tímto řešením.  Můžete zadat více než jeden název a jednotlivé názvy oddělit středníky (v hodnotách se rozlišují malá a velká písmena).  Je podporováno použití zástupného znaku, pokud jsou cílem virtuální počítače ve všech skupinách prostředků v rámci předplatného.
   * Vyberte hodnotu pro položku **Plán**. Jedná se o opakované datum a čas spouštění a zastavování virtuálních počítačů v cílových skupinách prostředků.  
9. Po dokončení konfigurace počátečních nastavení vyžadovaných pro příslušné řešení vyberte možnost **Vytvořit**.  Všechna nastavení budou ověřena a poté se provede pokus o nasazení řešení v rámci vašeho předplatného.  Dokončení tohoto procesu může trvat několik sekund a průběh zpracování můžete sledovat prostřednictvím možnosti nabídky **Oznámení**. 

## Četnost shromažďování dat
Data protokolu úloh služby Automation a datových proudů úloh jsou přijímána v úložišti OMS každých pět minut.  

## Použití řešení
Když přidáte řešení pro správu virtuálních počítačů, v pracovním prostoru OMS se na řídicí panel OMS přidá dlaždice **Zobrazení StartStopVM**.  Na této dlaždici se zobrazuje počet a grafická reprezentace úloh runbooků pro příslušné řešení, které byly spuštěny a úspěšně dokončeny.<br><br> ![Dlaždice Zobrazení StartStopVM správy virtuálních počítačů](media/automation-solution-vm-management/vm-management-solution-startstopvm-view-tile.png)  

V rámci svého účtu služby Automation máte možnost přístupu k řešení a jeho správy po výběru dlaždice **Řešení** a potom v okně **Řešení** výběrem řešení **Start-Stop-VM [pracovní prostor]** ze seznamu.<br><br> ![Seznam řešení služby Automation](media/automation-solution-vm-management/vm-management-solution-autoaccount-solution-list.png)  

Výběrem řešení zobrazíte okno řešení **Start-Stop-VM [pracovní prostor]**, ve kterém můžete zkontrolovat důležité podrobnosti, například dlaždici **StartStopVM** (podobně jako v pracovním prostoru OMS), na které je zobrazen počet a grafická reprezentace úloh runbooků pro příslušné řešení, které byly spuštěny a úspěšně dokončeny.<br><br> ![Okno řešení pro virtuální počítače služby Automation](media/automation-solution-vm-management/vm-management-solution-solution-blade.png)  

Odtud můžete také otevřít prostor OMS a provést detailnější analýzu záznamů úlohy.  Stačí kliknout na **Všechna nastavení**, v okně **Nastavení** vybrat **Rychlý start** a poté v okně **Rychlý start** vybrat **Portál OMS**.   Otevře se nová karta nebo nová relace prohlížeče a zobrazí se pracovní prostor OMS přidružený k vašemu účtu a předplatnému služby Automation.  

### Konfigurace e-mailových oznámení
Chcete-li povolit e-mailová oznámení po dokončení spouštění a zastavování runbooků pro virtuální počítače, budete muset změnit přihlašovací údaj **O365Credential** a alespoň následující proměnné:

* SendMailO365-IsSendEmail-MS-Mgmt
* StartByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt
* StopByResourceGroup-SendMailO365-EmailToAddress-MS-Mgmt

Chcete-li konfigurovat přihlašovací údaj **O365Credential**, proveďte následující kroky:

1. V rámci svého účtu služby Automation klikněte na možnost **Všechna nastavení** v horní části okna. 
2. V okně **Nastavení** v části **Prostředky služby Automation** vyberte možnost **Prostředky**. 
3. V okně **Prostředky** vyberte dlaždici **Přihlašovací údaj** a v okně **Přihlašovací údaj** vyberte položku **O365Credential**.  
4. Zadejte platné uživatelské jméno a heslo Office 365 a potom kliknutím na možnost **Uložit** uložte provedené změny.  

Pokud chcete konfigurovat proměnné zvýrazněné dříve, proveďte následující kroky:

1. V rámci svého účtu služby Automation klikněte na možnost **Všechna nastavení** v horní části okna. 
2. V okně **Nastavení** v části **Prostředky služby Automation** vyberte možnost **Prostředky**. 
3. V okně **Prostředky** vyberte dlaždici **Proměnné** a v okně **Proměnné** vyberte proměnnou uvedenou výše a pak upravte její hodnotu za příslušným popisem zadaným dříve v části [proměnná](##variables).  
4. Kliknutím na možnost **Uložit** uložte změny do proměnné.   

### Úprava plánů spouštění a vypínání
Správa plánů spouštění a vypínání v tomto řešení probíhá podle stejných kroků uvedených v tématu [Plánování runbooku ve službě Azure Automation](automation-scheduling-a-runbook.md).  Pamatujte, že nelze upravit konfiguraci plánu.  Bude třeba zakázat existující plán, následně vytvořit nový plán a propojit jej s runbookem **StartByResourceGroup-MS-Mgmt-VM** nebo **StopByResourceGroup-MS-Mgmt-VM**, na který chcete plán použít.   

## Záznamy služby Log Analytics
Služba Automation vytváří v úložišti OMS dva typy záznamů.

### Protokoly úloh
| Vlastnost | Popis |
| --- | --- |
| Volající |Kdo operaci zahájil.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy. |
| Kategorie |Klasifikace typu dat.  Službě Automation odpovídá hodnota JobLogs. |
| CorrelationId |Identifikátor GUID, který představuje ID korelace úlohy runbooku. |
| JobId |Identifikátor GUID, který představuje ID úlohy runbooku. |
| operationName |Určuje typ operace prováděné v Azure.  Službě Automation odpovídá hodnota Job. |
| resourceId |Určuje typ prostředku v Azure.  V případě služby Automation je hodnotou účet služby Automation přidružený k příslušnému runbooku. |
| ResourceGroup |Určuje název skupiny prostředků příslušné úlohy runbooku. |
| ResourceProvider |Určuje službu Azure poskytující prostředky, které můžete nasadit a spravovat.  Službě Automation odpovídá hodnota Azure Automation. |
| ResourceType |Určuje typ prostředku v Azure.  V případě služby Automation je hodnotou účet služby Automation přidružený k příslušnému runbooku. |
| resultType |Stav úlohy runbooku.  Možné hodnoty:<br>- Spuštěno<br>- Zastaveno<br>- Pozastaveno<br>- Neúspěch<br>- Úspěch |
| resultDescription |Popisuje výsledný stav úlohy runbooku.  Možné hodnoty:<br>- Úloha se spustila<br>- Zpracování úlohy se nezdařilo<br>- Úloha je dokončená |
| RunbookName |Určuje název runbooku. |
| SourceSystem |Určuje zdrojový systém pro odeslaná data.  Službě Automation odpovídá hodnota :OpsManager. |
| StreamType |Určuje typ události. Možné hodnoty:<br>- Podrobné<br>- Výstup<br>- Chyba<br>- Varování |
| SubscriptionId |Určuje ID předplatného úlohy. |
| Time |Datum a čas provedení úlohy runbooku. |

### Datové proudy úlohy
| Vlastnost | Popis |
| --- | --- |
| Volající |Kdo operaci zahájil.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy. |
| Kategorie |Klasifikace typu dat.  Službě Automation odpovídá hodnota JobStreams. |
| JobId |Identifikátor GUID, který představuje ID úlohy runbooku. |
| operationName |Určuje typ operace prováděné v Azure.  Službě Automation odpovídá hodnota Job. |
| ResourceGroup |Určuje název skupiny prostředků příslušné úlohy runbooku. |
| resourceId |Určuje ID prostředku v Azure.  V případě služby Automation je hodnotou účet služby Automation přidružený k příslušnému runbooku. |
| ResourceProvider |Určuje službu Azure poskytující prostředky, které můžete nasadit a spravovat.  Službě Automation odpovídá hodnota Azure Automation. |
| ResourceType |Určuje typ prostředku v Azure.  V případě služby Automation je hodnotou účet služby Automation přidružený k příslušnému runbooku. |
| resultType |Výsledek úlohy runbooku v době, kdy byla událost vygenerována.  Možné hodnoty:<br>- Probíhá zpracování |
| resultDescription |Zahrnuje výstupní datový proud z runbooku. |
| RunbookName |Název runbooku. |
| SourceSystem |Určuje zdrojový systém pro odeslaná data.  Službě Automation odpovídá hodnota OpsManager. |
| StreamType |Typ datového proudu úlohy. Možné hodnoty:<br>- Průběh<br>- Výstup<br>- Varování<br>- Chyba<br>- Ladění<br>- Podrobné |
| Time |Datum a čas provedení úlohy runbooku. |

Při provádění jakékoli hledání v protokolech, které vrací záznamy kategorie **JobLogs** nebo **JobStreams**, můžete vybrat zobrazení **JobLogs** nebo **JobStreams**, které obsahuje sadu dlaždic se souhrnem aktualizací vrácených hledáním.

## Ukázky hledání v protokolech
V následující tabulce jsou uvedeny ukázky hledání v protokolech pro záznamy úloh shromážděné tímto řešením. 

| Dotaz | Popis |
| --- | --- |
| Najít úlohy runbooku StartVM, které byly úspěšně dokončeny |Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Succeeded &#124; measure count() by JobId_g |
| Najít úlohy runbooku StopVM, které byly úspěšně dokončeny |Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" ResultType=Failed &#124; measure count() by JobId_g |
| Zobrazit stav úloh v čase pro runbooky StartVM a StopVM |Category=JobLogs RunbookName_s="StartByResourceGroup-MS-Mgmt-VM" OR "StopByResourceGroup-MS-Mgmt-VM" NOT(ResultType="started") |

## Další kroky
* Další informace o tom, jak vytvářet různé vyhledávací dotazy a kontrolovat protokoly úloh služby Automation s použitím služby Log Analytics, najdete v článku [Vyhledávání protokolů v Log Analytics](../log-analytics/log-analytics-log-searches.md)
* Další informace o spouštění runbooků, postupy při monitorování úloh runbooků a další technické podrobnosti najdete v článku [Sledování úlohy runbooku](automation-runbook-execution.md).
* Další informace o službě Log Analytics v OMS a o zdrojích pro shromažďování dat najdete v článku [Přehled shromažďování dat úložiště Azure ve službě Log Analytics](../log-analytics/log-analytics-azure-storage.md).

<!--HONumber=Oct16_HO3-->


