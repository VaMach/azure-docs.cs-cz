---
title: "Provádění datové vědy projekty - Azure | Microsoft Docs"
description: "Jak vědecký pracovník dat můžete spustit projekt vědecké účely data v organizovaným, verze kontrolovat a způsob spolupráce."
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: bradsev;
ms.openlocfilehash: 1015a9f24ca2c175ff367b1748f05bb3e464457f
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="execution-of-data-science-projects"></a>Provádění datové vědy projekty

Tento dokument popisuje, jak můžete vývojáři spustit projekt vědecké účely data v systematické, verze řízené a spolupráce způsob, jak v rámci projektový tým pomocí [proces vědecké účely dat Team](overview.md) (TDSP). TDSP je architektura vyvinuté společností Microsoft, která poskytuje strukturovaných posloupnost aktivit k úspěšnému provedení řešení založená na cloudu, prediktivní analýzy. Přehled role pracovníky a jejich přidružených úloh, které zpracovávají datové vědy team standardizace na tento proces, najdete v části [proces vědecké účely dat Team rolí a úloh](roles-tasks.md). 

Tento článek obsahuje pokyny o tom, jak: 

1. proveďte **plánování sprintu** pro pracovní položky spojených s projektem.<br> Pokud jste obeznámeni s plánování sprintu, můžete najít podrobnosti a obecné informace [sem](https://en.wikipedia.org/wiki/Sprint_(software_development) "zde"). 
2. **Přidání pracovních položek** k sprintů.
3. **propojení pracovních položek pomocí kódování aktivity** sledován pomocí git.
4. proveďte **code zkontrolujte**. 

> [!NOTE]
> Kroky potřebné k nastavení prostředí team TDSP pomocí Visual Studio Team Services (VSTS) jsou uvedeny v následující sadu pokynů. Určí, jak provést tyto úlohy s služby VSTS, protože se jedná o tom, jak implementovat TDSP v Microsoft.  Pokud si zvolíte používání služby VSTS, položky (3) a (4) v předchozím seznamu jsou výhody, které jste získali přirozeně. Pokud jiný kód hostování platformy slouží pro vaši skupinu, úlohy, které je potřeba dokončit vedoucí týmu obecně se nezmění. Ale způsob k dokončení těchto úloh se bude lišit. Například na položku v části půl, **propojit pracovní položky pomocí git větev**, nemusí být stejně snadná jako na služby VSTS.
>
>

Následující obrázek ukazuje typické sprintu plánování, kódování a pracovní postup správy zdrojového kódu, které jsou součástí implementace projektu vědecké účely dat:

![1](./media/project-execution/project-execution-1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminologie 

V TDSP úbytek sprintu plánování framework, existují čtyři často používané typy **pracovní položky**: **funkce**, **uživatelský scénář**, **úloh**, a **Chyb**. Každý týmový projekt udržuje jeden nevyřízených položek pro všechny pracovní položky. Neexistuje žádná nevyřízená úloha na úrovni úložiště git v části týmového projektu. Zde jsou jejich definice:

- **Funkce**: funkce odpovídá engagement projektu. Různé oznámeních podporujících zapojení uživatelů s klientem jsou považovány za různých funkcí. Podobně je vhodné vzít v úvahu různých fází projektu s klientem jako různých funkcí. Pokud například vyberete schéma ***ClientName EngagementName*** na název vaší funkce, pak snadno poznáte kontextu projektu nebo engagement z názvů sami.
- **Scénáře**: scénářů se jiný pracovní položky, které jsou nutné k dokončení funkce (projekt) začátku do konce. Příklady scénářů:
    - Získání dat 
    - Zkoumání dat 
    - Generování funkce
    - Vytváření modelů
    - Zprovozňování modely 
    - Přeučení modelů
- **Úloha**: úlohy jsou přiřadit kód nebo dokumentu pracovní položky nebo jiných aktivit, které je potřeba k dokončení konkrétní scénáře. Například úloh v článku *získávání dat* může být:
    -  Získání pověření systému SQL Server 
    -  Nahrávání dat do SQL Data Warehouse. 
- **Chyby**: chyby jsou obvykle odkazovat na opravy, které jsou potřebné pro existující kód nebo dokumentu, které se provádějí při dokončení úlohy. Pokud chyb je způsobená chybějícím fázích nebo úlohy v uvedeném pořadí, můžete eskalovat pro scénáře nebo úlohu. 

> [!NOTE]
> Funkce, scénářů, úlohy a chyby ze softwaru kód správy (SCM), který se má použít v vědecké zpracování dat vždy pouze vypůjčí koncepty. Se může mírně lišit od jejich konvenční SCM definice.
>
>

Datových vědců může mít více možnost pomocí agilní šablony, která konkrétně zarovnaná s fáze životního cyklu TDSP. Si uvědomit byl vytvořen sprintu odvozené agilní plánování šablony, kde Epics atd. scénářů jsou nahrazovány fáze životního cyklu TDSP nebo substages. Můžete najít dokumentaci o tom, jak vytvořit agilní šablona [zde](https://msdata.visualstudio.com/AlgorithmsAndDataScience/TDSP/_git/TDSP?path=%2FDocs%2Fteam-data-science-process-agile-template.md&version=GBxibingao&_a=preview).


##  2. <a name='SprintPlanning-2'></a>Plánování sprintu 

Plánování sprintu je užitečné pro stanovení priorit projekt a plánování prostředků a přidělení. Mnoho datových vědců pověření s více projekty, z nichž každá může trvat měsíců k dokončení. Projekty často pokračovat v různých mezerami. Na serveru služby VSTS můžete snadno vytvářet, spravovat a sledování pracovních položek v týmových projektech a proveďte sprintu plánování zajistit, že projekty přesouváte dál podle očekávání. 

Postupujte podle [tento odkaz](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) podrobné pokyny k plánování v služby VSTS sprintu. 


##  3. <a name='AddFeature-3'></a>Přidání funkce  

Po vytvoření projektu úložiště v rámci týmového projektu, přejděte do týmu **přehled** a klikněte na tlačítko **spravovat pracovní**.

![2](./media/project-execution/project-execution-2-sprint-team-overview.png)

Chcete-li zahrnout funkce nevyřízené položky, klikněte na tlačítko **nevyřízené položky** --> **funkce** --> **nový**, typ ve funkci **název**(obvykle název projektu) a potom klikněte na **přidat** .

![3](./media/project-execution/project-execution-3-sprint-team-add-work.png)

Dvakrát klikněte na funkce, kterou jste vytvořili. Zadejte popis, přiřadit členové týmu pro tuto funkci a nastavení plánování parametry pro tuto funkci. 

Tuto funkci můžete také propojit úložiště projektu. Klikněte na tlačítko **odkaz Přidat** pod **vývoj** části. Po dokončení úprav funkci, klikněte na tlačítko **uložit a zavřít** ukončíte.


##  4. <a name='AddStoryunderfeature-4'></a>Přidat scénáře s funkcí 

V části funkce lze přidat scénářů k popisu důležitými kroky potřebnými k dokončení projektu (funkce). Chcete-li přidat nový článek, klikněte na tlačítko  **+**  přihlásit nalevo od funkci v zobrazení nevyřízených položek.  

![4](./media/project-execution/project-execution-4-sprint-add-story.png)

Můžete upravit podrobnosti o scénáře, jako je stav, popis, komentáře, plánování a priority v místním okně.

![5](./media/project-execution/project-execution-5-sprint-edit-story.png)

Tento scénář můžete propojit existující úložiště kliknutím **+ přidat odkaz** pod **vývoj**. 

![6](./media/project-execution/project-execution-6-sprint-link-existing-branch.png)


##  5. <a name='AddTaskunderstory-5'></a>Přidání úkolu do scénáře 

Úlohy jsou konkrétní podrobné kroky, které jsou nutné k dokončení každé scénáře. Po dokončení všech úloh scénáře by se měly článek příliš dokončit. 

Chcete-li přidat úlohu do článku, klikněte na tlačítko  **+**  přihlašovací vedle položky scénáře, vyberte **úloh**a pak zadejte podrobné informace o této úlohy v místním okně.

![7](./media/project-execution/project-execution-7-sprint-add-task.png)

Po vytvoření funkce, scénáře a úlohy, můžete je zobrazit **nevyřízených položek** nebo **Tabule** zobrazení sledovat jejich stav.

![8](./media/project-execution/project-execution-8-sprint-backlog-view.png)

![9](./media/project-execution/project-execution-9-link-to-a-new-branch.png)


##  6. <a name='Linkaworkitemwithagitbranch-6'></a>Propojit pracovní položky s větev git 

Služby VSTS poskytuje pohodlný způsob, jak připojit pracovní položky (článek nebo úloh) git větev. To vám umožní propojit článek nebo úloha přímo na kód s ním spojená. 

Pro připojení pracovní položku k nové větve, dvakrát klikněte na pracovní položky a v místním okně, klikněte na tlačítko **vytvoření nové větve** pod **+ přidat odkaz**.  

![10](./media/project-execution/project-execution-10-sprint-board-view.png)

Zadejte informace pro toto nové větve, jako je například názvu větve úložiště git základní a větev. Zvolené úložiště git musí být v rámci stejné týmový projekt, který pracovní položka patří do úložiště. Základní větev může být hlavní větve nebo některé stávající firemní pobočky.

![11](./media/project-execution/project-execution-11-create-a-branch.png)

Dobrým postupem je vytvoření větve git pro každý scénář pracovní položku. Potom pro každou položku pracovních úloh, vytvoříte větev podle scénáře firemní pobočky. Uspořádání větve tento hierarchický takovým způsobem, který odpovídá vztahy scénáře úloh je užitečné, pokud máte více lidí pracujících na různých scénářů stejného projektu, nebo máte několik lidí pracujících na různých úloh pro stejný článek. Konflikty můžete minimalizovat, když každý člen týmu funguje na různých větve a každý člen funguje na různých kódů nebo jiných artefakty při sdílení větev. 

Následující obrázek znázorňuje doporučené větvení strategie pro TDSP. Nemusí potřebovat mnoho větví jako zde se zobrazují, zejména pokud máte pouze jednu nebo dvě lidí pracujících na stejném projektu, nebo na všechny úlohy scénáře funguje jenom jedna osoba. Ale vždy oddělení vývoj větev z hlavní větve je vhodné. To může pomoct zabránit přerušení aktivitami vývoj verze větev. Podrobnější popis modelu větve git naleznete v [A úspěšné Git vytvoření větve modelu](http://nvie.com/posts/a-successful-git-branching-model/).

![12](./media/project-execution/project-execution-12-git-branches.png)

Chcete-li přepnout do větve, kterou chcete pracovat na, spusťte následující příkaz v prostředí příkazu (Windows nebo Linux). 

    git checkout <branch name>

Změna *< názvu větve\>*  k **hlavní** přepínače zpět do **hlavní** firemní pobočky. Po přepnutí na pracovní větev, můžete spustit funguje na pracovní položky, vývoj kódu nebo v dokumentaci artefakty potřebné k dokončení položky. 

Můžete také propojit pracovní položky do existující větve. V **podrobností** stránku pracovní položky, místo kliknutí na **vytvoření nové větve**, kliknutí na tlačítko **+ přidat odkaz**. Pak vyberte větve, který chcete propojit pracovních položek. 

![13](./media/project-execution/project-execution-13-link-to-an-existing-branch.png)

Můžete také vytvořit nové větve v úložišti git bash příkazy. Pokud < název základní pobočky\> chybí, < nový název větve\> je založena na _hlavní_ firemní pobočky. 
    
    git checkout -b <new branch name> <base branch name>


##  7. <a name='WorkonaBranchandCommittheChanges-7'></a>Práce na větev a provést změny 

Nyní předpokládejme, že provedete některé změny *data\_přijímání* větve pro pracovní položky, jako je například přidávání soubor R na větev v místním počítači. Soubory R přidané do větve pro tuto pracovní položku můžete potvrdit, zadaný jste uvedené pobočky v prostředí vaší Git, pomocí následujících příkazů Git:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![14](./media/project-execution/project-execution-14-sprint-push-to-branch.png)

##  8. <a name='CreateapullrequestonVSTS-8'></a>Vytvořit žádost o přijetí změn na služby VSTS 

Až budete připravení po několik potvrzení a oznámení, sloučit aktuální větev do jeho základní větve, můžete odeslat **žádosti o přijetí změn** na serveru služby VSTS. 

Přejděte na hlavní stránce týmového projektu a klikněte na tlačítko **kód**. Vyberte větev slučované a název úložiště git, který chcete sloučit větev do. Pak klikněte na tlačítko **žádosti o přijetí změn**, klikněte na tlačítko **nové žádosti o přijetí změn** vytvořit kontrolu žádost o přijetí změn, před prací na větev sloučena do jeho základní větve.

![15](./media/project-execution/project-execution-15-spring-create-pull-request.png)

Vyplňte některé popis o tuto žádost o přijetí změn, přidání revidujících a poslat ho.

![16](./media/project-execution/project-execution-16-spring-send-pull-request.png)

##  9. <a name='ReviewandMerge-9'></a>Přečtěte si a sloučení 

Při vytváření žádosti o přijetí změn recenzenti získat e-mail s oznámením ke kontrole žádosti o přijetí změn. Kontroloři muset zkontrolujte, jestli změny pracují nebo Ne a pokud je to možné otestovat změn pomocí žadatel. Na základě jejich posouzení, můžete kontroloři schválit nebo odmítnout žádosti o přijetí změn. 

![17](./media/project-execution/project-execution-17-add_comments.png)

![18](./media/project-execution/project-execution-18-spring-approve-pullrequest.png)

Po dokončení kontrola je pracovní větev sloučena do jeho základní větve kliknutím **Complete** tlačítko. Můžete se rozhodnout odstranit pracovní větev po se sloučil. 

![19](./media/project-execution/project-execution-19-spring-complete-pullrequest.png)

Potvrďte v levém horním rohu, který žádost je označen jako **DOKONČENO**. 

![20](./media/project-execution/project-execution-20-spring-merge-pullrequest.png)

Při návratu zpět do úložiště v rámci **kód**, výzva, aby vám byla přepnuta do hlavní větve.

![21](./media/project-execution/project-execution-21-spring-branch-deleted.png)

Můžete taky následující příkazy Gitu sloučení pracovní větev do jeho základní větve a po slučování odstraňte pracovní větev:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![22](./media/project-execution/project-execution-22-spring-branch-deleted-commandline.png)


##  10. <a name='DataQualityReportUtility-10'></a>Interaktivní zkoumání dat, analýzu a vytváření sestav nástroje (IDEAR)

Tento nástroj na základě markdownu R poskytuje flexibilní a interaktivní nástroj k vyhodnocení a prozkoumejte datových sad. Uživatele můžete rychle vytvořit sestavy z datové sady s minimální kódování. Uživatelé mohou klepnutím na tlačítko Exportovat výsledky zkoumání v nástroji pro interaktivní do konečné zprávu, která lze doručit do klientů nebo použít rozhodnout o které proměnné, které mají být zahrnuty v kroku následné modelování.

V tomto okamžiku nástroj funguje pouze v datových rámců v paměti. Soubor .yaml je potřeba zadat parametry sadu dat má být zkoumána. Další informace najdete v tématu [IDEAR v TDSP datové vědy nástroje](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


##  11. <a name='ModelingUtility-11'></a>Modelování směrného plánu a vytváření sestav nástroje

Tento nástroj poskytuje přizpůsobitelné, poloautomatických nástroj pro vytvoření modelu provádět pomocí technologie hyper parametr komínů na a porovnání přesnost těchto modelů. 

Nástroj pro vytvoření modelu je soubor markdownu R, který lze spustit a vytvoření výstupu html nezávislý s obsah easy navigace prostřednictvím jednotlivých částech. Tři algoritmy jsou provést při spuštění (knit) souboru markdownu: Vyřešeno regrese pomocí glmnet balíčku, náhodné doménové struktury pomocí balíčku randomForest a zvyšovat skóre stromy pomocí balíčku xgboost). Všechny tyto algoritmy vytvoří modulu trained model. Přesnost tyto modely se pak porovná a funkce relativní důležitost pozemků jsou hlášeny. V současné době existují dva nástroje: jeden je pro úlohu binární klasifikace a jeden pro úlohu regrese. Primární rozdíly mezi nimi je způsob řízení parametry a metriky přesnost jsou určené pro tyto úlohy učení. 

Soubor Yaml slouží k určení:

- vstupní data (zdroje SQL nebo R datového souboru) 
- jaká část dat se používá pro školení a jaká část pro testování
- které algoritmy ke spuštění 
- Výběr parametrů pro ovládací prvek pro optimalizaci modelu:
    - křížové ověření 
    - zavedení spouštěcího programu
    - složení křížové ověření
- technologie hyper parametr nastaví pro každý algoritmus. 

Počet algoritmů, počet složení pro optimalizaci, technologie hyper parametry a počet sad hyper parametrů oblouku přes můžete také upravit v souboru Yaml ke spuštění modely rychle. Například se může běžet s nižší počet složení odchylka nákladů, používá menší počet sady parametrů. Jejich lze také spustit více komplexněji s vyšší počet složení odchylka nákladů nebo větší počet sad parametrů, pokud, který je oprávněná.

Další informace najdete v tématu [automatizované modelování a vytváření sestav nástroje v TDSP datové vědy nástroje](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).


##  12. <a name='PowerBI-12'></a>Sledování průběhu projektů pomocí řídicích panelů Power BI

Data vědecké účely skupiny správců, týmů a potřeba zájemců projektu sledovat průběh jejich projektů, jaké pracovní bylo provedeno na ně a kým a zůstane v seznamu úkolů. Pokud používáte služby VSTS, budete moci vytvářet řídicí panely Power BI a sledovat aktivity a pracovní položky přidružené úložiště Git. Další informace o tom, jak připojit Power BI k Visual Studio Team Services najdete v tématu [propojte Power BI s Team Services](https://www.visualstudio.com/en-us/docs/report/powerbi/connect-vso-pbi-vs). 

Naučte se vytvářet řídicí panely Power BI a sestavy ke sledování aktivit úložiště Git a pracovní položky po data služby VSTS je připojená k Power BI, najdete v tématu [Power BI vytvořit řídicí panely a sestavy](https://www.visualstudio.com/en-us/docs/report/powerbi/report-on-vso-with-power-bi-vs). 

Tady jsou dvě jednoduchý příklad řídicí panely, které jsou vytvořené a sledování činností Git pracovní položky. Na řídicím panelu prvního příkladu aktivity závazků git jsou uvedené různými uživateli na různá data a na různých úložišť. Můžete snadno vyfiltrování a rozčlenění vyfiltrujete ty, které vás zajímají.

![23](./media/project-execution/project-execution-23-powerbi-git.png)

Na řídicím panelu druhý příklad uvádíme pracovních položek (scénáře a úlohy) v různých iterací. Jsou seskupené podle uživatele, jimž přiřazen a úroveň priority a barevné podle stavu.

![24](./media/project-execution/project-execution-24-powerbi-workitem.png)

 
## <a name="next-steps"></a>Další kroky

Úplné návody začátku do konce, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [příklad návody](walkthroughs.md) článku. Se ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace. 

Příklady provedením kroků v procesu Team dat. vědecké účely, které používají Azure Machine Learning Studio najdete v tématu [s Azure ML](http://aka.ms/datascienceprocess) kurzů.