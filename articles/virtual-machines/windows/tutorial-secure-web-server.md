---
title: "Zabezpečení služeb IIS s certifikáty protokolu SSL v Azure | Microsoft Docs"
description: "Zjistěte, jak zabezpečit webový server IIS s certifikáty protokolu SSL na virtuální počítač s Windows v Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 43f06422e1120f1c3b2a9d9d5d4be515213c0937
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a>Zabezpečení webového serveru IIS s certifikáty protokolu SSL na virtuální počítač Windows v Azure
Pro zabezpečení webové servery, certifikátu vrstvy SSL (Secure Sockets) slouží k šifrování webový provoz. Tyto certifikáty SSL můžou být uložená v Azure Key Vault a povolit zabezpečená nasazení certifikátů na virtuálních počítačích (VM) s Windows v Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření Azure Key Vault
> * Generovat nebo nahrajte certifikát do služby Key Vault
> * Vytvoření virtuálního počítače a instalaci webového serveru IIS
> * Vložit certifikát do virtuálního počítače a konfigurace služby IIS s vazbou SSL

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="overview"></a>Přehled
Azure Key Vault chrání kryptografické klíče a tajné klíče, tyto certifikáty a hesla. Key Vault pomáhá zjednodušit proces správy certifikátů a zajišťuje vám kontrolu nad klíči, které přístup těchto certifikátů. Můžete vytvořit certifikát podepsaný svým držitelem v Key Vault nebo nahrát certifikát existující, důvěryhodné, který už vlastníte.

Místo použití vlastní image virtuálního počítače, který zahrnuje certifikáty zaručená v, vložit certifikáty do spuštěného virtuálního počítače. Tento proces zajišťuje, že nejaktuálnější certifikáty jsou nainstalovány na webovém serveru během nasazení. Je-li obnovit nebo nahradit certifikát, nemáte také vytvořit novou vlastní imagi virtuálního počítače. Nejnovější certifikáty jsou automaticky vložit, jako je vytváření dalších virtuálních počítačů. Během celého procesu certifikáty nikdy nechte platformy Azure nebo jsou viditelné ve skriptu, historie příkazového řádku nebo šablony.


## <a name="create-an-azure-key-vault"></a>Vytvoření Azure Key Vault
Před vytvořením Key Vault a certifikáty, vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupSecureWeb* v *východní USA* umístění:

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Dále vytvořte Key Vault s [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Každý Key Vault vyžaduje jedinečný název a musí být všechny malá písmena. Nahraďte `<mykeyvault>` v následujícím příkladu se svůj vlastní jedinečný název pro Key Vault:

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Vygenerování certifikátu a uložit v Key Vault
Pro použití v provozním prostředí, měli byste importovat platný certifikát podepsaný službou důvěryhodného zprostředkovatele s [Import AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate). V tomto kurzu následující příklad ukazuje, jak můžete vygenerovat certifikát podepsaný svým držitelem s [přidat AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) používající výchozí zásady certifikátu z [ Nové AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy). 

```powershell
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače
Nastavte uživatelské jméno a heslo správce pro virtuální počítač s [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Nyní můžete vytvořit virtuální počítač s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Následující příklad vytvoří virtuální sítě požadované součásti, konfigurace operačního systému a poté vytvoří virtuální počítač s názvem *Můjvp*:

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myVnet" `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleRDP"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 443
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleWWW"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name "myNic" `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2" | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName "myVM" -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2016-Datacenter" -Version "latest" | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

# Use the Custom Script Extension to install IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

Trvá několik minut pro virtuální počítač, který se má vytvořit. Poslední krok používá rozšíření vlastních skriptů Azure k instalaci webového serveru IIS pomocí [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).


## <a name="add-a-certificate-to-vm-from-key-vault"></a>Přidání certifikátu do virtuálního počítače z Key Vault
Přidání certifikátu z Key Vault pro virtuální počítač, získat číslo ID vašeho certifikátu s [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret). Přidání certifikátu do virtuálního počítače s [přidat AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a>Konfigurace služby IIS na použití certifikátu
Pomocí rozšíření vlastních skriptů znovu s [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) k aktualizaci konfigurace služby IIS. Tato aktualizace použije certifikát vložili z Key Vault pro službu IIS a nakonfiguruje webové vazby:

```powershell
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-the-secure-web-app"></a>Testování zabezpečení webové aplikace
Získat veřejnou IP adresu vašeho virtuálního počítače s [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress). Následující příklad, získá IP adresu pro `myPublicIP` vytvořili dříve:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

Nyní můžete otevřít webový prohlížeč a zadejte `https://<myPublicIP>` na panelu Adresa. Chcete-li přijímat upozornění, pokud použijete certifikát podepsaný svým držitelem zabezpečení, vyberte **podrobnosti** a potom **přejděte na webovou stránku**:

![Přijetí upozornění zabezpečení webového prohlížeče](./media/tutorial-secure-web-server/browser-warning.png)

Zabezpečené webu služby IIS se následně zobrazí jako v následujícím příkladu:

![Web služby IIS spuštěná zabezpečené zobrazení](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Další kroky

V tomto kurzu zabezpečená certifikátem SSL uložené v Azure Key Vault webový server služby IIS. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření Azure Key Vault
> * Generovat nebo nahrajte certifikát do služby Key Vault
> * Vytvoření virtuálního počítače a instalaci webového serveru IIS
> * Vložit certifikát do virtuálního počítače a konfigurace služby IIS s vazbou SSL

Tento odkaz zobrazíte ukázky skriptu předdefinovaných virtuálního počítače.

> [!div class="nextstepaction"]
> [Ukázky skriptu Windows virtuálního počítače](./powershell-samples.md)
