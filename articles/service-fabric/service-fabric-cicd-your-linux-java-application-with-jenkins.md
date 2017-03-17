---
title: "Průběžné sestavování a nasazování linuxových aplikací v Javě pomocí Jenkinse | Dokumentace Microsoftu"
description: "Průběžné sestavování a nasazování linuxových aplikací v Javě pomocí Jenkinse"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: raunakp
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: sayantancs
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: d3c64e3ed78a5379592b5e4276c83d26f04d2e1c
ms.lasthandoff: 03/08/2017


---
# <a name="build-and-deploy-your-linux-java-application-using-jenkins"></a>Sestavování a nasazování linuxových aplikací v Javě pomocí Jenkinse
Jenkins je oblíbený nástroj pro průběžnou integraci a nasazování (CI/CD). V tomto dokumentu vás provedeme sestavováním a nasazováním aplikace Service Fabric pomocí Jenkinse.

## <a name="general-prerequisites"></a>Obecné požadavky
1. Potřebujete mít lokálně nainstalovaný Git. V závislosti na vašem operačním systému si můžete nainstalovat odpovídající verzi Gitu [odtud](https://git-scm.com/downloads). Pokud jste Git ještě nikdy nepoužívali, můžete postupovat podle pokynů uvedených v [dokumentaci](https://git-scm.com/docs) a seznámit se s ním.
2. Potřebujete mít po ruce modul plug-in Jenkinse pro Service Fabric. Stáhněte si modul plug-in Jenkinse pro Service Fabric [odtud](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi).

## <a name="setting-up-jenkins-inside-a-service-fabric-cluster"></a>Nastavení Jenkinse uvnitř clusteru Service Fabric

### <a name="prerequisites"></a>Požadavky
1. Máte připravený cluster Service Fabric s Linuxem. Na clusterech Service Fabric vytvořených z webu Azure Portal je již nainstalován Docker. Pokud provozujete místní cluster, pomocí příkazu ``docker info`` zkontrolujte, jestli je Docker nainstalován. Pokud ne, nainstalujte ho pomocí –
```sh
sudo apt-get install wget
wget -qO- https://get.docker.io/ | sh
```
2. Máte nasazenou aplikaci Service Fabric typu kontejner v clusteru ``http://PublicIPorFQDN:19080``. Můžete postupovat podle následujících kroků –
```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git -b JenkinsDocker && cd service-fabric-java-getting-started/Services/JenkinsDocker/
azure servicefabric cluster connect http://PublicIPorFQDN:19080 # Azure CLI cluster connect command
bash Scripts/install.sh
```
  Tím se nainstaluje kontejner s Jenkinsem do clusteru, ke kterému jste se připojili. Svoji aplikaci typu kontejner můžete monitorovat pomocí nástroje Service Fabric Explorer.

### <a name="steps"></a>Kroky
1. Přejděte v prohlížeči na adresu URL ``http://PublicIPorFQDN:8081``. Najdete tam cestu k počátečnímu heslu správce vyžadovanému k přihlášení. Jenkinse můžete nadále používat jako uživatel s právy pro správu, nebo můžete po přihlášení pomocí počátečního účtu správce vytvořit a změnit uživatele.
> [!NOTE]
> Je potřeba zajistit, aby během vytváření clusteru byl jako koncový bod aplikace zadán port 8081.
>

2. Získejte ID instance kontejneru pomocí příkazu ``docker ps -a``.
3. Přihlaste se přes SSH ke kontejneru a vložte cestu, která se zobrazila na portálu Jenkinse. Pokud se například na portálu zobrazila cesta `PATH_TO_INITIAL_ADMIN_PASSWORD`, můžete provést –  
  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash # This takes you inside docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Nastavte GitHub pro práci s Jenkinsem pomocí postupu uvedeného [tady](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    * Pomocí pokynů z GitHubu vygenerujte klíč SSH a přidejte ho do účtu GitHubu, který je (nebo bude) hostitelem úložiště.
    * Spusťte příkazy popsané na výše uvedeném odkazu v prostředí Jenkins Dockeru (ne na hostiteli).
    * Pokud se chcete k prostředí Jenkinse přihlásit z hostitele, budete muset –
  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="setting-up-jenkins-outside-a-service-fabric-cluster"></a>Nastavení Jenkinse mimo cluster Service Fabric

### <a name="prerequisites"></a>Požadavky
1. Potřebujete mít nainstalovaný Docker. Pokud ho nemáte, zadáním následujících příkazů v terminálu Docker nainstalujte.
```sh
sudo apt-get install wget
wget -qO- https://get.docker.io/ | sh
```
Když teď v terminálu spustíte příkaz ``docker info``, na výstupu by se mělo zobrazit, že je spuštěná služba Docker.

### <a name="steps"></a>Kroky
  1. Stáhněte si kontejner s Jenkinsem pro Service Fabric: ``docker pull IMAGE_NAME ``
  2. Spusťte image kontejneru: ``docker run -itd -p 8080:8080 IMAGE_NAME``
  3. Získejte ID vašeho kontejneru Dockeru, který obsahuje image Jenkinse (kterou jste právě nainstalovali). Pomocí příkazu ``docker ps –a`` můžete vypsat všechny kontejnery Dockeru.
  4. Získejte heslo správce pro účet Jenkinse. To můžete provést následovně –
    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Pokud je tedy ID v tomto případě 2d24a73b5964, je potřeba zadat pouze 2d24.
    * Toto heslo se bude vyžadovat pro přihlášení k řídicímu panelu Jenkinse z portálu, což je ``http://<HOST-IP>:8080``.
    * Jakmile se poprvé přihlásíte, můžete si vytvořit vlastní uživatelský účet, který budete používat pro další účely, nebo můžete dál používat účet uživatele s právy pro správu. Když vytvoříte nového uživatele, budete s ním muset pokračovat.
  5. Nastavte GitHub pro práci s Jenkinsem pomocí postupu uvedeného [tady](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
      * Pomocí pokynů z GitHubu vygenerujte klíč SSH a přidejte ho do účtu GitHubu, který je (nebo bude) hostitelem úložiště.
      * Spusťte příkazy popsané na výše uvedeném odkazu v prostředí Jenkins Dockeru (ne na hostiteli).
      * Pokud se chcete k prostředí Jenkinse přihlásit z hostitele, budete muset –
    ```sh
    docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
    ```

> [!NOTE]
> Ujistěte se, že cluster nebo počítač, který hostuje image kontejneru s Jenkinsem, má veřejnou IP adresu, aby instance Jenkinse mohla přijímat oznámení z GitHubu.
>

## <a name="install-the-service-fabric-jenkins-plugin-from-portal"></a>Nainstalujte z portálu modul plug-in Jenkinse pro Service Fabric.

1. Přejděte na ``http://PublicIPorFQDN:8081``.
2. Na řídicím panelu Jenkinse vyberte ``Manage Jenkins`` (Správa Jenkinse) -> ``Manage Plugins`` (Správa modulů plug-in) -> ``Advanced`` (Rozšířené).
Tady můžete nahrát modul plug-in. Vyberte možnost ``Choose file`` (Vybrat soubor) a potom vyberte soubor serviceFabric.hpi, který jste si stáhli v části Požadavky. Jakmile zvolíte nahrání, Jenkins modul plug-in automaticky nainstaluje za vás. Může vás také vyzvat k restartování, povolte ho.

## <a name="creating-and-configuring-a-jenkins-job"></a>Vytvoření a konfigurace úlohy Jenkinse

1. Z řídicího panelu vytvořte ``new item`` (nová položka).
2. Zadejte název nové položky, třeba ``MyJob``, vyberte volný styl projektu a klikněte na OK.
3. Potom přejděte na stránku úlohy a klikněte na ``Configure`` (Konfigurovat) -
  * V části Obecné do pole ``Github project`` (Projekt GitHubu) zadejte adresu URL vašeho projektu GitHubu, který je hostitelem aplikace Service Fabric v Javě, kterou chcete integrovat s postupy CI/CD Jenkinse (např. ``https://github.com/sayantancs/SFJenkins``).
  * V části ``Source Code Management`` (Správa zdrojového kódu) vyberte ``Git``. Zadejte adresu URL úložiště, které je hostitelem aplikace Service Fabric v Javě, kterou chcete integrovat s postupy CI/CD Jenkinse (např. ``https://github.com/sayantancs/SFJenkins.git``). V této části můžete také zadat, jaká větev se má sestavit, například ``*/master``.
4. Nakonfigurujte *GitHub* (tedy hostitele úložiště) tak, aby mohl komunikovat s Jenkinsem. Použijte k tomu následující postup –
  - Přejděte na stránku vašeho úložiště Github. Přejděte do ``Settings`` (Nastavení) -> ``Integrations and Services`` (Integrace a služby).
  - Vyberte ``Add Service`` (Přidat službu), zadejte Jenkins a vyberte ``Jenkins-Github plugin``.
  - Zadejte adresu URL webhooku Jenkinse (ve výchozím nastavení by měla být ``http://<PublicIPorFQDN>:8081/github-webhook/``). Klikněte na „Add/update service“ (Přidat/aktualizovat službu).
  - Do instance Jenkinse se odešle testovací událost. V GitHubu by se měla vedle webhooku zobrazit zelená značka zaškrtnutí a váš projekt se sestaví!
  - V části ``Build Triggers`` (Aktivační události sestavení) vyberte požadovanou možnost sestavení – pro tento případ použití plánujeme sestavení aktivovat při každém vložení metodou Push do úložiště, takže odpovídající možnost bude ``GitHub hook trigger for GITScm polling`` (dříve se nazývala „Build when a change is pushed to GitHub“ – „Sestavit při vložení změny metodou Push do GitHubu“).
  - V části ``Build section`` (Sestavení) z rozevírací nabídky ``Add build step`` (Přidat krok sestavení) vyberte možnost ``Invoke Gradle Script`` (Vyvolání skriptu Gradle). Ve widgetu, který se zobrazí, zadejte do pole ``Root build script`` (Kořenový skript sestavení) cestu ke kořenovému skriptu sestavení pro vaši aplikaci. Sestavení si ze zadané cesty vezme soubor build.gradle a bude pracovat odpovídajícím způsobem. Mějte na paměti, že pokud vytvoříte projekt ``MyActor`` (pomocí modulu plug-in Eclipse nebo generátoru Yeoman), pole pro kořenový skript sestavení by mělo obsahovat ``${WORKSPACE}/MyActor``. Pro ukázku tato část většinou vypadá nějak takto –

    ![Akce sestavení v Jenkinsu pro Service Fabric][build-step]
  - V rozevírací nabídce ``Post-Build Actions`` (Akce po sestavení) vyberte ``Deploy Service Fabric Project`` (Nasazení projektu Service Fabric). V této části je potřeba zadat podrobnosti o clusteru, ve kterém se bude nasazovat aplikace Service Fabric zkompilovaná Jenkinsem, a další podrobnosti o aplikaci, které se používají k jejímu nasazení. Následující snímek obrazovky můžete použít jako referenci:

    ![Akce sestavení v Jenkinsu pro Service Fabric][post-build-step]

 >[!Note]
 > Zde uvedený cluster může být stejný jako cluster, který hostuje aplikaci Jenkins typu kontejner, pokud k nasazení image kontejneru Jenkins používáte Service Fabric.
 >

### <a name="end-to-end-scenario"></a>Kompletní scénář
Nyní je nakonfigurovaný GitHub i Jenkins a můžete provést nějaké ukázkové změny v projektu ``MyActor`` například v úložišti https://github.com/sayantancs/SFJenkins a metodou Push změny vložit do vzdálené větve ``master`` (nebo jakékoli větve, kterou jste si pro práci nakonfigurovali). Tím se teď aktivuje nakonfigurovaná úloha Jenkinse ``MyJob``. Tato úloha jednoduše načte změny z GitHubu a použije je k sestavení aplikace, kterou nasadí do koncového uzlu clusteru, který jste zadali v akcích po sestavení.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png

