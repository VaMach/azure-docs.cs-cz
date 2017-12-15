---
title: "Azure názvy alias serveru služby Analysis Services | Microsoft Docs"
description: "Popisuje, jak vytvořit a používat aliasy název serveru."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: a947dde1551c653faa54f088c1712c41c7657aa0
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="alias-server-names"></a>Názvy serverů alias

Pomocí aliasu název serveru, uživatelé se mohou připojit k serveru Azure Analysis Services s kratší *alias* místo názvu serveru. Při připojování z klientské aplikace, alias je zadán jako k koncový bod pomocí **odkaz: / /** formátu protokolu. Koncový bod pak vrátí název skutečné serveru připojit.

Názvy serverů alias jsou vhodné pro:

- Migrace modely mezi servery bez ovlivnění uživatelé. 
- Názvy serverů popisný jsou usnadňuje uživatelům zapamatovat. 
- Uživatelé na jiné servery v různých časech dne. 
- Uživatelé v různých oblastech instancí, které jsou geograficky co nejblíže jako při použití Azure Traffic Manager. 

Žádný koncový bod protokolu HTTP, který vrátí platný název serveru Azure Analysis Services může sloužit jako alias.

![Alias odkazu formátu](media/analysis-services-alias/aas-alias-browser.png)

Při připojení z klienta, je zadán název serveru alias pomocí **odkaz: / /** formátu protokolu. Například v Power BI Desktop:

![Připojení Power BI Desktop](media/analysis-services-alias/aas-alias-connect-pbid.png)

## <a name="create-an-alias"></a>Vytvoření aliasu

Chcete-li vytvořit alias koncový bod, můžete použít libovolnou metodu, která vrací platný název serveru Azure Analysis Services. Například odkaz na soubor v Azure Blob Storage obsahující skutečné server name, nebo vytvořte a publikování aplikace webových formulářů ASP.NET.

V tomto příkladu se vytvoří webové formuláře aplikace ASP.NET v sadě Visual Studio. Stránky předlohy odkaz a uživatelský ovládací prvek se odeberou ze stránky Default.aspx. Obsah Default.aspx je jednoduše následující direktiva stránky:

```
<%@ Page Title="Home Page" Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="FriendlyRedirect._Default" %>
```

Událost Page_Load v Default.aspx.cs používá metodu Response.Write() k vrácení název serveru Azure Analysis Services.

```
protected void Page_Load(object sender, EventArgs e)
{
    this.Response.Write("asazure://<region>.asazure.windows.net/<servername>");
}
```

## <a name="see-also"></a>Viz také

[Knihovny klienta](analysis-services-data-providers.md)   
[Připojit se z Power BI Desktop](analysis-services-connect-pbi.md)
