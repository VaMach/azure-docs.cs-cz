---
title: "Azure zásobníku úvodní – vytvoření virtuálního počítače portálu"
description: "Azure zásobníku rychlý Start – vytvoření virtuálního počítače s Linuxem pomocí portálu"
services: azure-stack
cloud: azure-stack
author: vhorne
manager: byronr
ms.service: azure-stack
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: f986c060b26489e412f6230665ec76a4730293f1
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="create-a-linux-virtual-machine-with-the-azure-stack-portal"></a>Vytvoření virtuální počítač s Linuxem pomocí portálu Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Virtuální počítače Azure zásobníku lze vytvořit pomocí portálu Azure zásobníku. Tato metoda poskytuje založené na prohlížeči uživatelské rozhraní pro vytvoření a konfigurace virtuálního počítače a všechny související prostředky. Tento rychlý start ukazuje, jak rychle vytvořit virtuální počítač s Linuxem a na něj nainstalovat webový server.

## <a name="prerequisites"></a>Požadavky

* **Bitovou kopii systému Linux v zásobníku Azure marketplace**

   Zásobník Azure marketplace neobsahuje bitovou kopii systému Linux ve výchozím nastavení. Tak, než vytvoříte virtuální počítač s Linuxem, ujistěte se, že operátor zásobník Azure stáhla **Ubuntu Server 16.04 LTS** bitovou kopii pomocí kroků popsaných v [stáhnout z Azure do Azure položky marketplace. Zásobník](../azure-stack-download-azure-marketplace-item.md) tématu.

