---
title: "Spravovat plán služby App Service v Azure | Microsoft Docs"
description: "Zjistěte, jak do služby App Service plány provádění různých úloh pro správu plán služby App Service."
keywords: "služby App service, služby azure app service, škálování, plán služby app service, změnit, vytvořit, spravovat, správy"
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: 5369946b1e8a4851ee940cf6fe91a1bdb94db5f3
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="manage-an-app-service-plan-in-azure"></a>Spravovat plán služby App Service v Azure

[Plán služby App Service](azure-web-sites-web-hosting-plans-in-depth-overview.md) poskytuje prostředky aplikace služby App Service je potřeba spustit. Tento postup ukazuje, jak spravovat plán služby App Service. 

## <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

> [!TIP]
> Pokud máte služby App Service Environment, najdete v části [vytvořit plán služby App Service ve službě App Service Environment](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

Můžete vytvořit plán služby App Service prázdný nebo jako součást vytváření aplikace.

V [portál Azure](https://portal.azure.com), klikněte na tlačítko **nový** > **Web + mobilní**a potom vyberte **webové aplikace** nebo jiného typu aplikace služby App Service.

![Vytvoření aplikace v portálu Azure.][createWebApp]

Potom můžete vybrat existující plán služby App Service nebo vytvořit plán pro novou aplikaci.

 ![Vytvořte plán služby App Service.][createASP]

Chcete-li vytvořit plán služby App Service, klikněte na tlačítko **[+] vytvořit nové**, typ **plán služby App Service** název a potom vyberte odpovídající **umístění**. Klikněte na tlačítko **cenová úroveň**a potom vyberte příslušné cenovou úroveň pro službu. Vyberte **zobrazit všechny** do zobrazení více ceny možnosti, jako například **volné** a **sdílené**. 

Až vyberete cenovou úroveň, klikněte na **vyberte** tlačítko.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Přesuňte aplikace na jiný plán aplikační služby

Aplikace můžete přesunout na jiný plán aplikační služby, pokud jsou plán zdroj a cíl plán v _stejnou skupinu prostředků a geografické oblasti_.

Chcete-li přesunout aplikaci do jiného plánu, přejděte do aplikace, který chcete přesunout v [portál Azure](https://portal.azure.com).

V **nabídky**, vyhledejte **plán služby App Service** části.

Vyberte **plán služby App Service změnu** ke spuštění procesu.

**Plán služby App Service změnu** otevře **plán služby App Service** selektor. Vyberte, chcete-li přesunout tuto aplikaci do existujícího plánu. 

> [!IMPORTANT]
> **Plán aplikační služby vyberte** stránky se filtrují podle následujících kritérií: 
> - Existuje ve stejné skupině prostředků 
> - Existuje ve stejné zeměpisné oblasti 
> - Existuje ve stejné webový prostor  
> 
> A _webový prostor_ je logická konstrukce ve službě App Service, která definuje seskupení prostředků serveru. Geografické oblasti (například západní USA) obsahuje mnoho webspaces aby bylo možné přidělit zákazníky používající službu aplikace. V současné době není možné přesunout mezi webspaces prostředky služby App Service. 
> 

![Selektor plán služby App Service.][change]

Každý plán má svou vlastní cenová úroveň. Například přesun lokalitu **volné** vrstvy do **standardní** vrstvy, umožňuje všechny aplikace, které jsou přiřazené použití funkcí a prostředky **standardní** vrstvy. Přesunutí aplikace z plánu vrstvené vyšší do nižší vrstvené plán však znamená, že už máte přístup k určité funkce. Pokud vaše aplikace používá funkce, která není k dispozici v cílové plánu, dojde k chybě, která zobrazuje funkce, které se používá, který není k dispozici. Například pokud některé z aplikací používá certifikáty protokolu SSL, mohou se zobrazit chybová zpráva: `Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`v takovém případě budete muset škálování cenové úrovně plánu cíl na **základní** nebo vyšší, nebo je třeba odebrat všechny připojení SSL k aplikace, než aplikace můžete přesunout do cílového plánu.

## <a name="move-an-app-to-a-different-region"></a>Přesunutí aplikace v jiné oblasti

Oblast, ve kterém běží vaše aplikace je oblasti, které je v plánu služby App Service. Nelze však změnit plán služby App Service oblast. Pokud chcete ke spouštění vaší aplikace v jiné oblasti, jeden alternativou je aplikace klonování. Klonování zhotoví kopii aplikaci v nové nebo existující plán služby App Service v libovolné oblasti.

Můžete najít **klonování aplikace** v **nástroje pro vývoj** části nabídky.

> [!IMPORTANT]
> Klonování má určitá omezení, které si můžete přečíst o na [klonování aplikace Azure App Service](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Škálovat plán služby App Service

Škálování App Service je plán cenová úroveň najdete v tématu [škálování aplikace v Azure](web-sites-scale.md).

Chcete-li škálovat počet instancí aplikace, přečtěte si téma [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Odstranit plán služby App Service

Abyste předešli neočekávané poplatky, když odstraníte poslední aplikace v rámci plánu služby App Service, služby App Service také odstraní plán ve výchozím nastavení. Pokud místo toho zachovat plán, měli byste změnit plán **volné** vrstvy, aby vám nemáte získat účtovat.

> [!IMPORTANT]
> **Plánů služby App Service** mají žádné aplikace přidružené k je stále platit poplatky vzhledem k tomu, že budou nadále rezervovat nakonfigurované instance virtuálních počítačů.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Škálování aplikace v Azure](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
