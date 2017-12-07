---
title: "Možnosti výpočtů Azure – cloudové služby | Microsoft Docs"
description: "Další informace o Azure výpočetní hostování možnosti a funkce: služby App Service, cloudové služby a virtuální počítače"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 17ecf39128994dad93f017f87f105254f3017230
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="should-i-choose-cloud-services-or-something-else"></a>Mám zvolit cloudové služby, nebo něco jiného?
Můžete je volba Azure Cloud Services? Azure poskytuje různé hostování modely pro spouštění aplikací. Každé z nich poskytuje jinou sadu služeb, kterého vystavitele si zvolíte, závisí na přesně co se pokoušíte provést.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-cloud-services"></a>Informace o cloudové služby
Cloudové služby je příkladem [platforma jako služba](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Jako [služby App Service](../app-service/app-service-web-overview.md), tato technologie je navržen pro podporu aplikace, které jsou škálovatelný, spolehlivý a levných pracovat. Stejně jako App Service je hostovaná na virtuálních počítačích, takže příliš jsou cloudové služby, ale máte větší kontrolu nad virtuálními počítači. Vlastní software můžete nainstalovat na virtuální počítače cloudové služby a můžete do nich vzdálené.

![cs_diagram](./media/cloud-services-choose-me/diagram.png)

Další ovládací prvek také znamená menší snadné použití. Pokud budete potřebovat možnosti Další ovládací prvek, je obvykle rychlejší a snadnější ke zprovoznění webové aplikace a spuštěna ve službě Web Apps ve službě App Service ve srovnání s cloudové služby.

Existují dva typy rolí cloudové služby. Jediným rozdílem mezi těmito dvěma je, jak se vaše role hostuje na virtuálním počítači.

* **Webová role**  
Automaticky nasadí a je hostitelem vaší aplikace pomocí služby IIS.

* **Role pracovního procesu**  
Nepoužívá službu IIS a spustí vaší samostatné aplikace.

Například jednoduchou aplikaci může použít pouze jeden webové role, obsluhující webu. Složitější aplikaci může zpracovávat příchozí požadavky od uživatelů a pak předejte tyto požadavky na roli pracovního procesu pro zpracování použít webové role. (Tato komunikace může používat [Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) nebo [front Azure](../storage/common/storage-introduction.md).)

Jak na předchozím obrázku naznačuje, všechny virtuální počítače v jedné aplikaci spustit v rámci stejné cloudové služby. Přístup uživatelů aplikace prostřednictvím jednu veřejnou IP adresu, s požadavky automaticky načíst rovnoměrně rozdělen mezi virtuálními počítači aplikace. Platforma [Škáluje a nasadí](cloud-services-how-to-scale-portal.md) virtuální počítače v cloudové služby aplikaci způsobem, který zabraňuje jediný bod selhání hardwaru.

I když aplikace spustit ve virtuálních počítačích, je důležité si uvědomit, že cloudové služby poskytuje PaaS, není IaaS. Tady je jedním ze způsobů myslíte o něm: V modelu IaaS, jako je například virtuální počítače Azure, nejprve vytvořit a nakonfigurovat vaše aplikace běží v prostředí, pak nasazení aplikace do tohoto prostředí. Jste zodpovědní za správu velkou část této world plnění úkolů, jako je nasazení nové opravou verze operačního systému v jednotlivých virtuálních počítačů. V PaaS naopak je jako prostředí již existuje. Všechny, které musíte udělat, je nasazení aplikace. Správa platformy, na které běží na, včetně nasazení nové verze operačního systému, je zajistit sami.

## <a name="scaling-and-management"></a>Škálování a Správa
S cloudovými službami nevytvářejte virtuálních počítačů. Místo toho zadat konfigurační soubor informuje Azure, kolik jednotlivých chcete, jako například **tři instance webových rolí** a **dvě instance role pracovního procesu**, a platformy je pro vás vytvoří.  Stále zvolíte [jakou velikost](cloud-services-sizes-specs.md) by měla být ty zálohování virtuálních počítačů, ale nevytvoříte explicitně je sami. Pokud aplikace potřebuje ke zpracování větší zatížení, můžete požádat o další virtuální počítače a Azure vytvoří těchto instancí. Pokud snížení zatížení je možné vypnout těchto instancí a zastavit platícího pro ně.

Cloudové služby aplikace obvykle je k dispozici uživatelům přes ve dvou krocích. Vývojář první [nahrávání aplikace](cloud-services-how-to-create-deploy-portal.md) do pracovní oblasti platformu. Když vývojář je připraven k zpřístupnění aplikace za provozu, použijí portál Azure se Prohodit pracovní s produkčním. To [přepínat mezi pracovní a provozní](cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production) lze provést bez výpadků, která umožní spuštěné aplikace upgradovat na novou verzi bez narušení svým uživatelům.

## <a name="monitoring"></a>Monitorování
Cloudové služby poskytuje také monitorování. Jako virtuální počítače Azure, zjistí selhání fyzického serveru a restartuje virtuální počítače, které byly spuštěné na daném serveru na nový počítač. Ale cloudové služby také zjistí selhání virtuální počítače a aplikace, ne jenom selhání hardwaru. Na rozdíl od virtuálních počítačů má agenta v každé webové a pracovní role, a proto je možné spustit nové virtuální počítače a instance aplikací, když dojde k selhání.

PaaS povaha cloudové služby má jiné důsledky příliš. Jedním z nejdůležitějších je, že aplikace založené na tuto technologii zasílány správně spustit, když všechny instance role web nebo pracovní proces se nezdaří. Toho dosáhnete tak, by neměl cloudové služby aplikace Udržovat stav v systému souborů svůj vlastní virtuální počítače. Na rozdíl od virtuální počítače vytvořené pomocí Azure Virtual Machines nejsou trvalé; zápisy provedené cloudové služby virtuálních počítačů není nic jako datový disk virtuálního počítače. Místo toho aplikace cloudové služby by měl explicitně zapsat všechny stav do databáze SQL, objekty BLOB, tabulek nebo jiné externí úložiště. Vytváření aplikací s tímto způsobem je umožňuje snazší škálování a odolnější proti selhání, jak důležité cíle cloudové služby.

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace cloudové služby v rozhraní .NET](cloud-services-dotnet-get-started.md)  
[Vytvoření aplikace cloudové služby v Node.js](cloud-services-nodejs-develop-deploy-app.md)  
[Vytvoření cloudové služby aplikace v jazyce PHP](../cloud-services-php-create-web-role.md)  
[Vytvoření aplikace cloudové služby v Pythonu](cloud-services-python-ptvs.md)

