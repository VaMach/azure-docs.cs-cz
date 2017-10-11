---
title: "Instalace a konfigurace Terraform ke zřízení virtuálních počítačů a další infrastrukturou v Azure | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat Terraform vytváření prostředků Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jtalkar
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: echuvyrov
ms.openlocfilehash: da567097be38ac649c6bf1de1508de24d21cb877
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="install-and-configure-terraform-to-provision-vms-and-other-infrastructure-into-azure"></a>Instalace a konfigurace Terraform ke zřízení virtuálních počítačů a další infrastrukturou do Azure 
Tento článek popisuje kroky potřebné k instalaci a konfiguraci Terraform k poskytování prostředkům, například virtuálních počítačů do Azure. Se dozvíte, jak vytvořit a použít přihlašovací údaje Azure umožňující Terraform ke zřízení cloudovým prostředkům zabezpečené způsobem.

HashiCorp Terraform poskytuje snadný způsob, jak definovat a nasazovat infrastruktury cloudu s využitím vlastní ukázka jazyk, kterému se říká HashiCorp konfigurace jazyk (kompatibilního hardwaru). Tato vlastní jazyk je [snadno k zápisu a snadno pochopitelné](terraform-create-complete-vm.md). Kromě toho pomocí `terraform plan` příkazů, můžete vizualizovat změny k infrastruktuře předtím, než je potvrzení. Postupujte podle těchto kroků k použití Terraform s Azure.

