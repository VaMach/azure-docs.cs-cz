---
title: "Nasazení LEMP na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Kurz – instalace zásobníku LEMP na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: danlep
ms.openlocfilehash: c77cd0148a7e3e7b99e90e29bc1499dae8f95028
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a>Nainstalujte LEMP webový server na virtuálním počítači Azure
Tento článek vás provede procesem nasazení webového serveru NGINX, MySQL a PHP (LEMP stack) na virtuálního počítače s Ubuntu v Azure. LEMP zásobník představuje alternativu k oblíbených [svítilna zásobníku](tutorial-lamp-stack.md), které si můžete také nainstalovat v Azure. Informace o serveru LEMP v akci, můžete volitelně nainstalovat a nakonfigurovat web WordPress. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu ("L" v zásobníku LEMP)
> * Otevření portu 80 pro webový provoz
> * Instalace NGINX, MySQL a PHP
> * Ověření instalace a konfigurace
> * Instalace aplikace WordPress na serveru LEMP


Tento instalační program pro rychlé testů nebo testování konceptu.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>Instalace NGINX, MySQL a PHP

Spusťte následující příkaz k aktualizaci zdroje balíčků Ubuntu a nainstalujte NGINX, MySQL a PHP. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

Zobrazí se výzva k instalaci balíčků a další závislosti. Po zobrazení výzvy, nastavte kořenové heslo pro databázi MySQL a potom [Enter] pokračovat. Postupujte podle zbývajících pokynů. Tento proces nainstaluje minimální požadované rozšíření PHP potřeba k použití PHP s MySQL. 

![Stránka MySQL kořenového hesla][1]

## <a name="verify-installation-and-configuration"></a>Ověření instalace a konfigurace


### <a name="nginx"></a>NGINX

Kontrola verze NGINX pomocí následujícího příkazu:
```bash
nginx -v
```

NGINX nainstalován, a port 80 je otevřená pro virtuální počítač webový server je nyní přístupný z Internetu. Chcete-li zobrazit úvodní stránce NGINX, otevřete webový prohlížeč a zadejte veřejnou IP adresu virtuálního počítače. Použijte veřejnou IP adresu, kterou jste použili k SSH pro virtuální počítač:

![NGINX výchozí stránka][3]


### <a name="mysql"></a>MySQL

Kontrola verze databáze MySQL pomocí následujícího příkazu (Poznámka: velké `V` parametr):

```bash
mysql -V
```

Chcete-li pomoc se zabezpečením instalaci MySQL, spusťte `mysql_secure_installation` skriptu. Pokud jsou pouze nastavení dočasný server, můžete tento krok přeskočit. 

```bash
mysql_secure_installation
```

Zadejte kořenové heslo pro databázi MySQL a nakonfigurovat nastavení zabezpečení pro vaše prostředí.

Pokud budete chtít zkusit MySQL funkce (vytvoření databáze MySQL, přidat uživatele nebo změnit nastavení konfigurace), přihlášení k MySQL. Tento krok není nutný k dokončení tohoto kurzu. 


```bash
mysql -u root -p
```

Až budete hotoví, ukončete řádku mysql zadáním `\q`.

### <a name="php"></a>PHP

Zkontrolujte verzi PHP pomocí následujících příkazů:

```bash
php -v
```

Nakonfigurujte NGINX používat Správce proces FastCGI PHP (PHP paměť FPM). Spusťte následující příkazy zálohovat původní bloku konfigurační soubor serveru NGINX a pak upravit původní soubor v editoru podle vašeho výběru:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

V editoru, nahraďte obsah `/etc/nginx/sites-available/default` následujícím kódem. Zobrazte komentáře vysvětlení nastavení. Nahraďte veřejnou IP adresu vašeho virtuálního počítače pro *yourPublicIPAddress*a ponechejte zbývající nastavení. Pak soubor uložte.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Zkontrolujte konfiguraci NGINX chyby syntaxe:

```bash
sudo nginx -t
```

Pokud je správná syntaxe, restartujte NGINX pomocí následujícího příkazu:

```bash
sudo service nginx restart
```

Pokud chcete testovat další, vytvořte rychlé stránka informace o PHP zobrazíte v prohlížeči. Následující příkaz vytvoří stránku s informacemi o PHP:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Nyní můžete zkontrolovat stránku s informacemi o PHP, kterou jste vytvořili. Otevřete prohlížeč a přejděte na `http://yourPublicIPAddress/info.php`. Nahraďte veřejnou IP adresu vašeho virtuálního počítače. By měla vypadat podobně jako tento obrázek.

![Stránka informace o PHP][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nasadili server LEMP v Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu
> * Otevření portu 80 pro webový provoz
> * Instalace NGINX, MySQL a PHP
> * Ověření instalace a konfigurace
> * Instalace aplikace WordPress v zásobníku LEMP

Přechodu na v dalším kurzu se dozvíte, jak zabezpečit webové servery s certifikáty protokolu SSL.

> [!div class="nextstepaction"]
> [Zabezpečení webového serveru pomocí protokolu SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
