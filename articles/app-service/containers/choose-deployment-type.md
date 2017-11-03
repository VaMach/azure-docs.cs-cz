---
title: "Azure App Service na Linux nasazení – vlastní obrázek nebo integrovanou platformu?  | Dokumentace Microsoftu"
description: "Rozhodování mezi vlastní nasazení kontejner Docker a integrované aplikace framework pro službu App Service v systému Linux"
keywords: "služby Azure app service, webové aplikace, linux, operačních systémů"
services: app-service
documentationCenter: 
authors: nickwalk
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: nickwalk
ms.openlocfilehash: 4a04bba2375b5a107bc3cb8cdc1a75d037c50af6
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="custom-image-or-built-in-platform-image"></a>Vlastní obrázek nebo integrovanou platformu?

[Aplikační služby v systému Linux](app-service-linux-intro.md) nabízí dva různé cesty pro získávání aplikace publikování na webu:

- **Nasazení vlastní image**: "Dockerize" vaší aplikace do Docker bitové kopie, který obsahuje všechny soubory a závislosti v balíčku připravené k použití.
- **Nasazení aplikace s integrovanou platformu image**: Naše integrovanou platformu Image obsahují běžné moduly runtime webové aplikace a závislostí, jako je například uzel a PHP. Použijte jednu z [metody nasazení služby Azure App Service](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) nasazení vaší aplikace do webové aplikace úložiště, a pak použít bitovou kopii integrovanou platformu ji spustit.

Jakou metodu je pro vaši aplikaci? Jsou primární faktory vzít v úvahu:

- **Dostupnost Docker v pracovním postupu vývoj**: vývoj vlastní image vyžaduje základní znalosti o pracovní postup vývoje Docker. Nasazení vlastní image do webové aplikace vyžaduje publikace vlastní bitovou kopii k hostiteli úložiště jako úložiště Docker Hub. Pokud jste obeznámeni s Docker a Docker úlohy můžete přidat do pracovního postupu sestavení nebo pokud jsou již publikování aplikace jako obrázek na Docker, je nejlepší volbou skoro určitě vlastní image.
- **Požadavky na jedinečné runtime**: integrovanou platformu bitové kopie jsou navržené tak, aby vyhovovaly většina webových aplikací, ale jsou v jejich možnosti přizpůsobení omezeny. Aplikace může mít jedinečný závislostí nebo jiných runtime požadavků, které překračují, co jsou schopná využívat integrované bitové kopie.
- **Požadavky na sestavení**: S [průběžné nasazování](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), můžete získat aplikace spuštěná v Azure přímo ze zdrojového kódu. Není třeba žádné externího procesu sestavení nebo publikace. Existuje ale omezení možnosti přizpůsobení a dostupnosti nástroje sestavení v rámci [Kudu](https://github.com/projectkudu/kudu/wiki) modul nasazení. Aplikace může odrůst Kudu na možnosti růstem v jeho závislosti nebo požadavky na sestavení vlastní logiky.
- **Požadavky na čtení/zápis disku**: všechny webové aplikace se přidělují svazek úložiště pro webový obsah. Tento svazek založenou na Azure Storage je připojena k `/home` v systému souborů aplikace. Na rozdíl od soubory v kontejneru systému souborů soubory obsahu svazku jsou k dispozici ve všech instancích škálování aplikace a změny se zachová napříč restartování aplikace. Ale latence disku svazku obsahu je vyšší, a další proměnná než latence místní kontejneru systému souborů a přístup může být ovlivněno upgradů platformy, neplánované výpadky a problémy se síťovým připojením. Aplikace, které vyžadují velkou přístup jen pro čtení k soubory obsahu mohou využívat vlastní image nasazení, který nahradí soubory v systému souborů bitové kopie místo na svazku obsahu.
- **Sestavení využití prostředků**: když se aplikace nasadí ze zdroje, skripty nasazení spustit pomocí modulu Kudu stejných plán služby App Service výpočetních a úložných prostředků jako spuštěné aplikaci. Nasazení velkých aplikací může spotřebovat víc prostředků nebo dobu, než požadovaný. Konkrétně mnoho pracovních postupů nasazení generovat aktivitu velkou disku na svazku obsahu aplikace, která není optimalizována pro tyto aktivity. Vlastní image nabízí všechny soubory a závislosti vaší aplikace do Azure v jeden balíček se pro přenos dalších souborů nebo akce nasazení není nutné.
- **Pro rychlé iterace potřebovat**: Dockerizing aplikace vyžaduje další kroky. Změny se projeví musí push vaší novou bitovou kopii do úložiště při každé aktualizaci. Tyto aktualizace jsou pak vyžádány do prostředí Azure. Pokud jeden z předdefinovaných kontejnery vyhovovat potřebám vaší aplikace, může nasazení ze zdroje nabízejí rychlejší pracovní postup vývoje.