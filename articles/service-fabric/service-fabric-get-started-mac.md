---
title: "Nastavení vývojového prostředí v Mac OS X | Dokumentace Microsoftu"
description: "Nainstalujte modul runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace a nastavení budete moci sestavovat aplikace v Mac OS X."
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: d5a04e34a3f46097b3ad1b04ac6b2b845fc946af
ms.openlocfilehash: beb869fd5c1fcc19101b16149244187c37a6d6a3


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

* [Vagrant (verzi 1.8.4 nebo novější)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

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

## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Instalace modulu plug-in služby Service Fabric pro Eclipse Neon (volitelné)
Service Fabric poskytuje modul plug-in pro integrovaná vývojové prostředí Eclipse Neon, který může zjednodušit proces sestavování a nasazování služeb v Javě.

1. V nástroji Eclipse se ujistěte, že máte nainstalovanou verzi Buildship 1.0.17 nebo novější. Verze nainstalovaných komponent můžete zkontrolovat tak, že zvolíte **Nápověda > Podrobnosti o instalaci**. Buildship můžete aktualizovat pomocí pokynů [zde][buildship-update].
2. Chcete-li nainstalovat modul plug-in Service Fabric, zvolte **Nápověda > Instalace nového softwaru**.
3. Do textového pole „Pracovat s“ zadejte: http://dl.windowsazure.com/eclipse/servicefabric.
4. Klikněte na Přidat.

    ![Modul plug-in Eclipse Neon pro Service Fabric][sf-eclipse-plugin-install]
5. Zvolte modul plug-in služby Service Fabric a klikněte na Další.
6. Pokračujte v instalaci a přijměte licenční smlouvu pro koncového zákazníka.

## <a name="next-steps"></a>Další kroky
<!-- Links -->

* [Vytvoření první aplikace Service Fabric pro Linux](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření clusteru služby Service Fabric na webu Azure Portal](service-fabric-cluster-creation-via-portal.md)
* [Vytvoření clusteru služby Service Fabric pomocí Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md)
* [Pochopení aplikačního modelu služby Service Fabric](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship



<!--HONumber=Jan17_HO1-->


