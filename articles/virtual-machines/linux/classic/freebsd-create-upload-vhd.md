---
title: "Vytvoření a nahrání image virtuálního počítače FreeBSD | Microsoft Docs"
description: "Naučte se vytvořit a odeslat virtuální pevný disk (VHD), který obsahuje operační systém FreeBSD k vytvoření virtuálního počítače Azure"
services: virtual-machines-linux
documentationcenter: 
author: thomas1206
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: huishao
ms.openlocfilehash: 0010e01d4333b96696680ec6fbbeee74b17f46a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Vytvoření a nahrání virtuálního pevného disku FreeBSD do Azure
Tento článek ukazuje, jak vytvořit a odeslat virtuální pevný disk (VHD), který obsahuje FreeBSD operační systém. Po odeslání, můžete ho použít jako vlastní image pro vytvoření virtuálního počítače (VM) v Azure.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Informace o nahrávání virtuální pevný disk pomocí modelu Resource Manager najdete v tématu [zde](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte následující položky:

* **Předplatné Azure**– Pokud nemáte účet, můžete vytvořit jeden si během několika minut. Pokud máte předplatné MSDN, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Jinak, zjistěte, jak [vytvořit Bezplatný zkušební účet](https://azure.microsoft.com/pricing/free-trial/).  
* **Nástroje Azure PowerShell**– modul PowerShell Azure musí být nainstalované a nakonfigurované na používání vašeho předplatného Azure. Si můžete stáhnout modul [Azure stáhne](https://azure.microsoft.com/downloads/). Zde jsou k dispozici kurz, který popisuje, jak nainstalovat a nakonfigurovat modul. Použití [Azure stáhne](https://azure.microsoft.com/downloads/) rutiny odeslání disku VHD.
* **FreeBSD operační systém nainstalovaný v souboru VHD**– podporované FreeBSD operačního systému musí být nainstalována na virtuální pevný disk. Postup vytvoření souborů VHD existovat několik nástrojů. Řešení virtualizace, jako je například technologie Hyper-V můžete například použít k vytvoření souboru VHD a instalace operačního systému. Pokyny o tom, jak nainstalovat a používat technologie Hyper-V najdete v tématu [instalaci technologie Hyper-V a vytvoření virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Novější formát VHDX není podporovaný v Azure. Disk můžete převést do formátu virtuálního pevného disku pomocí Správce technologie Hyper-V nebo rutiny [convert-VHD prostředí](https://technet.microsoft.com/library/hh848454.aspx). Kromě toho je [kurz na webu MSDN o tom, jak používat FreeBSD s technologií Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).
>
>

Tato úloha obsahuje pět takto:

## <a name="step-1-prepare-the-image-for-upload"></a>Krok 1: Příprava bitové kopie pro odeslání
Na virtuálním počítači, kam jste nainstalovali FreeBSD operačního systému proveďte následující postupy:

1. Povolte protokol DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. Povolte SSH.

    Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění. Ve výchozím nastavení je povoleno po instalaci z disku FreeBSD. 
3. Nastavení konzoly sériového portu.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. Nainstalujte sudo.

    Kořenový účet je zakázán v Azure. To znamená, že potřebujete využívat sudo z Neprivilegovaný uživatelský ke spuštění příkazů se zvýšenými oprávněními.

        # pkg install sudo
   
5. Požadavky pro agenta Azure.

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. Nainstalujte agenta Azure.

    Nejnovější verzi agenta Azure vždy najdete na [githubu](https://github.com/Azure/WALinuxAgent/releases). Verze 2.0.10 + oficiálně podporuje FreeBSD 10 & 10.1 a verze 2.1.4 + (včetně 2.2.x) oficiálně podporuje FreeBSD 10.2 a pozdějších verzích.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    Pro 2.0 použijeme 2.0.16 jako příklad:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Pro 2.1 použijeme 2.1.4 jako příklad:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > Po instalaci agenta Azure, je vhodné ověřit, zda je spuštěna:
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. Zrušení zřízení systému.

    Zrušení zřízení systému a vyčistit ho a nastavit jej jako vhodný pro reprovisioning. Následující příkaz taky odstraní poslední zřízené uživatelský účet a přidružená data:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Nyní můžete vypnout virtuální počítač.

## <a name="step-2-create-a-storage-account-in-azure"></a>Krok 2: Vytvoření účtu úložiště v Azure
Budete potřebovat účet úložiště v Azure a nahrát soubor VHD, proto ji můžete použít k vytvoření virtuálního počítače. Portál Azure classic můžete použít k vytvoření účtu úložiště.

1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Na panelu příkazů vyberte **nový**.
3. Vyberte **datové služby** > **úložiště** > **rychle vytvořit**.

    ![Rychlé vytvoření účtu úložiště](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. Vyplňte pole následujícím způsobem:

   * V **URL** pole, zadejte název subdomény pro použití v adrese URL účtu úložiště. Položka může obsahovat ze 3 – 24 čísla a malá písmena. Tento název se změní na název hostitele v rámci adresu URL, která se používá k řešení úložiště objektů Blob v Azure, Azure Queue storage nebo Azure Table storage prostředky pro předplatné.
   * V **umístění/skupině vztahů** rozevírací nabídky vyberte **umístění nebo skupina vztahů** pro účet úložiště. Skupiny vztahů umožňuje umístit cloudové služby a úložiště ve stejném datovém centru.
   * V **replikace** pole, rozhodnout, jestli se má používat **geograficky redundantní** replikace pro účet úložiště. Geografická replikace je zapnutá ve výchozím nastavení. Tato možnost replikuje data do sekundárního umístění, bez nákladů pro vás, tak, aby úložiště převezme do tohoto umístění, pokud dojde k selhání hlavní v primární lokalitě. Sekundární umístění bude přiřazena automaticky a nedá se změnit. Pokud potřebujete větší kontrolu nad umístění cloudové úložiště z důvodu právní požadavky nebo zásad organizace, můžete vypnout geografická replikace. Však mějte na paměti, že pokud později zapnout geografická replikace, vám bude účtována úplatu přenos jednorázové data replikace existující data do sekundárního umístění. Služby úložiště bez geografická replikace se nabízí se slevou. Další informace o správě geografická replikace účtů úložiště najdete tady: [replikace Azure Storage](../../../storage/common/storage-redundancy.md).

     ![Zadejte podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. Vyberte **vytvořit účet úložiště**. Účet se teď zobrazí v části **úložiště**.

    ![Účet úložiště byl úspěšně vytvořen](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. Dále vytvořte kontejner pro vaše soubory nahrávat VHD. Vyberte název účtu úložiště a pak vyberte **kontejnery**.

    ![Podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. Vyberte **vytvořit kontejner**.

    ![Podrobnosti o účtu úložiště](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. V **název** pole, zadejte název vašeho kontejneru. Potom v **přístup** rozevírací nabídky, vyberte typ zásady přístupu, které chcete.

    ![Název kontejneru](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > Ve výchozím kontejneru je privátní a můžete přistupovat pouze vlastníka účtu. Chcete-li povolit veřejný přístup pro čtení objektů BLOB v kontejneru, ale ne vlastnosti kontejneru a metadata, použijte **veřejného objektu Blob** možnost. Chcete-li povolit úplné veřejný přístup pro čtení pro kontejner a objekty BLOB, použijte **veřejném kontejneru** možnost.
   >
   >

## <a name="step-3-prepare-the-connection-to-azure"></a>Krok 3: Příprava připojení k Azure
Předtím, než můžete nahrát soubor VHD, budete muset navázat zabezpečené připojení mezi vaším počítačem a vašeho předplatného Azure. Můžete to udělat metodu Azure Active Directory (Azure AD) nebo metodu certifikátu.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Použijte metodu Azure AD pro nahrání souboru VHD
1. Otevřete konzolu prostředí Azure PowerShell.
2. Zadejte následující příkaz:  
    `Add-AzureAccount`

    Tento příkaz otevře okno přihlášení, kde se můžete přihlásit pomocí svého pracovního nebo školního účtu.

    ![Okno prostředí PowerShell](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. Azure ověřuje a uloží informace o přihlašovacích údajích. Potom zavře okno.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Pomocí této metody certifikát odeslat soubor VHD
1. Otevřete konzolu prostředí Azure PowerShell.
2. Typ: `Get-AzurePublishSettingsFile`.
3. Otevře okno prohlížeče a výzva ke stažení souboru .publishsettings. Tento soubor obsahuje informace a certifikát pro vaše předplatné Azure.

    ![Stránka pro stažení prohlížeče](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. Uložte soubor .publishsettings.
5. Typ: `Import-AzurePublishSettingsFile <PathToFile>`, kde `<PathToFile>` je úplná cesta k souboru .publishsettings.

   Další informace najdete v tématu [Začínáme s Azure rutiny](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Další informace o instalaci a konfiguraci prostředí PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="step-4-upload-the-vhd-file"></a>Krok 4: Nahrání souboru VHD
Při nahrávání souboru VHD, můžete je umístit kamkoli do úložiště objektů Blob. Tady jsou některé podmínky, které budete používat při nahrávání souboru:

* **BlobStorageURL** je adresa URL pro účet úložiště, který jste vytvořili v kroku 2.
* **YourImagesFolder** je kontejneru v úložiště objektů Blob, kam chcete uložit vaše Image.
* **VHDName** je štítek, který se zobrazí na portálu Azure classic k identifikaci virtuální pevný disk.
* **PathToVHDFile** je úplná cesta a název souboru VHD.

V okně Azure PowerShell, které jste použili v předchozím kroku zadejte:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Krok 5: Vytvoření virtuálního počítače pomocí souboru nahrávat VHD
Po odeslání souboru VHD, můžete ho přidat bitovou kopii do seznamu vlastních bitových kopií, které jsou spojeny s předplatným a vytvoření virtuálního počítače s touto bitovou kopií vlastní.

1. V okně Azure PowerShell, které jste použili v předchozím kroku zadejte:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

   > [!NOTE]
   > Použijte jako typ operačního systému Linux. Aktuální verzi prostředí Azure PowerShell přijímá pouze "Linux" nebo "Systém Windows" jako parametr.
   >
   >
2. Po dokončení předchozích kroků, je uvedena nová bitová kopie, když zvolíte **bitové kopie** karta na portálu Azure classic.  

    ![Vyberte bitovou kopii](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. Vytvoření virtuálního počítače z galerie. Tuto novou bitovou kopii je nyní k dispozici v části **Moje image**.
4. Vyberte novou bitovou kopii. V dalším kroku projít výzev a nastavte název hostitele, heslo, klíč SSH a tak dále.

    ![Vlastní image](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. Po dokončení zřizování, uvidíte FreeBSD virtuální počítač běží v Azure.

    ![Obrázek FreeBSD v azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