* **Přístup k klientem SSH**

   Pokud používáte sadu Azure zásobníku Development Kit (ASDK), nemusí mít přístup k klientem SSH ve vašem prostředí. Pokud je to tento případ, můžete mezi více balíčků, které zahrnují klientem SSH. Například můžete nainstalovat PuTTY, která zahrnuje služby Klient SSH a generátor klíče SSH (puttygen.exe). Další informace o možných parametrech najdete v tématu Azure článek týkající se následující: [jak klíče použití SSH se systémem Windows v Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   Tento rychlý start využívá PuTTY generování klíčů SSH a připojit k virtuálnímu počítači Linux. Chcete-li stáhnout a nainstalovat PuTTY, přejděte na [http://www.putty.org/](http://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>Vytvoření páru klíčů SSH

Je nutné dvojici klíčů SSH k dokončení tento rychlý start. Pokud máte existující pár klíčů SSH, můžete tento krok přeskočit.

1. Přejděte do složky instalace PuTTY (výchozí umístění je ```C:\Program Files\PuTTY```) a spusťte ```puttygen.exe```.
2. Ujistěte se, v okně generátor klíče PuTTY **typ klíče pro generování** je nastaven na **RSA**a **počet bitů v generovaný klíč** je nastaven na **2048**. Až budete připravení, klikněte na tlačítko **generování**.

   ![puttygen.exe](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Dokončete proces generování klíčů, umístěte ukazatel myši v rámci okna Generátor klíče PuTTY.
4. Po dokončení generování klíčů, klikněte na tlačítko **uložit veřejný klíč** a **uložit privátní klíč** pro veřejné a soukromé klíče ukládají do souborů.

   ![PuTTY klíče](media/azure-stack-quick-linux-portal/Putty02.PNG)



## <a name="sign-in-to-the-azure-stack-portal"></a>Přihlaste se k portálu Azure zásobníku

Přihlaste se k portálu Azure zásobníku. Na adresu na portálu Azure zásobníku závisí, na který produkt Azure zásobníku se připojujete k:

* Pro Azure zásobníku Development Kit (ASDK) přejděte na: https://portal.local.azurestack.external.
* Pro systém Azure zásobníku integrované přejděte na adresu URL, kterou vaše operátor zásobník Azure poskytuje.

## <a name="create-the-virtual-machine"></a>Vytvoření virtuálního počítače

1. Klikněte **nový** v levém horním rohu na portálu Azure zásobníku nalezeno tlačítko.

2. Vyberte **Compute** a potom vyberte **Ubuntu Server 16.04 LTS**.
3. Klikněte na možnost **Vytvořit**.

4. Zadejte informace o virtuálním počítači. Jako **Typ ověřování** vyberte **Veřejný klíč SSH**. Když vložte veřejný klíč SSH (který jste si předtím uložili do souboru), vezměte v potaz odebrat všechny začínat ani končit mezerou prázdné. Jakmile budete hotovi, klikněte na **OK**.

   ![Základní informace o virtuálním počítači](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Vyberte **D1_V2** pro virtuální počítač.

   ![Velikost počítače](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. Na **nastavení** , ponechejte výchozí hodnoty a klikněte na tlačítko **OK**.

7. Na **Souhrn** klikněte na tlačítko **OK** ke spuštění nasazení virtuálního počítače.


## <a name="connect-to-the-virtual-machine"></a>Připojení k virtuálnímu počítači

1. Klikněte na tlačítko **Connect** na stránce virtuálního počítače. Zobrazí řetězec připojení SSH, které je možné se připojit k virtuálnímu počítači.

   ![Připojit virtuální počítač](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. Otevřete PuTTY.
3. Na **konfigurace PuTTY** obrazovce, v části **kategorie**, rozbalte položku **SSH** a pak klikněte na **Auth**. Klikněte na tlačítko **Procházet** a vyberte soubor privátního klíče, který jste si předtím uložili.

   ![PuTTY privátní klíč](media/azure-stack-quick-linux-portal/Putty03.PNG)
4. V části **kategorie**, vraťte se na začátek a klikněte na **relace**.
5. V **název hostitele (nebo IP adresa)** pole, vložte připojovací řetězec z portálu Azure zásobníku, kterou jste viděli dříve. V tomto příkladu je řetězec ```asadmin@192.168.102.34```.
 
   ![Relace puTTY](media/azure-stack-quick-linux-portal/Putty04.PNG)
6. Klikněte na tlačítko **otevřete** otevřít relaci k virtuálnímu počítači.

   ![Linus relace](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-nginx"></a>Instalace serveru NGINX

Pomocí následujícího skriptu bash k aktualizaci zdroje balíčků a instalovat nejnovější balíček NGINX na virtuálním počítači. 

```bash 
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Až budete hotoví, ukončení relace SSH a vrátí stránku přehled virtuálního počítače na portálu Azure zásobníku.


## <a name="open-port-80-for-web-traffic"></a>Otevření portu 80 pro webový provoz 

Skupina zabezpečení sítě (NSG) zabezpečuje příchozí a odchozí provoz. Při vytvoření virtuálního počítače z portálu Azure zásobníku se na port 22 pro SSH připojení vytvoří pravidlo pro příchozí. Protože tento virtuální počítač je hostitelem webového serveru, musí být vytvořenou pro port 80 pravidlo NSG.

1. Na virtuálním počítači **přehled** klikněte na název **skupiny prostředků**.
2. Vyberte **skupinu zabezpečení sítě** pro virtuální počítač. NSG můžete identifikovat pomocí sloupce **Typ**. 
3. V levé nabídce v části **nastavení**, klikněte na tlačítko **příchozí pravidla zabezpečení**.
4. Klikněte na tlačítko **Přidat**.
5. Do pole **Název** zadejte **http**. Zkontrolujte, že **Rozsah portů** je nastavený na 80 a **Akce** je nastavená na **Povolit**. 
6. Klikněte na **OK**.


## <a name="view-the-nginx-welcome-page"></a>Zobrazení úvodní stránky serveru NGINX

NGINX nainstalován, a port 80 otevřete na virtuálním počítači webový server je nyní přístupný na veřejnou IP adresu virtuálního počítače. Veřejnou IP adresu naleznete na stránce Přehled virtuálního počítače na portálu Azure zásobníku.

Otevřete webový prohlížeč a přejděte do ```http://<public IP address>```.

![Výchozí web NGINX](media/azure-stack-quick-linux-portal/linux-04.PNG)


## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, odstraňte skupinu prostředků, virtuální počítač a všechny související prostředky. To pokud chcete udělat, vyberte skupinu prostředků ze strany virtuálního počítače a klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V této úvodní jste nasazení jednoduché virtuální počítač s Linuxem, pravidla skupiny zabezpečení sítě a instalaci webového serveru. Další informace o virtuálních počítačích Azure zásobníku, nadále [důležité informace pro virtuální počítače v Azure zásobníku](azure-stack-vm-considerations.md).

