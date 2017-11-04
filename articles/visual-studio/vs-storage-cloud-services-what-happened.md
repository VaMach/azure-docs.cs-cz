---
title: "Co se stalo s Moje projekt cloudové služby? | Dokumentace Microsoftu"
description: "Popisuje, co se stane v projektu cloudové služby po připojení k účtu úložiště Azure pomocí sady Visual Studio připojené služby"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 4c9de56f6daf07097c0f593db37d14dce3bce05f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Co se stalo s Moje projekt cloudových služeb (Visual Studio Azure Storage připojeno service)?
## <a name="references-added"></a>Přidanými referencemi
Balíček NuGet úložiště Azure byl přidán do projektu sady Visual Studio.  
Tento balíček přidá následující odkazy na rozhraní .NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Připojovací řetězec pro Azure Storage přidán
Elementy byly vytvořeny s připojovací řetězec a klíč účtu vybrané úložiště. Změny byly provedeny následující soubory:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

