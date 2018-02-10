---
title: "Požadovaného stavu konfigurace rozšíření pomocí šablon Azure Resource Manager | Microsoft Docs"
description: "Definice šablony Resource Manageru pro potřeby stav konfigurace rozšíření v Azure"
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: f638d1530541526316f6e409f1efd44f136992a5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Požadovaný stav konfigurace rozšíření s šablon Azure Resource Manageru

Tento článek popisuje šablony Azure Resource Manageru pro [obslužná rutina rozšíření konfigurace požadovaného stavu](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

*Poznámka: může dojít příklady mírně odlišné schématu. * 
 *Vydání říjnu 2016 došlo ke změně ve schématu.* 
 *Podrobnosti jsou uvedené v části této stránky s názvem*
*[aktualizace z předchozí formátu](##Updating-from-the-Previous-Format)*.

## <a name="template-example-for-a-windows-vm"></a>Příklad šablony pro virtuální počítač s Windows

Následující fragment kódu přejde do části prostředků šablony.
Rozšíření DSC dědí vlastnosti rozšíření výchozí, jak je uvedeno v [VirtualMachineExtension – třída](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.)

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.72",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                    "configurationArguments": {
                        {
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        "RegistrationUrl" : "[parameters('registrationUrl1')]",
                        "NodeConfigurationName" : "nodeConfigurationNameValue1"
                        }
                        },
                        "protectedSettings": {
                            "Items": {
                                        "registrationKeyPrivate": "[parameters('registrationKey1']"
                                    }
                        }
                    }
```

## <a name="template-example-for-windows-vmss"></a>Příklad šablony Windows VMSS

Oddíl "vlastnosti" "VirtualMachineProfile", "extensionProfile" atribut má VMSS uzel. V části "rozšíření" se přidá DSC.

Rozšíření DSC dědí vlastnosti rozšíření výchozí, jak je uvedeno v [VirtualMachineScaleSetExtension třída](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.72",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configurationArguments": {
                                {
                                    "Name": "RegistrationKey",
                                    "Value": {
                                        "UserName": "PLACEHOLDER_DONOTUSE",
                                        "Password": "PrivateSettingsRef:registrationKeyPrivate"
                                    },
                                },
                                "RegistrationUrl" : "[parameters('registrationUrl1')]",
                                "NodeConfigurationName" : "nodeConfigurationNameValue1"
                        }
                        },
                        "protectedSettings": {
                            "Items": {
                                        "registrationKeyPrivate": "[parameters('registrationKey1']"
                                    }
                        }
                    }
                ]
            }
        }
```

## <a name="detailed-settings-information"></a>Informace o podrobné nastavení

Následující schéma je pro část nastavení rozšíření Azure DSC v šablonu Azure Resource Manager.

