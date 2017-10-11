---
title: "Určení nastavení DNS v konfiguračním souboru služby | Microsoft Docs"
description: "určení vlastních nastavení DNS pro virtuální síť pomocí konfigurační soubor služby"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.openlocfilehash: 0fba2ea06827aff29a7a092933edb8120d668b29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Určení nastavení DNS v konfiguračním souboru služby
## <a name="dns-elements"></a>Elementy DNS
Konfigurační soubor služby může obsahovat element DnsServers seznam adres IPv4 pro servery systému DNS (Domain Name), které budou používat službu. Nastavení v konfiguračním souboru služby mají přednost před nastavením v konfiguračním souboru na síti. Další informace najdete v tématu [schéma konfigurace služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> **Název** atribut **Server_dns** element se používají pouze jako název odkazu. Nereprezentuje název hostitele pro DNS server. Každý **Server_dns** hodnota atributu musí být jedinečný v rámci celé předplatné Microsoft Azure.
> 
> 

## <a name="see-also"></a>Viz také
[Schéma konfigurace služby Azure (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Schéma konfigurace virtuální sítě Azure](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurace virtuální sítě pomocí konfiguračních souborů síť](http://go.microsoft.com/fwlink/?LinkId=248094)

[Informace o nastavení virtuální sítě v portálu pro správu](http://go.microsoft.com/fwlink/?LinkId=248092)

