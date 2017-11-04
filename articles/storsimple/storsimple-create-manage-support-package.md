---
title: "Vytvoření balíčku pro podporu StorSimple | Microsoft Docs"
description: "Naučte se vytvářet, dešifrování a upravovat balíčku pro podporu pro zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: addfb01998271ee3a431d92bf2a6fec70f0577a6
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a>Vytvoření a Správa balíčku pro podporu zařízení StorSimple
> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Pokud chcete zobrazit verzi tohoto článku na nový portál Azure, přejděte na [vytvoření a Správa balíčku pro podporu StorSimple](storsimple-8000-create-manage-support-package.md). Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Přehled
Balíček pro podporu zařízení StorSimple je mechanismus snadné použití, který shromažďuje všechny relevantní protokoly Microsoft Support pomáhající při řešení potíží všechny problémy zařízení StorSimple. Shromážděné protokoly jsou zašifrované a komprimované.

Tento kurz obsahuje podrobné pokyny k vytváření a správě balíček pro podporu.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Vytvoření a odeslání balíčku pro podporu na portálu Azure classic
Můžete vytvořit a odeslat na web Microsoft Support prostřednictvím balíčku pro podporu **údržby** stránka služby v portálu Azure classic.

> [!NOTE]
> Nahrávání vyžaduje podporu klíč. Pracovníka podpory by měl poskytovat to pro vás e-mailem.
> 
> 

Podpoře šifrované a komprimované balíčku (.cab) je vytvořen a odesláno na webu podpory. Pracovník podpory pak můžete načíst tento balíček z webu podpory k řešení problému.

