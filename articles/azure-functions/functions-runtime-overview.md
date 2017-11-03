---
title: "Přehled modulu Runtime Azure Functions | Microsoft Docs"
description: "Přehled Azure Functions Runtime Preview"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: cb98d5f2aaa526555820c15ba5a32fb7e78ffc5a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-runtime-overview"></a>Přehled Azure Functions modulu Runtime

Azure Functions Runtime nabízí nový způsob můžete využívat jednoduchost a flexibilitu Azure Functions programovací model na místě. Postavené na stejném kořeny s otevřeným zdrojem jako funkce Azure, Azure Functions Runtime je nasadit místně a poskytuje prostředí téměř shodná vývoj jako cloudová služba.

![Modul Runtime Azure Functions na portálu Preview][1]

Azure Functions Runtime poskytuje způsob, jak jste prostředí Azure Functions před potvrzením do cloudu. Tímto způsobem kód prostředků, které vytvoříte pak děláte s vámi do cloudu při migraci.  Modul runtime také otevře nové možnosti pro vás, například pomocí k výměně za chodu výpočetní výkon vašich počítačů místní spuštění dávky procesy přes noc. Zařízení můžete také použít v rámci vaší organizace podmíněně odesílání dat do jiných systémů, jak místně a v cloudu.

Azure Functions Runtime se skládá ze dvou částí:
* Role správy Runtime Azure Functions
* Role pracovního procesu modulu Runtime Azure Functions

## <a name="azure-functions-management-role"></a>Role správy Azure Functions

Role Azure funkce správy poskytuje pro správu funkce místní hostitel. Tato role se provádí tyto úlohy:

* Z portálu Azure funkce správy, který je hostitelem stejný jako ten, můžete vidět v [portál Azure](https://portal.azure.com). Díky tomu můžete vyvíjet funkcí stejným způsobem, jako byste na portálu Azure.
* Distribuci funkcí mezi více pracovníků funkce.
* Poskytování publikování koncový bod, aby mohly publikovat vaše funkce přímo ze sady Microsoft Visual Studio.

## <a name="azure-functions-worker-role"></a>Role pracovního procesu Azure Functions

Role pracovního procesu Azure funkce nasazených v kontejnerech Windows a toto je, kde se provádí funkce kódu.  Můžete nasadit více rolí pracovního procesu ve vaší organizaci a je klíče způsob, ve kterém zákazníci měli používat k výměně za chodu výpočetním výkonu.

## <a name="minimum-requirements"></a>Minimální požadavky

Začít s Azure Functions Runtime musí mít počítač s **systému Windows Server 2016 nebo Windows 10 Creators Update** s přístupem k **systému SQL Server** instance.

## <a name="next-steps"></a>Další kroky

Nainstalujte [Runtime funkce Azure preview](https://aka.ms/azafr)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