## <a name="install-terraform"></a>Nainstalujte Terraform
Chcete-li nainstalovat Terraform, [Stáhnout](https://www.terraform.io/downloads.html) balíčku pro váš operační systém do samostatné instalační adresář. Stahování obsahuje jeden spustitelný soubor, pro které byste měli také definovat globální cestu. Pokyny o tom, jak nastavit cestu, na Linuxu a Macu, přejděte na [tato webová stránka](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux). Pokyny o tom, jak nastavit cestu, v systému Windows, přejděte na [tato webová stránka](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows). Pokud chcete ověřit instalaci, spusťte `terraform` příkaz. Zobrazí seznam dostupných možností Terraform jako výstup.

Dále musíte povolit Terraform přístup k vašemu předplatnému Azure, chcete-li provést zřízení infrastruktury.

## <a name="set-up-terraform-access-to-azure"></a>Nastavení přístupu k Terraform do Azure
Pokud chcete povolit Terraform k prostředkům zřídit do Azure, musíte vytvořit dvě entity ve službě Azure Active Directory (Azure AD): aplikace Azure AD a instanční objekt služby Azure AD. Poté použít tyto entity identifikátory v Terraform skripty. Hlavní název služby je místní instanci globální Azure AD aplikace. Objekt služby umožňuje řídit granulární místní přístup k globální prostředky.

Existuje několik způsobů, jak vytvořit objekt aplikaci Azure AD a službou Azure AD. Nejjednodušší a nejrychlejší způsob dnes je použití Azure CLI 2.0, který [můžete stáhnout a nainstalovat na Windows, Linuxu nebo Macu](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). K vytvoření infrastruktury potřebné zabezpečení můžete použít také prostředí PowerShell nebo Azure CLI 1.0. Následující pokyny popisují, jak konfigurace Terraform pro Azure pomocí všech těchto přístupů.

### <a name="use-azure-cli-20-for-windows-linux-or-mac-users"></a>Použití Azure CLI 2.0 (pro Windows, Linux nebo Mac. uživatelů) 
Po stažení a instalaci [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), přihlaste se ke správě vašeho předplatného Azure po vydání následujícího příkazu:

```
az login
```

>[!NOTE]
>Pokud používáte Čína, Azure v Německu a cloudy Azure Government, musíte nejdřív nakonfigurovat rozhraní příkazového řádku Azure pro práci s tímto cloudem. Můžete provést spuštěním následující:

```
az cloud set --name AzureChinaCloud|AzureGermanCloud|AzureUSGovernment
```

Pokud máte víc předplatných Azure, jsou jejich podrobnosti vrácený `az login` příkaz. Nastavte `SUBSCRIPTION_ID` proměnnou prostředí pro uložení hodnota vrácený `id` pole z předplatného, kterou chcete použít. 

Nastavte předplatné, které chcete použít pro tuto relaci.

```
az account set --subscription="${SUBSCRIPTION_ID}"
```

Dotaz na účet, který chcete získat předplatné hodnoty ID ID a klienta.

```
az account show --query "{subscriptionId:id, tenantId:tenantId}"
```

Dále vytvořte samostatné přihlašovací údaje pro Terraform.

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

AppId, heslo, sp_name a klienta se vrátí. Poznamenejte si ID aplikace a heslo.

Pokud chcete ověřit vaše přihlašovací údaje (instanční objekt), otevřete nové prostředí a spusťte následující příkazy. Nahraďte vrácené hodnoty sp_name, heslo a klienta:

```
az login --service-principal -u SP_NAME -p PASSWORD --tenant TENANT
az vm list-sizes --location westus
```

### <a name="use-powershell-for-windows-users"></a>Pomocí prostředí PowerShell (pro uživatele systému Windows) 
Počítače s Windows k zápisu a spustit skripty Terraform a jak pomocí prostředí PowerShell pro úlohy konfigurace, nakonfigurujte svůj počítač správné nástroje prostředí PowerShell. 

1. Instalace nástrojů pro prostředí PowerShell pomocí následujících kroků v [nainstalovat a nakonfigurovat Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps). 

2. Stažení a spuštění [skriptu azure setup.ps1](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) z konzoly Powershellu.

3. Pro spuštění skriptu azure setup.ps1, stažení a spuštění `./azure-setup.ps1 setup` v konzole PowerShell příkaz. Přihlaste se k předplatnému Azure s oprávněními správce.

4. Zadejte název aplikace (libovolný řetězec, vyžaduje) po zobrazení výzvy. Volitelně můžete zadejte silné heslo po zobrazení výzvy. Pokud nezadáte heslo, silné heslo se generuje pro můžete pomocí knihovny .NET zabezpečení.

### <a name="use-azure-cli-10-for-linux-or-mac-users"></a>Použití Azure CLI 1.0 (pro Linux nebo Mac. uživatelů)
Začít pracovat s Terraform na počítače se systémem Linux nebo počítače Mac pomocí Azure CLI 1.0, nainstalujte správné knihovny na váš počítač.  

1. Instalace nástrojů příkazového řádku Azure xPlat podle kroků v [nainstalovat Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

2. Stáhněte a nainstalujte podle pokynů v mapě JSON [stáhnout jq](https://stedolan.github.io/jq/download/).

3. Stažení a spuštění [skriptu azure setup.sh](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash skriptu z konzoly.

4. Pro spuštění skriptu azure setup.sh, stažení a spuštění `./azure-setup setup` příkazu z konzoly. Přihlaste se k předplatnému Azure s oprávněními správce.
 
5. Zadejte název aplikace (libovolný řetězec, vyžaduje) po zobrazení výzvy. Volitelně můžete zadejte silné heslo po zobrazení výzvy. Pokud nezadáte heslo, silné heslo se generuje pro můžete pomocí knihovny .NET zabezpečení.

Všechny předchozí skripty vytvoření aplikace Azure AD a služby hlavní. Objekt služby získá Přispěvatel nebo vlastníka úroveň přístupu u předplatného. Z důvodu vysokou úroveň udělení přístupu byste měli vždy chránit informace o zabezpečení generované tyto skripty. Poznamenejte si všechny čtyři kusy informace o zabezpečení poskytované tyto skripty: appId, heslo, ID_ODBĚRU a tenant_id.

## <a name="set-environment-variables"></a>Proměnné prostředí sady
Po vytvoření a konfigurace objektu služby Azure AD, budete muset umožní Terraform vědět klienta ID, ID předplatného, ID klienta a tajný klíč klienta použít. Můžete to provést vložením tyto hodnoty v Terraform skripty, jak je popsáno v [vytvořit základní infrastruktura pomocí Terraform](terraform-create-complete-vm.md). Alternativně můžete nastavit následující proměnné prostředí (a tedy vyhnout se omylem přihlašuje nebo sdílení přihlašovacích údajů):

- ARM_SUBSCRIPTION_ID
- ARM_CLIENT_ID
- ARM_CLIENT_SECRET
- ARM_TENANT_ID

Chcete-li nastavit tyto proměnné můžete použít tento ukázkový skript prostředí:

```
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id
```

Kromě toho pokud použijete Terraform s Azure v Číně nebo buď Azure Government nebo Azure v Německu, budete muset nastavit proměnnou prostředí odpovídajícím způsobem.

## <a name="next-steps"></a>Další kroky
Teď už máte nainstalovaný Terraform a nakonfigurovat přihlašovací údaje Azure, takže můžete začít nasazovat infrastruktury do vašeho předplatného Azure. Dále se naučíte, jak [vytvoření infrastruktury s Terraform](terraform-create-complete-vm.md).
