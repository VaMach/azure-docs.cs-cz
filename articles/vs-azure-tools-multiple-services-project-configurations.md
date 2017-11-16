---
title: "Konfigurace projektu Azure pomocí více konfigurace služby | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat změnou ServiceDefinition.csdef a souboru ServiceConfiguration.cscfg souborů projektu Azure cloud service."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a4fb79ed-384f-4183-9f74-5cac257206b9
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 59492ccc64a70680d71ad1de0700ed30f9e45306
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfigurace projektu Azure pomocí více konfigurace služby
Projekt Azure cloud service zahrnuje dvě konfigurační soubory: ServiceDefinition.csdef a souboru ServiceConfiguration.cscfg. Tyto soubory jsou zabalit s aplikací Azure cloud service a nasazení do Azure.

* **ServiceDefinition.csdef** soubor obsahuje metadata, která je požadované pro prostředí Azure pro požadavky na aplikace cloudové služby, včetně rolí, které obsahuje. Tento soubor zároveň obsahuje nastavení konfigurace, které se vztahují na všechny instance. Tato nastavení konfigurace můžete přečíst v době běhu pomocí služby hostování modulu Runtime rozhraní API služby Azure. Tento soubor nelze aktualizovat, když vaše služba běží v Azure.
* **Souboru ServiceConfiguration.cscfg** souboru nastaví hodnoty pro nastavení konfigurace definované v definičním souboru služby a určuje počet instancí pro jednotlivé role. Tento soubor můžete aktualizovat, když cloudové služby běží v Azure.

Nástroje pro Azure pro sadu Microsoft Visual Studio poskytují stránky vlastností, které můžete použít k nastavení konfigurace uložené v těchto souborech. Pro přístup k stránek vlastností, dvakrát klikněte na odkaz na roli pod projektu Azure cloudové služby v Průzkumníku řešení, nebo klikněte pravým tlačítkem na odkaz na roli a vyberte **vlastnosti**, jak je znázorněno na následujícím obrázku.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Informace o základní schémata pro definice služby a soubory konfigurace služby najdete v tématu [.csdef schématu XML](cloud-services/schema-csdef-file.md) a [.cscfg schématu XML](cloud-services/schema-cscfg-file.md) články. Další informace o konfiguraci služby najdete v tématu [postup konfigurace cloudové služby](cloud-services/cloud-services-how-to-configure-portal.md).

## <a name="configuring-role-properties"></a>Konfigurace vlastností role
Stránky vlastností pro webovou roli a roli pracovního procesu jsou podobné, i když existuje několik rozdílů, na kterou v následujících částech.

Z **ukládání do mezipaměti** stránky, můžete nakonfigurovat ukládání do mezipaměti služby Azure.

### <a name="configuration-page"></a>Stránka Konfigurace
Na **konfigurace** stránky, můžete nastavit tyto vlastnosti:

**Instance**

Nastavte **Instance** počet vlastnosti tak, aby se počet instancí služby měly být spuštěny pro tuto roli.

Nastavte **velikost virtuálního počítače** vlastnost **navíc malé**, **malé**, **střední**, **velké**, nebo **Velmi velké**.  Další informace najdete v tématu [velikosti pro cloudové služby](cloud-services/cloud-services-sizes-specs.md).

**Po spuštění akce** (jenom webové Role)

Nastavte tuto vlastnost k určení, že by měl Visual Studio při spuštění ladění spustit webový prohlížeč, koncových bodů protokolu HTTP nebo HTTPS koncových bodů, nebo obojí.

Možnost koncový bod HTTPS je k dispozici pouze v případě, že jste definovali koncový bod HTTPS již pro vaši roli. Můžete definovat koncový bod HTTPS na **koncové body** stránku vlastností.

Pokud jste už přidali koncový bod HTTPS, ve výchozím nastavení je povolena možnost koncový bod HTTPS a Visual Studio spustí prohlížeč pro tento koncový bod, když spustíte, ladění, kromě prohlížeče pro svůj koncový bod HTTP. Předpokladem je, že jsou povolené oba možnosti spuštění.

**Diagnostika**

