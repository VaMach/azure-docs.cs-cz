---
title: "Vytvoření externího Azure App Service environment"
description: "Vysvětluje, jak k vytvoření prostředí služby App Service při vytváření aplikace nebo samostatné"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 94dd0222-b960-469c-85da-7fcb98654241
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 70c43b25aea364d7254137b46af31f851dcf8bc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-external-app-service-environment"></a>Vytvoření externího App Service environment #

Azure App Service Environment je nasazení služby Azure App Service do používanou podsíť virtuální sítě Azure (VNet). Existují dva způsoby, jak nasadit služby App Service environment (App Service Environment):

- V VIP na externí IP adresu často označuje jako externí App Service Environment.
- S virtuální IP adresu na interní IP adresu často říká App Service Environment ILB protože vnitřní koncový bod je vyrovnávání interní zatížení (ILB).

Tento článek ukazuje, jak vytvořit externí App Service Environment. Přehled App Service Environment, najdete v části [Úvod do služby App Service Environment][Intro]. Informace o tom, jak vytvořit App Service Environment ILB najdete v tématu [vytvoření a použití App Service Environment ILB][MakeILBASE].

## <a name="before-you-create-your-ase"></a>Před vytvořením vaší App Service Environment ##

Po vytvoření vaší App Service Environment, nelze změnit následující:

- Umístění
- Předplatné
- Skupina prostředků
- Virtuální síť použít
- Podsítě používané
- Velikost podsítě

> [!NOTE]
> Vyberete virtuální síť a zadejte podsíť, ujistěte se, že je dostatečně velký pro přizpůsobení budoucímu růstu. Doporučujeme velikost `/25` adresy 128.
>

## <a name="three-ways-to-create-an-ase"></a>Tři způsoby, jak vytvořit App Service Environment ##

Existují tři způsoby, jak vytvořit App Service Environment:

- **Při vytváření plán služby App Service**. Tato metoda vytvoří App Service Environment a plán služby App Service v jednom kroku.
- **Jako samostatné akce**. Tato metoda vytváří samostatný App Service Environment, což je App Service Environment bez obsahu. Tato metoda je pokročilejší procesu vytvořit App Service Environment. Použít k vytvoření App Service Environment s ILB.
- **Z šablony Azure Resource Manager**. Tato metoda je pro zkušené uživatele. Další informace najdete v tématu [ze šablony vytvořit App Service Environment][MakeASEfromTemplate].

Externí App Service Environment má veřejné VIP, což znamená, že všechny přenosy HTTP/HTTPS k aplikacím v App Service Environment dotkne přístupné z Internetu IP adresu. App Service Environment s ILB, obsahuje IP adresu z podsítě používané App Service Environment. Aplikace hostované v App Service Environment ILB nejsou zveřejněné přímo k Internetu.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Vytvořit App Service Environment a plán služby App Service společně ##

Plán služby App Service je kontejner aplikací. Když vytvoříte aplikaci v App Service, zvolte nebo vytvořte plán služby App Service. V prostředí, v kontejneru modelu podržte plány služby App Service a plány služby App Service uchování aplikace.

Vytvořit App Service Environment, když vytvoříte plán služby App Service:

