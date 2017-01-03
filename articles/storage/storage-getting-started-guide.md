---
title: "Zprovoznění Azure Storage v pěti minutách | Dokumentace Microsoftu"
description: "Ukážeme si, jak rychle začít používat Microsoft Azure Blobs, Table a Queues pomocí ukázkových projektů Rychlý start Azure Storage, sady Visual Studio a emulátoru úložiště Azure. Zprovozněte si svoji první aplikaci Azure Storage za pět minut."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 582f76f8-c814-4a69-8a5c-1fd0e0d5d8f2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 12/08/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 12ce6b6bccf3ea2aa2945ddd775716f29cf01e1f
ms.openlocfilehash: 47b2623eb3b83220ef8e3cfafde06dab3ac3d22e


---
# <a name="get-started-with-azure-storage-in-five-minutes"></a>Zprovoznění aplikace Azure Storage v pěti minutách
## <a name="overview"></a>Přehled
Pomocí služby Azure Storage můžete snadno a rychle začít vyvíjet aplikace. V tomto kurzu si ukážeme, jak rychle zprovoznit a spustit aplikaci Azure Storage. Použijete šablony Rychlý start, které jsou součástí sady Azure SDK for .NET. Tyto ukázkové projekty obsahují připravený kód, na kterém si ukážeme některé základní scénáře programování s Azure Storage.

