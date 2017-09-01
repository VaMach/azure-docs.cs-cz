---
title: "Kurz služby Azure Analysis Services – Lekce 13: Nasazení | Dokumentace Microsoftu"
description: "Popisuje, jak nasadit projekt Kurz služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/17/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 70dbf5786262f75199270aa8009e03b9b48b8559
ms.contentlocale: cs-cz
ms.lasthandoff: 08/21/2017

---
# <a name="lesson-13-deploy"></a>Lekce 13: Nasazení

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci nakonfigurujete vlastnosti nasazení – zadáte server služby Azure Analysis Services, na který se má nasazení provést, a název modelu. Pak model nasadíte do příslušné instance. Jakmile bude váš model nasazen, uživatelé se k němu budou moci připojit pomocí klientské aplikace pro vytváření sestav. Další informace najdete v tématu [Nasazení do služby Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).  
  
Odhadovaný čas dokončení této lekce: **5 minut**  
  
## <a name="prerequisites"></a>Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 12: Analýza v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).  

> [!IMPORTANT]  
> Abyste mohli nasadit vzdálený server služby Analysis Services, musíte k němu mít [oprávnění správce](../analysis-services-server-admins.md).  

> [!IMPORTANT]  
> Pokud jste nainstalovali ukázkovou databázi AdventureWorksDW2014 na místní SQL Server a model nasazujete na server služby Azure Analysis Services, vyžaduje se [Místní brána dat](../analysis-services-gateway.md).
  
## <a name="deploy-the-model"></a>Nasazení modelu  
  
#### <a name="to-configure-deployment-properties"></a>Konfigurace vlastností nasazení  

  
1.  V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **AW Internet Sales** a pak klikněte na **Vlastnosti**.  
  
2.  V dialogovém okně **AW Internet Sales – Stránky vlastností** v části **Server nasazení** zadejte do vlastnosti **Server** úplný název serveru.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  Do vlastnosti **Databáze** zadejte **Adventure Works Internet Sales**.  
  
4.  Do vlastnosti **Název modelu** zadejte **Adventure Works Internet Sales Model**.  
  
5.  Zkontrolujte výběr a klikněte na **OK**.  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a>Nasazení modelu Adventure Works Internet Sales
  
1.  V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **AW Internet Sales** > **Sestavit**.  

2.  Klikněte pravým tlačítkem na projekt **AW Internet Sales** > **Nasadit**.

    Při nasazování do služby Azure Analysis Services můžete být vyzváni k zadání účtu. Zadejte účet organizace a heslo, například nancy@adventureworks.com. Tento účet musí být mezi správci serveru.
  
    Zobrazí se dialogové okno Nasadit ukazující stav nasazení metadat a každé tabulky zahrnuté v modelu.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Po úspěšném dokončení nasazení pokračujte kliknutím na **Zavřít**.  
  
## <a name="conclusion"></a>Závěr  
Blahopřejeme! Vytvořili a nasadili jste první tabelární model služby Analysis Services. Tento kurz vás provedl dokončením nejběžnějších úkolů v rámci vytváření tabelárního modelu. Teď, když je váš model Adventure Works Internet Sales nasazený, můžete ho spravovat pomocí aplikace SQL Server Management Studio, vytvořit skripty pro zpracování a plán zálohování. Uživatelé se nyní také můžou k modelu připojit pomocí klientské aplikace pro vytváření sestav, jako je Microsoft Excel nebo Power BI.  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>Co dále?
[Propojení s Power BI Desktop](../analysis-services-connect-pbi.md)   
[Doplňková lekce – Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Doplňková lekce – Řádky podrobností](../tutorials/aas-supplemental-lesson-detail-rows.md)   
[Doplňková lekce – Nepravidelné hierarchie](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   

