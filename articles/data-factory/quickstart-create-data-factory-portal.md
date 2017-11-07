---
title: "Vytvoření datové továrny Azure pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Vytvořte datovou továrnu Azure ke zkopírování dat z cloudového úložiště dat (Azure Blob Storage) do jiného cloudového úložiště dat (Azure SQL Database)."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 09/19/2017
ms.author: jingwang
ms.openlocfilehash: 9f49fbff9f66dc081e94b35e1318a2c9aee60dc0
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-data-factory-using-the-azure-portal"></a>Vytvoření datové továrny pomocí webu Azure Portal
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Verze 1 – GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Verze 2 – Preview](quickstart-create-data-factory-portal.md)

Azure Data Factory je cloudová služba pro integraci dat umožňující vytváření pracovních postupů řízených daty v cloudu za účelem orchestrace a automatizace přesunu a transformace dat. Pomocí služby Azure Data Factory můžete vytvářet a plánovat pracovní postupy řízené daty (nazývané kanály) se schopností ingestovat data z různorodých úložišť dat, zpracovat a transformovat tato data pomocí výpočetních služeb, jako je Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics a Azure Machine Learning, a publikovat výstupní data do úložišť dat, jako je Azure SQL Data Warehouse, aby je mohly využívat aplikace business intelligence (BI). 

V tomto rychlém startu použijete Azure Portal k vytvoření datové továrny. Po vytvoření datové továrny budete muset pomocí PowerShellu, sady .NET SDK, Python SDK nebo rozhraní REST API (stejně jako v jiných rychlých startech) vytvořit datový kanál, který kopíruje data ze zdrojového do cílového úložiště dat. V současné době není možné vytvářet kanály v datové továrně pomocí webu Azure Portal.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [úvod do služby Data Factory verze 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-data-factory"></a>Vytvoření datové továrny
V rámci tohoto rychlého startu provedete tyto kroky:
1. V nabídce vlevo klikněte na **Nový**, klikněte na **Data + analýzy** a pak na **Data Factory**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)
2. V okně **Nová datová továrna** jako **název** zadejte **ADFTutorialDataFactory**. 
      
     ![Okno Nový objekt pro vytváření dat](./media/quickstart-create-data-factory-portal/new-azure-data-factory.png)
 
   Název objektu pro vytváření dat Azure musí být **globálně jedinečný**. Pokud se zobrazí následující chyba, změňte název objektu pro vytváření dat (třeba na váš_název_ADFTutorialDataFactory) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.
  
       `Data factory name “ADFTutorialDataFactory” is not available`
3. Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
4. Pro **Skupinu prostředků** proveďte jeden z následujících kroků:
     
      - Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků. 
      - Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.   
         
      Některé kroky v tomto rychlém startu vychází z předpokladu, že pro skupinu prostředků použijete název **ADFTutorialResourceGroup**. Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
4. Jako **verzi** vyberte **V2 (Preview)**.
5. Vyberte **umístění** pro objekt pro vytváření dat. V současné době můžete datové továrny V2 vytvářet jenom v oblasti **Východní USA**. Výpočetní prostředí a úložiště dat používané v datových továrnách však mohou být v jiných oblastech. 
6. Zaškrtněte **Připnout na řídicí panel**.     
7. Klikněte na možnost **Vytvořit**.
      
      > [!IMPORTANT]
      > Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.
      > 
      > Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.             
3. Na řídicím panelu vidíte následující dlaždice se statusem: **Nasazování datové továrny**. 

    ![nasazování dlaždice datové továrny](media//quickstart-create-data-factory-portal/deploying-data-factory.png)
1. Po vytvoření se zobrazí okno **Objekt pro vytváření dat**, jak je znázorněno na obrázku.
   
   ![Domovská stránka objektu pro vytváření dat](./media/quickstart-create-data-factory-portal/data-factory-home-page.png)


## <a name="next-steps"></a>Další kroky
Kanál v této ukázce kopíruje data z jednoho umístění do jiného umístění v úložišti objektů blob v Azure. Projděte si [kurzy](tutorial-copy-data-dot-net.md), kde se dozvíte o použití služby Data Factory ve více scénářích. 