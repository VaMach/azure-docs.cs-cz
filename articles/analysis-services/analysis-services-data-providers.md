---
title: "Klient knihovny potřebné pro připojení k Azure Analysis Services | Microsoft Docs"
description: "Popisuje klientské knihovny potřebné pro klientské aplikace a nástroje pro připojení služby Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 12/14/2017
ms.author: owend
ms.openlocfilehash: 870d430d1926859894f452e0af812d794272a9e6
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Klientské knihovny pro připojení k Azure Analysis Services

Knihovny klienta jsou nezbytné pro klientské aplikace a nástroje pro připojení k serverům služby Analysis Services. 

## <a name="download-the-latest-client-libraries"></a>Stáhněte si nejnovější knihovny klienta  

|Ke stažení  |Verze  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    14.0.801.241      |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |    14.0.801.241      |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   14.0.800.117      |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    14.0.801.241      |

## <a name="understanding-client-libraries"></a>Principy klientské knihovny

Služba Analysis Services, používají tři klientské knihovny, také známé jako zprostředkovatelé dat. ADOMD.NET a služby Analysis Services Management Objects (AMO) jsou spravované klientské knihovny. Zprostředkovatele Analysis Services OLE DB (MSOLAP DLL) je knihovna nativního klienta. Obvykle jsou všechny tři nainstalovány ve stejnou dobu. **Azure Analysis Services vyžaduje nejnovější verze všechny tři knihovny**. 

Microsoft klientské aplikace jako Power BI Desktop a Excel nainstalujte všechny tři klientské knihovny a provede jejich aktualizaci, jakmile je k dispozici nová verze. V závislosti na verzi nebo četnost aktualizací nemusí být některé klientské knihovny nejnovější verze vyžaduje Azure Analysis Services. To samé platí pro vlastní aplikace a další rozhraní, jako jsou AsCmd, Tom nebo ADOMD.NET. Tyto aplikace vyžadují ručně nebo prostřednictvím kódu programu instalace knihovny. Klientské knihovny pro ruční instalaci jsou zahrnuty v balíčcích funkcí systému SQL Server jako distribuovatelného balíčky. Ale tyto knihovny klienta, jsou svázané s verze systému SQL Server a nemusí být nejnovější.  

Klientské knihovny pro připojení klientů se liší od zprostředkovatelů dat. požadované pro připojení ke zdroji dat ze serveru Azure Analysis Services. Další informace o připojení zdroje dat naleznete v tématu [připojení zdroje dat](analysis-services-datasource.md).

## <a name="client-library-types"></a>Knihovny typů klientů

### <a name="analysis-services-ole-db-provider-msolap"></a>Zprostředkovatele Analysis Services OLE DB (MSOLAP) 

 Analysis Services OLE DB Provider (MSOLAP) je nativní Klientská knihovna pro připojení k databázi služby Analysis Services. Používá se nepřímo ADOMD.NET i AMO, delegování žádosti o připojení k poskytovateli data. Zprostředkovatele OLE DB můžete také volat přímo v kódu aplikace.  
  
 Zprostředkovatele Analysis Services OLE DB se instaluje automaticky tak, že většina nástrojů a klientské aplikace používá pro přístup k databáze služby Analysis Services. Musí být nainstalován na počítačích pro přístup k datům Analysis Services.  
  
 OLE DB – zprostředkovatelé jsou často určené v připojovací řetězce. Řetězec připojení služby Analysis Services používá k odkazování na zprostředkovatele OLE DB různé klasifikace: MSOLAP. \<verze > .dll.

### <a name="amo"></a>AMO  

 Sada AMO je spravované klientské knihovny používané pro správu serveru a data definice. Má nainstalovat a používat nástroje a klientské aplikace. Například SQL Server Management Studio (SSMS) používá AMO k připojení ke službě Analysis Services.  
  
 Připojení pomocí AMO je obvykle minimální, který se skládá z `“data source=\<servername>”`. Po navázání připojení můžete použít rozhraní API pro práci s kolekcí databázi a hlavní objekty. Rozšíření SSDT i SSMS použít AMO pro připojení k instanci služby Analysis Services.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET je spravovaná data knihovna klienta použitého k dotazování dat služby Analysis Services. Má nainstalovat a používat nástroje a klientské aplikace. 
  
 Při připojování k databázi, jsou podobné vlastnosti řetězec připojení pro všechny tři knihovny. Téměř jakoukoli připojovacího řetězce je definována pro ADOMD.NET pomocí [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](https://msdn.microsoft.com/library/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring.aspx) funguje i pro AMO a Analysis Services OLE DB Provider (MSOLAP). Další informace najdete v tématu [vlastnosti připojovacího řetězce &#40; Služba Analysis Services &#41; ](https://docs.microsoft.com/sql/analysis-services/instances/connection-string-properties-analysis-services).  

  
##  <a name="bkmk_LibUpdate"></a>Určení verze klienta knihovny   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  Přejděte do části `C:\Program Files\Microsoft Analysis Services\AS OLEDB\140` (Soubor > Nový > Jiné). Pokud máte více než jednu složku a zvolte vyšší číslo.
  
2.  Klikněte pravým tlačítkem na **msolap140.dll** > **vlastnosti** > **podrobnosti**.  
    
    ![Podrobnosti ke knihovně klienta](media/analysis-services-data-providers/aas-msolap-details.png)
  
### <a name="amo"></a>AMO

1. Přejděte do části `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\v4.0_14.0.0.0__89845dcd8080cc91` (Soubor > Nový > Jiné).
2. Klikněte pravým tlačítkem na **Microsoft.AnalysisServices** > **vlastnosti** > **podrobnosti**.  

### <a name="adomd"></a>ADOMD

1. Přejděte do části `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\v4.0_14.0.0.0__89845dcd8080cc91` (Soubor > Nový > Jiné).
2. Klikněte pravým tlačítkem na **Microsoft.AnalysisServices.AdomdClient** > **vlastnosti** > **podrobnosti**.  


## <a name="next-steps"></a>Další kroky
[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)    
[Propojení s Power BI](analysis-services-connect-pbi.md)
