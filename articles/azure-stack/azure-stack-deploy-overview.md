---
title: "Azure rychlý start zásobníku Development Kit nasazení | Microsoft Docs"
description: "Zjistěte, jak nasadit Azure zásobníku Development Kit"
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/10/2017
ms.author: erikje
ms.custom: mvc
ms.openlocfilehash: 04742a587284cf72632360a9575a63a576da36e8
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="azure-stack-development-kit-deployment-quickstart"></a>Rychlý úvod k nasazení vývojové sady pro Azure Stack

*Platí pro: Azure zásobníku Development Kit*

[Azure zásobníku Development Kit](azure-stack-poc.md) je testovací a vývojové prostředí, které můžete nasadit k vyhodnocení a předvedení funkcí Azure zásobníku a služby. Se dá stáhnout spuštěná, musíte připravit prostředí hardwaru a spustit některé skripty (bude to trvat i několik hodin). Potom můžete se přihlásit na portály správců a uživatelů ke správě Azure zásobníku a otestovat nabídky. 

1. [**Plánování hardwaru, softwaru a síti**](azure-stack-deploy.md). Počítač, který je hostitelem development kit (hostitel development kit) musí splňovat požadavky na hardware, software a síť. Také musí zvolit pomocí Azure Active Directory nebo Active Directory Federation Services. Ujistěte se, že jste před spuštěním nasazení tak, aby proces instalace plynule v souladu se tyto požadavky. 

2. [**Stažení a extrakci balíčku pro nasazení**](azure-stack-run-powershell-script.md#download-and-extract-the-development-kit). Balíček pro nasazení můžete stáhnout na development kit hostitele nebo do jiného počítače. Soubory extrahované nasazení trvat až 60 GB volného místa na disku, aby pomocí jiného počítače může pomoct snížit požadavky na hardware pro hostitele development kit.

3. [**Příprava hostitele development kit** ](azure-stack-run-powershell-script.md) pomocí Instalační služby. Po provedení tohoto kroku se development kit hostitele spustí Cloudbuilder.vhdx (virtuálního pevného disku, který obsahuje spouštěcí operačního systému a zásobník Azure instalaci souborů).

4. [**Nasazení development kit** ](azure-stack-run-powershell-script.md) na hostiteli development kit.

5. Pokud vaše nasazení zásobník Azure používá Azure Active Directory, je nutné [zaregistrovat zásobník Azure s Azure](azure-stack-register.md) , aby bylo možné [stažení položky Azure marketplace](azure-stack-download-azure-marketplace-item.md) do protokolů Azure.

Po dokončení těchto kroků, budete mít vývojovém prostředí sady s portály pro správce i uživatele. Dále můžete [připojit a přihlásit](azure-stack-connect-azure-stack.md) na portál. Potom můžete spustit nasazení poskytovatelů prostředků, vytváření [nabízí](azure-stack-key-features.md#regions-services-plans-offers-and-subscriptions)a naplnění zásobník Azure [marketplace](azure-stack-marketplace.md).
