---
title: "Připojení ke službě Azure Analysis Services | Microsoft Docs"
description: "Zjistěte, jak se připojit k a získat data ze serveru služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: deb3ef28d20decef01826450bd6091f87dd069de
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-to-an-azure-analysis-services-server"></a>Připojit k serveru Azure Analysis Services

Tento článek popisuje připojení k serveru pomocí modelování dat a správu aplikace jako SQL Server Management Studio (SSMS) nebo SQL Server Data Tools (SSDT). Nebo s klientem služby generování sestav aplikace, jako je Microsoft Excel, Power BI Desktop nebo vlastních aplikací. Připojení k Azure Analysis Services používat protokol HTTPS.

## <a name="client-libraries"></a>Klientské knihovny
[Získat nejnovější knihovny klienta](analysis-services-data-providers.md)

Všechna připojení k serveru, bez ohledu na typ, vyžadují aktualizované AMO ADOMD.NET a OLEDB klientské knihovny pro připojení k a rozhraní se serverem služby Analysis Services. Pro aplikace SSMS, rozšíření SSDT, aplikace Excel 2016 a Power BI nejnovější knihovny klienta instalaci nebo aktualizaci s měsíční verzemi. V některých případech je však možné že aplikace nemusí mít nejnovější. Například když zpoždění zásady aktualizace nebo aktualizace Office 365 jsou na odložené kanálu.

## <a name="server-name"></a>Název serveru

Při vytváření serveru služby Analysis Services v Azure, je třeba zadat jedinečný název a oblasti, kde je server, který se má vytvořit. Při zadávání názvu serveru v připojení, je schéma pojmenování serveru:

```
<protocol>://<region>/<servername>
```
 Kde protokol je řetězec **asazure**, oblast je identifikátor Uri, které bylo vytvořeno serveru (například westus.asazure.windows.net) a servername je název jedinečný serveru v rámci oblasti.

### <a name="get-the-server-name"></a>Získat název serveru
V **portál Azure** > serveru > **přehled** > **název serveru**, zkopírujte název celý server. Pokud ostatní uživatelé ve vaší organizaci se připojují k tomuto serveru příliš, můžete tento název serveru sdílet s nimi. Při zadávání názvu serveru, třeba zadat celou cestu.

![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Připojovací řetězec

Při připojování k serveru Azure Analysis Services pomocí tabulkový objektový Model, použijte následující formáty řetězec připojení:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrované ověřování služby Azure Active Directory
Integrované ověřování převezme mezipaměti přihlašovacích údajů Azure Active Directory, pokud je k dispozici. V opačném případě se zobrazí okno přihlášení k Azure.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory ověřování pomocí uživatelského jména a hesla

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Ověřování systému Windows (integrované zabezpečení)
Použijte účet systému Windows s aktuálním procesu.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a>Připojit pomocí souboru ODC
Ve starších verzích aplikace Excel mohou uživatelé připojit k serveru Azure Analysis Services pomocí souboru (ODC) Office Data Connection. Další informace najdete v tématu [vytvořit soubor dat ODC (Office Connection)](analysis-services-odc.md).


## <a name="next-steps"></a>Další kroky
[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)    
[Připojit k Power BI](analysis-services-connect-pbi.md)   
[Správa serveru](analysis-services-manage.md)   

