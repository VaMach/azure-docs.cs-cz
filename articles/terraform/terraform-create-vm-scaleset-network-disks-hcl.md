---
title: "Použijte Terraform a vytvoření škálování virtuálního počítače Azure, nastavit pomocí kompatibilního hardwaru."
description: "Terraform použijte ke konfiguraci a verze škálování virtuálního počítače Azure nastavit dokončení s virtuální sítí a spravované připojenými disky."
keywords: "terraform, devops, škálovat nastaveno, virtuální počítače, sítě, úložiště, moduly"
ms.service: virtual-machines-linux
author: dcaro
ms.author: dcaro
ms.date: 10/04/2017
ms.topic: article
ms.openlocfilehash: 7a4e21d547b3d2b2399f9f68b1babd9f82a421b7
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="use-terraform-to-plan-and-create-a-networked-azure-vm-scale-set-with-managed-storage"></a>Použít Terraform k naplánování a vytváření sad s úložiště spravovaný síťových škálování virtuálního počítače Azure

V tomto článku, použijete [Terraform](https://www.terraform.io/) vytvořit a nasadit [scaleset virtuální počítač Azure](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) s spravovaná diskům použít [Hashicorp konfigurace jazyk](https://www.terraform.io/docs/configuration/syntax.html) (kompatibilního hardwaru).  

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Nastavení nasazení Terraform
> * Použití proměnných a výstupy Terraform nasazení 
> * Vytvoření a nasazení síťové infrastruktury
> * Vytvářet a nasazovat škálovací sadu virtuálních počítačů a jeho připojení k síti
> * Vytvoření a nasazení jumpbox pro připojení k virtuálním počítačům pomocí protokolu SSH

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="before-you-begin"></a>Než začnete

- [Terraform instalace a konfigurace přístupu k Azure](/azure/virtual-machines/linux/terraform-install-configure)
- [Vytvořte dvojici klíčů SSH](/azure/virtual-machines/linux/mac-create-ssh-keys) Pokud jste ještě nemáte.

## <a name="create-the-file-structure"></a>Vytvořit strukturu souborů

Vytvořte tři nové soubory v prázdného adresáře s těmito názvy:

- `variables.tf`Tento soubor obsahuje hodnoty proměnných použité v šabloně.
- `output.tf`Tento soubor popisuje nastavení, která se zobrazí po nasazení.
- `vmss.tf`Tento kód pro škálování virtuálního počítače nastavit infrastruktury.

## <a name="create-the-variables-and-output-definitions"></a>Vytvoření proměnné a výstupní definice

V tomto kroku definujete proměnné, které prostředky vytvořené Terraform přizpůsobit.

Upravit `variables.tf` souboru, zkopírujte následující kód a potom uložte změny.

```tf 
variable "location" {
  description = "The location where resources will be created"
  default     = "West US"
}

variable "resource_group_name" {
  description = "The name of the resource group in which the resources will be created"
  default     = "myResourceGroup"
}
```

Upravit `output.tf` soubor a zkopírujte následující kód ke zveřejnění plně kvalifikovaný název domény pro virtuální počítače. 

```hcl 
output "vmss_public_ip" {
    value = "${azurerm_public_ip.vmss.fqdn}"
}
```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definování síťové infrastruktury v šabloně

V tomto kroku vytvoříte následující síťové infrastruktury do nové skupiny prostředků Azure: 

  - Jedna virtuální síť s adresním prostorem 10.0.0.0/16 
  - Jednu podsíť s adresním prostorem 10.0.2.0/24
  - Dvě veřejné IP adresy. Jeden používá virtuální počítač škálování sadu Vyrovnávání zatížení dalších používaný pro připojení k SSH jumpbox.


Upravit a zkopírujte následující kód v `vmss.tf` souboru: 

```tf
resource "azurerm_resource_group" "vmss" {
  name     = "${var.resource_group_name}"
  location = "${var.location}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_network" "vmss" {
  name                = "vmss-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_subnet" "vmss" {
  name                 = "vmss-subnet"
  resource_group_name  = "${azurerm_resource_group.vmss.name}"
  virtual_network_name = "${azurerm_virtual_network.vmss.name}"
  address_prefix       = "10.0.2.0/24"
}

resource "azurerm_public_ip" "vmss" {
  name                         = "vmss-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

``` 

> [!NOTE]
> Je vhodné označit prostředky nasazované v Azure a usnadnění jejich identifikace v budoucnu.

## <a name="create-the-network-infrastructure"></a>Vytvořit síťovou infrastrukturu

Inicializace Terraform prostředí tak, že spustíte následující příkaz v adresáři, kde jste vytvořili `.tf` soubory:

```bash
terraform init 
```

Spusťte následující příkaz pro nasazení infrastruktury v Azure.

```bash
terraform apply
```

Ověřte, jestli odpovídá plně kvalifikovaný název domény veřejné IP adresy do vaší konfigurace: ![VMSS terraform plně kvalifikovaný název domény pro veřejnou IP adresu](./media/tf-create-vmss-step4-fqdn.png)


Skupina prostředků musí mít následující prostředky: ![VMSS terraform síťové prostředky](./media/tf-create-vmss-step4-rg.png)


## <a name="edit-the-infrastructure-to-add-the-virtual-machine-scale-set"></a>Upravit infrastrukturu k přidání sady škálování virtuálního počítače

V tomto kroku přidáte do šablony v následujících zdrojích informací:

- Jeden Azure zatížení vyrovnávání a pravidla aplikace a připojte ji k dříve nakonfigurovali veřejnou IP adresu.
- Azure back-end fondu adres a přiřaďte ho do loadbalancer 
- Port testu stavu používá aplikace a nakonfigurovaná na loadbalancer 
- Nastavit uložený za nástroj pro vyrovnávání zatížení, systémem virtuální sítě předtím škálování virtuálního počítače
- [Nginx](http://nginx.org/) na uzlech škálování virtuálních počítačů pomocí rozšíření vlastních skriptů.

Přidejte následující kód do konce `vmss.tf` souboru.

```tf
resource "azurerm_lb" "vmss" {
  name                = "vmss-lb"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  frontend_ip_configuration {
    name                 = "PublicIPAddress"
    public_ip_address_id = "${azurerm_public_ip.vmss.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_lb_backend_address_pool" "bpepool" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "BackEndAddressPool"
}

resource "azurerm_lb_probe" "vmss" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "ssh-running-probe"
  port                = "${var.application_port}"
}

resource "azurerm_lb_rule" "lbnatrule" {
    resource_group_name            = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id                = "${azurerm_lb.vmss.id}"
    name                           = "http"
    protocol                       = "Tcp"
    frontend_port                  = "${var.application_port}"
    backend_port                   = "${var.application_port}"
    backend_address_pool_id        = "${azurerm_lb_backend_address_pool.bpepool.id}"
    frontend_ip_configuration_name = "PublicIPAddress"
    probe_id                       = "${azurerm_lb_probe.vmss.id}"
}

resource "azurerm_virtual_machine_scale_set" "vmss" {
  name                = "vmscaleset"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  upgrade_policy_mode = "Manual"

  sku {
    name     = "Standard_DS1_v2"
    tier     = "Standard"
    capacity = 2
  }

  storage_profile_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_profile_os_disk {
    name              = ""
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_profile_data_disk {
    lun          = 0
    caching        = "ReadWrite"
    create_option  = "Empty"
    disk_size_gb   = 10
  }

  os_profile {
    computer_name_prefix = "vmlab"
    admin_username       = "azureuser"
    admin_password       = "Passwword1234"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  network_profile {
    name    = "terraformnetworkprofile"
    primary = true

    ip_configuration {
      name                                   = "IPConfiguration"
      subnet_id                              = "${azurerm_subnet.vmss.id}"
      load_balancer_backend_address_pool_ids = ["${azurerm_lb_backend_address_pool.bpepool.id}"]
    }
  }

  extension { 
    name = "vmssextension"
    publisher = "Microsoft.OSTCExtensions"
    type = "CustomScriptForLinux"
    type_handler_version = "1.2"
    settings = <<SETTINGS
    {
        "commandToExecute": "sudo apt-get -y install nginx"
    }
    SETTINGS
  }

  tags {
    environment = "codelab"
  }
}
```

Přizpůsobení nasazení přidáním následující kód do `variables.tf`:

```tf 
variable "application_port" {
    description = "The port that you want to expose to the external load balancer"
    default     = 80
}

variable "admin_password" {
    description = "Default password for admin"
    default = "Passwwoord11223344"
}
``` 


## <a name="deploy-the-virtual-machine-scale-set-in-azure"></a>Nasadit v Azure sad škálování virtuálního počítače

Spusťte následující příkaz k vizualizaci nasazení sady škálování virtuálního počítače:

```bash
terraform plan
```

Výstup příkazu by měl vypadat následovně.
![Terraform přidat vmss plán](./media/tf-create-vmss-step6-plan884d3aefd9708a711bc09a66e85eb149c23a3ccff959655ec00418168b2bd481.png)

Potom nasaďte další prostředky v Azure: 

```bash
terraform apply 
```

Obsah skupiny prostředků by měl vypadat podobně jako:

![Skupina prostředků scaleset Terraform virtuálních počítačů](./media/tf-create-vmss-step6-apply.png)

Otevřete prohlížeč a připojte se k plně kvalifikovaný název domény, který byl vrácen příkazem. 

## <a name="add-an-ssh-jumpbox-to-the-existing-network"></a>Přidat jumpbox SSH do existující síť 

V tomto kroku nakonfigurujete v následujících zdrojích informací:
- Síťové rozhraní připojené k stejné podsíti jako škálovací sadu virtuálních počítačů.
- Virtuální počítač připojen k toto síťové rozhraní. Tento jumpbox je vzdáleně přístupný. Po připojení můžete SSH k některému z virtuálních počítačů v sadě škálování.



Přidejte následující kód do konce `vmss.tf` souboru:

```hcl 
resource "azurerm_public_ip" "jumpbox" {
  name                         = "jumpbox-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}-ssh"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_network_interface" "jumpbox" {
  name                = "jumpbox-nic"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  ip_configuration {
    name                          = "IPConfiguration"
    subnet_id                     = "${azurerm_subnet.vmss.id}"
    private_ip_address_allocation = "dynamic"
    public_ip_address_id          = "${azurerm_public_ip.jumpbox.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_machine" "jumpbox" {
  name                  = "jumpbox"
  location              = "${var.location}"
  resource_group_name   = "${azurerm_resource_group.vmss.name}"
  network_interface_ids = ["${azurerm_network_interface.jumpbox.id}"]
  vm_size               = "Standard_DS1_v2"

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_os_disk {
    name              = "jumpbox-osdisk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  os_profile {
    computer_name  = "jumpbox"
    admin_username = "azureuser"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  tags {
    environment = "codelab"
  }
}
```

Upravit `outputs.tf` a přidejte následující kód, který zobrazí název hostitele jumpbox po dokončení nasazení:

```
output "jumpbox_public_ip" {
    value = "${azurerm_public_ip.jumpbox.fqdn}"
}
```

## <a name="deploy-the-jumpbox"></a>Nasazení jumpbox

Nasaďte jumpbox.

```bash
terraform apply 
```

Po dokončení nasazení obsahu skupiny prostředků vypadá takto:

![Skupina prostředků scaleset Terraform virtuálních počítačů](./media/tf-create-create-vmss-step8.png)

> [!NOTE]
> Přihlášení pomocí hesla je zakázána na jumpbox a škálování virtuálního počítače nastavit, že jste nasadili. Přihlaste se pomocí SSH pro přístup virtuálních počítačů.

## <a name="clean-up-the-environment"></a>Vyčistěte prostředí.

Následující příkazy odstranit prostředky vytvořené v tomto kurzu:

```bash
terraform destroy
```

Typ `yes` Pokud budete vyzváni k potvrzení pro odstranění prostředků. Proces odstraňování trvá několik minut.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nasadili nastavit v Azure pomocí Terraform škálování virtuálního počítače. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Inicializace Terraform nasazení
> * Použití proměnných a výstupy Terraform nasazení 
> * Vytvoření a nasazení síťové infrastruktury
> * Vytvářet a nasazovat škálovací sadu virtuálních počítačů a připojte ji do existujícího prostředí
> * Vytvoření a nasazení jumpbox pro připojení k virtuálním počítačům pomocí protokolu SSH 
