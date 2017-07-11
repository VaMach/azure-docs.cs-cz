---
title: "Použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse"
description: "Agenti virtuálních počítačů Azure jako podřízené servery Jenkinse"
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: 5f2df414b4d0e8798b7ed6d90d0ea0fb79d42fc2
ms.contentlocale: cs-cz
ms.lasthandoff: 06/21/2017

---
<a id="use-azure-vm-agents-for-continuous-integration-with-jenkins" class="xliff"></a>

# Použití agentů virtuálních počítačů Azure pro průběžnou integraci pomocí Jenkinse

Tento rychlý start předvádí, jak pomocí modulu plug-in Jenkins Azure VM Agents vytvořit v Azure linuxového (Ubuntu) agenta na vyžádání.

<a id="prerequisites" class="xliff"></a>

## Požadavky

K dokončení tohoto rychlého startu je potřeba:

* Pokud ještě nemáte hlavní server Jenkinse, můžete začít s využitím [šablony řešení](install-jenkins-solution-template.md). 
* Pokud ještě nemáte instanční objekt Azure, přečtěte si téma [Vytvoření instančního objektu Azure pomocí Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json).

<a id="install-azure-vm-agents-plugin" class="xliff"></a>

## Instalace modulu plug-in Azure VM Agents

Pokud začínáte z [šablony řešení](install-jenkins-solution-template.md), modul plug-in Azure VM Agents je nainstalovaný na hlavním serveru Jenkinse.

Jinak nainstalujte modul plug-in **Azure VM Agents** z řídicího panelu Jenkinse.

<a id="configure-the-plugin" class="xliff"></a>

## Konfigurace modulu plug-in

* Na řídicím panelu Jenkinse klikněte na **Manage Jenkins (Správa Jenkinse) -> Configure System (Konfigurace systému)**. Přejděte do dolní části stránky a vyhledejte část s rozevíracím seznamem **Add new cloud** (Přidat nový cloud). Z nabídky vyberte **Microsoft Azure VM Agents**.
* Vyberte existující účet z rozevíracího seznamu Azure Credentials (Přihlašovací údaje Azure).  Pokud chcete přidat nový **Microsoft Azure Service Principal** (Instanční objekt Microsoft Azure), zadejte následující hodnoty: Subscription ID (ID předplatného), Client ID (ID klienta), Client Secret (Tajný klíč klienta) a OAuth 2.0 Token Endpoint (Koncový bod tokenu OAuth 2.0).

![Přihlašovací údaje Azure](./media/jenkins-azure-vm-agents/service-principal.png)

* Klikněte na **Verify configuration** (Ověřit konfiguraci) a zkontrolujte správnost konfigurace profilu.
* Uložte konfiguraci a pokračujte k dalšímu kroku.

<a id="template-configuration" class="xliff"></a>

## Konfigurace šablony

<a id="general-configuration" class="xliff"></a>

### Obecná konfigurace
Dále nakonfigurujte šablonu, která se použije k definování agenta virtuálního počítače Azure. 

* Kliknutím na **Add** (Přidat) přidejte šablonu. 
* Zadejte název nové šablony. 
* Jako popisek zadejte ubuntu. Tento popisek se používá během konfigurace úlohy.
* V poli se seznamem vyberte požadovanou oblast.
* Vyberte požadovanou velikost virtuálního počítače.
* Zadejte název účtu Azure Storage. Pokud pole ponecháte prázdné, použije se výchozí název jenkinsarmst.
* Zadejte dobu uchování v minutách. Toto nastavení definuje počet minut, po které může Jenkins čekat před automatickým odstraněním nečinného agenta. Pokud nechcete, aby se nečinní agenti automaticky odstraňovali, zadejte 0.

![Obecná konfigurace](./media/jenkins-azure-vm-agents/general-config.png)

<a id="image-configuration" class="xliff"></a>

### Konfigurace image

Pokud chcete vytvořit linuxového (Ubuntu) agenta, vyberte **Image reference** (Odkaz na image) a použijte následující konfiguraci jako příklad. Nejnovější podporované image Azure najdete na webu [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1).

* Image Publisher (Vydavatel image): Canonical
* Image Offer (Nabídka image): UbuntuServer
* Image Sku (Skladová jednotka image): 14.04.5-LTS
* Image version (Verze image): latest (nejnovější)
* OS Type (Typ operačního systému): Linux
* Launch method (Metoda spuštění): SSH
* Zadejte přihlašovací údaje správce
* Jako skript pro inicializaci virtuálního počítače zadejte:
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Konfigurace image](./media/jenkins-azure-vm-agents/image-config.png)

* Kliknutím na **Verify Template** (Ověřit šablonu) ověřte konfiguraci.
* Klikněte na **Uložit**.

<a id="create-a-job-in-jenkins" class="xliff"></a>

## Vytvoření úlohy v Jenkinsu

* Na řídicím panelu Jenkinse klikněte na **New Item** (Nová položka). 
* Zadejte název, vyberte **Freestyle project** (Volný projekt) a klikněte na **OK**.
* Na kartě **General** (Obecné) vyberte možnost „Restrict where project can be run“ (Omezit, kde je možné projekt spustit) a do pole Label Expression (Výraz popisku) zadejte ubuntu. V rozevíracím seznamu se teď zobrazí ubuntu.
* Klikněte na **Uložit**.

![Nastavení úlohy](./media/jenkins-azure-vm-agents/job-config.png)

<a id="build-your-new-project" class="xliff"></a>

## Sestavení nového projektu

* Vraťte se na řídicí panel Jenkinse.
* Klikněte pravým tlačítkem na novou úlohu, kterou jste vytvořili, a pak klikněte na **Build now** (Sestavit). Spustí se sestavování. 
* Jakmile bude sestavování dokončeno, přejděte na **Console output** (Výstup konzoly). Uvidíte, že se sestavení provedlo vzdáleně v Azure.

![Výstup konzoly](./media/jenkins-azure-vm-agents/console-output.png)

<a id="reference" class="xliff"></a>

## Referenční informace

* Video Azure Friday: [Průběžná integrace pomocí Jenkinse s využitím agentů virtuálních počítačů Azure](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)
* Informace o podpoře a možnosti konfigurace: [Wikiweb modulu plug-in Azure VM Agent pro Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) 


