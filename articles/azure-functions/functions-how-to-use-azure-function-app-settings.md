---
title: "Konfigurovat nastavení aplikace Azure funkce | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat nastavení aplikace Azure funkce."
services: 
documentationcenter: .net
author: rachelappel
manager: cfowler
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: glenga
ms.openlocfilehash: 3229fe0b5aacbbc716039a4a6abfd235dbc4c4f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Správa funkce aplikace na portálu Azure 

Funkce aplikace v Azure Functions poskytuje kontext spuštění pro jednotlivých funkcí. Chování funkce aplikace platí pro všechny funkce, které jsou hostované aplikací danou funkci. Toto téma popisuje, jak konfigurovat a spravovat aplikace pro funkce na portálu Azure.

Chcete-li začít, přejděte na [portál Azure](http://portal.azure.com) a přihlaste se k účtu Azure. Na panelu hledání v horní části portálu zadejte název vaší aplikace Function App a vyberte ji ze seznamu. Po výběru funkce aplikace, se zobrazí následující stránka:

![Přehled funkce aplikace na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <a name="manage-app-service-settings"></a>Karta nastavení aplikace – funkce

![Přehled funkce aplikace na portálu Azure.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

**Nastavení** karta je, kde můžete aktualizovat verzi modulu runtime funkce, kterou vaše aplikace funkce. Je také kde budete spravovat hostitele klíčů používaných omezit přístup protokolu HTTP na všechny funkce, které jsou hostované aplikací funkce.

Funkce podporuje spotřeba hostování a hostování plány služby App Service. Další informace najdete v tématu [vyberte plán správné služby pro Azure Functions](functions-scale.md). Pro lepší předvídatelnost v plánu spotřeby funkce vám umožní omezit využití platformy a to nastavením denní kvóty využití, v GB sekundách. Jakmile bude dosaženo kvóty denní využití, funkce aplikace je zastavena. Aplikaci funkce zastavit v důsledku dosažení útraty kvótu může být ze stejné oblasti jako zřízení denně výdaje kvóty znovu zapnout. Najdete v článku [Azure Functions stránce s cenami](http://azure.microsoft.com/pricing/details/functions/) podrobnosti o fakturaci.   

## <a name="platform-features-tab"></a>Karta funkcí platformy

![Karta funkcí platformy funkce aplikace.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Funkce aplikace spouštět ve a udržované platformou Azure App Service. Jako takový funkce aplikací mít přístup k většinu funkcí Azure základní webového hostingu platformy. **Funkce** karta je, odkud si řadu funkcí služby App Service platformy, na které můžete použít ve svých aplikacích funkce. 

> [!NOTE]
> Ne všechny funkce služby App Service jsou k dispozici při spuštění aplikace funkce na spotřebu hostování plánu.

Zbývající část tohoto tématu se zaměřuje na těchto funkcích služby App Service na portálu Azure, které jsou užitečné pro funkce:

+ [Editor služby App Service](#editor)
+ [Nastavení aplikace](#settings) 
+ [Console](#console)
+ [Pokročilé nástroje (Kudu)](#kudu)
+ [Možnosti nasazení](#deployment)
+ [CORS](#cors)
+ [Ověřování](#auth)
+ [Definice rozhraní API.](#swagger)

Další informace o tom, jak pracovat s nastavením služby App Service najdete v tématu [nakonfigurovat nastavení aplikace služby Azure](../app-service/web-sites-configure.md).

### <a name="editor"></a>Editor služby aplikace

| | |
|-|-|
| ![Funkce aplikace služby App Service editor.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | Editor služby App Service je editor pokročilé v portálu, který můžete použít k úpravě JSON konfigurační soubory a soubory kódu agentem. Výběrem této možnosti se spustí na záložce prohlížeče samostatné s základní editor. To umožňuje integraci se službou úložiště Git, spuštění a ladění kódu a upravit nastavení funkce aplikace. Tohoto editoru poskytuje vylepšené vývojářské prostředí pro funkce v porovnání s výchozím okně funkce aplikace.    |

![Editor služby App Service](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="settings"></a>Nastavení aplikace

| | |
|-|-|
| ![Nastavení aplikace funkce aplikace.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | App Service **nastavení aplikace** okno je, kde můžete konfigurovat a spravovat framework verze, vzdálené ladění, nastavení aplikace a připojovací řetězce. Při integraci s jinými Azure a služby třetích stran, funkce aplikace, můžete upravit tato nastavení zde. |

![Konfigurace nastavení aplikace](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <a name="console"></a>Konzola

| | |
|-|-|
| ![Funkce aplikace konzoly na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | Konzole v portálu je nástroj pro ideální developer, když chcete spolupracovat s vaší aplikací funkce z příkazového řádku. Běžné příkazy zahrnují adresáře a vytváření souborů a navigace, a také provádění dávkové soubory a skripty. |

![Funkce aplikace konzoly](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Pokročilé nástroje (Kudu)

| | |
|-|-|
| ![Funkce aplikace Kudu na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | Rozšířené nástroje pro službu App Service (také označované jako Kudu) poskytují přístup k funkcím pro správu pokročilé funkce aplikace. Z modulu Kudu spravovat informace o systému, nastavení aplikace, proměnné prostředí, rozšíření lokality, hlaviček protokolu HTTP a proměnných serveru. Můžete také spustit **Kudu** procházením SCM koncový bod pro funkce aplikace, jako je třeba`https://<myfunctionapp>.scm.azurewebsites.net/` |

![Konfigurace modulu Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Možnosti nasazení

| | |
|-|-|
| ![Funkce Možnosti nasazení aplikace na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Funkce vám umožní vyvíjet funkce kódu na místním počítači. Potom můžete nahrát projektu místní funkce aplikace do Azure. Kromě tradičních odeslání na server FTP funkce umožňuje nasadit funkce aplikace pomocí Oblíbené průběžnou integraci řešení, jako jsou Githubu, služby VSTS, Dropbox, Bitbucket a dalších. Další informace najdete v tématu [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md). Pokud chcete nahrát ručně pomocí protokol FTP nebo místní Git, je také nutné [nakonfigurovat přihlašovací údaje nasazení](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![Funkce aplikace CORS na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Pokud chcete zabránit spuštění škodlivého kódu v službách, služby App Service blokuje volání funkce aplikací z externích zdrojů. Funkce podporuje prostředků mezi zdroji (CORS) a umožňují definovat "povolených" povolené zdroje, ze kterých může přijmout funkce vzdálené žádosti pro sdílení.  |

![Konfigurace funkce aplikace CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Ověřování

| | |
|-|-|
| ![Funkce ověřování aplikací na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | Když funkce používají aktivační procedury protokolu HTTP, můžete vyžadovat volání nejdřív ověřit. App Service podporuje ověřování Azure Active Directory a přihlaste se pomocí sociálních sítí, jako je Facebook, Microsoft a Twitter. Podrobnosti o konfiguraci zprostředkovatele konkrétní ověřování najdete v tématu [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md). |

![Konfigurovat ověřování pro aplikaci funkce](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>Definice rozhraní API.

| | |
|-|-|
| ![Funkce aplikace API swagger definice na portálu Azure](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Funkce podporuje Swagger tak, aby klienti snadno využívat funkce aktivované protokolem HTTP. Další informace týkající se vytváření se Swagger definice rozhraní API najdete v článku [začít pracovat s aplikacemi API a Swaggerem v Azure](../app-service/app-service-web-tutorial-rest-api.md). Funkce proxy lze také definovat jeden prostor rozhraní API pro víc funkcí. Další informace najdete v tématu [práce s Azure funkce proxy](functions-proxies.md). |

![Konfigurace funkce aplikace API](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Další kroky

+ [Konfigurace nastavení služby Azure App Service](../app-service/web-sites-configure.md)
+ [Průběžné nasazování se službou Azure Functions](functions-continuous-deployment.md)



