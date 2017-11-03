---
title: "Postup sledování cloudové služby | Microsoft Docs"
description: "Naučte se monitorovat cloudové služby pomocí portálu Azure classic."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 5c48d2fb-b8ea-420f-80df-7aebe2b66b1b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2015
ms.author: adegeo
ms.openlocfilehash: c369b22cf068a473343b006eb1b06fdd350d31db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-monitor-cloud-services"></a>Jak monitorovat Cloud Services
[!INCLUDE [disclaimer](../../includes/disclaimer.md)]

Můžete sledovat `key` metrika výkonu pro vaše cloudové služby na portálu Azure classic. Můžete nastavit úroveň sledování pro minimální a podrobné pro každou roli služby a můžete přizpůsobit monitorování zobrazí. Podrobné monitorování data se ukládají v účtu úložiště, které je přístupné mimo portál. 

Monitorování zobrazí na portálu Azure classic jsou vysoce konfigurovatelné. Můžete vybrat podle metrik, které chcete monitorovat v seznamu metriky na **monitorování** stránky a vy můžete zvolit, které metriky k vykreslení v metriky grafy na **monitorování** stránku a řídicí panel. 

## <a name="concepts"></a>Koncepty
Ve výchozím nastavení se minimální monitorování poskytuje pro novou cloudovou službu pomocí čítače výkonu získané z hostitelského operačního systému pro instance rolí (virtuální počítače). Minimální metriky jsou omezeny na procento využití procesoru, Data v, Data se, propustnost čtení disku a zápis propustnost disku. Podle konfigurace, podrobného sledování, může přijímat další metriky na základě dat o výkonu v rámci virtuálních počítačů (instance rolí). Podrobné metriky povolit blíže analyzovat problémy, ke kterým došlo během operací aplikace.

Ve výchozím nastavení se data čítače výkonu z instancí role vzorků a přenést z role instance v intervalech 3 minut. Pokud povolíte podrobné monitorování, hrubý výkon čítač agregovaná data pro každou instanci role a napříč instancemi role pro každou roli v intervalech 5 minut, 1 hodina a 12 hodin. Agregovaná data se vyprazdňují po 10 dní.

Po povolení podrobného monitorování agregovaná data monitorování je ukládat do tabulek ve vašem účtu úložiště. Pokud chcete povolit podrobné sledování pro roli, musíte nakonfigurovat připojovací řetězec diagnostiky odkazující na účet úložiště. Jiným účtům úložiště můžete použít pro různé role.

Povolení podrobného monitorování zvyšuje náklady na úložiště týkající se úložiště dat, přenos dat a úložiště transakce. Minimální monitorování nevyžaduje žádný účet úložiště. Data pro metriky, které jsou umístěné na minimální monitorování úrovni nejsou uložené v účtu úložiště, i v případě, že nastavíte úroveň monitorování na podrobné.

## <a name="how-to-configure-monitoring-for-cloud-services"></a>Postupy: Konfigurace monitorování pro cloudové služby
Následující postupy použijte ke konfiguraci podrobné nebo minimální monitorování na portálu Azure classic. 