Diagnostika je ve výchozím nastavení povolena pro webovou roli. Účet služby Azure cloud projektu a úložiště jsou nastaveny na používat emulátor místního úložiště. Pokud budete chtít nasadit do Azure, můžete vybrat tlačítko Tvůrce (**...** ) se aktualizovat účet úložiště pro použití úložiště Azure v cloudu. Diagnostická data můžou přenášet k účtu úložiště na vyžádání nebo v automaticky naplánovaných intervalech. Další informace o Azure diagnostics najdete v tématu [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Nastavení stránky
Na **nastavení** stránky, můžete přidat nastavení konfigurace služby. Nastavení konfigurace jsou páry název hodnota. Kód spuštěný v roli může číst hodnoty nastavení konfigurace v době běhu pomocí třídy poskytované [spravované knihovny Azure](http://go.microsoft.com/fwlink?LinkID=171026). Konkrétně [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) vrátí metoda hodnotu s názvem konfigurace nastavení za běhu.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Konfigurace připojovacího řetězce k účtu úložiště
Připojovací řetězec je nastavení konfigurace, která poskytuje informace o připojení a ověřování pro emulátor úložiště nebo pro účet úložiště Azure. Vždy, když kód musí přístup k datům služby úložiště Azure – to znamená, objektů blob, fronty nebo dat v tabulce – Kód spuštěný v roli, je nutné zadat připojovací řetězec pro daný účet úložiště.

Připojovací řetězec, který odkazuje na účet úložiště Azure, musíte použít definované formát. Informace o tom, jak vytvořit připojovacích řetězců najdete v tématu [nakonfigurování připojovacích řetězců úložiště Azure](storage/common/storage-configure-connection-string.md).

Až budete připraveni k testování služby pro služby Azure storage, nebo když budete připraveni k nasazení cloudové služby na Azure, můžete změnit hodnotu libovolné púřipojovací řetězce tak, aby odkazovaly na váš účet úložiště Azure. Vyberte (**...** ), vyberte **zadejte přihlašovací údaje účtu úložiště**. Zadejte informací o vašem účtu, který obsahuje název účtu a klíč účtu. V **připojovací řetězce k účtu úložiště** dialogové okno, můžete také znamená to, zda použít výchozí HTTPS koncové body (výchozí možnost), výchozí koncové body protokolu HTTP nebo vlastní koncové body. Můžete se rozhodnout používat vlastní koncové body, pokud je zaregistrovaný vlastní název domény pro vaši službu, jak je popsáno v [nakonfigurovat vlastní název domény pro data objektů blob v účtu úložiště Azure](storage/blobs/storage-custom-domain-name.md).

> [!IMPORTANT]
> Je třeba upravit připojovací řetězce tak, aby odkazoval na účet úložiště Azure, před nasazením služby. V opačném případě to může způsobit vaše role není spuštění a cyklicky přepínat mezi stavy inicializace, zaneprázdněn a ukončení.
> 
> 

## <a name="endpoints-page"></a>Stránka Koncové body
Role pracovních procesů může mít libovolný počet koncových bodů protokolu HTTP, HTTPS nebo TCP. Koncové body můžou být vstupních koncových bodů, které jsou k dispozici pro externí klienty, nebo vnitřních koncových bodů, které jsou k dispozici na jiných rolí, které běží ve službě.

* Koncový bod HTTP zpřístupnit do externích klientů a webové prohlížeče, změňte typ koncového bodu jako vstup a zadejte název a číslo veřejného portu.
* Aby byl dostupný koncový bod HTTPS do externích klientů a webových prohlížečů, změnit typ koncového bodu na **vstupní**a zadejte název, číslo veřejného portu a název certifikátu pro správu.
  
    Všimněte si, že abyste mohli zadat certifikát pro správu, je nutné na definovat certifikát **certifikáty** stránku vlastností.
* Koncový bod zpřístupnit pro interní přístupu podle rolí v rámci cloudové služby, změňte typ koncového bodu na interní a zadejte název a privátní porty možné pro tento koncový bod.

## <a name="local-storage-page"></a>Stránka Místní úložiště
Můžete použít **místní úložiště** stránka vlastností můžete vyhradit jeden nebo více prostředků místní úložiště pro roli. Prostředek Místní úložiště je vyhrazené adresáře v systému souborů Azure virtuálního počítače, ve kterém je spuštěna instance role.

## <a name="certificates-page"></a>Stránka certifikátů
Na **certifikáty** stránky, certifikáty můžete přidružit vaši roli. Certifikáty, které přidáte lze nakonfigurovat na koncové body HTTPS **koncové body** stránku vlastností.

**Certifikáty** stránka vlastností přidá informace o certifikáty konfigurace služby. Všimněte si, že certifikáty nejsou spojených s službou; musíte nahrát certifikáty samostatně do Azure pomocí [portál Azure](http://portal.azure.com).

Pokud chcete přiřadit certifikát vaše role, zadejte název pro certifikát. Tento název je použít k odkazování na certifikátu, když konfigurujete na koncový bod HTTPS **koncové body** stránku vlastností. V dalším kroku určete, zda je úložiště certifikátů **místního počítače** nebo **aktuální uživatel** a název úložiště. Nakonec zadejte kryptografický otisk certifikátu. Pokud je certifikát v aktuální User\Personal (My) úložiště, můžete zadat kryptografický otisk certifikátu výběrem certifikátu z vyplněná seznamu. Pokud se nachází v jiném umístění, zadejte hodnotu kryptografického otisku ručně.

Když přidáte certifikát z úložiště certifikátů, nastavení konfigurace pro vás se automaticky přidají všechny zprostředkující certifikáty. Chcete-li správně nakonfigurovat služby pro protokol SSL, musí se tyto zprostředkující certifikáty také nahrát do Azure.

Všechny certifikáty správy, které přidružit služby použít k službě jenom v případě, že je spuštěna v cloudu. Pokud vaše služba běží v místní vývojové prostředí, používá standardní certifikát, který je spravován pomocí emulátoru služby výpočty v.

## <a name="configuring-the-azure-cloud-service-project"></a>Konfigurace projektu Azure cloudové služby
Chcete-li nakonfigurovat nastavení vztahující se k projektu celý Azure cloud service, poprvé otevřete místní nabídku pro tento uzel projektu a zvolte Vlastnosti otevřete jeho stránky vlastností. V následující tabulce jsou tyto stránky vlastností.

| Stránky vlastností | Popis |
| --- | --- |
| Aplikace |Z této stránky můžete zobrazit informace o verzi nástroje Azure, která používá tento projekt cloudové služby, a můžete upgradovat na aktuální verzi nástroje. |
| Události sestavení |Na této stránce můžete nastavit před a po sestavení události. |
| Vývoj |Z této stránky můžete zadat pokyny ke konfiguraci sestavení a podmínky, za kterých se spustit žádné události po sestavení. |
| Web |Z této stránky můžete nakonfigurovat nastavení, které se vztahují na webový server. |

