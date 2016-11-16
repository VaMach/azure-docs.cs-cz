---
title: "Azure AD Domain Services: Aktualizace nastavení DNS pro virtuální síť Azure | Dokumentace Microsoftu"
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
ms.date: 09/21/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 81c0564390c57c7ee001ce5a574ab670faf898b4


---
# <a name="azure-ad-domain-services-update-dns-settings-for-the-azure-virtual-network"></a>Azure AD Domain Services – Aktualizace nastavení DNS pro virtuální síť Azure
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Úloha 4: Aktualizace nastavení DNS pro virtuální síť Azure
V předchozích úlohách konfigurace jste úspěšně povolili službu Azure AD Domain Services pro svůj adresář. Dalším úkolem je zajistit, že se počítače v rámci virtuální sítě mohou k těmto službám připojit a využívat je. Aktualizujte nastavení serveru DNS svojí virtuální sítě tak, aby odkazoval na dvě IP adresy, na kterých je ve virtuální síti dostupná služba Azure AD Domain Services.

> [!NOTE]
> Jakmile pro svůj adresář povolíte službu Azure AD Domain Services , zapište si IP adresy služby Azure AD Domain Services zobrazené na kartě **Konfigurovat** adresáře.
> 
> 

Následujícím konfiguračním postupem aktualizujte server DNS virtuální sítě, ve které jste povolili službu Azure AD Domain Services.

1. Přejděte do **portálu Azure Classic** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. V levém podokně vyberte uzel **Sítě**.
   
    ![Uzel virtuálních sítí](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. Na kartě **Virtuální sítě** vyberte virtuální síť, ve které jste povolili službu Azure AD Domain Services, a zobrazte její vlastnosti.
4. Klikněte na kartu **KONFIGUROVAT**.
   
    ![Uzel virtuálních sítí](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. V části **Servery DNS** zadejte IP adresy služby Azure AD Domain Services.
6. Nezapomeňte zadat obě IP adresy, které byly zobrazeny v části **Domain Services** na kartě **Konfigurace** adresáře.
7. V podokně úloh v dolní části stránky klikněte na **Uložit** a uložte nastavení serveru DNS této virtuální sítě.
   
   ![Aktualizujte nastavení serveru DNS virtuální sítě.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
> Po aktualizaci nastavení serveru DNS virtuální sítě může chvíli trvat, než virtuální počítače v síti získají aktualizovanou konfiguraci DNS. Pokud se virtuální počítač nemůže připojit k doméně, můžete vyprázdnit mezipaměť DNS (např. „ipconfig/flushdns“) na virtuálním počítači. Tento příkaz vynutí obnovení nastavení DNS na virtuálním počítači.
> 
> 

## <a name="task-5-enable-password-synchronization-to-azure-ad-domain-services"></a>Úloha 5 – Povolení synchronizace hesel do služby Azure AD Domain Services
Dalším úkolem konfigurace je [povolení synchronizace hesel do služby Azure AD Domain Services](active-directory-ds-getting-started-password-sync.md).




<!--HONumber=Nov16_HO2-->


