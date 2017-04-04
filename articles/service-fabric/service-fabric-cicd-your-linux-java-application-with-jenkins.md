---
title: "Průběžné sestavování a integrace linuxových aplikací Azure Service Fabric v Javě pomocí Jenkinse | Dokumentace Microsoftu"
description: "Průběžné sestavování a integrace linuxových aplikací v Javě pomocí Jenkinse"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 71e3d130f22515d22dc7f486f3dede936b874049
ms.lasthandoff: 03/25/2017


---
# <a name="use-jenkins-to-build-and-deploy-your-linux-java-application"></a>Sestavování a nasazování linuxových aplikací v Javě pomocí Jenkinse
Jenkins je oblíbený nástroj pro průběžnou integraci a nasazování aplikací. Tady je postup, kterým můžete sestavit a nasadit aplikaci Azure Service Fabric s využitím Jenkinse.

## <a name="general-prerequisites"></a>Obecné požadavky
- Máte lokálně nainstalovaný Git. Odpovídající verzi Gitu (v závislosti na operačním systému) můžete nainstalovat ze [stránky pro stažení Gitu](https://git-scm.com/downloads). Pokud jste ještě Git nepoužívali, další informace najdete v [dokumentaci ke Gitu](https://git-scm.com/docs).
- Máte po ruce modul plug-in Jenkinse pro Service Fabric. Můžete ho stáhnout ze stránky pro [stažení Service Fabric](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Nastavení Jenkinse uvnitř clusteru Service Fabric

Jenkinse můžete nastavit uvnitř clusteru Service Fabric nebo mimo něj. Následující části vysvětlují, jak ho nastavit v clusteru.

### <a name="prerequisites"></a>Požadavky
1. Máte připravený cluster Service Fabric s Linuxem. Na clusteru Service Fabric vytvořeném z webu Azure Portal je už nainstalovaný Docker. Pokud cluster spouštíte místně, můžete ke kontrole, jestli je Docker nainstalovaný, použít příkaz ``docker info``. Pokud není nainstalovaný, nainstalujte ho pomocí následujících příkazů:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. Máte nasazenou aplikaci Service Fabric typu kontejner v clusteru pomocí následujícího postupu:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker
cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080   # Azure CLI cluster connect command
bash Scripts/install.sh
```
Tím se do clusteru nainstaluje kontejner s Jenkinsem, který můžete monitorovat pomocí Service Fabric Exploreru.

### <a name="steps"></a>Kroky
1. V prohlížeči přejděte na ``http://PublicIPorFQDN:8081``. Najdete tam cestu k počátečnímu heslu správce vyžadovanému k přihlášení. Můžete Jenkinse dál používat jako uživatel s oprávněními správce. Nebo můžete po přihlášení pomocí počátečního účtu správce vytvořit a změnit uživatele.

   > [!NOTE]
   > Zajistěte, aby během vytváření clusteru byl jako koncový bod aplikace zadaný port 8081.
   >

2. Získejte ID instance kontejneru pomocí příkazu ``docker ps -a``.
3. Přihlaste se přes SSH (Secure Shell) ke kontejneru a vložte cestu, která se zobrazila na portálu Jenkinse. Pokud se na portálu zobrazila například cesta `PATH_TO_INITIAL_ADMIN_PASSWORD`, spusťte:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Nastavte GitHub pro práci s Jenkinsem, a to pomocí kroků uvedených v tématu věnovaném [vygenerování nového klíče SSH a jeho přidání k agentovi SSH](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    * Pomocí pokynů z GitHubu vygenerujte klíč SSH a přidejte ho do účtu GitHubu, který je hostitelem vašeho úložiště.
    * Příkazy popsané u předchozího odkazu spusťte v prostředí Jenkins Dockeru (ne na hostiteli).
    * Pokud se chcete k prostředí Jenkinse přihlásit z hostitele, použijte následující příkaz:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Nastavení Jenkinse mimo cluster Service Fabric

Jenkinse můžete nastavit uvnitř clusteru Service Fabric nebo mimo něj. Následující části vysvětlují, jak ho nastavit mimo cluster.

### <a name="prerequisites"></a>Požadavky
Potřebujete mít nainstalovaný Docker. Pomocí následujících příkazů můžete Docker nainstalovat z terminálu:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Když teď v terminálu spustíte příkaz ``docker info``, na výstupu by se mělo zobrazit, že je spuštěná služba Docker.

### <a name="steps"></a>Kroky
  1. Stáhněte si image kontejneru s Jenkinsem pro Service Fabric: ``docker pull raunakpandya/jenkins:v1``
  2. Spusťte image kontejneru: ``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``
  3. Získejte ID instance image kontejneru. Pomocí příkazu ``docker ps –a`` můžete vypsat všechny kontejnery Dockeru.
  4. Přihlaste se k portálu Jenkinse pomocí následujících kroků:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Pokud ID kontejneru je 2d24a73b5964, použijte 2d24.
    * Toto heslo se vyžaduje pro přihlášení k řídicímu panelu Jenkinse z portálu, což je ``http://<HOST-IP>:8080``.
    * Jakmile se poprvé přihlásíte, můžete si vytvořit vlastní uživatelský účet, který budete používat pro další účely, nebo můžete dál používat účet uživatele s právy pro správu. Když vytvoříte uživatele, budete s ním muset pokračovat.
  5. Nastavte GitHub pro práci s Jenkinsem, a to pomocí kroků uvedených v tématu věnovaném [vygenerování nového klíče SSH a jeho přidání k agentovi SSH](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
        * Pomocí pokynů z GitHubu vygenerujte klíč SSH a přidejte ho do účtu GitHubu, který je hostitelem úložiště.
        * Příkazy popsané u předchozího odkazu spusťte v prostředí Jenkins Dockeru (ne na hostiteli).
        * Pokud se chcete k prostředí Jenkinse přihlásit z hostitele, použijte následující příkazy:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Zkontrolujte, že cluster nebo počítač, ve kterém se hostuje image kontejneru Jenkinse, má veřejnou IP adresu. To umožňuje, aby instance Jenkinse dostávala oznámení z GitHubu.

## <a name="install-the-service-fabric-jenkins-plug-in-from-the-portal"></a>Instalace modulu plug-in Jenkinse pro Service Fabric z portálu

1. Přejděte na ``http://PublicIPorFQDN:8081``.
2. Na řídicím panelu vyberte **Manage Jenkins** (Správa Jenkinse) > **Manage Plugins** (Správa modulů plug-in) > **Advanced** (Rozšířené).
Tady můžete nahrát modul plug-in. Vyberte **Choose file** (Zvolit soubor) a potom vyberte soubor **serviceFabric.hpi**, který jste si stáhli v části Požadavky. Jakmile vyberete **Upload** (Nahrát), Jenkins modul plug-in automaticky nainstaluje. Pokud je vyžadováno restartování, povolte ho.

## <a name="create-and-configure-a-jenkins-job"></a>Vytvoření a konfigurace úlohy Jenkinse

1. Na řídicím panelu vytvořte **novou položku**.
2. Zadejte název položky (třeba **MyJob**). Vyberte **free-style project** (volný styl projektu) a klikněte na **OK**.
3. Přejděte na stránku úlohy a klikněte na **Configure** (Konfigurovat).

   a. V sekci obecných informací v části **GitHub project** (Projekt GitHub) zadejte adresu URL projektu. Tato adresa je hostitelem aplikace Service Fabric v Javě, kterou chcete integrovat s postupy průběžného nasazování a integrace (CI/CD) Jenkinse (např. ``https://github.com/sayantancs/SFJenkins``).

   b. V části **Source Code Management** (Správa zdrojového kódu) vyberte **Git**. Zadejte adresu URL úložiště, které je hostitelem aplikace Service Fabric v Javě, kterou chcete integrovat s postupy CI/CD Jenkinse (např. ``https://github.com/sayantancs/SFJenkins.git``). V této části můžete také zadat, jaká větev se má sestavit (například **/master**).
4. Nakonfigurujte *GitHub* (který je hostitelem úložiště) tak, aby mohl komunikovat s Jenkinsem. Použijte k tomu následující postup:

   a. Přejděte na stránku vašeho úložiště Github. Přejděte do části **Settings** (Nastavení) > **Integrations and Services** (Integrace a služby).

   b. Vyberte **Add Service** (Přidat službu), zadejte **Jenkins** a vyberte **Jenkins-Github plugin** (Modul plug-in Jenkins-Github).

   c. Zadejte adresu URL webhooku Jenkinse (ve výchozím nastavení by měla být ``http://<PublicIPorFQDN>:8081/github-webhook/``). Klikněte na **add/update service** (Přidat/aktualizovat službu).

   d. Do instance Jenkinse se odešle testovací událost. V GitHubu by se měla vedle webhooku zobrazit zelená značka zaškrtnutí a váš projekt se sestaví.

   e. V části **Build Triggers** (Triggery sestavení) vyberte požadovanou možnost sestavení. V tomto příkladě použití chcete aktivovat sestavení při každém vložení metodou Push do úložiště. Proto vyberete **GitHub hook trigger for GITScm polling** (Trigger webhooku GitHubu pro cyklické dotazování GitHubu). (Dřív se tato možnost nazývala **Build when a change is pushed to GitHub** (Sestavit při vložení změny metodou Push do GitHubu).

   f. V části **Build** (Sestavení) z rozevírací nabídky **Add build step** (Přidat krok sestavení) vyberte možnost **Invoke Gradle Script** (Vyvolání skriptu Gradle). Ve widgetu, který se zobrazí, zadejte do pole **Root build script** (Kořenový skript sestavení) cestu ke kořenovému skriptu sestavení pro vaši aplikaci. Sestavení si ze zadané cesty vezme soubor build.gradle a bude pracovat odpovídajícím způsobem. Pokud vytvoříte projekt ``MyActor`` (pomocí modulu plug-in Eclipse nebo generátoru Yeoman), pole pro kořenový skript sestavení by mělo obsahovat ``${WORKSPACE}/MyActor``. Příklad toho, jak by to mělo vypadat, najdete na následujícím snímku:

    ![Akce sestavení v Jenkinsu pro Service Fabric][build-step]

   g. V rozevírací nabídce **Post-Build Actions** (Akce po sestavení) vyberte **Deploy Service Fabric Project** (Nasazení projektu Service Fabric). V této části je potřeba zadat podrobnosti o clusteru, ve kterém se bude nasazovat aplikace Service Fabric zkompilovaná Jenkinsem. Můžete také zadat další podrobnosti o aplikaci, které se používají k jejímu nasazení. Příklad toho, jak by to mělo vypadat, najdete na následujícím snímku:

    ![Akce sestavení v Jenkinsu pro Service Fabric][post-build-step]

   > [!NOTE]
   > Zde uvedený cluster může být stejný jako cluster, který hostuje aplikaci Jenkins typu kontejner, pokud k nasazení image kontejneru Jenkins používáte Service Fabric.
   >

## <a name="next-steps"></a>Další kroky
GitHub a Jenkins jsou teď nakonfigurované. Můžete provést nějaké ukázkové změny v projektu například v úložišti ``MyActor``https://github.com/sayantancs/SFJenkins. Metodou Push změny vložte do vzdálené větve ``master`` (nebo jakékoli jiné větve, kterou jste si pro práci nakonfigurovali). Tím se aktivuje nakonfigurovaná úloha Jenkinse ``MyJob``. Tato úloha načte změny z GitHubu a použije je k sestavení aplikace, kterou nasadí do koncového bodu clusteru, který jste zadali v akcích po sestavení.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

