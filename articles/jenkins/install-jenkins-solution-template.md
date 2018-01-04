---
title: "Vytvoření serveru Jenkins v Azure"
description: "Instalace Jenkinse na virtuálním počítači Azure s Linuxem ze šablony řešení Jenkins a sestavení ukázkové aplikace v Javě."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 0cf24d8db845beb63b1b895f895da761488fd8ec
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-the-azure-portal"></a>Vytvoření serveru Jenkins na virtuálním počítači Azure s Linuxem pomocí webu Azure Portal

V tomto rychlém startu se dozvíte, jak na virtuálním počítači s Ubuntu Linuxem nainstalovat [Jenkinse](https://jenkins.io) s nakonfigurovanými nástroji a moduly plug-in pro práci s Azure. Až budete hotovi, budete mít v Azure spuštěný server Jenkins sestavující ukázkovou aplikaci v Javě z [GitHubu](https://github.com).

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure
* Přístup k SSH na příkazovém řádku vašeho počítače (například prostředí Bash nebo nástroj [PuTTY](http://www.putty.org/))

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a>Vytvoření virtuálního počítače s Jenkinsem ze šablony řešení

Ve webovém prohlížeči otevřete [image Jenkinse na webu Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) a na levé straně stránky vyberte **ZÍSKAT**. Zkontrolujte podrobnosti o cenách a vyberte **Pokračovat**, pak vyberte **Vytvořit** a nakonfigurujte server Jenkins na webu Azure Portal. 
   
![Dialogové okno na webu Azure Portal](./media/install-jenkins-solution-template/ap-create.png)

Na kartě **Konfigurace základního nastavení** vyplňte následující pole:

![Konfigurace základního nastavení](./media/install-jenkins-solution-template/ap-basic.png)

* Jako **Název** použijte **Jenkins**.
* Zadejte **Uživatelské jméno**. Uživatelské jméno musí splňovat [určité požadavky](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
* Jako **Typ ověřování** vyberte **Heslo** a zadejte heslo. Heslo musí obsahovat velké písmeno, číslici a jeden speciální znak.
* Jako **Skupina prostředků** použijte **myJenkinsResourceGroup**.
* Z rozevíracího seznamu **Umístění** zvolte [Oblast Azure](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) **USA – východ**.

Vyberte **OK** a pokračujte na kartu **Konfigurace dalších možností**. Zadejte jedinečný název domény pro identifikaci serveru Jenkins a vyberte **OK**.

![Nastavení dalších možností](./media/install-jenkins-solution-template/ap-addtional.png)  

 Jakmile úspěšně proběhne ověření, znovu vyberte **OK** na kartě **Souhrn**. Nakonec vyberte **Koupit** a vytvoří se virtuální počítač s Jenkinsem. Jakmile bude váš server připravený, na webu Azure Portal se vám zobrazí oznámení:   

![Oznámení o připravenosti Jenkinse](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-to-jenkins"></a>Připojení k Jenkinsu

Ve webovém prohlížeči přejděte ke svému virtuálnímu počítači (například http://jenkins2517454.eastus.cloudapp.azure.com/). Konzola Jenkinse není přístupná přes nezabezpečený protokol HTTP, proto se na stránce zobrazí pokyny pro zabezpečený přístup ke konzole Jenkinse z vašeho počítače pomocí tunelu SSH.

![Odemknutí Jenkinse](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Nastavte tunel z příkazového řádku s použitím příkazu `ssh` uvedeného na stránce, ve kterém nahradíte `username` názvem uživatele s právy pro správu virtuálního počítače, který jste zvolili dříve při nastavování virtuálního počítače ze šablony řešení.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Po spuštění tunelu přejděte na místním počítači na adresu http://localhost:8080/. 

Získejte počáteční heslo spuštěním následujícího příkazu v příkazovém řádku, zatímco jste připojeni k virtuálnímu počítači s Jenkinsem prostřednictvím SSH.

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

Pomocí tohoto počátečního hesla poprvé odemkněte řídicí panel Jenkinse.

![Odemknutí Jenkinse](./media/install-jenkins-solution-template/jenkins-unlock.png)

Na další stránce vyberte **Install suggested plugins** (Nainstalovat doporučené moduly plug-in) a pak vytvořte uživatele s právy pro správu Jenkinse, který bude sloužit pro přístup k řídicímu panelu Jenkinse.

![Jenkins je připraven!](./media/install-jenkins-solution-template/jenkins-welcome.png)

Server Jenkins je teď připravený sestavovat kód.

## <a name="create-your-first-job"></a>Vytvoření první úlohy

V konzole Jenkinse vyberte **Create new jobs** (Vytvořit nové úlohy), zadejte název **mySampleApp**, vyberte **Freestyle project** (Volný projekt) a pak vyberte **OK**.

![Vytvoření nové úlohy](./media/install-jenkins-solution-template/jenkins-new-job.png) 

Vyberte kartu **Source Code Management** (Správa zdrojového kódu), povolte **Git** a do pole **Repository URL** (Adresa URL úložiště) zadejte následující adresu URL: `https://github.com/spring-guides/gs-spring-boot.git`.

![Definování úložiště Git](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

Vyberte kartu **Build** (Sestavení) a pak vyberte **Add build step** (Přidat krok sestavení) a **Invoke Gradle script** (Vyvolání skriptu Gradle). Vyberte **Use Gradle Wrapper** (Použít obálku Gradle) a pak zadejte `complete` do pole **Wrapper location** (Umístění obálky) a `build` do pole **Tasks** (Úlohy).

![Použití obálky Gradle k sestavení](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

Vyberte **Advanced...** (Upřesnit...) a pak zadejte `complete` do pole **Root Build script** (Kořenový skript sestavení). Vyberte **Uložit**.

![Nastavení upřesňujících nastavení v kroku sestavení obálky Gradle](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a>Sestavení kódu

Vyberte **Build Now** (Sestavit). Kód se zkompiluje a ukázková aplikace se zabalí do balíčku. Po dokončení sestavení vyberte odkaz **Workspace** (Pracovní prostor) pro projekt.

![Přechod do pracovního prostoru a získání souboru JAR ze sestavení](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

Přejděte do umístění `complete/build/libs` a ujistěte se, že je tam soubor `gs-spring-boot-0.1.0.jar`, abyste ověřili, že sestavení proběhlo úspěšně. Váš server Jenkins je teď připravený k sestavování vašich vlastních projektů v Azure.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Přidání virtuálních počítačů Azure jako agentů Jenkinse](jenkins-azure-vm-agents.md)