Proveďte následující kroky na portálu classic k vytvoření balíčku pro podporu.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>K vytvoření balíčku pro podporu na portálu Azure classic
1. Vyberte **zařízení** > **údržby**.
2. V **balíček pro podporu** vyberte **vytvoření a nahrání balíčku pro podporu**.
3. V **vytvoření a nahrání balíčku pro podporu** dialogové okno pole, postupujte takto:
   
    ![Vytvoření balíčku pro podporu](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * V **podporu přístupový klíč** textové pole, zadejte klíč. Pracovníka podpory společnosti Microsoft, by měli tento klíč poslat vám v e-mailu.
   * Zaškrtněte políčko souhlasu zajistit automaticky odeslat balíček pro podporu na webu Microsoft Support.
   * Klikněte na ikonu zaškrtnutí ![Ikona zaškrtnutí](./media/storsimple-create-manage-support-package/IC740895.png).

## <a name="manually-create-a-support-package"></a>Ruční vytvoření balíčku pro podporu
V některých případech musíte ručně vytvořit balíček pro podporu prostřednictvím Windows Powershellu pro StorSimple. Například:

* Pokud je třeba odebrat citlivé informace ze souborů protokolu před sdílení s Microsoft Support.
* Pokud máte potíže s odeslání balíčku kvůli problémům s připojením.

Váš balíček ručně generovaného podpory můžete sdílet s Microsoft Support prostřednictvím e-mailu. Proveďte následující kroky k vytvoření balíčku pro podporu ve Windows Powershellu pro StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Vytvoření balíčku pro podporu ve Windows Powershellu pro StorSimple
1. Chcete-li spustit relaci prostředí Windows PowerShell s oprávněními správce na vzdáleném počítači, který se používá k připojení k zařízení StorSimple, zadejte následující příkaz:
   
    `Start PowerShell`
2. V relaci prostředí Windows PowerShell se připojte ke konzole SSAdmin vašeho zařízení:
   
   * Na příkazovém řádku zadejte:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * V dialogovém okně, které se otevře zadejte heslo správce zařízení. Výchozí heslo je:
     
      `Password1`
     
      ![Dialogové okno přihlašovacích údajů prostředí PowerShell](./media/storsimple-create-manage-support-package/IC740962.png)
   * Vyberte **OK**.
   * Na příkazovém řádku zadejte:
     
      `Enter-PSSession $MS`
3. V této relaci, které se otevře zadejte příslušný příkaz.
   
   * Pro sdílené síťové složky, které jsou chráněné heslem zadejte:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       (Protože balíček pro podporu je zašifrován) budete vyzváni k zadání hesla, cestu ke sdílené síťové složce a šifrovací přístupové heslo. Balíček pro podporu se pak vytvoří v zadané složce.
   * Pro sdílené složky, které nejsou chráněné heslem, není nutné `-Credential` parametr. Zadejte následující příkaz:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       Podpora balíček je vytvořen pro oba řadiče v zadané síťové sdílené složce. Je soubor zašifrované, komprimované, který lze odeslat společnosti Microsoft Support pro řešení potíží. Další informace najdete v tématu [obraťte se na podporu společnosti Microsoft](storsimple-contact-microsoft-support.md).

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Parametry rutiny Export-HcsSupportPackage
Pomocí rutiny Export-HcsSupportPackage můžete použít následující parametry.

| Parametr | Požadované a volitelné | Popis |
| --- | --- | --- |
| `-Path` |Požaduje se |Pomocí zadejte umístění síťové sdílené složky, ve kterém je umístěn balíček pro podporu. |
| `-EncryptionPassphrase` |Požaduje se |Pomocí lze zadat přístupové heslo k pomoci podporu balíček šifrovat. |
| `-Credential` |Nepovinné |Slouží k poskytování přihlašovací údaje pro sdílené síťové složce přístup. |
| `-Force` |Nepovinné |Použijte šifrovací přístupové heslo potvrzení krok přeskočit. |
| `-PackageTag` |Nepovinné |Použijte k určení adresáře v rámci *cesta* v balíčku pro podporu je umístěn. Výchozí hodnota je [název]-[aktuální datum a time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |Nepovinné |Zadejte jako **clusteru** (výchozí) k vytvoření balíčku pro podporu pro oba řadiče. Pokud chcete vytvořit balíček pouze pro aktuální kontroler, zadejte **řadič**. |

## <a name="edit-a-support-package"></a>Upravte balíček podpory
Po vygenerování balíčku pro podporu, možná budete muset upravit balíček, který chcete odebrat citlivé informace. To může zahrnovat názvů svazků, zařízení IP adresy a názvy zálohování ze souborů protokolů.

> [!IMPORTANT]
> Můžete upravit pouze podporu balíček, který byl vytvořen pomocí Windows Powershellu pro StorSimple. Nelze upravit balíček vytvořený na portálu Azure classic pomocí služby StorSimple Manager.
> 
> 

Upravit balíčku pro podporu před nahráním na webu Microsoft Support, nejprve dešifrovat balíček pro podporu, upravte soubory a pak ho znovu zašifrovat. Proveďte následující kroky.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Chcete-li upravit balíčku pro podporu ve Windows Powershellu pro StorSimple
1. Generovat balíček podporu jak je popsáno dříve, v [k vytvoření balíčku pro podporu ve Windows Powershellu pro StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Stáhnout skript](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně na vašeho klienta.
3. Naimportujte modul prostředí Windows PowerShell. Zadejte cestu k místní složce, ve které jste stáhli skript. Chcete-li importovat modul, zadejte:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Všechny soubory jsou *.aes* soubory, které jsou komprimovaná a šifrovaná. Chcete-li dekomprimovat a dešifrování souborů, zadejte:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Všimněte si, že skutečný soubor rozšíření se nyní zobrazují všechny soubory.
   
    ![Upravte balíček podpory](./media/storsimple-create-manage-support-package/IC750706.png)
5. Když se zobrazí výzva k šifrovací přístupové heslo, zadejte heslo, které jste použili při vytvoření balíčku podpory.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Přejděte do složky, která obsahuje soubory protokolu. Protože soubory protokolu jsou nyní dekomprimovat a dešifrovat, to bude mít původní přípony souborů. Tyto soubory odebrat jakékoli informace o zákazníkovi například názvů svazků a zařízení IP adresy, upravovat a ukládat soubory.
7. Zavřete soubory a komprese je s gzip, je šifrování pomocí standardu AES 256. Toto je pro rychlost a zabezpečení v balíčku pro podporu přenosu přes síť. Možnost komprimace a šifrování souborů, zadejte následující příkaz:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Upravte balíček podpory](./media/storsimple-create-manage-support-package/IC750707.png)
8. Po zobrazení výzvy zadejte šifrovací přístupové heslo pro balíček upravené podpory.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. Zapište nové přístupové heslo, takže ho můžete sdílet s Microsoft Support na požádání.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Příklad: Úprava souborů v balíčku pro podporu ve sdílené složce chráněné heslem
Následující příklad ukazuje, jak k dešifrování, upravit a znovu zašifrovat balíčku pro podporu.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak [k řešení potíží s nasazením zařízení použít podporu balíčky a protokolů zařízení](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Zjistěte, jak [použít službu StorSimple Manager ke správě zařízení StorSimple](storsimple-manager-service-administration.md).