1. V [portál Azure](https://portal.azure.com/), vyberte **nový** > **Web + mobilní** > **webové aplikace**.

    ![Vytvoření webové aplikace][1]

2. Vyberte své předplatné. Aplikace a App Service Environment se vytvoří ve stejné předplatných.

3. Vyberte nebo vytvořte skupinu prostředků. Pomocí skupin prostředků můžete spravovat souvisejících prostředků Azure jako jednotku. Skupiny prostředků také jsou užitečné, když vytvoříte pravidla řízení přístupu na základě rolí pro vaše aplikace. Další informace najdete v tématu [přehled Azure Resource Manageru][ARMOverview].

4. Vyberte plán služby App Service a pak vyberte **vytvořit nový**.

    ![Nový plán aplikační služby][2]

5. V **umístění** rozevíracího seznamu vyberte oblast, kde chcete vytvořit App Service Environment. Pokud vyberete existující App Service Environment, se nevytvoří nové App Service Environment. Plán služby App Service je vytvořen v App Service Environment, kterou jste vybrali. 

6. Vyberte **cenová úroveň**a vyberte jednu z **izolovaná** ceny SKU. Pokud se rozhodnete **izolovaná** karty SKU a umístění, které není App Service Environment, nové App Service Environment se vytvoří v tomto umístění. Chcete-li spustit proces vytvoření App Service Environment, vyberte **vyberte**. **Izolovaná** SKU je k dispozici pouze ve spojení s App Service Environment. Také nemůžete použít jiné cenové SKU App Service Environment jiné než **izolovaná**.

    ![Výběr cenové úrovně][3]

7. Zadejte název pro vaše App Service Environment. Tento název se používá v názvu adresovatelné pro vaše aplikace. Pokud je název App Service Environment _appsvcenvdemo_, je název domény *. appsvcenvdemo.p.azurewebsites.net*. Pokud vytvoříte aplikaci s názvem *mytestapp*, je adresovatelné v mytestapp.appsvcenvdemo.p.azurewebsites.net. Prázdné znaky nelze použít v názvu. Pokud používáte velká písmena, název domény je celkový počet malých verzi tímto názvem.

    ![Nový název plánu aplikační služby][4]

8. Zadejte vaše Azure podrobnosti virtuální sítě. Vyberte buď **vytvořit nový** nebo **vyberte existující**. Vyberte stávající virtuální síť možnost je dostupná pouze v případě, že máte virtuální síť ve vybrané oblasti. Pokud vyberete **vytvořit nový**, zadejte název virtuální sítě. Vytvoří se nové virtuální sítě Resource Manageru s tímto názvem. Používá adresní prostor `192.168.250.0/23` ve vybrané oblasti. Pokud vyberete **vybrat existující**, budete muset:

    a. Blok adres virtuální sítě, vyberte, pokud máte více než jednou.

    b. Zadejte nový název podsítě.

    c. Vyberte velikost podsítě. *Mějte na paměti, vyberte velikost dostatečně velký pro přizpůsobení budoucímu růstu vašeho App Service Environment.* Doporučujeme, abyste `/25`, která je 128 adresy a dokáže zpracovat App Service Environment maximální velikost. Nedoporučujeme `/28`, například, protože nejsou k dispozici pouze 16 adres. Infrastruktura používá alespoň pět adres. V `/28` podsíť, jste zbývajících s maximální škálování 11 instancí.

    d. Vyberte rozsah IP adres podsítě.

9. Vyberte **vytvořit** vytvořit App Service Environment. Tento proces také vytvoří plán aplikační služby a aplikace. App Service Environment, plán služby App Service a aplikace jsou všechny v rámci stejného předplatného a také ve stejné skupině prostředků. Pokud vaše App Service Environment musí skupinu samostatné prostředků nebo pokud potřebujete App Service Environment ILB, postupujte podle kroků pro vytvoření App Service Environment samostatně.

## <a name="create-an-ase-by-itself"></a>Vytvořit App Service Environment samostatně ##

Pokud vytvoříte samostatné App Service Environment, má nic v ní. Prázdný App Service Environment stále způsobuje měsíčních poplatků infrastruktury. Postupujte podle těchto kroků k vytvoření App Service Environment s ILB nebo vytvořit App Service Environment ve vlastní skupině prostředků. Po vytvoření vaší App Service Environment, se můžou vytvářet aplikace pomocí běžných postupů. Vyberte vaše nové App Service Environment jako umístění.

1. Hledání v Azure Marketplace pro **App Service Environment**, nebo vyberte **nový** > **mobilní webové** > **App Service Environment**. 

2. Zadejte název vaší App Service Environment. Tento název se používá pro aplikace vytvořené v App Service Environment. Pokud je název *mynewdemoase*, je název subdomény *. mynewdemoase.p.azurewebsites.net*. Pokud vytvoříte aplikaci s názvem *mytestapp*, je adresovatelné v mytestapp.mynewdemoase.p.azurewebsites.net. Prázdné znaky nelze použít v názvu. Pokud používáte velká písmena, název domény je celkový počet malých verzi název. Pokud používáte ILB, název App Service Environment není používáno ve vašem subdomény, ale je místo výslovně uvedeno během vytváření App Service Environment.

    ![Pojmenování App Service Environment][5]

3. Vyberte své předplatné. Toto předplatné je také ten, který použít všechny aplikace app Service Environment. Nelze vložit vaše App Service Environment ve virtuální síti, který je v jiné předplatné.

4. Vyberte nebo zadejte novou skupinu prostředků. Skupina prostředků pro vaše App Service Environment musí být stejný jako ten, který se používá pro vaši virtuální síť. Pokud vyberete stávající virtuální síť, výběr skupiny prostředků pro vaše App Service Environment je aktualizována tak, aby odrážela u vaší virtuální sítě. *Můžete vytvořit App Service Environment se skupinou prostředků, které se liší od skupiny prostředků virtuální sítě, pokud používáte šablonu Resource Manager.* Vytvořit App Service Environment ze šablony, přečtěte si článek [vytvoření služby App Service environment ze šablony][MakeASEfromTemplate].

    ![Výběr skupiny prostředků][6]

5. Vyberte virtuální síť a umístění. Můžete vytvořit novou virtuální síť nebo vybrat stávající virtuální síť: 

    * Pokud vyberete novou virtuální síť, můžete zadat název a umístění. Nové virtuální sítě má 192.168.250.0/23 rozsah adres a podsíť s názvem výchozí. Podsíť je definován jako 192.168.250.0/24. Můžete vybrat pouze virtuální sítě Resource Manageru. **VIP typ** výběr určuje, zda vaše App Service Environment jsou přímo přístupné z Internetu (externí) nebo, pokud používá ILB. Další informace o těchto možnostech najdete v tématu [vytvoření a použití vyrovnávání zatížení interní služby App Service environment][MakeILBASE]. 

      * Pokud vyberete **externí** pro **VIP typ**, můžete vybrat kolik externí IP adresy v systému je vytvořena s pro účely založená na protokolu IP. 
    
      * Pokud vyberete **interní** pro **VIP typ**, musíte zadat doménu, která používá vaše App Service Environment. App Service Environment můžete nasadit do virtuální sítě využívající veřejného nebo soukromého adresního rozsahy. Chcete-li používat virtuální síť s veřejnou adresu rozsahu, vytvořte síť VNet dopředu. 
    
    * Pokud vyberete stávající virtuální síť, se vytvoří novou podsíť, když je vytvořen App Service Environment. *Podsíť předem vytvořené nelze použít na portálu. Pokud používáte šablonu Resource Manager, můžete vytvořit App Service Environment s existující podsítí.* Vytvořit App Service Environment ze šablony, přečtěte si článek [vytvoření služby App Service Environment ze šablony][MakeASEfromTemplate].

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##

Přesto můžete vytvořit instance první verze součásti služby App Service Environment (ASEv1). Tento proces zahájíte vyhledávání na webu Marketplace pro **App Service Environment v1**. Můžete vytvořit App Service Environment stejným způsobem, který vytvoříte samostatné App Service Environment. Po dokončení, vaše ASEv1 má dva elementy end front a dvě pracovních procesů. S ASEv1 musí spravovat front-end a pracovních procesů. Budou se automaticky přidá, není při vytváření plánu služby App Service. Skončení front fungovat jako koncové body protokolu HTTP nebo HTTPS a odesílat provoz zaměstnancům. Zaměstnanci jsou role, které umožňuje hostování vašich aplikací. Po vytvoření vaší App Service Environment, můžete upravit množství front-end a pracovních procesů. 

Další informace o ASEv1 najdete v tématu [Úvod do služby App Service Environment v1][ASEv1Intro]. Další informace o škálování, správu a sledování ASEv1, najdete v části [postup konfigurace služby App Service Environment][ConfigureASEv1].

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png



<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
