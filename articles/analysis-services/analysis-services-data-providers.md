---
title: "Klient knihovny potřebné pro připojení k Azure Analysis Services | Microsoft Docs"
description: "Popisuje klientské knihovny potřebné pro klientské aplikace a nástroje pro připojení služby Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: a96e7fe671dc051da35168fa7b49ecba53b4c4a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Klientské knihovny pro připojení k Azure Analysis Services

Knihovny klienta jsou nezbytné pro klientské aplikace a nástroje pro připojení k serverům služby Analysis Services. 

Služba Analysis Services, používají tři knihovny klienta. ADOMD.NET a Analysis Services Management Objects (AMO), jsou spravované klientské knihovny. Zprostředkovatel OLE DB služby Analysis Services (MSOLAP DLL) je nativní Klientská knihovna. Obvykle jsou všechny tři nainstalovány ve stejnou dobu. Azure Analysis Services vyžaduje nejnovější verze. 

Microsoft klientské aplikace, třeba Power BI Desktop a Excel nainstalovat všechny tři klientské knihovny. V závislosti na verzi nebo četnost aktualizací, ale klientské knihovny nemusí být nejnovější verze vyžaduje Azure Analysis Services. To samé platí pro vlastní aplikace a další rozhraní, jako jsou AsCmd, Tom nebo ADOMD.NET. Tyto aplikace vyžadují ruční instalaci knihovny. Klientské knihovny pro ruční instalaci jsou zahrnuty v balíčcích funkcí systému SQL Server jako distribuovatelného balíčky. Ale tyto knihovny klienta, jsou svázané s verze systému SQL Server a nemusí být nejnovější.  

Klientské knihovny pro připojení klientů se liší od zprostředkovatelů dat. požadované pro připojení ke zdroji dat ze serveru Azure Analysis Services. Další informace o připojení zdroje dat naleznete v tématu [připojení zdroje dat](analysis-services-datasource.md).

## <a name="download-the-latest-client-libraries"></a>Stáhněte si nejnovější knihovny klienta  
Pomocí následujících klientských knihoven, pokud jsou v produkčním prostředí a vyžadovat plně vydaná a podporované verze.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Další kroky
[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)    
[Propojení s Power BI](analysis-services-connect-pbi.md)
