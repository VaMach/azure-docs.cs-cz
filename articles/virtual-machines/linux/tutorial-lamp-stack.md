---
title: "Nasazení svítilna na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Kurz – instalace zásobníku svítilna na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: c00e6a190633348411f47490808739d570cafd69
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a>Nainstalujte svítilna webový server na virtuální počítač Azure
Tento článek vás provede procesem nasazení webového serveru Apache, MySQL a PHP (svítilna stack) na virtuálního počítače s Ubuntu v Azure. Pokud dáváte přednost NGINX webový server, najdete v článku [LEMP zásobníku](tutorial-lemp-stack.md) kurzu. Informace o serveru svítilna v akci, můžete volitelně nainstalovat a nakonfigurovat web WordPress. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu ("L" v zásobníku svítilna)
> * Otevření portu 80 pro webový provoz
> * Instalace Apache, MySQL a PHP
> * Ověření instalace a konfigurace
> * Instalace aplikace WordPress na serveru svítilna


Další informace v zásobníku svítilna, včetně doporučení pro produkční prostředí, najdete [Ubuntu dokumentaci](https://help.ubuntu.com/community/ApacheMySQLPHP).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Instalace Apache, MySQL a PHP

Spusťte následující příkaz k aktualizaci zdroje balíčků Ubuntu a instalace Apache, MySQL a PHP. Poznámka: šipka nahoru (^) na konci příkazu.


```bash
sudo apt update && sudo apt install lamp-server^
```



Zobrazí se výzva k instalaci balíčků a další závislosti. Po zobrazení výzvy, nastavte kořenové heslo pro databázi MySQL a potom [Enter] pokračovat. Postupujte podle zbývajících pokynů. Tento proces nainstaluje minimální požadované rozšíření PHP potřeba k použití PHP s MySQL. 

![Stránka MySQL kořenového hesla][1]

## <a name="verify-installation-and-configuration"></a>Ověření instalace a konfigurace


### <a name="apache"></a>Apache

Kontrola verze Apache pomocí následujícího příkazu:
```bash
apache2 -v
```

Apache nainstalován, a port 80 je otevřená pro virtuální počítač webový server je nyní přístupný z Internetu. Chcete-li zobrazit Apache2 Ubuntu výchozí stránku, otevřete webový prohlížeč a zadejte veřejnou IP adresu virtuálního počítače. Použijte veřejnou IP adresu, kterou jste použili k SSH pro virtuální počítač:

![Apache výchozí stránka][3]


### <a name="mysql"></a>MySQL

Kontrola verze databáze MySQL pomocí následujícího příkazu (Poznámka: velké `V` parametr):

```bash
mysql -V
```

Doporučujeme spustit následující skript pro pomoc se zabezpečením instalaci MySQL:

```bash
mysql_secure_installation
```

Zadejte heslo kořenové MySQL a nakonfigurovat nastavení zabezpečení pro vaše prostředí.

Pokud chcete k vytvoření databáze MySQL, přidat uživatele nebo změnit nastavení konfigurace, přihlášení k MySQL:

```bash
mysql -u root -p
```

Až budete hotoví, ukončete řádku mysql zadáním `\q`.

### <a name="php"></a>PHP

Zkontrolujte verzi PHP pomocí následujících příkazů:

```bash
php -v
```

Pokud chcete testovat další, vytvořte rychlé stránka informace o PHP zobrazíte v prohlížeči. Následující příkaz vytvoří stránku s informacemi o PHP:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Nyní můžete zkontrolovat stránku s informacemi o PHP, kterou jste vytvořili. Otevřete prohlížeč a přejděte na `http://yourPublicIPAddress/info.php`. Nahraďte veřejnou IP adresu vašeho virtuálního počítače. By měla vypadat podobně jako tento obrázek.

![Stránka informace o PHP][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nasadili server svítilna v Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu
> * Otevření portu 80 pro webový provoz
> * Instalace Apache, MySQL a PHP
> * Ověření instalace a konfigurace
> * Instalace aplikace WordPress na serveru svítilna

Přechodu na v dalším kurzu se dozvíte, jak zabezpečit webové servery s certifikáty protokolu SSL.

> [!div class="nextstepaction"]
> [Zabezpečení webového serveru pomocí protokolu SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png