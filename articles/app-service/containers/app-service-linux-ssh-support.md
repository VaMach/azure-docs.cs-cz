---
title: "Podpora SSH pro Azure App Service v systému Linux | Microsoft Docs"
description: "Další informace o použití SSH s Azure App Service v systému Linux."
keywords: "služby Azure app service, webové aplikace, linux, operačních systémů"
services: app-service
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: 7e6bb974565810ebb8d8e21d1c274d42d6d39e55
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>Podpora SSH pro Azure App Service v systému Linux

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) je kryptografických síťový protokol pro bezpečné používání síťové služby. Toto pravidlo se používá nejčastěji přihlásit do systému vzdáleně bezpečně z příkazového řádku a spouštět příkazy pro správu vzdáleně.

Aplikační služby v systému Linux poskytuje podporu SSH do kontejneru aplikace s jednotlivými předdefinované Docker obrázků použitých pro modul Runtime zásobník nové webové aplikace.

![Zásobníky modulu runtime](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Můžete taky použití SSH se vlastních bitových kopií Docker včetně SSH serveru v rámci bitové kopie a její konfiguraci, jak je popsáno v tomto tématu.

## <a name="making-a-client-connection"></a>Navazování připojení klienta

Chcete-li připojení klienta SSH, je nutné spustit hlavního webu.

Koncový bod zdroje řízení správy (SCM) pro webovou aplikaci vložte do prohlížeče v následující podobě:

```bash
https://<your sitename>.scm.azurewebsites.net/webssh/host
```

Pokud již nejsou ověřené, je nutné k ověření s předplatným Azure připojit.

![Připojení SSH](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)

## <a name="ssh-support-with-custom-docker-images"></a>Podpora SSH s vlastními obrázky Docker

Aby pro vlastní image Docker pro podporu SSH komunikace mezi klienta a kontejneru na portálu Azure proveďte následující kroky pro Docker image.

Tyto kroky nejsou v úložišti Azure App Service, jako jsou uvedeny [příklad](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Zahrnout `openssh-server` instalace v [ `RUN` instrukce](https://docs.docker.com/engine/reference/builder/#run) v soubor Docker pro bitovou kopii a heslo pro kořenový účet sady `"Docker!"`.

    > [!NOTE]
    > Tato konfigurace neumožňuje externí připojení ke kontejneru. SSH lze přistupovat pouze prostřednictvím Kudu nebo webu SCM, což je ověřen pomocí přihlašovací údaje pro publikování.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

1. Přidat [ `COPY` instrukce](https://docs.docker.com/engine/reference/builder/#copy) na soubor Docker zkopírovat [sshd_config](http://man.openbsd.org/sshd_config) do souboru */atd/ssh/* adresáře. Konfigurační soubor by měla být založena na našem sshd_config souboru v úložišti GitHub službě Azure-App-Service [zde](https://github.com/Azure-App-Service/node/blob/master/8.2.1/sshd_config).

    > [!NOTE]
    > *Sshd_config* nebo připojení selže, soubor musí zahrnovat následující: 
    > * `Ciphers`musí obsahovat alespoň jednu z následujících: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`musí obsahovat alespoň jednu z následujících: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```

1. Zahrnout port 2222 v [ `EXPOSE` instrukce](https://docs.docker.com/engine/reference/builder/#expose) pro soubor Docker. I když se označuje kořenové heslo, port 2222 není přístupný z Internetu. Je k interní pouze portu přístupné pouze pomocí kontejnery v rámci sítě most privátní virtuální sítě.

    ```docker
    EXPOSE 2222 80
    ```

1. Zajistěte, abyste [spustit služby ssh](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) pomocí skriptu prostředí v */bin* adresáře.

    ```bash
    #!/bin/bash
    service ssh start
    ```

Soubor Docker používá [ `CMD` instrukce](https://docs.docker.com/engine/reference/builder/#cmd) pro spuštění skriptu.

    ```docker
    COPY init_container.sh /bin/
    ...
    RUN chmod 755 /bin/init_container.sh
    ...
    CMD ["/bin/init_container.sh"]
    ```

## <a name="next-steps"></a>Další kroky

V následujících tématech Další informace týkající se webové aplikace pro kontejnery. Otázky a aspekty můžete zveřejnit na [našem fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Jak používat vlastní image Docker pro webovou aplikaci pro kontejnery](quickstart-custom-docker-image.md)
* [Pomocí .NET Core v Azure App Service v systému Linux](quickstart-dotnetcore.md)
* [Pomocí Ruby v Azure App Service v systému Linux](quickstart-ruby.md)
* [Webové aplikace Azure App Service pro kontejnery – nejčastější dotazy](app-service-linux-faq.md)