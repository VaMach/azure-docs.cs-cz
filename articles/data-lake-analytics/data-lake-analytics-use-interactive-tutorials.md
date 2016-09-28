<properties 
   pageTitle="Seznámení se službou Data Lake Analytics a jazykem U-SQL pomocí interaktivních kurzů webu Azure Portal | Azure" 
   description="Rychlý start při seznámení se službou Data Lake Analytics a jazykem U-SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>



# Použití interaktivních kurzů Azure Data Lake Analytics

web Azure Portal poskytuje interaktivní kurz, který vám umožní začít se službou Data Lake Analytics. Tento článek ukazuje, jak absolvovat kurz a analyzovat webové protokoly.


>[AZURE.NOTE] Pokud chcete absolvovat stejný kurz pomocí sady Visual Studio, informace najdete v tématu [Analýza webových protokolů pomocí Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
>Další interaktivní kurzy budou přidány na portál.


Další kurzy najdete v tématech:

- [Začínáme s Data Lake Analytics pomocí Portálu Azure](data-lake-analytics-get-started-portal.md)
- [Začínáme s Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Začínáme s Data Lake Analytics pomocí sady .NET SDK](data-lake-analytics-get-started-net-sdk.md)
- [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) 

**Požadavky**

Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:

- **Účet Data Lake Analytics**.  Viz [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md).

##Vytvoření účtu Data Lake Analytics 

Je nutné, abyste před spuštěním jakékoli úlohy měli účet Data Lake Analytics.

Každý účet Data Lake Analytics má závislost účtu [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).  Tento účet se nazývá výchozí účet Data Lake Store.  Účet Data Lake Store můžete vytvořit předem nebo při vytváření účtu Data Lake Analytics. V tomto kurzu vytvoříte účet Data Lake Store s účtem Analytics.

**Postup vytvoření účtu Data Lake Analytics**

1. Přihlaste se k [webu Azure Portal](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Kliknutím na položku **Microsoft Azure** v levém horním rohu otevřete Úvodní panel.
3. Klikněte na dlaždici **Marketplace**.  
3. Do vyhledávacího pole v okně **Vše** napište **Azure Data Lake Analytics** a stiskněte klávesu **ENTER**. V seznamu se zobrazí položka **Azure Data Lake Analytics**.
4. Klikněte v seznamu na položku **Azure Data Lake Analytics**.
5. V dolní části okna klikněte na možnost **Vytvořit**.
6. Zadejte nebo vyberte tyto položky:

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Název**: Zadejte název účtu Analytics.
    - **Data Lake Store**: Každý účet Data Lake Analytics má závislý účet Data Lake Store. Účet Data Lake Analytics a závislý účet Data Lake Store se musí nacházet ve stejném datovém centru Azure. Postupujte podle pokynů a vytvořte nový účet Data Lake Store nebo vyberte některý z existujících.
    - **Předplatné**: Zvolte předplatné Azure použité pro účet Analytics.
    - **Skupina prostředků**. Vyberte některou z existujících skupin prostředků Azure nebo vytvořte novou. Aplikace obvykle obsahují celou řadu součástí, například webovou aplikaci, databázi, databázový server, úložiště a služby třetích stran. Azure Resource Manager (ARM) umožňuje pracovat s prostředky v aplikaci jako se skupinou, která se nazývá Skupina prostředků Azure. Všechny prostředky pro aplikaci můžete nasadit, aktualizovat, sledovat nebo odstranit v rámci jediné koordinované operace. Pro nasazení použijete šablonu a tato šablona může fungovat v různých prostředích, jako je testovací, přípravné nebo produkční prostředí. Můžete zpřehlednit fakturaci ve své organizaci tím, že zobrazíte souhrnné náklady za celou skupinu. Další informace najdete v tématu [Přehled Azure Resource Manageru](resource-group-overview.md). 
    - **Umístění**. Vyberte datové centrum Azure pro účet Data Lake Analytics. 
7. Vyberte položku **Připnout na Úvodní panel**. Je to nezbytné pro absolvování tohoto kurzu.
8. Klikněte na možnost **Vytvořit**. Tím přejdete na Úvodní panel portálu. Na domovskou stránku se přidá nová dlaždice s popiskem „Nasazení Azure Data Lake Analytics“. Vytvoření účtu Data Lake Analytics chvíli trvá. Jakmile je účet vytvořený, portál ho otevře v novém okně.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##Spuštění interaktivního kurzu Analýza webového protokolu

**Postup spuštění interaktivního kurzu Analýza webového protokolu**

1. Na portálu klikněte v levé nabídce na položku **Microsoft Azure** a otevřete Úvodní panel.
2. Klikněte na dlaždici, která je propojená s vaším účtem Data Lake Analytics.
3. Na panelu **Základy** klikněte na položku **Prozkoumat interaktivní kurzy**.

    ![Interaktivní kurzy Data Lake Analytics](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Pokud se zobrazí oranžové upozornění „Nejsou nastaveny ukázky, klikněte...“, kliknutím na možnost **Kopírovat ukázková data** zkopírujte ukázková data do výchozího účtu Data Lake Store. Interaktivní kurz nejde bez těchto dat spustit.
5. V okně **Interaktivní kurzy** klikněte na možnost **Analýza webového protokolu**. Portál otevře kurz v novém okně portálu.
5. Klikněte na položku **1 Úvod** a postupujte podle pokynů.

##Viz také

- [Přehled Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
- [Začínáme s Data Lake Analytics pomocí Portálu Azure](data-lake-analytics-get-started-portal.md)
- [Začínáme s Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)



<!--HONumber=Sep16_HO3-->


