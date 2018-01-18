---
title: "Azure Active Directory Domain Services: Konfigurace řešení potíží s skupinu zabezpečení sítě | Microsoft Docs"
description: "Řešení potíží s konfigurací skupina NSG pro Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: ergreenl
ms.openlocfilehash: 447f9119ea379e278be77d8699c7dcb751ea3085
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-network-security-group-configuration"></a>Azure AD Domain Services – konfigurace řešení potíží s skupinu zabezpečení sítě



## <a name="aadds104-network-error"></a>AADDS104: Network error

**Upozornění:** *společnosti Microsoft je možné vás zastihnout na řadiče domény pro toto spravované domény. To může dojít, pokud skupina zabezpečení sítě (NSG) nakonfigurované na vaší virtuální sítě blokuje přístup k spravované doméně. Jiné možných příčin je, pokud je trasu definovanou uživatelem aby bloky příchozí provoz z Internetu.*

Nejběžnější příčina chyby sítě Azure AD Domain Services je spojena nesprávné konfigurace NSG. Zajistit, že Microsoft může služby a udržovat spravované domény, musíte použít skupiny zabezpečení sítě (NSG) pro povolení přístupu k [určité porty](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services) uvnitř podsíť. Pokud jsou tyto porty zablokované, Microsoft nemůže přístup k prostředkům, které potřebuje, což může být škodlivé pro vaši službu. Při vytváření vaše skupina NSG, nechte tyto porty otevřené pro zajištění bez přerušení služby.

## <a name="sample-nsg"></a>Ukázka NSG
Následující tabulka znázorňuje ukázku NSG, který by zabezpečit vaší spravované domény současně Microsoft monitorovat, spravovat a aktualizovat informace.

![Ukázka NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Azure AD Domain Services vyžaduje neomezenou odchozí přístup. Doporučujeme, abyste nemusíte vytvářet žádné další outboud pravidlo skupiny Nsg.

## <a name="adding-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Přidat pravidlo do skupiny zabezpečení sítě pomocí portálu Azure
Pokud nechcete, jak pomocí prostředí PowerShell, můžete ručně přidat jednoho pravidla do skupiny zabezpečení sítě pomocí portálu Azure. Postupujte podle těchto kroků k vytvoření pravidel ve skupině zabezpečení sítě.

1. Přejděte na [skupin zabezpečení sítě](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) na portálu Azure
2. Zvolte NSG přidružené k vaší doméně z tabulky.
3. V části nastavení v levém navigačním panelu klikněte na možnost **příchozí pravidla zabezpečení** nebo **odchozí pravidla zabezpečení**.
4. Kliknutím na vytvořit pravidlo **přidat** a vyplněním informace. Klikněte na **OK**.
5. Ověřte, zda že byla vytvořena pravidla tím, že v tabulce pravidel.


## <a name="create-a-default-nsg-using-powershell"></a>Vytvoří výchozí NSG pomocí prostředí PowerShell

Předcházející jsou kroky, které můžete použít k vytvoření nové NSG pomocí prostředí PowerShell, který uchovává všechny porty potřebné ke spuštění Azure AD Domain Services otevřít při odepření jiný nežádoucí přístup.


Toto řešení vyžaduje instalaci a používání [Azure AD Powershell](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?toc=%2Fazure%2Factive-directory-domain-services%2Ftoc.json&view=azureadps-2.0).

1. Připojení k adresáři služby Azure AD.

  ```PowerShell
  # Connect to your Azure AD directory.
  Connect-AzureAD
  ```
2. Přihlaste se k předplatnému Azure.

  ```PowerShell
  # Log in to your Azure subscription.
  Login-AzureRmAccount
  ```

3. Vytvořte skupinu NSG s tři pravidla. Následující skript definuje tři pravidla NSG, které umožní přístup k portům potřebné ke spuštění služby Azure AD Domain Services. Tento skript vytvoří nová skupina NSG, který obsahuje tato pravidla. Jste Vítejte přidat další pravidla, podle potřeby používá stejný formát.

  ```PowerShell
  # Create the rules needed
  $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443
  $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389
  $rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986
  # Create the NSG with the 3 rules above
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDS-NSG" -SecurityRules $rule1,$rule2,$rule3
  ```

4. Nakonec skript přidruží NSG virtuální síť a podsíť výběru.

  ```PowerShell
  # Find vnet and subnet
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
  # Set the nsg to the subnet and save the changes
  $subnet.NetworkSecurityGroup = $nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

### <a name="full-script"></a>Celý skript

```PowerShell
#Change the following values to match your deployment
$resourceGroup = "ResourceGroupName"
$vnetName = "exampleVnet"
$subnetName = "exampleSubnet"

$serviceIPs = "52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161"

# Create the rules needed
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

$rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389


# Connect to your Azure AD directory.
Connect-AzureAD

# Log in to your Azure subscription.
Login-AzureRmAccount

# Create the NSG with the 3 rules above
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "NSG-Default" -SecurityRules $rule1,$rule2,$rule3

# Find vnet and subnet
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName

# Set the nsg to the subnet and save the changes
$subnet.NetworkSecurityGroup = $nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

> [!NOTE]
>Toto výchozí nastavení NSG není zablokovat přístup k portu, který používá pro zabezpečení protokolu LDAP. Chcete-li zjistit, jak vytvořit pravidlo pro tento port, navštivte [v tomto článku](active-directory-ds-troubleshoot-ldaps.md).
>

## <a name="contact-us"></a>Kontaktujte nás
Obraťte se na produktový tým Azure Active Directory Domain Services na [sdílet zpětnou vazbu nebo pro podporu](active-directory-ds-contact-us.md).
