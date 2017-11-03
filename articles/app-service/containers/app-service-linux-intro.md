---
title: "Úvod do služby App Service v systému Linux | Microsoft Docs"
description: "Další informace o Azure App Service v systému Linux."
keywords: "služby Azure app service, linux, operačních systémů"
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/16/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 89cb7dc488da42724f212d13f8550064ff8b9188
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Úvod do Azure App Service v systému Linux

[Webová aplikace](../app-service-web-overview.md) je plně spravovaná výpočetní platforma, která je optimalizována pro hostování webů a webových aplikací. Zákazníci mohou pomocí služby App Service v systému Linux do hostitele webové aplikace v systému Linux nativně pro zásobníky podporovaných aplikací. V následujících částech jsou uvedené zásobníky aplikace, které jsou aktuálně podporovány.

## <a name="languages"></a>Jazyky

Aplikační služby v systému Linux podporuje několik předdefinovaných obrázků zvýšení produktivity. Pokud vaše aplikace vyžaduje modul runtime není podporován v předdefinované bitové kopie, jsou k dispozici pokyny o tom, jak [sestavení vlastní image Docker](tutorial-custom-docker-image.md) k nasazení do webové aplikace pro kontejnery.

| Jazyk | Podporované verze |
|---|---|
| Node.js | 4.4, 4.5, 6.2, 6.6, 6.9-6.11, 8.0, 8.1 |
| PHP | 5.6, 7.0 |
| .NET Core | 1.0, 1.1 |
| Ruby | 2.3 |

## <a name="deployments"></a>Nasazení

* FTP
* Místní Git
* GitHubu
* Bitbucket

## <a name="devops"></a>DevOps

* Přípravná prostředí
* [Kontejner Azure registru](https://docs.microsoft.com/azure/container-registry/container-registry-intro) a DockerHub CI/CD

## <a name="console-publishing-and-debugging"></a>Konzola, publikování a ladění

* Prostředí
* Nasazení
* Základní konzoly
* SSH

## <a name="scaling"></a>Škálování

* Zákazníci mohou nahoru a dolů škálování webové aplikace tak, že změníte úroveň služby jejich [plán služby App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview?toc=%2fazure%2fapp-service-web%2ftoc.json)

## <a name="locations"></a>Umístění

Zkontrolujte [řídicí panel Azure stav](https://azure.microsoft.com/status).

## <a name="limitations"></a>Omezení

Portál Azure zobrazuje pouze funkce, které aktuálně fungují pro webovou aplikaci pro kontejnery. Jak jsme povolit další funkce, se bude zobrazovat na portálu.

Některé funkce, například integrace virtuální sítě, ověřování Azure Active Directory nebo třetích stran nebo rozšíření lokality Kudu, ještě nejsou k dispozici. Jakmile se tyto funkce jsou k dispozici, budeme aktualizovat naší dokumentaci a blog o změnách.

Aplikační služby v systému Linux je podporována pouze s [Basic a Standard](https://azure.microsoft.com/pricing/details/app-service/plans/) plány služby app service a nemá [volné nebo sdílené](https://azure.microsoft.com/pricing/details/app-service/plans/) vrstvy. Toto jsou také důležité omezení pro službu App Service v systému Linux:

* Nelze vytvořit webovou aplikaci pro kontejnery v plán služby App Service již hostitelem jiných Linux webové aplikace.
* Při vytváření webové aplikace pro kontejnery ve skupině prostředků obsahující bez Linux webové aplikace, musíte vytvořit plán služby App Service v jiné oblasti než existující plán služby App Service.

## <a name="troubleshooting"></a>Řešení potíží

Pokud vaše aplikace se nepodaří spustit nebo chcete provést kontrolu protokolování z vaší aplikace, zkontrolujte, že že docker protokolů v adresáři LogFiles. Buď prostřednictvím webu SCM nebo FTP, můžete přístup k tomuto adresáři.
Do protokolu `stdout` a `stderr` z kontejneru, je nutné povolit **kontejner Docker protokolování** pod **protokolů diagnostiky**.

![Povolení protokolování][2]

![Pokud chcete zobrazit protokoly Docker pomocí modulu Kudu][1]

Můžete získat přístup k webu SCM z **Rozšířené nástroje** v **nástroje pro vývoj** nabídky.

## <a name="next-steps"></a>Další kroky

V následujících tématech začít pracovat s App Service v systému Linux. Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Jak používat vlastní image Docker pro webovou aplikaci pro kontejnery](quickstart-custom-docker-image.md)
* [Pomocí .NET Core v Azure App Service v systému Linux](quickstart-dotnetcore.md)
* [Pomocí Ruby v Azure App Service v systému Linux](quickstart-ruby.md)
* [Webové aplikace Azure App Service pro kontejnery – nejčastější dotazy](app-service-linux-faq.md)
* [Podpora SSH pro Azure App Service v systému Linux](app-service-linux-ssh-support.md)
* [Nastavení přípravných prostředí v Azure App Service](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Docker Hub průběžné nasazování pomocí webové aplikace pro kontejnery](./app-service-linux-ci-cd.md)

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
