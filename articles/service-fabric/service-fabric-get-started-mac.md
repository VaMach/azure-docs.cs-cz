---
title: "Nastavení vývojového prostředí v Mac OS X | Dokumentace Microsoftu"
description: "Nainstalujte modul runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace a nastavení budete moci sestavovat aplikace v Mac OS X."
services: service-fabric
documentationcenter: java
author: saysa
manager: raunakp
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: fc73eedae7ec9664da714567f47a543e625cd023
ms.lasthandoff: 03/11/2017


---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Nastavení vývojového prostředí v Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Pomocí Mac OS X můžete sestavit aplikace Service Fabric, které poběží na clusterech s Linuxem. Tento článek popisuje nastavení počítače Mac pro vývoj.

## <a name="prerequisites"></a>Požadavky
Service Fabric nefunguje v OS X nativně. Pro spuštění místního clusteru služby Service Fabric poskytujeme předem nakonfigurovaný virtuální počítač se systémem Ubuntu pomocí aplikací Vagrant a VirtualBox. Než začnete, budete potřebovat:

* [Vagrant (verzi&1;.8.4 nebo novější)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
>  Musíte pro Vagrant a VirtualBox používat vzájemně podporované verze. Vagrant se může chovat chybně v nepodporované verzi VirtualBox.
>

## <a name="create-the-local-vm"></a>Vytvoření místního virtuálního počítače
Chcete-li vytvořit místní virtuální počítač s clusterem Service Fabric s 5 uzly, postupujte následovně:

1. Naklonujte úložiště **Vagrantfile**.

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
2. Přejděte do místního klonu úložiště.

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (Volitelné) Upravte výchozí nastavení virtuálního počítače.

    Ve výchozím nastavení je místní virtuální počítač nakonfigurován takto:

   * 3 GB přidělené paměti
   * Privátní hostitelská síť nakonfigurovaná na IP adresu 192.168.50.50, která umožňuje průchod přenosu dat z hostitelského počítače Mac

     Ve Vagrantfile můžete změnit kterékoli z těchto nastavení nebo k virtuálnímu počítači přidat další konfiguraci. Úplný seznam možností konfigurace najdete v [dokumentaci k Vagrant](http://www.vagrantup.com/docs).
4. Vytvořte virtuální počítač.

    ```bash
    vagrant up
    ```

   Tento krok stáhne předem nakonfigurovanou image virtuálního počítače, místně jej spustí a následně v něm nastaví místní cluster služby Service Fabric. Očekávejte, že tento proces zabere několik minut. Pokud se instalace úspěšně dokončí, zobrazí se ve výstupu zpráva s oznámením, že se spouští cluster.

    ![Spouštění instalace clusteru po zřízení virtuálního počítače][cluster-setup-script]

5. Otestujte správné nastavení clusteru tak, že přejdete do Service Fabric Exploreru na adrese http://192.168.50.50:19080/Explorer (za předpokladu, že jste ponechali výchozí IP adresu privátní sítě).

    ![Service Fabric Explorer zobrazený z hostitelského počítače Mac][sfx-mac]

## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>Instalace modulu plug-in Service Fabric pro Eclipse Neon

Service Fabric poskytuje modul plug-in pro **integrovaná vývojové prostředí Eclipse Neon pro Javu**, který může zjednodušit proces vytváření, sestavování a nasazování služeb v Javě. Můžete provést kroky instalace uvedené v této obecné [dokumentaci](service-fabric-get-started-eclipse.md#install-or-update-service-fabric-plugin-on-eclipse-neon) týkající se instalace nebo aktualizace modulu plug-in Service Fabric Eclipse.

## <a name="using-service-fabric-eclipse-plugin-on-mac"></a>Použití modulu plug-in Service Fabric Eclipse v systému Mac

Ujistěte se, jste prošli kroky v [dokumentaci modulu plug-in Service Fabric Eclipse](service-fabric-get-started-eclipse.md). Kroky pro vytvoření, sestavení a nasazení aplikace Service Fabric v Javě pomocí kontejneru hosta vagrant na hostiteli se systémem Mac je z větší části stejný, jak popisuje obecná dokumentace, kromě několika bodů, které musíte mít na paměti, jak je uvedeno dále:
* Vzhledem k tomu, že aplikace Service Fabric v Javě bude k úspěšnému sestavení vyžadovat knihovny Service Fabric, je potřeba vytvořit projekt Eclipse ve sdílené cestě. Ve výchozím nastavení je obsah v cestě na hostiteli, kde ``Vagrantfile`` existuje, sdílený s cestou ``/vagrant`` na hostovi.
* Jednoduše řečeno, pokud máte ``Vagrantfile`` v cestě, například ``~/home/john/allprojects/``, pak je potřeba vytvořit projekt Service Fabric ``MyActor`` v umístění ``~/home/john/allprojects/MyActor`` a cesta k pracovnímu prostoru Eclipse bude ``~/home/john/allprojects``.

## <a name="next-steps"></a>Další kroky
<!-- Links -->
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření clusteru služby Service Fabric na webu Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Vytvoření clusteru služby Service Fabric pomocí Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md)
* [Pochopení aplikačního modelu služby Service Fabric](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