*Seznam argumentů, která je k dispozici pro výchozí konfigurační skript*
*najdete v následující části s názvem*
*[výchozí konfigurační skript](##Default-Configuration-Script) *.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  }
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Podrobnosti

| Název vlastnosti | Typ | Popis |
| --- | --- | --- |
| settings.wmfVersion |řetězec |Určuje verzi systému Windows Management Framework, který by měly být nainstalovány na vašem virtuálním počítači. Nastavení této vlastnosti na 'nejnovější' nainstaluje nejaktuálnější verzi WMF. Pouze aktuální hodnoty této vlastnosti jsou **'4.0', '5.0', ' 5.0PP' a 'nejnovější'**. Tyto hodnoty jsou předmětem aktualizace. Výchozí hodnota je 'nejnovější'. |
| settings.configuration.url |řetězec |Určuje adresu URL umístění, ze kterého ke stažení souboru zip konfigurace DSC. Pokud zadaná adresa URL vyžaduje SAS token pro přístup, musíte nastavit vlastnost protectedSettings.configurationUrlSasToken na hodnotu tokenu SAS. Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.script nebo settings.configuration.function. Pokud pro tyto vlastnosti je zadána žádná hodnota, rozšíření bude volat výchozí konfigurační skript nastavit LCM metadat a musí být zadán argument. |
| settings.configuration.script |řetězec |Určuje název souboru skriptu, který obsahuje definici konfigurace DSC. Tento skript musí být v kořenové složce stáhnout z adresy URL určeného vlastností configuration.url souboru zip. Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.url nebo settings.configuration.script. Pokud pro tyto vlastnosti je zadána žádná hodnota, rozšíření bude volat výchozí konfigurační skript nastavit LCM metadata a argumenty se má použít. |
| settings.configuration.function |řetězec |Určuje název konfigurace DSC. Konfigurace s názvem musí být obsažené ve skriptu definované configuration.script. Tato vlastnost je vyžadována, pokud jsou definovány settings.configuration.url nebo settings.configuration.function. Pokud pro tyto vlastnosti je zadána žádná hodnota, rozšíření bude volat výchozí konfigurační skript nastavit LCM metadat a musí být zadán argument. |
| settings.configurationArguments |Kolekce |Definuje žádné parametry, které chcete předat do vaší konfigurace DSC. Tato vlastnost není zašifrován. |
| settings.configurationData.url |řetězec |Určuje adresu URL, ze kterého mají být staženy konfiguračního souboru dat (.psd1) chcete použít jako vstup pro konfiguraci DSC. Pokud zadaná adresa URL vyžaduje SAS token pro přístup, musíte nastavit vlastnost protectedSettings.configurationDataUrlSasToken na hodnotu tokenu SAS. |
| settings.privacy.dataEnabled |řetězec |Povolí nebo zakáže telemetrie kolekce. Pro tuto vlastnost pouze možné hodnoty jsou **povolit, "Zakázat", ", nebo $null**. Umožňuje telemetrie, a tato vlastnost prázdná nebo mít hodnotu null. Výchozí hodnota je ". [Další informace](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Kolekce |Určuje alternativní umístění, ze kterého mají být staženy WMF. [Další informace](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Kolekce |Definuje žádné parametry, které chcete předat do vaší konfigurace DSC. Tato vlastnost je zašifrován. |
| protectedSettings.configurationUrlSasToken |řetězec |Určuje token SAS, který má přístup k adrese URL definované configuration.url. Tato vlastnost je zašifrován. |
| protectedSettings.configurationDataUrlSasToken |řetězec |Určuje token SAS, který má přístup k adrese URL definované configurationData.url. Tato vlastnost je zašifrován. |

## <a name="default-configuration-script"></a>Výchozí konfigurační skript

Další informace o těchto hodnotách naleznete na stránce dokumentace [místní základní nastavení nástroje Configuration Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig#basic-settings).
Pouze vlastnosti LCM v následující tabulce jsou možné konfigurovat pomocí rozšíření DSC výchozí konfigurační skript.

| Název vlastnosti | Typ | Popis |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |Požadovaná vlastnost. Určuje, zda je klíč používaný pro uzel k registraci ve službě Azure Automation jako heslo objekt přihlašovacích údajů prostředí PowerShell. Tato hodnota může automaticky zjistit pomocí metody listkeys proti účet Automation a by měly být zabezpečeny jako chráněný nastavení. |
| settings.configurationArguments.RegistrationUrl |řetězec |Požadovaná vlastnost. Určuje adresu Url koncového bodu Azure Automation, kde uzlu, pokusí se registrovat. Tato hodnota může automaticky zjistit pomocí metody reference na účtu Automation. |
| settings.configurationArguments.NodeConfigurationName |řetězec |Požadovaná vlastnost. Určuje konfigurace uzlu v účet Azure Automation, který chcete přiřadit k uzlu. |
| settings.configurationArguments.ConfigurationMode |řetězec |Určuje režim pro správce místní konfigurace. Platné možnosti zahrnují "ApplyOnly", "ApplyandMonitor" a "ApplyandAutoCorrect".  Výchozí hodnota je "ApplyandMonitor". |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | Určuje, jak často se pokusí LCM obraťte se na účtu Automation aktualizací.  Výchozí hodnota je 30.  Minimální hodnota je 15. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | Určuje, jak často bude LCM ověřit aktuální konfiguraci.  Výchozí hodnota je 15.  Minimální hodnota je 15. |
| settings.configurationArguments.RebootNodeIfNeeded | Boolean | Určuje, zda uzel může být automaticky restartovat, pokud operace DSC požaduje.  Výchozí hodnota je false. |
| settings.configurationArguments.ActionAfterReboot | řetězec | Určuje, co se stane po restartování systému při aplikování konfigurace. Platné možnosti jsou "ContinueConfiguration" a "StopConfiguration". Výchozí hodnota je "ContinueConfiguration". |
| settings.configurationArguments.AllowModuleOverwrite | Boolean | Určuje, zda LCM přepíše existující modulů na uzlu.  Výchozí hodnota je false. |

## <a name="settings-vs-protectedsettings"></a>Nastavení vs. ProtectedSettings

Všechna nastavení se ukládají do textového souboru nastavení ve virtuálním počítači.
Vlastnosti v části "nastavení" jsou veřejné vlastnosti, protože nejsou šifrovány nastavení textového souboru.
Vlastnosti v části 'protectedSettings' jsou šifrované pomocí certifikátu a nejsou zobrazeny ve formátu prostého textu v tomto souboru na virtuálním počítači.

Pokud konfigurace potřebuje přihlašovací údaje, můžou být součástí protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Příklad:

V následujícím příkladu je výchozí chování v rozšíření DSC, která je k poskytování nastavení metadat pro správce místní konfigurace a registrace pomocí služby Azure Automation DSC.
Konfigurace argumenty jsou povinné a se předá skriptu konfigurace výchozí nastavení LCM metadat.

```json
"settings": {
    "configurationArguments": {
        {
            "Name": "RegistrationKey",
            "Value": {
                "UserName": "PLACEHOLDER_DONOTUSE",
                "Password": "PrivateSettingsRef:registrationKeyPrivate"
            },
        },
        "RegistrationUrl" : "[parameters('registrationUrl1')]",
        "NodeConfigurationName" : "nodeConfigurationNameValue1"
  }
},
"protectedSettings": {
    "Items": {
                "registrationKeyPrivate": "[parameters('registrationKey1']"
            }
}
```

## <a name="example-using-configuration-script-in-azure-storage"></a>Příklad použití konfigurační skript ve službě Azure Storage

V následujícím příkladu je odvozena z oddílu "Začínáme" [stránku přehled obslužná rutina rozšíření DSC](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
Tento příklad používá šablony Resource Manager místo rutiny pro nasazení rozšíření.
Uložte konfiguraci "IisInstall.ps1", jeho umístění. Soubor ZIP a odeslat soubor na adresu URL přístupné.
Tento příklad používá úložiště objektů blob v Azure, ale je možné stáhnout. Soubory ZIP libovolný odkudkoli.

Následující kód v šablony Azure Resource Manageru dává pokyn virtuálního počítače správný soubor stáhněte a spusťte odpovídající funkce prostředí PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Aktualizace z předchozí formátu

Všechna nastavení v předchozí formátu (obsahující veřejné vlastnosti ModulesUrl ConfigurationFunction, SasToken nebo vlastnosti) automaticky přizpůsobit na současný formát a spusťte stejně jako před.

Následující schéma je co předchozí nastavení schématu hledá jako:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1"
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": {
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Zde je, jak se v předchozím formátu přizpůsobuje na současný formát:

| Název vlastnosti | Předchozí ekvivalent schématu |
| --- | --- |
| settings.wmfVersion |nastavení. WMFVersion |
| settings.configuration.url |settings.ModulesUrl |
| settings.configuration.script |První část nastavení. ConfigurationFunction (před '\\\\.) |
| settings.configuration.function |Druhá část nastavení. ConfigurationFunction (po '\\\\.) |
| settings.configurationArguments |settings.Properties |
| settings.configurationData.url |protectedSettings.DataBlobUri (bez tokenu SAS) |
| settings.privacy.dataEnabled |settings.Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |settings.AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |settings.SasToken |
| protectedSettings.configurationDataUrlSasToken |Token SAS z protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Řešení potíží – kód chyby 1100

Kód chyby 1100 označuje, že došlo k potížím s vstupu uživatele na rozšíření DSC.
Text tyto chyby je proměnná a může změnit.
Zde jsou některé z chyb, které může spustit do a jak můžete opravit je.

### <a name="invalid-values"></a>Neplatné hodnoty

"Je Privacy.dataCollection: {0}.
Pouze možné hodnoty jsou ","Zapnout"a"Zakázat"".
"Je WmfVersion: {0}.
Pouze možné hodnoty jsou... a 'nejnovější' ".

Problém: Zadaná hodnota není povolena.

Řešení: Změňte na platnou hodnotu neplatnou hodnotu.
Najdete v tabulce v části Podrobnosti.

### <a name="invalid-url"></a>Neplatná adresa URL

"Je ConfigurationData.url: {0}. Toto není platná adresa URL.""je DataBlobUri: {0}. Toto není platná adresa URL.""je Configuration.url: {0}. Toto není platná adresa URL."

Problém: A, pokud že adresa URL není platná.

Řešení: Zkontrolujte všechny zadané URL.
Ujistěte se, že všechny adresy URL odkazující na platné umístění, aby měli přístup k rozšíření ve vzdáleném počítači.

### <a name="invalid-configurationargument-type"></a>Neplatný typ ConfigurationArgument

"Neplatný configurationArguments typu {0}"

Problém: Vlastnost ConfigurationArguments nelze přeložit na objekt zatřiďovací tabulky.

Řešení: Zkontrolujte vaši ConfigurationArguments vlastnost zatřiďovací tabulku.
Použijte formát najdete v příkladu který předchází.
Sledujte uvozovky, čárky a složené závorky.

### <a name="duplicate-configurationarguments"></a>Duplicitní ConfigurationArguments

"Nalezena ve veřejných i chráněný configurationArguments duplicitní argumenty {0}."

Problém: ConfigurationArguments v nastavení veřejných a ConfigurationArguments v chráněných nastavení obsahovat vlastnosti se stejným názvem.

Řešení: Odeberte jedno z duplicitní vlastnosti.

### <a name="missing-properties"></a>Chybí vlastnosti
"Configuration.function vyžaduje, aby byl zadán configuration.url nebo configuration.module"

"Configuration.url vyžaduje, aby byl že tento configuration.script je zadán"

"Configuration.script vyžaduje, aby byl že tento configuration.url je zadán"

"Configuration.url vyžaduje, aby byl že tento configuration.function je zadán"

"ConfigurationUrlSasToken vyžaduje, aby byl že tento configuration.url je zadán"

"ConfigurationDataUrlSasToken vyžaduje, aby byl že tento configurationData.url je zadán"

Problém: Definované vlastnosti potřebuje další vlastnost, která nebyla nalezena.

Řešení:

- Zadejte chybějící vlastnost.
- Odeberte vlastnost, která potřebuje chybí vlastnost.

## <a name="next-steps"></a>Další kroky

Další informace o DSC a sadách škálování virtuálních počítačů [použití sad škálování k virtuální počítač s příponou DSC Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).

Najít další podrobnosti na [správu zabezpečení přihlašovacích údajů DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Další informace o obslužná rutina rozšíření Azure DSC najdete v tématu [Úvod do rozšíření obslužné rutiny konfigurace požadovaného stavu Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Další informace o DSC Powershellu [přejděte do centra dokumentace k prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview).
