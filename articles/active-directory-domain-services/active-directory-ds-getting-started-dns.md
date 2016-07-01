<properties
    pageTitle="Azure AD Domain Services: Aktualizace nastavení DNS pro virtuální síť Azure | Microsoft Azure"
    description="Začínáme se službou Azure Active Directory Domain Services (Preview)"
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
    ms.date="04/25/2016"
    ms.author="maheshu"/>

# Azure AD Domain Services *(Preview)* – Aktualizace nastavení DNS pro virtuální síť Azure

## Úloha 4: Aktualizace nastavení DNS pro virtuální síť Azure
Úspěšně jste povolili službu Azure AD Domain Services pro svůj adresář a dalším úkolem je zajistit, aby se počítače v rámci virtuální sítě mohli připojit a tuto službu využívat. Pokud to chcete provést, je nutné aktualizovat nastavení serveru DNS virtuální sítě tak, aby odkazovalo na IP adresy, na kterých je ve virtuální síti dostupná služba Azure AD Domain Services.

> [AZURE.NOTE] Jakmile pro svůj adresář povolíte službu Azure AD Domain Services , zapište si IP adresy služby Azure AD Domain Services zobrazené na kartě **Konfigurovat** adresáře.

Následujícím konfiguračním postupem aktualizujte server DNS virtuální sítě, ve kterém jste povolili službu Azure AD Domain Services.

1. Přejděte do **portálu Azure Classic** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. V levém podokně vyberte uzel **Sítě**.

    ![Uzel virtuálních sítí](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Na kartě **Virtuální sítě** vyberte virtuální síť, ve které jste povolili službu Azure AD Domain Services, a zobrazte její vlastnosti.

4. Klikněte na kartu **Konfigurace**.

    ![Uzel virtuálních sítí](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. V části **Servery DNS** zadejte IP adresy služby Azure AD Domain Services.

6. Nezapomeňte zadat obě IP adresy, které byly zobrazeny v části **Domain Services** na kartě **Konfigurace** adresáře.

7. V podokně úloh v dolní části stránky klikněte na tlačítko **Uložit** a uložte nastavení serveru DNS této virtuální sítě.

   ![Aktualizujte nastavení serveru DNS virtuální sítě.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Po aktualizaci nastavení serveru DNS virtuální sítě může chvíli trvat, než virtuální počítače v síti získají aktualizovanou konfiguraci DNS. Pokud se virtuální počítač nemůže připojit k doméně, můžete vyprázdnit mezipaměť DNS (např. "ipconfig/flushdns") virtuálního počítače, a tím vynutit aktualizaci nastavení DNS na virtuálním počítači.


## Úloha 5 – Povolení synchronizace hesel do služby Azure AD Domain Services
Dalším úkolem konfigurace je [povolení synchronizace hesel do služby Azure AD Domain Services](active-directory-ds-getting-started-password-sync.md).



<!--HONumber=Jun16_HO2-->


