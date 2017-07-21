---
title: "Vytvoření prvního hlavního serveru Jenkinse na virtuálním počítači s Linuxem (Ubuntu) v Azure"
description: "Využijte k nasazení Jenkinse šablonu řešení."
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: multiple
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 748f1994d0ee5b6c6a988bca8e5b694b29c0b8c5
ms.contentlocale: cs-cz
ms.lasthandoff: 06/30/2017

---

<a id="create-your-first-jenkins-master-on-a-linux-ubuntu-vm-on-azure" class="xliff"></a>

# Vytvoření prvního hlavního serveru Jenkinse na virtuálním počítači s Linuxem (Ubuntu) v Azure

Tento rychlý start ukazuje, jak nainstalovat nejnovější stabilní verzi Jenkinse na virtuálním počítači s Linuxem (Ubuntu 14.04 LTS) společně s nástroji a moduly plug-in nakonfigurovanými pro práci s Azure. Mezi tyto nástroje patří:
<ul>
<li>Git pro správu zdrojového kódu</li>
<li>Modul plug-in Azure Credentials pro zabezpečené připojení</li>
<li>Modul plug-in Azure VM Agents pro elastické sestavování, testování a průběžnou integraci</li>
<li>Modul plug-in Azure Storage pro ukládání artefaktů</li>
<li>Azure CLI pro nasazování aplikací pomocí skriptů</li>
</ul>

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořit bezplatný účet Azure.
> * Vytvořit hlavní server Jenkinse na virtuálním počítači Azure pomocí šablony řešení. 
> * Provést počáteční konfiguraci Jenkinse.
> * Nainstalovat navrhované moduly plug-in.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

<a id="create-the-vm-in-azure-by-deploying-the-solution-template-for-jenkins" class="xliff"></a>

## Vytvoření virtuálního počítače v Azure nasazením šablony řešení pro Jenkinse

Pomocí šablon Azure pro rychlý start můžete v Azure rychle a spolehlivě nasazovat komplexní technologie.  Azure Resource Manager umožňuje zřizovat aplikace pomocí [deklarativní šablony](https://azure.microsoft.com/en-us/resources/templates/?term=jenkins). S jednou šablonou můžete nasadit několik služeb společně s jejich závislostmi. Stejnou šablonu můžete použít k opakovanému nasazení aplikace během každé fáze životního cyklu této aplikace.

Abyste porozuměli cenovým možnostem, podívejte se na informace o [plánech a cenách](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.jenkins?tab=PlansAndPrice) pro tuto šablonu.

Přejděte k [imagi Jenkinse na webu Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azure-oss.jenkins?tab=Overview) a klikněte na **ZÍSKAT**.  

Na webu Azure Portal klikněte na **Vytvořit**.  Tato šablona vyžaduje použití Resource Manageru, proto je rozevírací seznam modelu šablony neaktivní.
   
![Dialogové okno na webu Azure Portal](./media/install-jenkins-solution-template/ap-create.png)

Na kartě **Konfigurace základního nastavení**:

![Konfigurace základního nastavení](./media/install-jenkins-solution-template/ap-basic.png)

* Zadejte název instance Jenkinse.
* Vyberte typ disku virtuálního počítače.  Pro produkční úlohy zvolte větší virtuální počítač a jednotku SSD. Zajistíte tím lepší výkon.  Další informace o typech disků Azure si můžete přečíst [zde](https://docs.microsoft.com/en-us/azure/storage/storage-premium-storage).
* Uživatelské jméno: musí splňovat požadavky na délku a nesmí obsahovat vyhrazená slova ani nepodporované znaky. Jména jako „admin“ nejsou povolena.  Další informace a požadavky na uživatelské jméno a heslo najdete [zde](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq).
* Typ ověřování: vytvořte instanci zabezpečenou heslem nebo [veřejným klíčem SSH](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows). Pokud použijete heslo, musí splňovat 3 z těchto požadavků: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak.
* Jako typ vydané verze Jenkinse ponechte **LTS**.
* Vyberte předplatné.
* Vytvořte skupinu prostředků nebo použijte existující.
* Vyberte umístění.

Na kartě **Konfigurace dalších možností**:

![Nastavení dalších možností](./media/install-jenkins-solution-template/ap-addtional.png)

* Zadejte popisek názvu domény pro jedinečnou identifikaci hlavního serveru Jenkinse.

Kliknutím na **OK** přejděte k dalšímu kroku. 

Jakmile úspěšně proběhne ověření, klikněte na **OK** a stáhněte šablonu a parametry. 

Pak vyberte **Koupit** a zřiďte všechny prostředky.

<a id="setup-ssh-port-forwarding" class="xliff"></a>

## Nastavení přesměrování portu SSH

Instance Jenkinse ve výchozím nastavení používá protokol HTTP a naslouchá na portu 8080. Uživatelé by se neměli ověřovat prostřednictvím nezabezpečených protokolů.
    
Nastavte přesměrování portu, abyste zobrazili uživatelské rozhraní Jenkinse na místním počítači.

<a id="if-you-are-using-windows" class="xliff"></a>

### Pokud používáte Windows:

Nainstalujte PuTTY a pokud používáte k zabezpečení Jenkinse heslo, spusťte tento příkaz:
```
putty.exe -ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Pro přihlášení zadejte heslo.

![Pro přihlášení zadejte heslo.](./media/install-jenkins-solution-template/jenkins-pwd.png)

Pokud používáte SSH, spusťte tento příkaz:
```
putty -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

<a id="if-you-are-using-linux-or-mac" class="xliff"></a>

### Pokud používáte Linux nebo Mac:

Pokud používáte k zabezpečení hlavního serveru Jenkinse heslo, spusťte tento příkaz:
```
ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Pro přihlášení zadejte heslo.

Pokud používáte SSH, spusťte tento příkaz:
```
ssh -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

<a id="connect-to-jenkins" class="xliff"></a>

## Připojení k Jenkinsu
Po spuštění tunelu přejděte na místním počítači na adresu http://localhost:8080/.

Poprvé odemkněte řídicí panel Jenkinse pomocí počátečního hesla správce.

![Odemknutí Jenkinse](./media/install-jenkins-solution-template/jenkins-unlock.png)

Pokud chcete získat token, připojte se přes SSH k virtuálnímu počítači a spusťte příkaz `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.

![Odemknutí Jenkinse](./media/install-jenkins-solution-template/jenkins-ssh.png)

Zobrazí se výzva k instalaci navrhovaných modulů plug-in.

Dále vytvořte pro hlavní server Jenkinse uživatele s rolí správce.

Vaše instance Jenkinse je teď připravena k použití! Přístup k zobrazení jen pro čtení získáte tak, že přejdete na adresu http://\<Veřejný název DNS instance, kterou jste právě vytvořili\>.

![Jenkins je připraven!](./media/install-jenkins-solution-template/jenkins-welcome.png)

<a id="next-steps" class="xliff"></a>

## Další kroky

V tomto kurzu jste:

> [!div class="checklist"]
> * Vytvořili hlavní server Jenkinse pomocí šablony řešení.
> * Provedli počáteční konfiguraci Jenkinse.
> * Nainstalovali moduly plug-in.

Na tomto odkazu najdete informace o použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse.

> [!div class="nextstepaction"]
> [Virtuální počítače Azure jako agenti Jenkinse](jenkins-azure-vm-agents.md)

