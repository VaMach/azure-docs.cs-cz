---
title: "Co se stalo s Moje webová úloha projektu (Visual Studio Azure Storage připojeno service)? | Dokumentace Microsoftu"
description: "Popisuje, co se stalo v projektu webové úlohy Azure po připojení k účtu úložiště pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8891685a99c5ba366b74af0a21396d4a5e499835
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Co se stalo s Moje webová úloha projektu (Visual Studio Azure Storage připojeno service)?
## <a name="references-added"></a>Přidanými referencemi
Balíček NuGet úložiště Azure se přidání nebo aktualizaci v projektu sady Visual Studio.  
Tento balíček přidá následující odkazy na rozhraní .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Připojovací řetězec pro Azure Storage přidán
V souboru App.config vašeho projektu **AzureWebJobsStorage** a **AzureWebJobsDashboard** položky, byly aktualizovány s vybraným účtem úložiště pro připojovací řetězec a klíč.

Další informace najdete v tématu [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