### <a name="before-you-begin"></a>Než začnete
* Vytvoření *classic* účet úložiště pro ukládání dat monitorování. Jiným účtům úložiště můžete použít pro různé role. Další informace najdete v tématu [postup vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* Povolte Azure Diagnostics pro role cloudové služby. V tématu [konfigurace diagnostiky pro cloudové služby](cloud-services-dotnet-diagnostics.md).

Zajistěte, aby byl diagnostiky připojovací řetězec v konfiguraci Role. Nelze povolit, podrobného sledování, dokud povolíte Azure Diagnostics a zahrnout diagnostiky připojovací řetězec do konfigurace rolí.   

> [!NOTE]
> Projektech zacílených na Azure SDK 2.5 automaticky neobsahovala diagnostiky připojovací řetězec v šabloně projektů. Pro tyto projekty je nutné ručně přidat připojovací řetězec diagnostiky ke konfiguraci Role.
> 
> 

**Ručně přidat diagnostiky připojovací řetězec do konfigurace rolí**

1. Otevřete projekt cloudové služby v sadě Visual Studio
2. Dvakrát klikněte na **Role** otevřete roli návrháře a vyberte možnost **nastavení** karta
3. Vyhledejte nastavení s názvem **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Pokud toto nastavení není zadán, klikněte na **přidat nastavení** tlačítko přidejte ke konfiguraci a změnit typ pro nové nastavení, které **ConnectionString**
5. Nastavit hodnotu pro připojovací řetězec kliknutím na **...**  tlačítko. Tím se otevře dialogové okno, což vám umožní vybrat účet úložiště.
   
    ![Nastavení sady Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Chcete-li změnit úroveň monitorování verbose nebo minimální
1. V [portál Azure classic](https://manage.windowsazure.com/), otevřete **konfigurace** stránky pro nasazení cloudové služby.
2. V **úroveň**, klikněte na tlačítko **podrobné** nebo **minimální**. 
3. Klikněte na **Uložit**.

Po zapnutí podrobného monitorování, měli byste začít zobrazuje data sledování na portálu Azure classic v rámci hodiny.

Nezpracovaná data čítačů a agregovaná data monitorování jsou uložené v účtu úložiště v tabulkách kvalifikovaný identifikátor ID nasazení pro role. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Postupy: přijímat výstrahy metrik, které cloudové služby
Může přijímat výstrahy založené na cloudové služby monitorování metriky. Na **Management Services** stránky Azure classic portálu, můžete vytvořit pravidlo, které spustí výstrahu, když zvolíte metriku dosáhne hodnotu, která zadáte. Můžete také mít e-mailu odeslaného při aktivaci výstrahy. Další informace najdete v tématu [postupy: příjem oznámení výstrah a spravovat pravidla výstrah v Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Postupy: Přidání metriky do tabulky se metriky
1. V [portál Azure classic](http://manage.windowsazure.com/), otevřete **monitorování** stránky pro cloudovou službu.
   
    Ve výchozím nastavení zobrazí v tabulce metriky podmnožinu dostupné metriky. Na obrázku výchozí podrobné metriky pro cloudovou službu, která je omezená na čítač výkonu paměť\počet MB k dispozici s daty agregován na úrovni role. Použití **přidat metriky** vybrat další metriky agregační a úrovni role monitorovat na portálu Azure classic.
   
    ![Podrobné zobrazení](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
2. Přidání metriky do tabulky metriky:
   
   1. Klikněte na tlačítko **přidat metriky** otevřete **zvolte metriky**, vidíte níže.
      
       První dostupná metrika je rozbalit a zobrazit možnosti, které jsou k dispozici. Pro jednotlivé metriky možnost top zobrazuje agregovaná data monitorování u všech rolí. Kromě toho můžete zobrazit data pro jednotlivé role.
      
       ![Přidat metriky](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)
   2. Chcete-li vybrat metriky pro zobrazení
      
      * Klikněte na šipku dolů podle metriky rozšířit možnosti monitorování.
      * Zaškrtněte políčko pro jednotlivé možnosti monitorování, které chcete zobrazit.
        
        Až 50 metriky můžete zobrazit v tabulce metriky.
        
        > [!TIP]
        > Podrobné monitorování, může obsahovat seznam metriky desítek metriky. Zobrazíte posuvníku přejděte myší pravé straně dialogového okna. Chcete-li filtrovat seznam, klikněte na ikonu hledání a zadejte text do vyhledávacího pole, jak je uvedeno níže.
        > 
        > 
        
        ![Přidat metriky vyhledávání](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)
3. Po dokončení výběru metriky, klikněte na tlačítko OK (zaškrtnutí).
   
    Vybrané metriky se přidají do tabulky metriky, jak je uvedeno níže.
   
    ![monitorování metriky](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)
4. Pokud chcete odstranit metriky z tabulky metriky, klikněte na tlačítko metrika vyberte ho a potom klikněte na **odstranit metrika**. (Jenom zobrazí **odstranit metrika** Pokud máte vybrané metriky.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Chcete-li přidat vlastní metriky do tabulky metriky
**Podrobné** monitorování úroveň obsahuje seznam výchozích metrik, které můžete monitorovat na portálu. Kromě těchto můžete sledovat všechny vlastní metriky nebo čítače výkonu, které jsou definované aplikace prostřednictvím portálu.

Následující postup předpokládá, že jste zapnuli **podrobné** monitorování úroveň a nakonfigurovali vaše aplikace při úklidu a přenos vlastní čítače výkonu. 

Chcete-li zobrazit čítače výkonu vlastní na portálu budete muset aktualizovat konfiguraci v kontejneru ovládacího prvku wad:

1. Otevřete wad. řízení kontejneru objektů blob v účtu úložiště diagnostiky. K tomu můžete použít Visual Studio nebo jiné Průzkumníka úložiště.
   
    ![Průzkumníka serveru Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. Přejděte cesta blobu pomocí vzoru **DeploymentId/RoleName/RoleInstance** najít konfiguraci pro instanci role. 
   
    ![Storage Explorer sady Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Stáhněte si konfigurační soubor pro instanci role a aktualizovat tak, aby obsahovala všechny vlastní čítače výkonu. Například k monitorování *zápis disku bajtů/s* pro *jednotka C* přidejte následující pod **PerformanceCounters\Subscriptions** uzlu
   
    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Změny uložte a nahrajte konfiguračního souboru zpět do stejného umístění přepsal existující soubor do objektu BLOB.
5. Přepněte do režimu podrobné v konfiguraci portálu Azure classic. Pokud jste již v režimu s komentářem bude mít k přepnutí na minimální a zpět na podrobné.
6. Čítač výkonu vlastní bude nyní k dispozici v **přidat metriky** dialogové okno. 

## <a name="how-to-customize-the-metrics-chart"></a>Postupy: přizpůsobení metriky grafu
1. V tabulce metriky vyberte až 6 metriky k vykreslení v grafu metriky. Pokud chcete vybrat metriky, klikněte na zaškrtávací políčko na jeho levé straně. K odebrání metriky grafu metriky, zrušte jeho zaškrtnutí políčka v tabulce metriky.
   
    Při výběru metriky v tabulce metriky, podle metrik, které jsou přidány do grafu metriky. Na úzké zobrazení **n Další** rozevírací seznam obsahuje metriky hlavičky, které nebudou vyhovovat zobrazení.
2. Chcete-li přepnout mezi zobrazením relativní hodnoty (konečná hodnota jenom pro jednotlivé metriky) a absolutní hodnoty (osy Y zobrazit), vyberte relativní nebo absolutní v horní části grafu.
   
    ![Relativní nebo absolutní](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)
3. Chcete-li změnit časový rozsah grafu zobrazí metriky, vyberte 1 hod., 24 hodin nebo 7 dní v horní části grafu.
   
    ![Období zobrazení monitorování](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)
   
    V grafu metriky řídicí panel metoda pro vykreslení metriky se liší. Je k dispozici standardní sadu metriky a metriky přidávání nebo odebírání výběrem hlavičku metriky.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Chcete-li přizpůsobit graf metriky na řídicím panelu
1. Otevřete řídicí panel pro cloudovou službu.
2. Přidat nebo odebrat metriky z grafu:
   
   * K vykreslení nové metriky, zaškrtněte políčko pro metriku v hlavičkách grafu. Na úzké zobrazení, klikněte na šipku dolů ve  ***n* ?? metriky** k vykreslení metriky záhlaví oblast grafu nelze zobrazit.
   * Chcete-li odstranit metrika, který se vykreslí v grafu, zrušte zaškrtnutí políčka podle jeho záhlaví.
   
3. Přepínání mezi **relativní** a **absolutní** zobrazí.
4. Vyberte 1 hod., 24 hodin nebo 7 dní od data k zobrazení.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Postupy: přístup podrobné monitorování data mimo portál Azure classic
Podrobné sledování dat je ukládat do tabulek v účtech úložiště, které zadáte pro každou roli. Pro každé nasazení cloudové služby vytvoří se pro roli šesti tabulky. Dvě tabulky se vytváří pro každý (5 minut, 1 hodina a 12 hodin). Některé z těchto tabulek ukládá agregace na úrovni role; v další tabulce ukládá agregací pro instance rolí. 

Názvy tabulek mít následující formát:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Kde:

* *deploymentID* je identifikátor GUID přiřazený nasazení cloudové služby
* *aggregation_interval* = 5 M, 1 H nebo 12 H
* agregace na úrovni role = R
* agregace pro instance rolí = RI

Následující tabulky by například uložit podrobné data monitorování agregován v intervalu 1 hodin:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