Než se na tento kód podíváme, můžete si v části [Další kroky](#next-steps) přečíst další informace o Azure Storage.

## <a name="prerequisites"></a>Požadavky
Než začnete, bude nutné splnit následující požadavky:

1. Pro zkompilování a vybuildování aplikace budete muset mít na počítači nainstalovanou verzi sady [Visual Studio](https://www.visualstudio.com/).
2. Nainstalujte si nejnovější verzi sady [Azure SDK for .NET](https://azure.microsoft.com/downloads/). Tato sada SDK obsahuje ukázkové projekty Azure QuickStart, emulátor úložiště Azure a [Klientskou knihovnu pro úložiště Azure pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).
3. Na počítači musíte mít nainstalované rozhraní [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653). To je nutné pro ukázkové projekty Azure QuickStart, které budeme používat v tomto kurzu.

    Pokud si nejste jistí, kterou verzi rozhraní .NET Framework máte na počítači nainstalovanou, projděte si téma [Postupy: Zjištění nainstalovaných verzí rozhraní .NET Framework](https://msdn.microsoft.com/vstudio/hh925568.aspx). Také můžete stisknout tlačítko **Start** nebo klávesu Windows a napsat **Ovládací panely**. Pak klikněte na **Programy** > **Programy a funkce** a zjistěte, jestli je rozhraní .NET Framework 4.5 v seznamu nainstalovaných programů.
4. Budete potřebovat předplatné Azure a účet úložiště Azure.

   * Pokud si budete chtít pořídit předplatné Azure, podívejte se na témata věnovaná [získání bezplatné zkušební verze](https://azure.microsoft.com/pricing/free-trial/), [možnostem zakoupení](https://azure.microsoft.com/pricing/purchase-options/) a [nabídkám pro členy](https://azure.microsoft.com/pricing/member-offers/) (pro členy programů MSDN, Microsoft Partner Network, BizSpark a dalších programů společnosti Microsoft).
   * Pokud si budete chtít vytvořit účet úložiště v Azure, najdete postup [tady](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Spuštění vaší první aplikace Azure Storage ve službě Azure Storage v cloudu
Jakmile budete mít účet úložiště Azure, můžete si pomocí jednoho z ukázkových projektů Azure Rychlý start v sadě Visual Studio vytvořit jednoduchou aplikaci Azure Storage. Tento kurz se zaměřuje na ukázkové projekty pro Azure Storage: **Azure Storage: Blobs**, **Azure Storage: Files**, **Azure Storage: Queues** a **Azure Storage: Tables**:

1. Spusťte Visual Studio.
2. V nabídce **Soubor** klikněte na **Nový projekt**.
3. V dialogovém okně **Nový projekt** klikněte na **Nainstalováno** > **Šablony** > **Visual C#** > **Cloud** > **QuickStarts** > **Data Services**.
    a. Zvolte jednu z následujících šablon: **Azure Storage: Blobs**, **Azure Storage: Files**, **Azure Storage: Queues** nebo **Azure Storage: Tables**.
    b. Zkontrolujte, že je jako cílové rozhraní vybrané rozhraní **.NET Framework 4.5**.
    c. Zadejte název projektu a vytvořte nové řešení sady Visual Studio, jak je znázorněno na obrázku:

    ![Ukázkové projekty Azure Rychlý start][Image1]

Před spuštěním aplikace si můžete projít zdrojový kód. Pokud si ho budete chtít projít, vyberte v sadě Visual Studio v nabídce **Zobrazení** možnost **Průzkumník řešení**. Potom dvakrát klikněte na soubor Program.cs.

Dále pak stáhněte ukázkovou aplikaci:

1. V sadě Visual Studio v nabídce **Zobrazení** vyberte **Průzkumník řešení**. Otevřete soubor App.config a odkomentujte připojovací řetězec pro emulátor úložiště Azure:

   `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2. Odkomentujte připojovací řetězec pro službu Azure Storage a zadejte název účtu úložiště a přístupový klíč v souboru App.config:

   `<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

   Informace o tom, jak přístupový klíč účtu úložiště načíst, najdete v části věnované [správě přístupových klíčů úložiště](storage-create-storage-account.md#manage-your-storage-access-keys).
3. Po zadání názvu účtu úložiště a přístupového klíče v souboru App.config klikněte v nabídce **Soubor** na tlačítko **Uložit vše** a uložte všechny soubory projektu.
4. V nabídce **Sestavení** klikněte na **Sestavit řešení**.
5. V nabídce **Ladění** stiskněte klávesu **F11**. Tím spustíte řešení v krokovacím režimu. Stisknutím klávesy **F5** také můžete řešení spustit tak, aby proběhlo od začátku až do konce.

## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Místní spuštění vaší první aplikace Azure Storage v emulátoru úložiště Azure
[Emulátor úložiště Azure](storage-use-emulator.md) zajistí místní prostředí, které emuluje služby Azure Blob, Queue a Table pro účely vývoje. Emulátor úložiště můžete použít k místnímu otestování aplikace úložiště bez vytváření předplatného Azure nebo účtu úložiště a bez jakýchkoli nákladů.

Vyzkoušejte si to: vytvoříme jednoduchou aplikaci Azure Storage pomocí jednoho z ukázkových projektů Azure Rychlý start v sadě Visual Studio. Tento kurz se zaměřuje na ukázkové projekty **Azure Blob Storage**, **Azure Table Storage** a **Azure Queue Storage**:

1. Spusťte Visual Studio.
2. V nabídce **Soubor** klikněte na **Nový projekt**.
3. V dialogovém okně **Nový projekt** klikněte na **Nainstalováno** > **Šablony** > **Visual C#** > **Cloud** > **QuickStarts** > **Data Services**.
    a. Zvolte jednu z následujících šablon: **Azure Storage: Blobs**, **Azure Storage: Files**, **Azure Storage: Queues** nebo **Azure Storage: Tables**.
    b. Zkontrolujte, že je jako cílové rozhraní vybrané rozhraní **.NET Framework 4.5**.
    c. Zadejte název projektu a vytvořte nové řešení sady Visual Studio, jak je znázorněno na obrázku:

    ![Ukázkové projekty Azure Rychlý start][Image1]

4. V sadě Visual Studio v nabídce **Zobrazení** vyberte **Průzkumník řešení**. Otevřete soubor App.config a odkomentujte připojovací řetězec pro svůj účet úložiště Azure, pokud jste ho už přidali. Potom odkomentujte připojovací řetězec pro emulátor úložiště Azure:

   `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Před spuštěním aplikace si můžete projít zdrojový kód. Pokud si ho budete chtít projít, vyberte v sadě Visual Studio v nabídce **Zobrazení** možnost **Průzkumník řešení**. Potom dvakrát klikněte na soubor Program.cs.

Dále pak spusťte ukázkovou aplikaci v emulátoru úložiště Azure:

1. Stiskněte tlačítko **Start** nebo klávesu Windows, vyhledejte *Emulátor úložiště Microsoft Azure* a spusťte aplikaci. Po spuštění emulátoru uvidíte ikonu a oznámení v oblasti zobrazení úkolů systému Windows.
2. V sadě Visual Studio klikněte na v nabídce **Sestavení** na **Sestavit řešení**.
3. Stisknutím klávesy **F11** v nabídce **Ladění** spustíte řešení v krokovacím režimu. Stisknutím klávesy **F5** spustíte řešení od začátku do konce.

## <a name="next-steps"></a>Další kroky
Další informace o službě Azure Storage najdete v těchto zdrojích informací:

* [Úvod do Microsoft Azure Storage](storage-introduction.md)
* [Začínáme s Azure Storage Explorerem](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Začínáme s úložištěm Azure Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md)
* [Začínáme s úložištěm Azure Table pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md)
* [Začínáme s úložištěm Azure Queue pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md)
* [Začínáme s úložištěm Azure File ve Windows](storage-dotnet-how-to-use-files.md)
* [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Klientská knihovna pro Microsoft Azure Storage pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby Azure Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png



<!--HONumber=Dec16_HO2-->


