---
title: "Kurz služby Azure Analysis Services – Lekce 13: Nasazení | Dokumentace Microsoftu"
description: "Popisuje, jak nasadit projekt Kurz služby Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 8e3e1be572aa66ab46f894a2e5f395d1e6f2ea23
ms.contentlocale: cs-cz
ms.lasthandoff: 06/03/2017

---
<a id="lesson-13-deploy" class="xliff"></a>

# Lekce 13: Nasazení

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

V této lekci nakonfigurujete vlastnosti nasazení – zadáte server Analysis Services v Azure nebo místní server SQL Server vNext Analysis Services a název modelu. Pak model nasadíte do příslušné instance. Jakmile bude váš model nasazen, uživatelé se k němu budou moci připojit pomocí klientské aplikace pro vytváření sestav. Další informace najdete v tématu [Nasazení do služby Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).  
  
Odhadovaný čas dokončení této lekce: **5 minut**  
  
<a id="prerequisites" class="xliff"></a>

## Požadavky  
Toto téma je součástí kurzu tabelárního modelování, který by se měl dokončit v daném pořadí. Před provedením úkolů v této lekci byste měli mít dokončenou předchozí lekci: [Lekce 12: Analýza v aplikaci Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).  

**Důležité:** Pokud jste nainstalovali ukázkovou databázi AdventureWorksDW2014 na místní SQL Server a model nasazujete na server Azure Analysis Services, vyžaduje se [Místní brána dat](../analysis-services-gateway.md).
  
<a id="deploy-the-model" class="xliff"></a>

## Nasazení modelu  
  
<a id="to-configure-deployment-properties" class="xliff"></a>

#### Konfigurace vlastností nasazení  

  
1.  V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **AW Internet Sales** a pak klikněte na **Vlastnosti**.  
  
2.  V dialogovém okně **AW Internet Sales – Stránky vlastností** v části **Server nasazení** zadejte do vlastnosti **Server** název serveru Analysis Services v Azure nebo místním prostředí.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
 
    > [!IMPORTANT]  
    > Abyste mohli nasadit vzdálenou instanci služby Analysis Services, musíte k ní mít oprávnění správce.  
  
3.  Do vlastnosti **Databáze** zadejte **Adventure Works Internet Sales**.  
  
4.  Do vlastnosti **Název modelu** zadejte **Adventure Works Internet Sales Model**.  
  
5.  Zkontrolujte výběr a klikněte na **OK**.  
  
<a id="to-deploy-the-adventure-works-internet-sales" class="xliff"></a>

#### Nasazení modelu Adventure Works Internet Sales
  
1.  V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt **AW Internet Sales** > **Sestavit**.  

2.  Klikněte pravým tlačítkem na projekt **AW Internet Sales** > **Nasadit**.

    Při nasazování do služby Azure Analysis Services můžete být vyzváni k zadání účtu. Zadejte účet organizace a heslo, například nancy@adventureworks.com. Tento účet musí být mezi správci instance serveru.
  
    Zobrazí se dialogové okno Nasadit ukazující stav nasazení metadat a každé tabulky zahrnuté v modelu.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Po úspěšném dokončení nasazení pokračujte kliknutím na **Zavřít**.  
  
<a id="conclusion" class="xliff"></a>

## Závěr  
Blahopřejeme! Vytvořili a nasadili jste první tabelární model služby Analysis Services. Tento kurz vás provedl dokončením nejběžnějších úkolů v rámci vytváření tabelárního modelu. Teď, když je váš model Adventure Works Internet Sales nasazený, můžete ho spravovat pomocí aplikace SQL Server Management Studio, vytvořit skripty pro zpracování a plán zálohování. Uživatelé se nyní také můžou k modelu připojit pomocí klientské aplikace pro vytváření sestav, jako je Microsoft Excel nebo Power BI.  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
<a id="whats-next" class="xliff"></a>

## Co dále?
*  [Doplňková lekce – Dynamické zabezpečení](../tutorials/aas-supplemental-lesson-dynamic-security.md)

*  [Doplňková lekce – Řádky podrobností](../tutorials/aas-supplemental-lesson-detail-rows.md)

*  [Doplňková lekce – Nepravidelné hierarchie](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)

