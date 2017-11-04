---
title: "Přehled vysokou dostupnost porty v Azure | Microsoft Docs"
description: "Další informace o na interní nástroj pro vyrovnávání zatížení porty vysokou dostupnost"
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: e72fc0d4323f7a2d203fee66311c3fea10ad7a09
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="high-availability-ports-overview-preview"></a>Přehled vysokou dostupnost porty (Preview)

Azure načíst vyrovnávání standardní zavádí novou schopnost načíst vyrovnávání na všechny porty TCP a UDP toky současně při použití interní Vyrovnávání zatížení. 

>[!NOTE]
> Vysoká dostupnost porty funkce je dostupná s standardní nástroje pro vyrovnávání zatížení a je momentálně ve verzi preview. Během období Preview tato funkce nemusí dosahovat stejné úrovně dostupnosti a spolehlivosti jako funkce, které jsou ve verzi všeobecné dostupnosti. Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Je nutné se přihlásit k vyrovnávání zatížení standardní Preview pro používání portů HA s prostředky standardní nástroje pro vyrovnávání zatížení. Postupujte podle pokynů pro registraci kromě Vyrovnávání zatížení [standardní Preview](https://aka.ms/lbpreview#preview-sign-up) také.

Pravidlo HA porty je varianta pravidlo nakonfigurované na vnitřní načíst vyrovnávání Standard Vyrovnávání zatížení.  Scénáře jsou zjednodušené zadáním jednoho LB pravidla pro vyrovnávání zatížení všechny TCP a UDP toky přicházejících na všech portech k interní nástroj pro vyrovnávání zatížení – standardní front-endu. Rozhodnutí Vyrovnávání zatížení se provádí na tok na pět-n-tice zdrojové IP adresy, zdrojového portu, cílové IP adresy, cílový Port a protokol.

HA porty umožňuje kritických scénářů, jako je vysoká dostupnost a škálování pro síť virtuálního zařízení (hodnocení chyb zabezpečení) uvnitř virtuální sítě, jakož i další scénáře, kde musí být velký počet portů skupině s vyrovnáváním zatížení. 

HA porty je nakonfigurovat pomocí nastavení front-endové a back-end porty na **0** a protokol pro **všechny**.  Interní nástroj pro vyrovnávání zatížení prostředků teď vyrovnává všechny toky TCP a UDP, bez ohledu na číslo portu.

## <a name="why-use-ha-ports"></a>Proč používat porty HA

### <a name="nva"></a>Virtuální síťová zařízení

Virtuální síťové zařízení (hodnocení chyb zabezpečení sítě) můžete použít pro zabezpečení vaše úloha Azure z více typů ohrožení zabezpečení. Pokud v těchto scénářích používají hodnocení chyb zabezpečení, musí být spolehlivé, vysoce dostupné a Škálováním na více systémů pro vyžádání.

Ve vašem scénáři můžete dosáhnout těchto cílů jednoduše přidáním hodnocení chyb zabezpečení instance do fondu back-end Azure interní služby Vyrovnávání zatížení a nakonfigurování pravidla HA porty pro vyrovnávání zatížení.

HA porty poskytují některé výhody pro scénáře HA hodnocení chyb zabezpečení sítě:
- rychlé převzetí služeb při selhání v pořádku instancí s na sondy stavu instance
- vyšší výkon se Škálováním na více systémů na n aktivní instance
- n aktivní a aktivní – pasivní scénáře
- povinnost komplexní řešení, jako jsou uzly Zookeeper pro monitorování zařízení

Následující příklad uvádí nasazení střed a paprsek virtuální sítě s koncových vynucené tunelování jejich provoz na virtuální síť rozbočovače a prostřednictvím hodnocení chyb zabezpečení před opuštěním důvěryhodné místa. NVAs jsou připojeni k interní zatížení vyrovnávání standardní s konfigurací HA porty.  Veškerý provoz může zpracovat a dál odpovídajícím způsobem. 

![ha porty příklad](./media/load-balancer-ha-ports-overview/nvaha.png)

Obrázek 1 – střed a paprsek virtuální síť s NVAs nasazené v režimu HA

Pokud používáte virtuální zařízení sítě, Zkontrolujte prosím u příslušného poskytovatele nejvhodnějším využití HA porty a jaké postupy se podporují.

### <a name="load-balancing-large-numbers-of-ports"></a>Velký počet portů Vyrovnávání zatížení

Můžete také použít HA porty pro scénáře aplikací, které vyžadují zatížení balanicng velkého počtu portů. Tyto scénáře můžete zjednodušit pomocí interní [standardní nástroje pro vyrovnávání zatížení](https://aka.ms/lbpreview) s HA porty kde jeden pravidlo Vyrovnávání zatížení nahrazuje více jednotlivých pravidel, jeden pro každou port Vyrovnávání zatížení.

## <a name="region-availability"></a>Dostupnost v oblastech

HA porty jsou k dispozici v [stejné oblasti jako standardní nástroje pro vyrovnávání zatížení](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Náhled registrace

K účasti ve verzi Preview funkci HA porty ve standardní nástroje pro vyrovnávání zatížení, zaregistrujte předplatné pro získání přístupu pomocí Azure CLI 2.0 nebo prostředí PowerShell.  Pomocí těchto tří kroků:

>[!NOTE]
>Abyste tuto funkci používat, musíte také registrace Nástroje pro vyrovnávání zatížení [standardní Preview](https://aka.ms/lbpreview#preview-sign-up) kromě HA porty. Registrace náhledy HA porty nebo standardní nástroje pro vyrovnávání zatížení může trvat až jednu hodinu.

### <a name="sign-up-using-azure-cli-20"></a>Zaregistrujte si pomocí Azure CLI 2.0

1. Zaregistrovat funkci s poskytovatelem
    ```cli
    az feature register --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
2. Předchozí operace může trvat až 10 minut.  Můžete zkontrolovat stav operace pomocí následujícího příkazu:

    ```cli
    az feature show --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
    Prosím přejděte ke kroku 3, když se stav registrace funkce vrátí "Registrovaná", jak je uvedeno níže:
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowILBAllPortsRule",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
3. Opakováním registrace předplatného s poskytovatelem prostředků dokončete registraci ve verzi preview:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    
### <a name="sign-up-using-powershell"></a>Zaregistrujte si pomocí prostředí PowerShell

1. Zaregistrovat funkci s poskytovatelem
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    
2. Předchozí operace může trvat až 10 minut.  Můžete zkontrolovat stav operace pomocí následujícího příkazu:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    Prosím přejděte ke kroku 3, když se stav registrace funkce vrátí "Registrovaná", jak je uvedeno níže:
   
    ```
    FeatureName          ProviderName      RegistrationState
    -----------          ------------      -----------------
    AllowILBAllPortsRule Microsoft.Network Registered
    ```
    
3. Opakováním registrace předplatného s poskytovatelem prostředků dokončete registraci ve verzi preview:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```


## <a name="limitations"></a>Omezení

Podporované konfigurace nebo výjimky pro porty HA jsou následující:

- Jedné front-endové konfigurace protokolu IP může mít jedno pravidlo Vyrovnávání zatížení DSR s HA porty, nebo může mít pravidlo služby load balancer jeden jiný DSR s HA porty. Nemůže mít obě.
- Na jednu konfiguraci protokolu IP rozhraní sítě může mít pouze jeden jiný DSR načíst pravidlo vyrovnávání se HA porty. Žádná další pravidla lze nastavit pro tento příkaz ipconfig.
- Na jednu konfiguraci protokolu IP rozhraní sítě může mít jeden nebo více pravidla nástroje pro vyrovnávání zatížení DSR s HA porty, za předpokladu, všechny jejich odpovídajících front-endové konfigurace protokolu IP, které jsou jedinečné.
- Pokud všechny služby Vyrovnávání zatížení pravidel, jsou HA porty (pouze DSR) nebo, všechna pravidla jsou jiný - HA porty (DSR a bez DSR), pravidla Vyrovnávání zatížení pro dva (nebo více) odkazující na stejné může fond back-end společně existovat. Dva takové pravidla vyrovnávání zátěže nemohou existovat společně, pokud je kombinace HA portů a jiných - HA pravidla.
- HA porty není k dispozici pro protokol IPv6.
- Tok symetrie pro scénáře hodnocení chyb zabezpečení sítě je podporováno pouze jednu síťovou kartu. Popis a diagram pro [síťových virtuálních zařízení](#nva). 



## <a name="next-steps"></a>Další kroky

- [Konfigurace portů HA na vnitřní zatížení vyrovnávání Standard](load-balancer-configure-ha-ports.md)
- [Další informace o standardní nástroje pro vyrovnávání zatížení preview](https://aka.ms/lbpreview)

