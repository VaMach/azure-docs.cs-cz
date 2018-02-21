---
title: "Nainstalovat certifikát klienta P2S | Azure"
description: "Nainstalujte certifikát klienta Mac nebo Windows pro P2S ověření certifikátu."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: de98201b65f5531f334aded1056f622cecb6e190
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2018
---
# <a name="install-a-client-certificate-for-point-to-site-azure-certificate-authentication-connections"></a>Nainstalovat certifikát klienta pro připojení ověřování Azure certifikát Point-to-Site

Všichni klienti, kteří připojit k virtuální síti pomocí ověřování Azure certifikát Point-to-Site vyžadují klientský certifikát. Tento článek vám pomůže nainstalovat klientský certifikát, který se používá k ověřování při připojení k virtuální síti pomocí P2S.

## <a name="generate"></a>Generování a exportujte certifikát klienta

Certifikát klienta můžete vygenerovat z kořenového certifikátu, který byl vytvořen pomocí řešení podnikové certifikační Autority nebo certifikát podepsaný svým držitelem kořenové. Najdete v článku [prostředí PowerShell](vpn-gateway-certificates-point-to-site.md) nebo [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) pokyny k provedení kroků. Po generování klientské certifikáty, je exportujte jako soubory PFX. Nezapomeňte zahrnout celý řetěz certifikátů při exportu.

## <a name="installwin"></a>Nainstalujte certifikát na klienty se systémem Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Nainstalujte certifikát na klienty systému Mac.

Klienti Mac VPN jsou podporovány pouze modelu nasazení Resource Manager. Některé funkce nejsou podporovány pro model nasazení classic.

> [!NOTE]
>  IKEv2 je aktuálně ve verzi Preview.
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="next-steps"></a>Další postup

Pokračujte kroky konfigurace Point-to-Site.

* [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
* [Azure Portal (Classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)