---
title: "Nelze odstranit virtuální sítě v Azure | Microsoft Docs"
description: "Zjistěte, jak vyřešit problém, ve kterém nelze odstranit virtuální sítě v Azure."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: e0e58e443897adad2b6029db4e64fa220cebb6b8
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Řešení potíží: Nepodařilo se odstranit virtuální sítě v Azure

Taky může docházet k chybám při pokusu o odstranění virtuální sítě v Microsoft Azure. Tento článek obsahuje postup pro odstraňování potíží při řešení tohoto problému. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Pokyny při řešení potíží 

1. [Zkontrolujte, zda bránu virtuální sítě běží ve virtuální síti](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Zkontrolujte, zda služby application gateway běží ve virtuální síti](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Zkontrolujte, zda je povolena služba Azure Active Directory Domain Services ve virtuální síti](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Zkontrolujte, zda je virtuální sítě připojený k jiný prostředek](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. [Zkontrolujte, zda je virtuální počítač stále spuštěna ve virtuální síti](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Zkontrolujte, zda virtuální sítě se zasekla v automatickém migrace](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>Řešení potíží

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Zkontrolujte, zda bránu virtuální sítě běží ve virtuální síti

Odebrat virtuální sítě, je nutné nejprve odebrat bránu virtuální sítě.

Klasické virtuální sítě, najdete **přehled** stránky klasické virtuální sítě na portálu Azure. V **připojení k síti VPN** část, pokud brána běží ve virtuální síti, zobrazí se na IP adresu brány. 

![Zkontrolujte, jestli je brána spuštěná.](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Pro virtuální sítě, přejděte na **přehled** stránky ve virtuální síti. Zkontrolujte **připojená zařízení** pro bránu virtuální sítě.

![Zkontrolujte připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Před odebráním bránu, nejprve odeberte **připojení** objekty v bráně. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Zkontrolujte, zda je ve virtuální síti spuštěn aplikační brány

Přejděte na **přehled** stránky ve virtuální síti. Zkontrolujte **připojená zařízení** pro službu application gateway.

![Zkontrolujte připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Pokud je služby application gateway, musí odebrat před odstraněním virtuální sítě.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Zkontrolujte, zda je povolena služba Azure Active Directory Domain Services ve virtuální síti

Pokud služba Active Directory Domain Services je povolen a připojen k virtuální síti, nelze odstranit tuto virtuální síť. 

![Zkontrolujte připojeného zařízení](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

Zakázat službu, postupujte takto:

1. Přejděte na [portál Azure Classic](https://manage.windowsazure.com).
2. V levém podokně vyberte **služby Active Directory**.
3. Vyberte adresář služby Azure Active Directory (Azure AD), který má služba Active Directory Domain Services povoleno.
4. Vyberte kartu **Konfigurace**.
5. V části **služby domény**, změnit **povolit doménové služby pro tento adresář** možnost k **ne**.  

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Zkontrolujte, zda virtuální síť je připojená k jiné prostředku

Zkontrolujte propojení okruhu, připojení a partnerských vztahů virtuální sítě. Některý z těchto může způsobit odstranění virtuální sítě k selhání. 

Pořadí odstranění doporučené vypadá takto:

1. Připojení brány
2. Brány
3. IP adresy
4. Partnerské vztahy virtuální sítě
5. Služba App Service Environment (App Service Environment)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Zkontrolujte, zda je virtuální počítač stále spuštěna ve virtuální síti

Ujistěte se, že žádný virtuální počítač je ve virtuální síti.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Zkontrolujte, zda virtuální sítě se zasekla v automatickém migrace

Pokud virtuální sítě se zasekla v automatickém migrace stavu, nelze jej odstranit. Spusťte následující příkaz k přerušení migrace a pak odstraňte virtuální sítě.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Další kroky

- [Azure Virtual Network](virtual-networks-overview.md)
- [Nejčastější dotazy ke službě Azure Virtual Network](virtual-networks-faq.md)
