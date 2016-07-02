<properties
    pageTitle="Vytvoření virtuálního počítače s Linuxem pomocí Portálu Azure | Microsoft Azure"
    description="Virtuální počítače s Linuxem si můžete vytvořit také pomocí Portálu Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="04/29/2016"
    ms.author="v-livech"
/>

# Vytvoření virtuálního počítače s Linuxem v Azure pomocí Portálu

Tento článek ukazuje, jak rychle vytvořit virtuální počítač s Linuxem pomocí [Portálu Azure](https://portal.azure.com/). Jediným požadavkem je mít [účet Azure](https://azure.microsoft.com/pricing/free-trial/) a [soubory veřejného a privátního klíče SSH](virtual-machines-linux-mac-create-ssh-keys.md).


1. Po přihlášení k Portálu Azure s vaší identitou účtu Azure klikněte v levém horním rohu na **+ Nový**:

    ![obrazovka1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. V seznamu **Marketplace** klikněte na **Virtuální počítače** a potom v seznamu imagí **Vybrané aplikace** vyberte **Ubuntu Server 14.04 LTS**.  V dolní části ověřte, že je nastavený model nasazení `Resource Manager`, a potom klikněte na **Vytvořit**.

    ![obrazovka2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Na stránce **Základy** zadejte:
    - Název virtuálního počítače
    - Uživatelské jméno pro účet Admin
    - Typ ověřování nastavený na **Veřejný klíč SSH**
    - Veřejný klíč SSH jako řetězec (ve výchozím nastavení z vašeho adresáře `~/.ssh/`)
    - Název skupiny prostředků (pro vytvoření nové skupiny nasazení) nebo vyberte existující skupinu

    Pak pokračujte kliknutím na **OK** a zvolte velikost virtuálního počítače. Obrazovka by měla vypadat nějak takto:

    ![obrazovka3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Zvolte velikost **DS1**, která nainstaluje Ubuntu na disk SSD typu Premium. Pak klikněte na **Vybrat** a nakonfigurujte nastavení.

    ![obrazovka4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. V okně **Nastavení** nechte nastavené výchozí hodnoty pro možnosti Úložiště a Síť a pak kliknutím na **OK** zobrazte souhrn.  Všimněte si, že zvolením možnosti DS1 se typ disku nastavil na Premium SSD: **S** v názvu označuje SSD.

    ![obrazovka5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Potvrďte nastavení svého nového virtuálního počítače s Ubuntu a klikněte na **OK**.

    ![obrazovka6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Otevřete řídicí panel Portálu a v části **Síťová rozhraní** zvolte svoji síťovou kartu.

    ![obrazovka7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Otevřete nabídku veřejných IP adres v nastaveních síťové karty.

    ![obrazovka8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. Přihlaste se přes protokol SSH k veřejnému IP protokolu pomocí svého veřejného klíče SSH.

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## Další kroky

Rychle jste si vytvořili virtuální počítač s Linuxem, abyste si ho mohli otestovat a abychom si ukázali, jak se to dělá. Pokud budete chtít vytvořit virtuální počítač s Linuxem přizpůsobený vaší infrastruktuře, můžete postupovat podle některého z článků níže.

- [Vytvoření virtuálního počítače s Linuxem v Azure pomocí šablon](virtual-machines-linux-cli-deploy-templates.md)
- [Vytvoření virtuálního počítače s Linuxem se zabezpečením SSH na platformě Azure pomocí šablon](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-create-cli-complete.md)

Tyto články vám pomůžou začít s vytvářením infrastruktury Azure a také celé řady proprietárních a open source nástrojů pro nasazení, konfiguraci a orchestraci infrastruktury.



<!--HONumber=Jun16_HO2-->


