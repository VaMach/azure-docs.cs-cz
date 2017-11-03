---
title: "Konfigurace DHCPv6 pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Postup konfigurace DHCPv6 pro virtuální počítače s Linuxem."
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
keywords: "protokol IPv6, nástroje pro vyrovnávání zatížení azure, duálním zásobníkem, veřejnou IP adresu, nativní protokol ipv6, mobilní, iot"
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 84558cb6e3a5524969f590eb0272a64ad8839ab5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurace protokolu DHCPv6 pro virtuální počítače s Linuxem

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Některé bitové kopie virtuálních počítačů Linux v Azure Marketplace nemají ve výchozím nastavení nakonfigurované DHCPv6. Chcete-li podporovat protokol IPv6, musí být nakonfigurován DHCPv6 v v rámci operačního systému Linux distribuce, kterou používáte. Různých distribucí Linux mají různé způsoby konfigurace DHCPv6, protože používají různé balíčky.

> [!NOTE]
> Poslední SUSE Linux a CoreOS bitové kopie v Azure Marketplace se předem nakonfigurovaným rozhraním DHCPv6. Při použití těchto imagí nejsou vyžadovány žádné další změny.

Tento dokument popisuje, jak povolit DHCPv6 tak, aby virtuální počítač Linux získá adresu IPv6.

> [!WARNING]
> Nesprávně úpravou konfiguračních souborů síť může způsobit ztrátu přístupu k síti k virtuálnímu počítači. Doporučujeme, abyste otestovali změny konfigurace v systémech nevýrobní prostředí. Podle pokynů v tomto článku jsme otestovali na nejnovější verze Linux obrázků v Azure Marketplace. V dokumentaci ke konkrétní verzi systému Linux podrobnější pokyny.

## <a name="ubuntu"></a>Ubuntu

1. Upravte soubor `/etc/dhcp/dhclient6.conf` a přidejte následující řádek:

        timeout 10;

2. Upravte konfiguraci sítě pro položku eth0 rozhraní s následující konfigurací:

   * Na **Ubuntu 12.04 a 14.04**, upravte soubor`/etc/network/interfaces.d/eth0.cfg`
   * Na **Ubuntu 16.04**, upravte soubor`/etc/network/interfaces.d/50-cloud-init.cfg`

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Obnovení IPv6 adresa:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Upravte soubor `/etc/dhcp/dhclient6.conf` a přidejte následující řádek:

        timeout 10;

2. Upravte soubor `/etc/network/interfaces` a přidejte následující konfigurace:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Obnovení IPv6 adresa:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL nebo CentOS nebo Oracle Linux

1. Upravte soubor `/etc/sysconfig/network` a přidejte následující parametr:

        NETWORKING_IPV6=yes

2. Upravte soubor `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte následující dva parametry:

        IPV6INIT=yes
        DHCPV6C=yes

3. Obnovení IPv6 adresa:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Poslední SLES a openSUSE bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6. Při použití těchto imagí nejsou vyžadovány žádné další změny. Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní SUSE, použijte následující kroky:

1. Nainstalujte `dhcp-client` balíčku, v případě potřeby:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Upravte soubor `/etc/sysconfig/network/ifcfg-eth0` a přidejte následující parametr:

        DHCLIENT6_MODE='managed'

3. Obnovení na adresu IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 a openSUSE přestupného

Poslední SLES a openSUSE bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6. Při použití těchto imagí nejsou vyžadovány žádné další změny. Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní SUSE, použijte následující kroky:

1. Upravte soubor `/etc/sysconfig/network/ifcfg-eth0` a nahraďte tento parametr

        #BOOTPROTO='dhcp4'

    s následující hodnotou:

        BOOTPROTO='dhcp'

2. Přidejte následující parametr k `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Obnovení na adresu IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Poslední CoreOS bitové kopie v Azure se předem nakonfigurovaným rozhraním DHCPv6. Při použití těchto imagí nejsou vyžadovány žádné další změny. Pokud máte virtuální počítač, na základě bitové kopie starší nebo vlastní CoreOS, použijte následující kroky:

1. Upravte soubor`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Obnovení na adresu IPv6:

    ```bash
    sudo systemctl restart systemd-networkd
    ```
