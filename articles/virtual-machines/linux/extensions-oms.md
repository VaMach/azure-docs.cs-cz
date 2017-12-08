---
title: "Rozšíření virtuálního počítače OMS Azure pro Linux | Microsoft Docs"
description: "Nasaďte agenta OMS na virtuální počítač s Linuxem pomocí rozšíření virtuálního počítače."
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: dcb7a777c66200c5046a6ad34dc4ff5d346f13e0
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="oms-virtual-machine-extension-for-linux"></a>OMS rozšíření virtuálního počítače pro Linux

## <a name="overview"></a>Přehled

Operations Management Suite (OMS) poskytuje možnosti nápravy monitorování, výstrahy a výstrahy v cloudové a místní prostředky. Rozšíření virtuálního počítače OMS agenta pro Linux je publikována a společnost Microsoft podporuje. Rozšíření nainstaluje agenta OMS na virtuálních počítačích Azure a zaregistruje virtuální počítače do existující pracovní prostor OMS. Tento dokument podrobně popisuje podporované platformy, konfigurace a možnosti nasazení pro rozšíření virtuálního počítače OMS pro Linux.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém

Rozšíření agenta OMS se může spouštět na těchto Linuxových distribucích.

| Distribuce | Verze |
|---|---|
| CentOS Linux | 5, 6 a 7 |
| Oracle Linux | 5, 6 a 7 |
| Red Hat Enterprise Linux Server | 5, 6 a 7 |
| Debian GNU/Linux | 6, 7 a 8 |
| Ubuntu | 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS |
| SUSE Linux Enterprise Server | 11 a 12 |

### <a name="azure-security-center"></a>Azure Security Center

Azure Security Center automaticky zřídí agenta OMS a připojí pomocí výchozí pracovní prostor log analytics předplatného Azure. Pokud používáte Azure Security Center, se nespustí provede kroky v tomto dokumentu. Tím přepíšete nakonfigurované pracovní prostor a přerušení připojení s Azure Security Center.

### <a name="internet-connectivity"></a>Připojení k internetu

Rozšíření OMS agenta pro Linux vyžaduje, aby cílový virtuální počítač je připojený k Internetu. 

## <a name="extension-schema"></a>Rozšíření schématu

Následujícím kódu JSON znázorňuje schéma pro rozšíření agenta OMS. Rozšíření vyžaduje ID a klíč pracovního prostoru z cílový pracovní prostor OMS; Tyto hodnoty můžete najít na portálu OMS. Vzhledem k tomu, že klíč pracovního prostoru by měl být považován za citlivá data, by měly být uložené v chráněném nastavení konfigurace. Data Azure nastavení rozšíření chráněný virtuální počítač je zašifrovaná a dešifrovat jenom na cílový virtuální počítač. Všimněte si, že **workspaceId** a **workspaceKey** malých a velkých písmen.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

### <a name="property-values"></a>Hodnoty vlastností

| Name (Název) | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.4 |
| ID pracovního prostoru (např.) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (např.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |


## <a name="template-deployment"></a>Nasazení šablon

Rozšíření virtuálního počítače Azure se dá nasadit pomocí šablon Azure Resource Manager. Šablony jsou ideální, pokud nasazujete jednu nebo více virtuálních počítačů, které vyžadují konfiguraci nasazení post jako je registrace k OMS. Ukázka šablonu Resource Manager, která obsahuje rozšíření virtuálního počítače agenta OMS naleznete na [Azure rychlý Start Galerie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Konfigurace JSON pro rozšíření virtuálního počítače můžete vnořit prostředek virtuálního počítače nebo umístěn na kořenový server WSUS nebo nejvyšší úrovně šablony JSON Resource Manager. Umístění konfigurace JSON ovlivňuje hodnota název prostředku a typem. Další informace najdete v tématu [nastavte název a typ pro podřízené prostředky](../../azure-resource-manager/resource-manager-template-child-resource.md). 

V následujícím příkladu se předpokládá, že je rozšíření OMS vnořit prostředek virtuálního počítače. Při vnoření rozšíření prostředků, JSON je umístěn v `"resources": []` objektu virtuálního počítače.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

Při vkládání rozšíření JSON v kořenovém adresáři šablony, názvu prostředku obsahuje odkaz na nadřazený virtuální počítač a typ odráží vnořené konfigurace.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.4",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Nasazení Azure CLI

Rozhraní příkazového řádku Azure můžete použít k nasazení rozšíření virtuálního počítače agenta OMS na existující virtuální počítač. Nahraďte klíč OMS a OMS ID s těmi, která z pracovního prostoru OMS. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.4 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Řešení potíží a podpora

### <a name="troubleshoot"></a>Řešení potíží

Z portálu Azure a pomocí rozhraní příkazového řádku Azure je možné načíst data o stavu nasazení rozšíření. Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz pomocí rozhraní příkazového řádku Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Výstupu spuštění rozšíření je zaznamenána do následujícího souboru:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Kódy chyb a jejich významů

| Kód chyby | Význam | Možné akce |
| :---: | --- | --- |
| 10 | Virtuální počítač je již připojen k pracovnímu prostoru OMS | Připojit virtuální počítač do pracovního prostoru, určená ve schématu rozšíření, nastavena na hodnotu false v nastavení veřejných stopOnMultipleConnections nebo odeberte tuto vlastnost. Tento virtuální počítač získá účtován pro každý pracovní prostor po připojení. |
| 11 | Neplatná konfigurace poskytuje rozšíření | Postupujte podle předchozích ukázkách nastavit všechny hodnoty vlastností nezbytné pro nasazení. |
| 12 | Správce balíčků dpkg je uzamčena. | Zajistěte, aby všechny dpkg operace aktualizace na počítači dokončení a akci opakujte. |
| 20 | Povolit názvem předčasně | [Aktualizovat Azure Linux Agent](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) na nejnovější dostupnou verzi. |
| 51 | Toto rozšíření není podporována na operační systém Virtuálního počítače | |
| 55 | Nelze připojit ke službě Microsoft Operations Management Suite | Zkontrolujte, jestli systém má přístup k Internetu nebo že bylo zadáno platný proxy server HTTP. Kromě toho zkontrolujte správnost ID pracovního prostoru. |

Další informace o odstraňování potíží naleznete na [Průvodce odstraňováním potíží OMS agenta pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#).

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
