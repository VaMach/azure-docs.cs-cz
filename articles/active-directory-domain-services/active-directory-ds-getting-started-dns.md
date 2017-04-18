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
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: abb27292d4b5533fe6f3d66d6921fea8c82f18dd
ms.lasthandoff: 04/12/2017


---
# <a name="update-dns-settings-for-the-azure-virtual-network"></a>Aktualizace nastavení DNS pro virtuální síť Azure
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Úloha 4: Aktualizace nastavení DNS pro virtuální síť Azure
V předchozích úlohách konfigurace jste úspěšně povolili službu Azure Active Directory Domain Services pro svůj adresář. Dalším úkolem je zajistit, že se počítače v rámci virtuální sítě mohou k těmto službám připojit a využívat je. V tomto článku provedete aktualizaci nastavení serveru DNS svojí virtuální sítě tak, aby odkazoval na dvě IP adresy, kde je ve virtuální síti dostupná služba Azure Active Directory Domain Services.

> [!NOTE]
> Jakmile pro svůj adresář povolíte službu Azure Active Directory Domain Services, zapište si IP adresy služby Azure Active Directory Domain Services zobrazené na kartě **Konfigurovat** adresáře.
>
>

Podle následujícího postupu aktualizujte server DNS virtuální sítě, ve které jste povolili službu Azure Active Directory Domain Services:

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
> Po aktualizaci nastavení serveru DNS virtuální sítě může chvíli trvat, než virtuální počítače v síti získají aktualizovanou konfiguraci DNS. Pokud se virtuální počítač nemůže připojit k doméně, můžete na virtuálním počítači vyprázdnit mezipaměť DNS (ipconfig /flushdns). Tento příkaz vynutí obnovení nastavení DNS na virtuálním počítači.
>
>

## <a name="next-steps"></a>Další kroky
Úloha 5: [Povolení synchronizace hesel do služby Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

