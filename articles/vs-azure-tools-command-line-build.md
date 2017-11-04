---
title: "Sestavení příkazového řádku Azure | Microsoft Docs"
description: "Sestavení příkazového řádku pro Azure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.openlocfilehash: 5fe910e2757dd5ec783538e23e7f52e2f5725b39
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="building-azure-projects-from-the-command-line"></a>Sestavení projektů Azure z příkazového řádku
Microsoft Build Engine (MSBuild) můžete vytvořit produkty v sestavení testovacích prostředích, kde není nainstalovaná sada Visual Studio. MSBuild používá formátu XML pro soubory projektu, které je rozšiřitelný a plně podporované společností Microsoft. Pomocí nástroje MSBuild formát souboru, můžete popisují, co položky musí být vytvořené pro jednu nebo více platforem a konfigurace.

Můžete také spouštět nástroje MSBuild v příkazovém řádku a toto téma popisuje tento přístup. Nastavením vlastnosti na příkazovém řádku, můžete vytvořit konkrétní konfigurace projektu. Podobně můžete také definovat cíle, které MSBuild sestavení. Další informace o parametry příkazového řádku a nástroje MSBuild najdete v tématu [Reference k příkazovému řádku MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>Parametry nástroje MSBuild
Nejjednodušší způsob, jak vytvořit balíček je spuštění MSBuild s parametrem `/t:Publish` možnost. Ve výchozím nastavení, tento příkaz vytvoří adresář ve vztahu k kořenové složce projektu, například `<ProjectDirectory>\bin\Configuration\app.publish\`. Při sestavování projektu Azure jsou generovány dva soubory: soubor balíčku samostatně a doprovodné konfigurační soubor:

* Soubor balíčku (`project.cspkg`)
* Konfigurační soubor (`ServiceConfiguration.TargetProfile.cscfg`)

Každý projekt, Azure ve výchozím nastavení, obsahuje jeden soubor konfigurace služby pro místní sestavení (ladění) a druhou pro sestavení cloudu (pracovním nebo produkčním). Můžete ale přidat nebo odebrat soubory konfigurace služby podle potřeby. Když vytvoříte balíček Visual Studia, zobrazí se výzva které souboru konfigurace služby společně se balíček. Když vytvoříte balíček pomocí nástroje MSBuild, místní služby konfigurační soubor je zahrnuté ve výchozím nastavení. Chcete-li zahrnout jiný soubor konfigurace služby, nastavte `TargetProfile` vlastnosti nástroje MSBuild příkazu (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Pokud chcete používat adresář alternativní uložené balíčku a konfigurační soubory, nastavte cestu pomocí `/p:PublishDir=Directory\` možnost, včetně koncové oddělovače zpětné lomítko.

## <a name="next-steps"></a>Další kroky
Jakmile je balíček vytvořen, můžete ho nasadit do Azure. Kurz, který ukazuje, jak k automatizaci tohoto procesu, najdete v části [nastavené průběžné doručování pro cloudové služby v Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).

