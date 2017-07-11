---
title: "Rychlý start Azure – Vytvoření virtuálního počítače pomocí portálu | Dokumentace Microsoftu"
description: "Rychlý start Azure – Vytvoření virtuálního počítače pomocí portálu"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 8c51af189e8086a509d44546882e1b26605dddae
ms.contentlocale: cs-cz
ms.lasthandoff: 05/31/2017

---

<a id="create-a-linux-virtual-machine-with-the-azure-portal" class="xliff"></a>

# Vytvoření virtuálního počítače s Linuxem pomocí webu Azure Portal

Virtuální počítače Azure je možné vytvářet na webu Azure Portal. Tato metoda poskytuje uživatelské rozhraní v prohlížeči, pomocí kterého můžete vytvářet a konfigurovat virtuální počítače a všechny související prostředky. Tento Rychlý start prochází jednotlivé kroky k vytvoření virtuálního počítače a instalaci webového serveru na virtuálním počítači.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

<a id="create-ssh-key-pair" class="xliff"></a>

## Vytvoření páru klíčů SSH

K dokončení tohoto rychlého startu potřebujete pár klíčů SSH. Pokud máte existující pár klíčů SSH, můžete tento krok přeskočit.

Z prostředí Bash spusťte tento příkaz a postupujte podle pokynů na obrazovce. Výstup příkazu zahrnuje název souboru veřejného klíče. Zkopírujte obsah souboru veřejného klíče do schránky.

```bash
ssh-keygen -t rsa -b 2048
```

<a id="log-in-to-azure" class="xliff"></a>

## Přihlaste se k Azure. 

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

<a id="create-virtual-machine" class="xliff"></a>

## Vytvoření virtuálního počítače

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.

2. Vyberte **Compute**, pak **Ubuntu Server 16.04 LTS** a ujistěte se, že je vybraný model nasazení **Resource Manager**. Klikněte na tlačítko **Vytvořit**. 

3. Zadejte informace o virtuálním počítači. Jako **Typ ověřování** vyberte **Veřejný klíč SSH**. Při vkládání veřejného klíče SSH nezapomeňte odebrat počáteční a koncové prázdné znaky. Jakmile budete hotovi, klikněte na **OK**.

    ![Zadání základních informací o virtuálním počítači v okně portálu](./media/quick-create-portal/create-vm-portal-basic-blade.png)

4. Vyberte velikost virtuálního počítače. Pokud chcete zobrazit další velikosti, vyberte **Zobrazit všechny** nebo změňte filtr **Podporovaný typ disku**. 

    ![Snímek obrazovky zobrazující velikosti virtuálních počítačů](./media/quick-create-portal/create-linux-vm-portal-sizes.png)  

5. V okně Nastavení v části **Použít spravované disky** vyberte **Ano**, pro zbytek nastavení ponechte výchozí hodnoty a klikněte na **OK**.

6. Na stránce Souhrn kliknutím na **Ok** spusťte nasazení virtuálního počítače.

7. Virtuální počítač se připne na řídicí panel webu Azure Portal. Po dokončení nasazení se automaticky otevře okno souhrnu virtuálního počítače.


<a id="connect-to-virtual-machine" class="xliff"></a>

## Připojení k virtuálnímu počítači

Vytvořte připojení SSH k virtuálnímu počítači.

1. Klikněte na tlačítko **Vytvořit** v okně virtuálního počítače. Tlačítko Připojit zobrazí připojovací řetězec SSH, pomocí kterého se můžete připojit k virtuálnímu počítači.

    ![Portál 9](./media/quick-create-portal/portal-quick-start-9.png) 

2. Spuštěním následujícího příkazu vytvořte relaci SSH. Nahraďte připojovací řetězec tím, který jste zkopírovali z webu Azure Portal.

```bash 
ssh azureuser@40.112.21.50
```

<a id="install-nginx" class="xliff"></a>

## Instalace serveru NGINX

Pomocí následujícího skriptu bash provedete aktualizaci zdrojů balíčku a nainstalujete nejnovější balíček NGINX. 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Až budete hotovi, ukončete relaci SSH a vraťte se do vlastností virtuálního počítače na webu Azure Portal.


<a id="open-port-80-for-web-traffic" class="xliff"></a>

## Otevření portu 80 pro webový provoz 

Skupina zabezpečení sítě (NSG) zabezpečuje příchozí a odchozí provoz. Když se virtuální počítač vytvoří na webu Azure Portal, pro připojení SSH se vytvoří příchozí pravidlo na portu 22. Protože je tento virtuální počítač hostitelem webového serveru, je potřeba vytvořit pravidlo NSG pro port 80.

1. Na virtuálním počítači klikněte na název **skupiny prostředků**.
2. Vyberte **skupinu zabezpečení sítě**. NSG můžete identifikovat pomocí sloupce **Typ**. 
3. V nabídce vlevo v části Nastavení klikněte na **Příchozí pravidla zabezpečení**.
4. Klikněte na **Přidat**.
5. Do pole **Název** zadejte **http**. Zkontrolujte, že **Rozsah portů** je nastavený na 80 a **Akce** je nastavená na **Povolit**. 
6. Klikněte na **OK**.


<a id="view-the-nginx-welcome-page" class="xliff"></a>

## Zobrazení úvodní stránky serveru NGINX

Když je teď NGINX nainstalovaný a port 80 k virtuálnímu počítači otevřený, webový server je přístupný z internetu. Otevřete webový prohlížeč a zadejte veřejnou IP adresu virtuálního počítače. Veřejnou IP adresu najdete v okně virtuálního počítače na webu Azure Portal.

![Výchozí web NGINX](./media/quick-create-cli/nginx.png) 

<a id="clean-up-resources" class="xliff"></a>

## Vyčištění prostředků

Pokud už je nepotřebujete, odstraňte skupinu prostředků, virtuální počítač a všechny související prostředky. Provedete to tak, že v okně virtuálního počítače vyberete skupinu prostředků a kliknete na **Odstranit**.

<a id="next-steps" class="xliff"></a>

## Další kroky

V tomto Rychlém startu jste nasadili jednoduchý virtuální počítač a pravidlo skupiny zabezpečení sítě a nainstalovali jste webový server. Další informace o virtuálních počítačích Azure najdete v kurzu pro virtuální počítače s Linuxem.

> [!div class="nextstepaction"]
> [Kurzy pro virtuální počítače Azure s Linuxem](./tutorial-manage-vm.md)

