<properties
    pageTitle="Azure AD Domain Services: Aktualizace nastavení DNS pro virtuální síť Azure | Microsoft Azure"
    description="Začínáme se službou Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# Azure AD Domain Services – Aktualizace nastavení DNS pro virtuální síť Azure

## Úloha 4: Aktualizace nastavení DNS pro virtuální síť Azure
V předchozích úlohách konfigurace jste úspěšně povolili službu Azure AD Domain Services pro svůj adresář. Dalším úkolem je zajistit, že se počítače v rámci virtuální sítě mohou k těmto službám připojit a využívat je. Aktualizujte nastavení serveru DNS svojí virtuální sítě tak, aby odkazoval na dvě IP adresy, na kterých je ve virtuální síti dostupná služba Azure AD Domain Services.

> [AZURE.NOTE] Jakmile pro svůj adresář povolíte službu Azure AD Domain Services , zapište si IP adresy služby Azure AD Domain Services zobrazené na kartě **Konfigurovat** adresáře.

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

> [AZURE.NOTE] Po aktualizaci nastavení serveru DNS virtuální sítě může chvíli trvat, než virtuální počítače v síti získají aktualizovanou konfiguraci DNS. Pokud se virtuální počítač nemůže připojit k doméně, můžete vyprázdnit mezipaměť DNS (např. „ipconfig/flushdns“) na virtuálním počítači. Tento příkaz vynutí obnovení nastavení DNS na virtuálním počítači.


## Úloha 5 – Povolení synchronizace hesel do služby Azure AD Domain Services
Dalším úkolem konfigurace je [povolení synchronizace hesel do služby Azure AD Domain Services](active-directory-ds-getting-started-password-sync.md).



<!--HONumber=Sep16_HO4-->


