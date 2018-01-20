---
title: "Jak používat rozhraní API (Python) – Průvodce funkcemi pro správu služby"
description: "Zjistěte, jak programově provádět běžné úlohy správy služby z Pythonu."
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: d0fd1063194ecbccb0af1abc0c441aa65b18883b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-service-management-from-python"></a>Jak používat správu služby z Pythonu
Tento průvodce vám ukáže, jak programově provádět běžné úlohy správy služby z Pythonu. **ServiceManagementService** třídy v [Azure SDK pro jazyk Python](https://github.com/Azure/azure-sdk-for-python) podporuje programový přístup k mnohem týkajících se správy funkcí služby, který je k dispozici v [Azure portál] [ management-portal] (například **vytváření, aktualizaci a odstraňování cloudové služby, nasazení, služby pro správu dat a virtuální počítače**). Tato funkce může být užitečné při vytváření aplikace, které potřebují programový přístup ke správě služby.

## <a name="WhatIs"></a>Co je služba správy
Service Management API zajišťují programový přístup ke většinu funkcí správy služby k dispozici prostřednictvím [portál Azure][management-portal]. Sada Azure SDK pro Python umožňuje spravovat cloudové služby a účty úložiště.

Chcete-li použít rozhraní API pro správu služby, je potřeba [vytvoření účtu Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"></a>Koncepty
Sada Azure SDK pro Python zabalí [Azure Service Management API][svc-mgmt-rest-api], což je rozhraní REST API. Všechny operace rozhraní API se provádí přes SSL a vzájemně se ověřují pomocí certifikátů X.509 v3. Ke službě pro správu se dá přistupovat ze služby spuštěné v Azure nebo přímo přes internet z jakékoliv aplikace, která umí poslat požadavek HTTPS a přijmout odpověď HTTPS.

## <a name="Installation"></a>Instalace
Všechny funkce, které jsou popsané v tomto článku jsou k dispozici v `azure-servicemanagement-legacy` balíčku, které můžete nainstalovat pomocí nástroje pip. Další informace o instalaci (například pokud jste novým uživatelem Python), najdete v tomto článku: [instalaci Python a sady Azure SDK](../python-how-to-install.md)

## <a name="Connect"></a>Postupy: připojení služby správy
Pro připojení ke koncovému bodu služby správy, musíte svoje ID předplatného Azure a certifikát pro správu platné. Můžete získat svoje ID předplatného prostřednictvím [portál Azure][management-portal].

> [!NOTE]
> Nyní je možné použít certifikáty vytvořené pomocí OpenSSL spuštěná v systému Windows.  Vyžaduje Python 2.7.4 nebo novější. Doporučujeme, abyste uživatelům používat OpenSSL místo .pfx, protože podpora pro .pfx, certifikáty budou odebrány pravděpodobně v budoucnu.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Certifikáty pro správu v systému Windows nebo Mac/Linux (OpenSSL)
Můžete použít [OpenSSL](http://www.openssl.org/) vytvořit svůj certifikát pro správu.  Ve skutečnosti je nutné vytvořit dva certifikáty, jeden pro server ( `.cer` souboru) a jeden pro klienta ( `.pem` souboru). Chcete-li vytvořit `.pem` soubor, spustit:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Chcete-li vytvořit `.cer` certifikátů, spustit:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Další informace o Azure certifikáty najdete v tématu [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md). Úplný popis parametrů OpenSSL, naleznete v dokumentaci na [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Po vytvoření těchto souborů, budete muset nahrát `.cer` souboru k Azure přes "Nahrávání" akce "Nastavení" karty [portál Azure][management-portal], a budete muset poznamenejte uloženému `.pem` souboru.

Po získání svoje ID předplatného, vytvořit certifikát a nahrán `.cer` souboru do Azure, se můžete připojit ke koncovému bodu správy Azure pomocí id předplatného a cestu k předání `.pem` do souboru  **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

V předchozím příkladu `sms` je **ServiceManagementService** objektu. **ServiceManagementService** třída je primární třída, která slouží ke správě služby Azure.

### <a name="management-certificates-on-windows-makecert"></a>Certifikáty pro správu v systému Windows (MakeCert)
Můžete vytvořit certifikát podepsaný svým držitelem správy na váš počítač pomocí `makecert.exe`.  Otevřete **příkazového řádku Visual Studia** jako **správce** a použijte následující příkaz, nahraďte *AzureCertificate* se název certifikátu, který chcete použít.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Příkaz vytvoří `.cer` souboru a nainstaluje do **osobní** úložiště certifikátů. Další informace najdete v tématu [Přehled certifikátů pro Azure Cloud Services](cloud-services-certs-create.md).

Po vytvoření certifikátu, budete muset nahrát `.cer` souboru k Azure přes "Nahrávání" akce "Nastavení" karty [portál Azure][management-portal].

Po získání svoje ID předplatného, vytvořit certifikát a nahrán `.cer` souboru do Azure, se můžete připojit ke koncovému bodu správy Azure pomocí předáním id předplatného a umístění certifikátu v vaší **osobní**  úložišti certifikátů **ServiceManagementService** (znovu, nahraďte *AzureCertificate* s názvem vašeho certifikátu):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

V předchozím příkladu `sms` je **ServiceManagementService** objektu. **ServiceManagementService** třída je primární třída, která slouží ke správě služby Azure.

## <a name="ListAvailableLocations"></a>Postupy: seznam dostupných umístění
K zobrazení seznamu umístění, které jsou k dispozici pro hostování služeb, použijte **seznamu\_umístění** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Při vytváření cloudové služby nebo služba úložiště, musíte zadat platné umístění. **Seznamu\_umístění** metoda vždy vrátí hodnotu aktuální seznam aktuálně dostupných umístění. Době psaní tohoto textu, jsou k dispozici umístění:

* Západní Evropa
* Severní Evropa
* Jihovýchodní Asie
* Východní Asie
* Střed USA
* Střed USA – sever
* Střed USA – jih
* Západní USA
* Východ USA
* Japonsko – východ
* Japonsko – západ
* Brazílie – jih
* Austrálie – východ
* Austrálie – jihovýchod

## <a name="CreateCloudService"></a>Postupy: vytvoření cloudové služby
Když vytvoříte aplikaci a spustíte ho v Azure, kód a konfigurace společně se nazývají Azure [Cloudová služba] [ cloud service] (označované jako *hostovaná služba* v dřívější Azure uvolní). **Vytvořit\_hostované\_služby** metoda vám umožní vytvořit novou hostovanou službu tím, že poskytuje název hostované služby (které musí být jedinečné v Azure), popisek (automaticky kódovaný formátu Base64), popis, a umístění.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Můžete vytvořit seznam všechny hostované služby pro vaše předplatné s **seznamu\_hostované\_služby** metoda:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Pokud chcete získat informace o konkrétní hostovanou službu, můžete tak učinit pomocí název hostované služby k předání **získat\_hostované\_služby\_vlastnosti** metoda:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Po vytvoření cloudové služby, můžete nasadit kódu na služby s **vytvořit\_nasazení** metoda.

## <a name="DeleteCloudService"></a>Postupy: odstranění cloudové služby
Cloudové služby můžete odstranit pomocí názvu služby k předání **odstranit\_hostované\_služby** metoda:

    sms.delete_hosted_service('myhostedservice')

Před odstraněním služby, musíte nejprve odstranit všechna nasazení pro službu. (Viz [postupy: odstranění nasazení](#DeleteDeployment) podrobnosti.)

## <a name="DeleteDeployment"></a>Postupy: odstranění nasazení
Pokud chcete odstranit nasazení, použijte **odstranit\_nasazení** metoda. Následující příklad ukazuje, jak odstranit nasazení s názvem `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>Postupy: vytvoření služby úložiště
A [služba úložiště](../storage/common/storage-create-storage-account.md) dává vám přístup k Azure [objekty BLOB](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabulky](../cosmos-db/table-storage-how-to-use-python.md), a [fronty](../storage/queues/storage-python-how-to-use-queue-storage.md). Vytvoření služby, úložiště, je třeba název služby (mezi 3 a 24 malých písmen a jedinečný v rámci Azure), popis, popisek (až 100 znaků, automaticky kódovaný formátu Base64) a umístění. Následující příklad ukazuje, jak vytvořit službu úložiště tak, že zadáte umístění.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Poznámka: v předchozím příkladu, stav **vytvořit\_úložiště\_účet** operaci se dá načíst pomocí předání výsledek vrácený **vytvořit\_úložiště\_účet** k **získat\_operace\_stav** metoda.  

Můžete vytvořit seznam účtů úložiště a jejich vlastnosti s **seznamu\_úložiště\_účty** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>Postupy: odstranění služby úložiště
Služba úložiště můžete odstranit pomocí předání názvu služby úložiště na **odstranit\_úložiště\_účet** metoda. Odstraněním úložiště služby se odstraní všechna data uložená ve službě (objekty BLOB, tabulek a front).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>Postupy: seznam dostupných operačních systémů
K zobrazení seznamu operačních systémů, které jsou k dispozici pro hostování služeb, použijte **seznamu\_operační\_systémy** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Alternativně můžete použít **seznamu\_operační\_systému\_rodiny** metodu, která skupiny operační systémy řady:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>Postupy: vytvoření image operačního systému
Chcete-li přidat bitovou kopii operačního systému do úložiště bitové kopie, použijte **přidat\_operačního systému\_bitové kopie** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

K zobrazení seznamu bitové kopie operačního systému, které jsou k dispozici, použijte **seznamu\_os\_bitové kopie** metoda. Obsahuje všechny Image platformy a uživatele bitové kopie:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"></a>Postupy: odstranění image operačního systému
Chcete-li odstranit uživatelskou image, použijte **odstranit\_operačního systému\_bitové kopie** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>Postupy: vytvoření virtuálního počítače
Pokud chcete vytvořit virtuální počítač, musíte nejprve vytvořit [Cloudová služba](#CreateCloudService).  Pak vytvořte pomocí nasazení virtuálního počítače **vytvořit\_virtuální\_počítač\_nasazení** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"></a>Postupy: odstranění virtuálního počítače
Pokud chcete odstranit virtuální počítač, je nejprve odstranit nasazení pomocí **odstranit\_nasazení** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Cloudové služby můžete odstranit pak pomocí **odstranit\_hostované\_služby** metoda:

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Postupy: Vytvoření virtuálního počítače z Image zaznamenané virtuálního počítače
K zachycení image virtuálního počítače, první volání **zaznamenat\_virtuálních počítačů\_image** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Další, abyste měli jistotu, že úspěšně zaznamenáte bitovou kopii, použijte **seznamu\_virtuálních počítačů\_bitové kopie** rozhraní api a zajistěte, aby bitové kopie se zobrazí ve výsledcích:

    images = sms.list_vm_images()

Nakonec vytvoření virtuálního počítače pomocí zaznamenané bitové kopie, použijte **vytvořit\_virtuální\_počítač\_nasazení** jako předtím, ale tentokrát předávat vm_image_name místo toho – metoda

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Další informace o tom, jak zachytit virtuální počítač s Linuxem najdete v tématu [jak zachytit virtuální počítač s Linuxem.](../virtual-machines/linux/classic/capture-image-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

Další informace o tom, jak zachytit virtuální počítač Windows najdete v tématu [jak zachytit virtuální počítač Windows.](../virtual-machines/windows/classic/capture-image-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="What's Next"></a>Další kroky
Teď, když jste se naučili základy používání služby Správa služby, dostanete [referenční dokumentace dokončení rozhraní API pro Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) a provádět komplexní úlohy snadno ke správě aplikace python.

Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://portal.azure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
