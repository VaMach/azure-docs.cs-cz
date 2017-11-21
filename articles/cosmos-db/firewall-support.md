---
title: "Řízení přístupu Azure Cosmos DB podpora brány firewall & IP | Microsoft Docs"
description: "Naučte se používat zásady řízení přístupu IP pro podporu brány firewall na účty databáze Azure Cosmos DB."
keywords: "Řízení přístupu IP, podporu brány firewall"
services: cosmos-db
author: shahankur11
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: c1b9ede0-ed93-411a-ac9a-62c113a8e887
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2017
ms.author: ankshah
ms.openlocfilehash: 1ceaa834ff68d5dca4abce561f9185e89af582af
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/20/2017
---
# <a name="azure-cosmos-db-firewall-support"></a>Podpora brány firewall Azure Cosmos DB
Zabezpečení dat uložených v databázi účet Azure Cosmos DB, Azure Cosmos DB poskytl podporu pro tajný klíč na základě [modelu autorizace](https://msdn.microsoft.com/library/azure/dn783368.aspx) , využívá ověřovací kód silné Hash-based zprávy (HMAC). Teď kromě tajný autorizace založené na modelu Azure Cosmos DB podporuje zásady řízené řízení přístupu na základě IP pro podporu brány firewall pro příchozí. Tento model je podobná pravidla brány firewall systému tradiční databází a poskytuje další úroveň zabezpečení pro databázový účet Azure Cosmos DB. Pomocí tohoto modelu teď můžete konfigurovat účet Azure Cosmos DB databáze tak, aby byla přístupná jenom z schválené sadu počítačů nebo cloudových služeb. Přístup k prostředkům Azure Cosmos DB z tyto schválené sady počítačů a služeb stále vyžadují volajícího, aby k dispozici platný autorizační token.

## <a name="ip-access-control-overview"></a>Přehled řízení přístupu IP
Ve výchozím nastavení je přístupné z veřejného Internetu účet Azure Cosmos DB databáze, tak dlouho, dokud žádosti je přiložena token platný autorizace. Konfigurace řízení přístupu na základě zásad IP, uživatel musí poskytnout sadu IP adresy nebo rozsahy IP adres ve formátu CIDR zahrnuty jako seznam povolených IP adresy klienta pro účet danou databázi. Jakmile tato konfigurace se použije, všechny požadavky z počítače mimo tento seznam povolených se zablokuje serverem.  Následující diagram popisuje připojení zpracování toku řízení přístupu na základě IP:

![Diagram zobrazující proces připojení u řízení přístupu na základě IP](./media/firewall-support/firewall-support-flow.png)

## <a name="connections-from-cloud-services"></a>Připojení z cloudové služby
Cloudové služby v Azure, jsou běžné způsob, jak hostování logikou střední úrovně služby pomocí Azure Cosmos DB. Pokud chcete umožnit přístup k účtu Azure Cosmos DB databáze z cloudové služby, veřejnou IP adresu cloudové služby musíte přidat do seznamu povolených IP adres, které jsou spojené s vaším účtem Azure Cosmos DB databáze pomocí [konfigurace zásad řízení přístupu IP](#configure-ip-policy).  To zajistí, že všechny instance rolí, služeb cloud mít přístup ke svému účtu databáze Azure Cosmos DB. IP adresy můžete načíst pro vaše cloudové služby na portálu Azure, jak je znázorněno na následujícím snímku obrazovky:

![Snímek obrazovky s veřejnou IP adresu pro cloudové služby zobrazí na portálu Azure](./media/firewall-support/public-ip-addresses.png)

Při škálování cloudové služby tak, že přidáte další role instance tyto nové instance automaticky mít přístup k účtu Azure Cosmos DB databáze, protože jsou součástí stejné cloudové služby.

## <a name="connections-from-virtual-machines"></a>Připojení z virtuálních počítačů
[Virtuální počítače](https://azure.microsoft.com/services/virtual-machines/) nebo [sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) lze také použít k hostování střední vrstvy služeb pomocí Azure Cosmos DB.  Jak nakonfigurovat účet Azure Cosmos DB databáze pro povolení přístupu z virtuálních počítačů, veřejné IP adresy virtuálního počítače nebo škálovací sadu virtuálních počítačů musí být nakonfigurován jako jednu z povolených IP adres pro váš účet Azure Cosmos DB databáze pomocí [konfigurace zásad řízení přístupu IP](#configure-ip-policy). IP adresy pro virtuální počítače na portálu Azure můžete načíst, jak je znázorněno na následujícím snímku obrazovky.

![Snímek obrazovky s veřejnou IP adresu pro virtuální počítač, který se zobrazí na portálu Azure](./media/firewall-support/public-ip-addresses-dns.png)

Když přidáte instance dalších virtuálních počítačů do skupiny, jsou automaticky k dispozici přístup ke svému účtu databáze Azure Cosmos DB.

## <a name="connections-from-the-internet"></a>Připojení z Internetu
Při přístupu k účtu Azure Cosmos DB databáze z počítače na Internetu, musí být IP adresa klienta nebo rozsah IP adres počítače přidány do seznamu povolených IP adresy pro účet Azure Cosmos DB databáze. 

## <a name="connections-from-azure-paas-service"></a>Připojení z Azure PaaS služby 
V Azure, PaaS služby, jako je Azure Stream analytics, Azure Functions se používají ve spojení s Azure Cosmos DB. Povolit přístup k účtu Azure Cosmos DB databáze ze tyto druh services, jehož IP adresa není snadno dostupné, musí být IP adresa 0.0.0.0 přidá do seznamu povolených IP adres, které jsou spojené s vaším účtem Azure Cosmos DB databáze pomocí [konfigurace zásad řízení přístupu IP](#configure-ip-policy).  To zajišťuje, aby služby Azure PaaS můžete přístup k účtu Azure Cosmos DB, který má toto pravidlo. 

 ## <a id="configure-ip-policy"></a>Konfigurace zásad řízení přístupu IP
Zásady řízení přístupu IP lze nastavit na portálu Azure nebo prostřednictvím kódu programu prostřednictvím [rozhraní příkazového řádku Azure](cli-samples.md), [prostředí Azure Powershell](powershell-samples.md), nebo [REST API](/rest/api/documentdb/) aktualizací `ipRangeFilter` vlastnost. IP adresy a rozsahy musí být oddělené čárkami a nesmí obsahovat žádné mezery. Příklad: "13.91.6.132,13.91.6.1/24". Při aktualizaci databázový účet prostřednictvím těchto metod, ujistěte se, že jste naplnit všechny vlastnosti, aby se zabránilo obnovení výchozího nastavení.

> [!NOTE]
> Povolením IP zásad řízení přístupu pro váš účet Azure Cosmos DB databáze, veškerý přístup ke svému účtu Azure Cosmos DB databáze z počítače mimo nastavenou povoleny, seznam rozsahů IP adres jsou zablokované. Na základě tento model procházení operace roviny dat z portálu také se zablokuje k zajištění integrity řízení přístupu.

Pro zjednodušení vývoje, portálu Azure vám pomůže identifikovat a přidejte do seznamu povolených IP klientského počítače, aby aplikace běžící váš počítač přístup k účtu Azure Cosmos DB. IP adresa klienta tady je zjištěna, jak je vidět na portálu. Může být IP adresa klienta počítače, ale mohou být také IP adresu brány vaší sítě. Nezapomeňte odebrat před přechodem do produkčního prostředí.

Chcete-li nastavit zásady řízení přístupu IP na portálu Azure, přejděte do okna Azure Cosmos DB účet, klikněte na **brány Firewall** v navigační nabídce klikněte **ON** 

![Snímek obrazovky ukazující, jak otevřít okno brány Firewall na portálu Azure](./media/firewall-support/azure-portal-firewall.png)

V podokně nové určete, jestli portálu Azure můžete přístup k účtu a přidání dalších adresy a rozsahy, protože je to vhodné a pak klikněte na **Uložit**.  

> [!NOTE]
> Když povolíte IP zásad řízení přístupu, je nutné přidat IP adresu pro portál Azure k získání přístupu. Portál IP adresy jsou:
> |Oblast|IP adresa|
> |------|----------|
> |Všechny oblasti s výjimkou těchto zadané níže|104.42.195.92,40.76.54.131,52.176.6.30,52.169.50.45,52.187.184.26|
> |Německo|51.4.229.218|
> |Čína|139.217.8.252|
> |Vláda USA|52.244.48.71|
>

![Snímek obrazovky ukazující, jak nakonfigurovat nastavení brány firewall na portálu Azure](./media/firewall-support/azure-portal-firewall-configure.png)

## <a name="troubleshooting-the-ip-access-control-policy"></a>Řešení potíží s zásad řízení přístupu IP
### <a name="portal-operations"></a>Operace portálu
Povolením IP zásad řízení přístupu pro váš účet Azure Cosmos DB databáze, veškerý přístup ke svému účtu Azure Cosmos DB databáze z počítače mimo nastavenou povoleny, seznam rozsahů IP adres jsou zablokované. Proto pokud chcete povolit portálu datové roviny operací, jako je procházení kolekcí a dotazů dokumentů, musíte výslovně povolit pomocí Azure přístup k portálu **brány Firewall** okno na portálu. 

![Snímek obrazovky ukazující, jak chcete umožnit přístup k portálu Azure](./media/firewall-support/azure-portal-access-firewall.png)

### <a name="sdk--rest-api"></a>Sadu SDK a Rest API
Pro z bezpečnostních důvodů, přístup prostřednictvím sady SDK nebo REST API z počítače není na seznamu povolených vrátí obecné 404 nebyl nalezen odpověď se žádné další podrobnosti. Ověřte IP nakonfigurovaných pro váš účet Azure Cosmos DB databáze seznamu povolených zajistit že správné zásady konfigurace se použije ke svému účtu databáze Azure Cosmos DB.

## <a name="next-steps"></a>Další kroky
Informace o síti tipy pro zvýšení výkonu související, najdete v článku [tipy pro zvýšení výkonu](performance-tips.md).

