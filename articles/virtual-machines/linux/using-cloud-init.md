---
title: "Přehled podpory init cloudu pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Přehled možností init cloudu v Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 3670676032eb71a5339bb1219cb794366b912147
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-in-azure"></a>Použít cloudové init k přizpůsobení virtuálního počítače s Linuxem v Azure
V tomto článku se dozvíte, jak používat [cloudu init](https://cloudinit.readthedocs.io) ke konfiguraci virtuálního počítače (VM) nebo virtuální počítač sadách škálování (VMSS) na zřizování čas v Azure. Tyto skripty cloudu init spustit při prvním spuštění počítače po prostředky se zřizují Azure.  

## <a name="cloud-init-overview"></a>Init cloudu – přehled
[Init cloudu](https://cloudinit.readthedocs.io) je často používaný přístup k přizpůsobení virtuálního počítače s Linuxem, jako při prvním spuštění. Init cloudu můžete použít k instalaci balíčků a zapisovat soubory nebo konfigurace zabezpečení a uživatelů. Protože init cloudu je volána v průběhu procesu počáteční spouštění, nejsou žádné další kroky nebo požadované agenty použít konfiguraci.  Další informace o tom, jak správně formátu vaše `#cloud-config` soubory, najdete v článku [web dokumentace cloudu init](http://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config`fiels jsou textové soubory kódovaný jako base64.

Init cloudu také funguje v různých distribucí. Například nepoužívejte **výstižný get instalace** nebo **yum nainstalovat** nainstalovat balíček. Místo toho můžete definovat seznam balíčků pro instalaci. Init cloudu automaticky používá nástroj pro správu nativní balíčku pro distro, kterou vyberete.

 Aktivně Pracujeme s našimi potvrzená distro partnery Linux aby bylo možné používat cloudové inicializací povoleno imagím v Azure marketplace. Tyto bitové kopie budou vaše cloudové init nasazení a konfigurace bezproblémově pracovat virtuálních počítačů a virtuálních počítačů škálování sady (VMSS). Následující tabulka popisuje aktuální dostupnosti cloudu inicializací povoleno bitové kopie na platformě Azure:

| Vydavatel | Nabídka | Skladová jednotka (SKU) | Verze | init cloudu připravené
|:--- |:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |16.04 LTS |nejnovější |ano | 
|Canonical |UbuntuServer |14.04.5-LTS |nejnovější |ano |
|CoreOS |CoreOS |Stable |nejnovější |ano |
|OpenLogic |CentOS |7 CI |nejnovější |verze Preview |
|RedHat |RHEL |7 NEZPRACOVANÁ POLOŽEK KONFIGURACE |nejnovější |verze Preview |

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Jaký je rozdíl mezi init cloudu a agenta systému Linux (WALA)?
WALA je použít pro zřídit a nakonfigurovat virtuální počítače a zpracování rozšíření Azure agenta specifické pro platformu Azure. Jsme se zlepší úloh konfigurace virtuálních počítačů pro použití cloudu init místo agenta systému Linux Chcete-li povolit existující cloudu init zákazníkům používat jejich aktuální cloudu init skripty.  Pokud jste už investovali do cloudu init skriptů pro konfiguraci systémů Linux, jsou **žádné další nastavení potřebná** a umožňuje jim. 

Pokud nepoužijete přepínač příkazového řádku AzureCLI `--custom-data` v zřizování čas, WALA udělá zřizování parametrů požadovaných pro zřízení virtuálního počítače a dokončení nasazení s výchozím nastavením minimální virtuálních počítačů.  Pokud odkazujete cloudu inicializací `--custom-data` přepínače, ať je součástí vaší vlastní data (jednotlivá nastavení nebo úplné skriptu) přepíše výchozí hodnoty WALA definované. 

WALA konfigurace virtuálních počítačů jsou čas omezené pro práci v rámci maximální dobu zřizování virtuálních počítačů.  Konfigurace cloudu init použít pro virtuální počítače nemají časová omezení a nezpůsobí selhání pomocí vypršení časového limitu nasazení. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Nasazení cloudu inicializací povoleno virtuálního počítače
Nasazení virtuálního počítače v cloudu inicializací povolená je stejně jednoduché jako odkazující cloudu inicializací povolená distribuce během nasazení.  Údržby Linux distribuční programu muset zvolit povolení a integrovat cloudu init do jejich základní Azure publikované Image. Jakmile ověříte, že je povoleno cloudu inicializací bitovou kopii, kterou chcete nasadit, můžete AzureCLI nasazení bitové kopie. 

Prvním krokem při nasazení této bitové kopie je vytvoření skupiny prostředků pomocí [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Dalším krokem je vytvoření souboru ve své aktuální prostředí s názvem *cloudu init.txt* a vložte následující konfigurace. V tomto příkladu vytvoření souboru v prostředí cloudu není na místním počítači. Můžete použít libovolný editor, které chcete. Zadejte `sensible-editor cloud-init.txt` k vytvoření tohoto souboru a zobrazit seznam dostupných editory. Zvolte #1 používat **nano** editor. Ujistěte se, že je soubor celou cloudu init zkopírován správně, obzvláště první řádek:

```yaml
#cloud-config
package_upgrade: true
packages:
  -httpd
```
Stiskněte klávesu `ctrl-X` ukončíte souboru zadejte `y` uložit soubor a stiskněte klávesu `enter` potvrďte název souboru na ukončení.

Posledním krokem je vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#az_vm_create) příkaz. 

Následující příklad vytvoří virtuální počítač s názvem *centos74* a vytvoří klíče SSH, pokud už neexistují ve výchozím umístění klíče. Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.  Použití `--custom-data` parametr předávat do cloudu init konfiguračního souboru. Zadejte úplnou cestu k *cloudu init.txt* konfigurace, pokud jste uložili soubor mimo pracovní adresář existuje. Následující příklad vytvoří virtuální počítač s názvem *centos74*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

Po vytvoření virtuálního počítače, rozhraní příkazového řádku Azure obsahuje informace specifické pro vaše nasazení. Poznamenejte si `publicIpAddress`. Tato adresa se používá pro přístup k virtuálnímu počítači.  Jak dlouho trvá delší dobu pro virtuální počítač, který se má vytvořit, balíčky určené k instalaci a aplikaci spusťte. Existují úlohy na pozadí, které dál běžet po rozhraní příkazového řádku Azure se vrátíte do řádku. SSH můžete do virtuálního počítače a postupujte podle kroků uvedených v části řešení potíží k zobrazení protokolů init cloudu. 

## <a name="troubleshooting-cloud-init"></a>Řešení potíží s inicializací cloudu
Jakmile zřídil virtuální počítač, cloudu init spustí prostřednictvím všechny moduly a skriptu definovaný v `--custom-data` Chcete-li nakonfigurovat virtuální počítač.  Pokud potřebujete vyřešit všechny chyby nebo opomenutí z konfigurace, budete muset vyhledat název modulu (`disk_setup` nebo `runcmd` třeba) v cloudu init protokolu - umístěný v **/var/log/cloud-init.log**.

> [!NOTE]
> Ne každý selhání modulu má za následek závažné init cloudu celkové Chyba konfigurace. Například pomocí `runcmd` modulu, pokud skript selže, cloudu init stále oznámí zřizování bylo úspěšné, protože modul runcmd provést.

Další podrobnosti o cloudu init protokolování, najdete v části [cloudu init dokumentace](http://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Další kroky
Příklady cloudu init změny konfigurace najdete v následujících dokumentech:
 
- [Přidání další uživatele Linux do virtuálního počítače](cloudinit-add-user.md)
- [Spusťte Správce balíčků aktualizovat existující balíčky na při prvním spuštění](cloudinit-update-vm.md)
- [Změňte název místního hostitele virtuálního počítače](cloudinit-update-vm-hostname.md) 
- [Instalace balíčku aplikace, aktualizace konfigurační soubory a vložit klíče](tutorial-automate-vm-deployment.md)