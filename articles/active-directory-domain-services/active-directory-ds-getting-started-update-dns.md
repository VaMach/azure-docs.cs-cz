---
title: "Azure Active Directory Domain Services: Aktualizace nastavení DNS pro virtuální síť Azure | Dokumentace Microsoftu"
description: "Začínáme se službou Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 8bee2a25f196d645b27f30f21305b1550e44e07a
ms.contentlocale: cs-cz
ms.lasthandoff: 06/30/2017


---
<a id="update-dns-settings-for-the-azure-virtual-network" class="xliff"></a>

# Aktualizace nastavení DNS pro virtuální síť Azure
<a id="task-4-update-dns-settings-for-the-azure-virtual-network" class="xliff"></a>

## Úloha 4: Aktualizace nastavení DNS pro virtuální síť Azure
V předchozích úlohách konfigurace jste úspěšně povolili službu Azure Active Directory Domain Services pro svůj adresář. Dalším úkolem je zajistit, že se počítače v rámci virtuální sítě mohou k těmto službám připojit a využívat je. V tomto článku provedete aktualizaci nastavení serveru DNS svojí virtuální sítě tak, aby odkazoval na dvě IP adresy, kde je ve virtuální síti dostupná služba Azure Active Directory Domain Services.

> [!NOTE]
> Jakmile pro svůj adresář povolíte službu Azure Active Directory Domain Services, zapište si IP adresy služby Azure Active Directory Domain Services zobrazené na kartě **Konfigurovat** adresáře.
>
>

Podle následujících kroků aktualizujte server DNS virtuální sítě, ve které jste povolili službu Azure Active Directory Domain Services:

1. Přejděte na [portál Azure Classic](https://manage.windowsazure.com).
2. V levém podokně vyberte **Sítě**.  
    Otevře se okno **Sítě**.

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. Na kartě **Virtuální sítě** vyberte virtuální síť, ve které jste povolili službu Azure Active Directory Domain Services, a zobrazte její vlastnosti.
4. Klikněte na kartu **KONFIGUROVAT**.

    ![Okno Virtuální sítě](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. V části **Servery DNS** zadejte obě IP adresy, které byly zobrazeny v části **Domain Services** na kartě **Konfigurace adresáře**.
6. V podokně úloh v dolní části okna klikněte na **Uložit** a uložte nastavení serveru DNS této virtuální sítě.

   ![Aktualizace nastavení serveru DNS virtuální sítě](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  Virtuální počítače v síti získají nové nastavení DNS až po restartování. Pokud chcete, aby aktualizované nastavení DNS získaly hned, aktivujte restartování z portálu, pomocí PowerShellu nebo pomocí rozhraní příkazového řádku.
>
>

<a id="next-steps" class="xliff"></a>

## Další kroky
Úloha 5: [Povolení synchronizace hesel do služby Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

