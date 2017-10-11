---
title: "Postup konfigurace cloudové služby (portálu classic) | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat cloudové služby v Azure. Naučte se aktualizovat konfiguraci této cloudové služby a konfigurace vzdáleného přístupu k instancí rolí."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 39bb294c96ce0c12d91cf8b3488ac3e1a7b2f7b2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-cloud-services"></a>Postup konfigurace cloudové služby
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-configure-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-configure.md)
> 
> 

Můžete nakonfigurovat nastavení nejčastěji používaných pro cloudové služby na portálu Azure classic. Pokud chcete místo toho aktualizovat konfigurační soubory přímo, stáhněte si příslušný konfigurační soubor služby a pak ho aktualizujte a nahrajte. Tím aktualizujete konfiguraci cloudové služby. V obou případech se aktualizovaná konfigurace projeví ve všech instancích rolí.

Portál Azure classic můžete taky [povolit připojení ke vzdálené ploše pro roli ve službě Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)

Azure můžete pouze zajistit dostupnost služby 99,95 % během aktualizace konfigurace Pokud máte aspoň dvě instance role pro každou roli. Umožňující jeden virtuální počítač ke zpracování požadavků klientů dalších během aktualizace. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Změnit cloudové služby
1. V [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **konfigurace**.
   
    ![Stránka Konfigurace](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    Na **konfigurace** stránky, můžete nakonfigurovat nastavení aktualizace role, sledování a zvolte hostovaného operačního systému a třídu u instancí role. 
2. V **monitorování**, nastavit Verbose nebo minimální úroveň monitorování a konfigurace diagnostiky připojovacích řetězců, které jsou požadovány pro podrobné monitorování.
3. Pro služby role (seskupené podle role) můžete aktualizovat následující nastavení:
   
    * **Nastavení** změnit hodnoty nastavení různé konfigurace, které jsou určené v *ConfigurationSettings* prvky souboru služby (.csdef).

    * **Certifikáty**  
        Změňte kryptografický otisk certifikátu, který se používá v šifrování SSL pro roli. Chcete-li změnit certifikát, musíte nejprve nahrát nový certifikát (na **certifikáty** stránky). Potom aktualizujte kryptografický otisk certifikátu řetězec zobrazí v nastavení role.
4. V **operačního systému**, můžete změnit operační systém řady nebo verze pro instance rolí, nebo zvolte **automatické** povolit automatické aktualizace aktuální verzi operačního systému. Nastavení operačního systému na webových rolí a rolí pracovního procesu vztahovat, ale neovlivní virtuálních počítačů.
   
    Během nasazení na všech instancích role je nainstalovaná nejnovější verze operačního systému a ve výchozím nastavení se automaticky aktualizují operační systémy. 
   
    Pokud budete potřebovat pro cloudové služby ke spuštění na jinou verzi operačního systému z důvodu požadavky na kompatibilitu ve vašem kódu, můžete operačního systému rodiny a verze. Když zvolíte konkrétní verze operačního systému, aktualizací automatické operačního systému pro cloudové služby jsou pozastavené. Musíte zajistit, že operační systémy přijímat aktualizace.
   
    -Li vyřešit všechny problémy s kompatibilitou, které aplikace mají s nejnovější verzí operačního systému, můžete povolit aktualizace automatické operačního systému tak verzi operačního systému na **automatické**. 
   
    ![Nastavení operačního systému](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. Pokud chcete uložit nastavení konfigurace a vložit je instance rolí, klikněte na tlačítko **Uložit**. (Klikněte na tlačítko **zahodit** zrušit změny.) **Uložit** a **zahodit** jsou přidány do panelu příkazů po provedení změny nastavení.

## <a name="update-a-cloud-service-configuration-file"></a>Aktualizace konfiguračního souboru cloudové služby
1. Stáhněte si cloudové služby konfiguračního souboru (.cscfg) s aktuální konfigurací. Na **konfigurace** stránky pro cloudovou službu, klikněte na tlačítko **Stáhnout**. Pak klikněte na tlačítko **Uložit**, nebo klikněte na tlačítko **uložit jako** k uložení souboru.
2. Po aktualizaci konfigurační soubor služby, odesílání a aktualizace konfigurace:
   
   1. Na **konfigurace** klikněte na tlačítko **nahrát**.
      
       ![Nahrát konfigurace](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. V **konfigurační soubor**, použijte **Procházet** a vyberte soubor .cscfg aktualizované.
   3. Pokud cloudové služby obsahuje všechny role, které mají jenom jednu instanci, vyberte **použít konfiguraci i v případě, že jeden nebo více rolí obsahuje jednu instanci** zaškrtávacího políčka umožníte aktualizace konfigurace pro role, aby bylo možné pokračovat.
      
       Pokud definujete aspoň dvě instance každé role, Azure nemůže zaručit alespoň 99,95 % dostupnosti cloudové služby během aktualizace služby konfigurace. Další informace najdete v tématu [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).
   4. Klikněte na tlačítko **OK** (zaškrtnutí). 

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [nasazení cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* [Správa služby cloud](cloud-services-how-to-manage.md).
* [Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure](cloud-services-role-enable-remote-desktop.md)
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).

