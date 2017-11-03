---
title: "Nástroje Azure Data Lake: Nástroje použití Azure Data Lake pro Visual Studio Code | Microsoft Docs"
description: "Další informace o použití nástroje Azure Data Lake pro Visual Studio Code pro vytvoření, testování a spouštění skriptů U-SQL. "
Keywords: "VScode, nástroje Azure Data Lake, místní spuštění souboru úložiště místní ladění, místní ladění preview, odešlete do cestu k úložišti, nahrávání a stahování"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/10/2017
ms.author: jejiang
ms.openlocfilehash: e724a8db4424a1e608ae7ee5625cd4cc16f6078f
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Pomocí nástroje Azure Data Lake pro Visual Studio Code

Další nástroje Azure Data Lake pro Visual Studio Code (kód VS) k vytváření, testování a spouštět skripty U-SQL. Informace jsou také obsaženy v následujícím videu:

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Požadavky

Nástroje Azure Data Lake pro VSCode podporuje Windows, Linux a systému MacOS.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).

Pro systému MacOS a Linux:
- [.NET core SDK 2.0](https://www.microsoft.com/net/download/core). 
- [Monofonní 5.2.x](http://www.mono-project.com/download/).

## <a name="install-data-lake-tools"></a>Nainstalovat nástroje Data Lake

Po instalaci požadavky, můžete nainstalovat nástroje Data Lake pro VS Code.

**Chcete-li nainstalovat nástroje Data Lake**

1. Otevřete Visual Studio Code.
2. Klikněte na tlačítko **rozšíření** v levém podokně. Zadejte **Azure Data Lake** do vyhledávacího pole.
3. Klikněte na tlačítko **nainstalovat** vedle **nástroje Azure Data Lake**. Za několik sekund **nainstalovat** tlačítko se změní na **opětovného načtení**.
4. Klikněte na tlačítko **opětovného načtení** aktivovat **nástroje Azure Data Lake** rozšíření.
5. Klikněte na tlačítko **opětovného načtení okno** k potvrzení. Můžete zobrazit **nástroje Azure Data Lake** v podokně rozšíření.

    ![Nástroje data Lake pro Visual Studio rozšíření kódu podokno](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

 
## <a name="activate-azure-data-lake-tools"></a>Aktivovat nástrojů Azure Data Lake
Vytvořte nový soubor .usql nebo otevře existující soubor .usql aktivovat rozšíření. 

## <a name="open-the-sample-script"></a>Otevřete ukázkový skript
Spusťte příkaz palety (Ctrl + Shift + P) a zadejte **ADL: Otevřete ukázkový skript**. Otevře další instanci této ukázce. Můžete také upravit, konfigurovat a odeslat skript v této instanci.

## <a name="work-with-u-sql"></a>Práce s jazykem U-SQL

Je nutné otevřít soubor U-SQL nebo složku pro práci s jazykem U-SQL.

**Otevření složky pro váš projekt U-SQL**

1. Visual Studio Code, vyberte **soubor** nabídce a potom vyberte **otevřít složku**.
2. Zadejte složku a potom vyberte **vyberte složku**.
3. Vyberte **soubor** nabídce a potom vyberte **nový**. Soubor bez názvu-1 se přidá do projektu.
4. Zadejte následující kód do souboru bez názvu-1:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    Tento skript vytvoří soubor departments.csv se některé dat obsažených ve složce/Output.

5. Uložte soubor jako **myUSQL.usql** ve složce otevřen. Konfigurační soubor adltools_settings.json je taky přidaná do projektu.
4. Otevřít a konfigurovat adltools_settings.json s následujícími vlastnostmi:

    - Účet: Účet Data Lake Analytics v rámci vašeho předplatného Azure.
    - Databáze: Databáze v rámci vašeho účtu. Výchozí hodnota je **hlavní**.
    - Schémat: Schéma v databázi. Výchozí hodnota je **dbo**.
    - Volitelné nastavení:
        - Priorita: Rozsah priority je od 1 do 1000 s 1 jako nejvyšší prioritou. Výchozí hodnota je **1000**.
        - Paralelismus: Paralelismus rozsah je od 1 do 150. Výchozí hodnota je maximální paralelismus v účtu Azure Data Lake Analytics povolen. 
        
        > [!NOTE] 
        > Pokud jsou neplatné nastavení, budou použity výchozí hodnoty.

        ![Nástroje data Lake pro Visual Studio Code konfiguračního souboru](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

        Výpočetní účet Data Lake Analytics je potřeba k zkompilování a spuštění úloh U-SQL. Předtím, než můžete zkompilování a spuštění úloh U-SQL je nutné nakonfigurovat účet počítače.
    
        Po uložení konfigurace se zobrazí na stavovém řádku v levém horním rohu odpovídající soubor .usql informace o účtu a schématu databáze. 
 
 
Porovnání s otevřením souboru, když otevřete složku, kterou můžete:

- Použijte soubor kódu. V režimu jednoho souboru kódu na pozadí nepodporuje.
- Pomocí konfiguračního souboru. Když otevřete složku, skripty ve složce pracovní sdílet jednom konfiguračním souboru.


Skript U-SQL zkompiluje vzdáleně pomocí služby Data Lake Analytics. Při vydání **zkompilovat** příkaz, skript U-SQL je odeslána do účtu Data Lake Analytics. Visual Studio Code později, obdrží výsledek kompilace. Z důvodu vzdálené kompilace Visual Studio Code vyžaduje, že uvedete informace pro připojení k vašemu účtu Data Lake Analytics v konfiguračním souboru.

**Kompilace skript U-SQL**

1. Vyberte Ctrl + Shift + P otevřete paletu příkaz. 
2. Zadejte **ADL: kompilace skriptu**. Výsledky kompilace se zobrazí v **výstup** okno. Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: kompilace skriptu** zkompilovat úlohy U-SQL. Výsledek kompilace se zobrazí v **výstup** podokně.
 

**Odeslat skript U-SQL**

1. Vyberte Ctrl + Shift + P otevřete paletu příkaz. 
2. Zadejte **ADL: odeslat úlohu**.  Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: odeslat úlohu**. 

Po odeslání úlohy U-SQL, odeslání protokolů se objeví v **výstup** okno v produktu VS Code. Pokud je odesílání úspěšné, úlohy adresa URL se zobrazí také. Adresa URL úlohu můžete otevřít ve webovém prohlížeči sledovat stav úlohy v reálném čase.

Chcete-li povolit výstup podrobnosti úlohy, nastavte **jobInformationOutputPath** v **vs kód u-sql_settings.json** souboru.
 
## <a name="use-a-code-behind-file"></a>Použít soubor kódu

Soubor kódu je soubor C# přidružené jednoho skriptu U-SQL. Můžete definovat skript vyhrazené UDO, UDA, UDT a UDF v souboru kódu na pozadí. UDO, UDA, UDT a UDF lze přímo ve skriptu bez nejprve registrace sestavení. Soubor kódu je uvést ve stejné složce jako jeho partnerského vztahu soubor skriptu U-SQL. Pokud tento skript má název xxx.usql, název modelu code-behind jako xxx.usql.cs. Pokud odstraníte ručně souboru kódu na pozadí, je zakázaná kódu pro jeho přidružené skript U-SQL. Další informace o psaní kódu zákazníka pro skript U-SQL najdete v tématu [zápis a pomocí vlastní kód v U-SQL: funkce definované uživatelem]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

Pro podporu kódu, je nutné otevřít pracovní složky. 

**Aby se vygeneroval soubor kódu**

1. Otevřete zdrojový soubor. 
2. Vyberte Ctrl + Shift + P otevřete paletu příkaz.
3. Zadejte **ADL: vygenerování kódu na pozadí**. Souboru kódu na pozadí se vytvoří ve stejné složce. 

Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: generování kódu za**. 

Pro zkompilování a odeslat skript U-SQL pomocí souboru kódu je stejný jako s samostatný soubor skriptu U-SQL.

Následující dva snímky obrazovky ukazují souboru kódu na pozadí a jeho přidružený soubor skriptu U-SQL:
 
![Nástroje data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![Soubor skriptu nástrojů data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

Podporujeme místní spuštění a místní ladění, podle pokynů v tématu [U-SQL místní spuštění a místní ladění s Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="use-assemblies"></a>Použití sestavení

Informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).

Nástroje Data Lake můžete pro registraci sestavení vlastní kód v katalogu Data Lake Analytics.

**Registrace sestavení**

Můžete zaregistrovat sestavení prostřednictvím **ADL: registrace sestavení** nebo **ADL: registrace sestavení prostřednictvím konfigurace** příkazy.

**Registrace přes ADL: příkaz registrace sestavení**
1.  Vyberte Ctrl + Shift + P otevřete paletu příkaz.
2.  Zadejte **ADL: registrace sestavení**. 
3.  Zadejte cestu k místní sestavení. 
4.  Vyberte účet Data Lake Analytics.
5.  Vyberte databázi.

Výsledky: Portál se otevře v prohlížeči a zobrazí proces registrace sestavení.  

Jiné pohodlný způsob, jak aktivovat **ADL: registrace sestavení** příkaz je kliknete pravým tlačítkem na soubor .dll v Průzkumníku souborů. 

**K registraci, když ADL: registrace sestavení pomocí příkazu konfigurace**
1.  Vyberte Ctrl + Shift + P otevřete paletu příkaz.
2.  Zadejte **ADL: registrace sestavení prostřednictvím konfigurace**. 
3.  Zadejte cestu k místní sestavení. 
4.  Zobrazí se souboru JSON. Zkontrolujte a v případě potřeby upravit závislosti sestavení a parametry prostředků. Pokyny, které se zobrazují v **výstup** okno. Chcete-li přejít k registraci sestavení, uložení (Ctrl + S) souboru JSON.

![Nástroje data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Závislosti sestavení: automatické nástrojů Azure Data Lake jestli knihovnu DLL nemá žádné závislosti. Po zjištění, zobrazí se závislostí v souboru JSON. 
>- Prostředky: Jako součást registrace k sestavení můžete nahrát vašich prostředků knihovny DLL (například TXT, .png a CSV). 

Jiný způsob, jak aktivovat **ADL: registrace sestavení prostřednictvím konfigurace** příkaz je kliknete pravým tlačítkem na soubor .dll v Průzkumníku souborů. 

Následující kód U-SQL ukazuje, jak volat sestavení. V ukázce název sestavení je *testování*.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    TO @"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```

## <a name="connect-to-azure"></a>Připojení k Azure

Předtím, než můžete zkompilování a spuštění skriptů U-SQL v Data Lake Analytics, je nutné připojit ke svému účtu Azure.

**Pro připojení k Azure**

1.  Vyberte Ctrl + Shift + P otevřete paletu příkaz. 
2.  Zadejte **ADL: přihlášení**. Přihlašovací údaje se zobrazí v **výstup** podokně.

    ![Nástroje data Lake pro Visual Studio Code příkaz palety](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![nástroje Data Lake pro Visual Studio Code zařízení přihlašovací informace](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Vyberte Ctrl + klikněte na adresu URL pro přihlášení: https://aka.ms/devicelogin otevřete webovou stránku přihlášení. Zadejte kód **G567LX42V** do textového pole a pak vyberte **pokračovat**.

   ![Nástroje data Lake pro Visual Studio Code přihlášení vložte kód](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Postupujte podle pokynů se přihlásit z webové stránky. Když jste připojení, název účtu Azure se zobrazí na stavovém řádku v levém horním rohu **VS Code** okno. 

    > [!NOTE] 
    > Pokud váš účet má dvě úrovně povoleno, doporučujeme použít ověřování phone místo pomocí PIN kódu.

Chcete-li odhlásit, zadejte příkaz **ADL: odhlášení**.

## <a name="list-your-data-lake-analytics-accounts"></a>Seznam svých účtů Data Lake Analytics

Chcete-li otestovat připojení, získáte seznam svých účtů Data Lake Analytics.

**Seznam účtů Data Lake Analytics v rámci vašeho předplatného Azure**

1. Vyberte Ctrl + Shift + P otevřete paletu příkaz.
2. Zadejte **ADL: seznamu účtů**. Účty se zobrazují v **výstup** podokně.


## <a name="access-the-data-lake-analytics-catalog"></a>Přístup ke katalogu Data Lake Analytics

Po připojení k Azure, můžete použít následující kroky přístup ke katalogu U-SQL.

**Přístup k metadatům Azure Data Lake Analytics**

1.  Vyberte Ctrl + Shift + P a potom zadejte **ADL: seznamu tabulek**.
2.  Vyberte jeden z účtů Data Lake Analytics.
3.  Vyberte jednu z databáze Data Lake Analytics.
4.  Vyberte jeden z schémat. Zobrazí se seznam tabulek.

## <a name="view-data-lake-analytics-jobs"></a>Úlohy zobrazení Data Lake Analytics

**Chcete-li zobrazit úlohy Data Lake Analytics**
1.  Otevřete příkaz palety (Ctrl + Shift + P) a vyberte **ADL: Zobrazit úlohy**. 
2.  Vyberte Data Lake Analytics nebo místní účet. 
3.  Počkejte, než pro účet, který chcete zobrazit v seznamu úloh.
4.  Vyberte ze seznamu úloh úlohu, nástrojů Data Lake otevře podrobnosti úlohy na portálu Azure a zobrazí soubor informací o úloze v produktu VS Code.

    ![Typy objektů nástroje data Lake pro Visual Studio kódu IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Integrace úložiště Azure Data Lake

Můžete použít příkazy související s Azure Data Lake Storage na:
 - Procházejte prostředky služby Azure Data Lake Storage. [Seznam cesta k úložišti](#list-the-storage-path). 
 - Zobrazte náhled souboru Azure Data Lake Storage. [Náhled souboru úložiště](#preview-the-storage-file). 
 - Nahrajte soubor přímo do Azure Data Lake Storage v produktu VS Code. [Nahrát soubor](#upload-file).
 - Stáhněte soubor přímo z Azure Data Lake Storage v produktu VS Code. [Stáhněte si soubor](#download-file).

## <a name="list-the-storage-path"></a>Seznam cesta k úložišti 

**Do seznamu Cesta k úložišti prostřednictvím příkazu palety**

Klikněte pravým tlačítkem na editor skriptů a vyberte **ADL: cestu k úložišti seznamu**.

Vyberte složku, v seznamu, nebo klikněte na tlačítko **zadejte cestu** nebo **procházet z kořenového** (používá zadejte cestu jako příklad). -> Vyberte vaše **ADLA účet**. -> Přejděte nebo zadejte cestu ke složce úložiště (například: / výstup /). -> Seznamy palety příkaz založené na vámi zadaných informací o cestě.

![Nástroje data Lake pro Visual Studio Code seznam výsledků cesta úložiště](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Pohodlnější způsob, jak zobrazit seznam relativní cesta je prostřednictvím v místní nabídce klikněte pravým tlačítkem.

**Do seznamu Cesta k úložišti prostřednictvím klikněte pravým tlačítkem na**

Klikněte pravým tlačítkem na řetězec cesty k výběru **cestu k úložišti seznamu** pokračujte.

![Nástroje data Lake pro Visual Studio Code, klikněte pravým tlačítkem na kontextové nabídky](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


## <a name="preview-the-storage-file"></a>Náhled souboru úložiště

Klikněte pravým tlačítkem na editor skriptů a vyberte **ADL: soubor úložiště Preview**.

Vyberte vaše **ADLA účet**. -> Zadejte cestu souboru úložiště Azure (například /output/SearchLog.txt). -> Výsledek: soubor se otevře v VSCode.

   ![Nástroje data Lake pro Visual Studio Code náhled souboru výsledků](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Dalším způsobem soubor úložiště preview je prostřednictvím v místní nabídce na úplnou cestu souboru nebo relativní cestu souboru v editoru skriptů. 

## <a name="upload-file"></a>Nahrání souboru 

Můžete nahrát soubory tak, že zadáte příkazy **ADL: nahrát soubor** nebo **ADL: nahrát soubor prostřednictvím konfigurace**.

**Chcete nahrát soubory, když ADL: nahrát soubor pomocí příkazu konfigurace**
1.  Klikněte pravým tlačítkem na editor skriptů a potom vyberte **nahrát soubor prostřednictvím konfigurace**.
2.  VS Code zobrazí soubor JSON. Můžete zadat cesty k souborům a uložení více souborů ve stejnou dobu. Pokyny, které se zobrazují v **výstup** okno. Aby bylo možné pokračovat pro nahrání souboru, uložte (Ctrl + S) souboru JSON.

       ![Cesta k souboru nástroje data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  Výsledky: **výstup** okně se zobrazí stav nahrávání souboru.

       ![Nástroje data Lake pro Visual Studio Code nahrát stav](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

Jako současně, můžete sledovat [odesílání stav](#check-storage-tasks-status).

**Chcete nahrát soubory, když ADL: nahrát soubor – příkaz**

Klikněte pravým tlačítkem na editor skriptů a vyberte **nahrát soubor**.

Zadejte vaše **místní cesta**. -> Zvolte složku, v seznamu, nebo klikněte na tlačítko **zadejte cestu** nebo **procházet z kořenového** (používá zadejte cestu jako příklad). -> Vyberte vaše **ADLA účet**. -> Přejděte nebo zadejte cestu ke složce úložiště (například: / výstup /). -> klikněte na tlačítko **zvolte aktuální složce** k určení cílového umístění pro ukládání.

![Nástroje data Lake pro Visual Studio Code nahrát stav](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    


Dalším způsobem odesílání souborů do úložiště je prostřednictvím v místní nabídce na úplnou cestu souboru nebo relativní cestu souboru v editoru skriptů.

Jako současně, můžete sledovat [odesílání stav](#check-storage-tasks-status).

## <a name="download-file"></a>Stažení souboru 
Soubory si můžete stáhnout tak, že zadáte příkazy **ADL: stáhnout soubor úložiště** nebo **ADL: stáhnout soubor úložiště prostřednictvím konfigurace**.

**Chcete-li stáhnout soubory, když ADL: stáhnout soubor pomocí příkazu konfigurace**
1. Klikněte pravým tlačítkem na editor skriptů a potom vyberte **stáhnout soubor úložiště prostřednictvím konfigurace**.
2. VS Code zobrazí soubor JSON. Můžete zadat cesty k souborům a stahovat víc souborů současně. Pokyny, které se zobrazují v **výstup** okno. Chcete-li pokračovat ke stažení souboru, uložte (Ctrl + S) souboru JSON.

    ![Nástroje data Lake pro Visual Studio Code stáhnout soubory pomocí konfigurace](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

3.  Výsledky: **výstup** okně se zobrazí stav nahrávání souboru.

    ![Nástroje data Lake pro Visual Studio Code stáhnout více souborů výsledky](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Jako současně, můžete sledovat [stahování stav](#check-storage-tasks-status).

**Ke stažení souborů, když ADL: stáhnout soubor úložiště – příkaz**

Klikněte pravým tlačítkem na editor skriptů a potom vyberte **stáhnout soubor úložiště**.

Vyberte složku, v seznamu, nebo klikněte na tlačítko **zadejte cestu** nebo **procházet z kořenového** (používá zadejte cestu jako příklad). -> Vyberte vaše **ADLA účet**. -> Přejděte nebo zadejte cestu ke složce úložiště (například: / výstup /) -> vyberte soubor, který chcete stáhnout.

   ![Stav stahování nástroje data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

   Výsledek obrázku soubor je uložen jako dočasnou složku. Můžete nastavit výchozí cesta pro parametr stahování **usql.defaultLocalFolderForDownload** sami, prostřednictvím nabídky VSCode podle **soubor** -> **Předvolby**  ->  **Nastavení**.

Dalším způsobem stahování souborů úložiště je prostřednictvím v místní nabídce na úplnou cestu souboru nebo relativní cestu souboru v editoru skriptů.

Jako současně, můžete sledovat [stahování stav](#check-storage-tasks-status).

## <a name="check-storage-tasks-status"></a>Zkontrolujte stav úlohy úložiště
Stav se zobrazí v dolní části stavový řádek po dokončení stahování a odesílání.
1. Klikněte na stavovém řádku mocí následujících a pak stažení a odesílání stav zobrazíte v **výstup** panelu.

   ![Nástroje data Lake pro Visual Studio Code zkontrolujte úložiště stavu](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="open-azure-storage-explorer"></a>Otevřete Azure storage Exploreru
Můžete otevřít **Azure Storage Explorer** zadáním příkazu **ADL: Otevřete Web Azure Storage Explorer** nebo výběrem v místní nabídce klikněte pravým tlačítkem.

**Otevřete Průzkumníka úložiště Azure**

1. Vyberte Ctrl + Shift + P otevřete paletu příkaz.
2. Zadejte **otevřete Web Azure Storage Explorer** nebo klikněte pravým tlačítkem na relativní cestu nebo úplnou cestu v editoru skript a potom vyberte **otevřete Web Azure Storage Explorer**.
3. Vyberte účet Data Lake Analytics.

Nástroje data Lake otevře cesta k úložišti Azure na portálu Azure. Můžete najít cestu a náhled souboru z webu.

## <a name="local-run-and-local-debug-for-windows-users"></a>Místní spuštění a místní ladění pro systém Windows uživatele
U-SQL místní spuštění testů místní data a ověří váš skript místně, než kódu je publikována ve službě Data Lake Analytics. Místní ladění funkce umožňuje než kódu je odeslána do Data Lake Analytics dokončit následující úlohy: 
- Ladění vaší C# kódu. 
- Krokovat kód. 
- Ověřte váš skript místně.

Pokyny v místním spuštění a místní ladění najdete v tématu [U-SQL místní spuštění a místní ladění s Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>Další funkce

Nástroje data Lake pro VS Code podporuje následující funkce:

-   Automatické dokončování IntelliSense: návrhy zobrazí v automaticky otevíraná okna kolem položky, jako jsou klíčová slova, metod a proměnné. Různé ikony představují různé typy objektů:

    - Datový typ Scala
    - Komplexní datový typ
    - Předdefinované UDT
    - Třídy a .NET collection
    - Výrazy jazyka C#
    - Předdefinované C# UDF, UDO a UDAAGs 
    - Funkce U-SQL
    - U-SQL oddílová funkce
 
    ![Typy objektů nástroje data Lake pro Visual Studio kódu IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   IntelliSense automatického dokončování v Data Lake Analytics metadata: nástrojů Data Lake stáhne informace metadat Data Lake Analytics místně. Funkci IntelliSense automaticky naplní objektů, včetně databáze, schéma, tabulka, zobrazení, funkce vracející tabulku, postupy a sestavení C#, z metadat Data Lake Analytics.
 
    ![Nástroje data Lake pro Visual Studio kódu IntelliSense metadat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   Chyba značky IntelliSense: nástrojů Data Lake podtrhne úpravy chyby U-SQL a C#. 
-   Označuje syntaxe: nástrojů Data Lake pomocí různých barev odlišit od položky, jako jsou proměnné, klíčová slova, datový typ a funkce. 

    ![Nástroje data Lake pro Visual Studio Code syntaxe označuje](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Další kroky

- Místní spuštění U-SQL a místní ladění s kódem jazyka Visual Studio najdete v tématu [U-SQL místní spuštění a místní ladění s Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).
- Začínáme informace o Data Lake Analytics najdete v tématu [kurz: Začínáme s Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Informace o nástrojů Data Lake pro Visual Studio najdete v tématu [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).



