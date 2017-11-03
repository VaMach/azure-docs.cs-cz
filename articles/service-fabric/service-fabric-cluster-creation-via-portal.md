---
title: "Vytvořit cluster Service Fabric na portálu Azure | Microsoft Docs"
description: "Tento článek popisuje postup nastavení zabezpečení clusteru Service Fabric v Azure pomocí portálu Azure a Azure Key Vault."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: 874cf647d4b708bbbc64246ac0dff133639ad86c
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Vytvořit cluster Service Fabric v Azure pomocí portálu Azure
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Toto je podrobný průvodce, který vás provede kroky nastavení zabezpečení clusteru Service Fabric v Azure pomocí portálu Azure. Tento průvodce vás provede následující kroky:

* Nastavte Key Vault pro správu klíčů pro zabezpečení clusteru.
* Vytvoření zabezpečeného clusteru v Azure prostřednictvím portálu Azure.
* Ověřování pomocí certifikátů správci.

> [!NOTE]
> Pro pokročilejší možnosti zabezpečení, jako je například ověřování uživatelů s Azure Active Directory a nastavení certifikátů pro zabezpečení aplikací [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].
> 
> 

Cluster s podporou zabezpečení je clusteru, který brání neoprávněným přístupem k operace správy, která zahrnuje nasazení, upgrade a odstranění aplikací, služeb a data, která obsahují. Nezabezpečená clusteru je cluster každý, kdo může připojit k kdykoli a provádět operace správy. Přestože je možné vytvořit nezabezpečená clusteru, je **důrazně doporučujeme vytvořit cluster zabezpečené**. Nezabezpečená clusteru **nelze zabezpečit později** -je nutné vytvořit nový cluster.

Koncepty jsou stejné pro vytvoření zabezpečeného clusterů, jestli jsou clusterů systému Windows nebo Linux clusterů. Další informace a pomocné rutiny skripty pro vytvoření zabezpečeného clusterů systému Linux, najdete v tématu [vytváření zabezpečené clusterů v systému Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters). Parametry získané zadané pomocné rutiny skript můžete zadat přímo do portálu, jak je popsáno v části [na portálu Azure vytvořit cluster](#create-cluster-portal).

## <a name="configure-key-vault"></a>Konfigurace trezoru klíčů 
### <a name="log-in-to-azure"></a>Přihlaste se k Azure.
Tato příručka používá [prostředí Azure PowerShell][azure-powershell]. Při spouštění novou relaci prostředí PowerShell, přihlaste se k účtu Azure a vybrat své předplatné před provedením Azure příkazy.

Přihlaste se k účtu azure:

```powershell
Login-AzureRmAccount
```

Vyberte předplatné:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="set-up-key-vault"></a>Nastavení služby Key Vault
Tato část průvodce vás provede procesem vytvoření trezoru klíč pro cluster Service Fabric v Azure a aplikace Service Fabric. Dokončení průvodce v Key Vault, najdete v článku [Key Vault Příručka Začínáme][key-vault-get-started].

Service Fabric používá certifikáty X.509 k zabezpečení clusteru. Azure Key Vault se používá ke správě certifikátů pro clusterů Service Fabric v Azure. Pokud cluster je nasazené v Azure, poskytovatel prostředků Azure, který je zodpovědný za vytváření clusterů Service Fabric vyžaduje certifikáty od Key Vault a nainstaluje je v clusteru virtuálních počítačů.

Následující diagram znázorňuje vztah mezi Key Vault, cluster Service Fabric a poskytovatel prostředků Azure, která používá certifikáty uložené v Key Vault, při vytváření clusteru:

![Instalace certifikátu][cluster-security-cert-installation]

#### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Prvním krokem je vytvoření nové skupiny prostředků speciálně pro Key Vault. Uvedení Key Vault do vlastní skupiny prostředků se doporučuje, aby odeberete skupiny prostředků výpočetního prostředí a úložiště – například skupiny prostředků, který má cluster Service Fabric – bez ztráty klíče a tajné klíče. Skupinu prostředků, která má Key Vault musí být ve stejné oblasti jako cluster, který se používá.

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

#### <a name="create-key-vault"></a>Vytvoření trezoru klíčů
Vytvoření trezoru klíč do nové skupiny prostředků. Key Vault **musí být povolen pro nasazení** umožňující poskytovateli prostředků Service Fabric získat certifikáty z něj a nainstalujte na uzly clusteru:

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all


    Tags                             :
```

Pokud máte existující Key Vault, můžete ji povolit pro nasazení pomocí rozhraní příkazového řádku Azure:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


### <a name="add-certificates-to-key-vault"></a>Přidejte certifikáty do Key Vault
Ve službě Service Fabric se k ověřování a šifrování pro zabezpečení různých aspektů clusteru a jeho aplikací využívají certifikáty. Další informace o tom, jak certifikáty se používají v Service Fabric najdete v tématu [scénáře zabezpečení clusteru Service Fabric][service-fabric-cluster-security].

#### <a name="cluster-and-server-certificate-required"></a>Cluster a server certifikátu (povinné)
Tento certifikát je vyžadován k zabezpečení clusteru a zabránit neoprávněnému přístupu k němu. Poskytuje zabezpečení clusteru několik způsoby:

* **Ověřování clusteru:** ověřuje komunikaci mezi uzly pro cluster federaci. Pouze uzly, které můžete prokázání své identity s tímto certifikátem může připojit ke clusteru.
* **Ověření serveru:** ověřuje koncové body správy clusteru klient pro správu, tak, aby klient správy zná je rozhovoru s skutečné clusteru. Tento certifikát také poskytuje SSL pro rozhraní API pro správu protokolu HTTPS a pro Service Fabric Explorer přes protokol HTTPS.

Poskytovat tyto účely, certifikát musí splňovat následující požadavky:

* Certifikát musí obsahovat privátní klíč.
* Certifikát se musí vytvořit pro výměnu klíčů, exportovat do souboru Personal Information Exchange (.pfx).
* Název předmětu certifikátu musí odpovídat doménu použitou pro přístup ke clusteru Service Fabric. To je potřeba zajistit SSL pro koncové body správy protokolu HTTPS a Service Fabric Explorer clusteru. Nelze získat certifikát SSL od certifikační autority (CA) pro `.cloudapp.azure.com` domény. Získejte vlastní název domény pro váš cluster. Při žádosti o certifikát od certifikační Autority název subjektu certifikátu musí odpovídat vlastní název domény pro váš cluster používá.

#### <a name="client-authentication-certificates"></a>Certifikáty pro ověřování klientů
Další klientské certifikáty ověřit správci pro úlohy správy clusteru. Service Fabric má dvě úrovně přístupu: **správce** a **jen pro čtení uživatele**. Minimálně je třeba použít jeden certifikát pro přístup pro správu. Pro další přístupu na úrovni uživatele je třeba zadat samostatný certifikát. Další informace o přístupu rolí najdete v tématu [řízení přístupu na základě rolí pro Service Fabric klienty][service-fabric-cluster-security-roles].

Není nutné nahrát certifikáty pro ověřování klientů pro Key Vault pro práci s Service Fabric. Tyto certifikáty pouze musí být zadané pro uživatele, kteří mají oprávnění pro správu clusteru. 

> [!NOTE]
> Azure Active Directory je doporučeným způsobem k ověřování klientů pro operace správy clusterů. Chcete-li používat Azure Active Directory, je potřeba [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Certifikáty aplikace (volitelné)
Libovolný počet dalších certifikátů lze nainstalovat na clusteru pro aplikace z bezpečnostních důvodů. Před vytvořením clusteru, vezměte v úvahu scénáře zabezpečení aplikací, které vyžadují certifikát nainstalovaný na uzlech, například:

* Šifrování a dešifrování hodnot konfigurace aplikace
* Šifrování dat mezi uzly během replikace 

Při vytváření clusteru prostřednictvím portálu Azure se nedá nakonfigurovat certifikáty k aplikaci. Konfigurace aplikace certifikáty během instalace clusteru, musíte [vytvořit cluster pomocí Azure Resource Manager][create-cluster-arm]. Certifikáty aplikace můžete také přidat do clusteru po jeho vytvoření.

#### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Certifikáty formátování pro použití poskytovatele prostředků Azure.
Soubory privátních klíčů (.pfx) můžete přidat a používat přímo přes Key Vault. Však poskytovatel prostředků Azure vyžaduje klíče ukládaly ve speciální formátu JSON, který zahrnuje .pfx jako base-64 kódovaný řetězec a heslo soukromého klíče. Abychom vyhověli tyto požadavky, klíče musí být umístěn do řetězce formátu JSON a pak uloženy jako *tajné klíče* v Key Vault.

Pro usnadnění tohoto procesu je modul prostředí PowerShell [dostupná na Githubu][service-fabric-rp-helpers]. Postupujte podle těchto kroků, které chcete použít modul:

1. Stáhněte si celý obsah úložiště do místního adresáře. 
2. Naimportujte modul v okně prostředí PowerShell:

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

`Invoke-AddCertToKeyVault` Příkaz v tento modul prostředí PowerShell automaticky formáty privátní klíč certifikátu do řetězce formátu JSON a odešle ji do služby Key Vault. Ho použijte k přidání certifikátu clusteru a všechny další aplikaci certifikáty do Key Vault. Tento krok opakujte pro všechny další certifikáty, které chcete nainstalovat v clusteru.

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Toto jsou všechny předpoklady Key Vault pro konfiguraci šablony správce prostředků clusteru Service Fabric, která nainstaluje certifikáty pro ověřování uzlu, zabezpečení koncového bodu správy a ověřování a funkcí zabezpečení další aplikace, které používají certifikáty X.509. V tomto okamžiku teď byste měli mít následující nastavení v Azure:

* Skupina prostředků Key Vault
  * Key Vault
    * Ověřovací certifikát serverů clusteru

< /a "Vytvoření cluster-portal" ></a>

## <a name="create-cluster-in-the-azure-portal"></a>Vytvoření clusteru na portálu Azure
### <a name="search-for-the-service-fabric-cluster-resource"></a>Vyhledejte prostředek clusteru Service Fabric
![vyhledávání pro šablonu clusteru Service Fabric na portálu Azure.][SearchforServiceFabricClusterTemplate]

1. Přihlaste se na web [Azure Portal][azure-portal].
2. Klikněte na tlačítko **nový** o přidání nového prostředku šablony. Vyhledejte šablony Service Fabric Cluster **Marketplace** pod **všechno, co**.
3. Vyberte **Service Fabric Cluster** ze seznamu.
4. Přejděte na **Service Fabric Cluster** okně klikněte na tlačítko **vytvořit**,
5. **Cluster Service Fabric vytvořit** okno obsahuje následující čtyři kroky.

#### <a name="1-basics"></a>1. Základy
![Snímek obrazovky vytváření nové skupiny prostředků.][CreateRG]

V okně základy budete muset zadat základní informace pro váš cluster.

1. Zadejte název clusteru.
2. Zadejte **uživatelské jméno** a **heslo** pro vzdálené plochy pro virtuální počítače.
3. Je nutné vybrat **předplatné** má cluster pro nasazení do, zvláště pokud máte více předplatných.
4. Vytvoření **novou skupinu prostředků**. Je nejlepší a použít stejný název jako cluster, protože pomáhá při hledání je později, zejména v případě, že se pokoušíte provést změny vašeho nasazení nebo odstranit cluster.
   
   > [!NOTE]
   > I když můžete se rozhodnete použít existující skupinu prostředků, je vhodné vytvořit novou skupinu prostředků. To usnadňuje odstranění clusterů, které nepotřebujete.
   > 
   > 
5. Vyberte **oblast** ve kterém chcete vytvořit cluster. Je nutné použít stejné oblasti, kterou Key Vault je v.

#### <a name="2-cluster-configuration"></a>2. Konfigurace clusteru
![Vytvoření typu uzlu][CreateNodeType]

Nakonfigurujte uzly clusteru. Typy uzlů definovat velikosti virtuálních počítačů, počet virtuálních počítačů a jejich vlastnosti. Cluster může mít více než jeden typ uzlu, ale typ primárního uzlu (první, kterou definujete na portálu) musí mít aspoň pět virtuálních počítačů, jedná se o typ uzlu, kde jsou umístěny Service Fabric systémových služeb. Nekonfigurujte **vlastnosti umístění** vzhledem k tomu je automaticky přidá výchozí umístění vlastnost "NodeTypeName".

> [!NOTE]
> Běžný scénář pro více typů uzlu je aplikace, která obsahuje službu front-endu a back endové službě. Chcete umístit front-endová služba menší virtuálních počítačů (velikosti virtuálních počítačů jako D2) s porty otevřené k Internetu, ale chcete umístit na větší virtuální počítače (s velikostí virtuálního počítače jako D4, D6, D15 a tak dále) bez internetového portů otevřete back endové službě.
> 
> 

1. Zvolte název pro váš typ uzlu (obsahující pouze písmena a čísla 1 až 12 znaků).
2. Minimální **velikost** virtuálních počítačů pro primární uzel typu vycházejí z **odolnost** vrstvy, které zvolíte pro cluster. Výchozí hodnota pro úroveň odolnosti je Bronzová. Další informace o odolnost, najdete v části [jak zvolit spolehlivost clusteru Service Fabric a odolnost][service-fabric-cluster-capacity].
3. Vyberte velikost virtuálního počítače a cenovou úroveň. Virtuální počítače D-series jednotek SSD a jsou doporučovány stavových aplikací. Použít všechny virtuálních počítačů SKU, který má částečné jader nebo mají méně než 7 GB kapacity disku. 
4. Minimální **číslo** virtuálních počítačů pro primární uzel typu vycházejí z **spolehlivost** vrstvy, které zvolíte. Výchozí hodnota pro úroveň spolehlivosti je Silver. Další informace o spolehlivosti, najdete v části [jak zvolit spolehlivost clusteru Service Fabric a odolnost][service-fabric-cluster-capacity].
5. Zvolte počet virtuálních počítačů pro typ uzlu. Je možné škálovat později na nahoru nebo dolů počet virtuálních počítačů v uzlu typu, ale na primárním uzlu, který typ minimální doprovází úroveň spolehlivosti, které jste vybrali. Ostatní typy uzlů může mít minimálně 1 virtuální počítač.
6. Nakonfigurujte vlastní koncové body. Toto pole umožňuje zadat seznam portů, které chcete zpřístupnit prostřednictvím nástroje pro vyrovnávání zatížení Azure do veřejného Internetu pro vaše aplikace oddělených čárkami. Například pokud plánujete nasadit webovou aplikaci pro váš cluster, zadejte "80" zde Pokud chcete povolit přenosy na portu 80 do clusteru. Další informace o koncových bodů najdete v tématu [komunikaci s aplikacemi][service-fabric-connect-and-communicate-with-services]
7. Konfigurace clusteru **diagnostiky**. Ve výchozím nastavení jsou diagnostiky povolené v clusteru, vám pomůže při řešení potíží. Pokud chcete zakázat diagnostiky změnu **stav** přepnutím **vypnout**. Vypnutí možnosti diagnostiky je **není** nedoporučuje.
8. Vyberte režim upgradu prostředků infrastruktury, které chcete nastavit váš cluster. Vyberte **automatické**, pokud chcete, aby systém automaticky vyzvedne, až na nejnovější dostupnou verzi a pokuste se upgrade clusteru na ni. Nastavit režim **ruční**, pokud chcete zvolit podporovanou verzi.

> [!NOTE]
> Podporujeme pouze clustery, které běží podporovaná verze služby prostředků infrastruktury. Výběrem **ruční** režimu přenášíte na odpovědnost cluster upgradovat na podporovanou verzi. Pro další informace o prostředcích infrastruktury upgradu naleznete v režimu [service fabric clusteru upgrade dokumentu.][service-fabric-cluster-upgrade]
> 
> 

#### <a name="3-security"></a>3. Zabezpečení
![Snímek obrazovky konfigurace zabezpečení na portálu Azure.][SecurityConfigs]

Posledním krokem je poskytnout informace o certifikátu k zabezpečení clusteru pomocí Key Vault a certifikát informace vytvořený dříve.

* Vyplňte pole primární certifikát s výstupem získané z odesílání **clusteru certifikát** pomocí Key Vault `Invoke-AddCertToKeyVault` příkaz prostředí PowerShell.

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* Zkontrolujte **konfigurovat upřesňující nastavení** pole k zadání klientské certifikáty pro **správce klienta** a **jen pro čtení klienta**. V těchto polích zadejte kryptografický otisk certifikátu klienta správce a kryptografický otisk certifikátu klienta uživatele jen pro čtení, pokud je k dispozici. Když se správce pokusí připojit ke clusteru, získají přístup pouze v případě, že mají certifikát s kryptografickým otiskem, který odpovídá hodnoty kryptografického otisku zadali tady.  

#### <a name="4-summary"></a>4. Souhrn

Chcete-li dokončit vytvoření clusteru, klikněte na tlačítko **Souhrn** najdete v části konfigurace, které jste zadali, nebo stáhnout šablony Azure Resource Manager, který používá k nasazení clusteru. Poté, co jste zadali povinná nastavení **OK** bude zelené tlačítko a procesu vytváření clusteru můžete spustit kliknutím.

Průběh vytváření můžete sledovat v oznámeních. (Klikněte na ikonu zvonku u stavového řádku v pravém horním rohu obrazovky.) Pokud jste klikli na **připnout na úvodní panel** při vytváření clusteru, zobrazí se **nasazení Cluster Service Fabric** připnuli k **spustit** panelu.

### <a name="view-your-cluster-status"></a>Zobrazení stavu clusteru
![Snímek obrazovky podrobnosti o clusteru v řídicím panelu.][ClusterDashboard]

Po vytvoření clusteru si můžete prohlédnout cluster v portálu:

1. Přejděte na **Procházet** a klikněte na tlačítko **clustery infrastruktury služby**.
2. Vyhledejte cluster a klikněte na něj.
3. Na řídicím panelu se zobrazí podrobnosti o clusteru, včetně veřejného koncového bodu clusteru a odkaz na Service Fabric Explorer.

**Uzlu monitorování** část v okně řídicí panel clusteru označuje počet virtuálních počítačů, které jsou v pořádku a není v pořádku. Můžete najít další podrobnosti o stavu clusteru na [Service Fabric stavu modelu ÚVOD][service-fabric-health-introduction].

> [!NOTE]
> Clusterů Service Fabric vyžadovat určité počet uzlů na být až vždycky zachování dostupnosti a zároveň zachovat stav – označuje jako "Správa kvora". Proto je obvykle není bezpečné vypnout všechny počítače v clusteru, pokud jste provedli nejprve [úplného zálohování vaší stavu][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Vzdálené připojení k instanci a sadu škálování virtuálního počítače nebo uzlu clusteru
Každý z NodeTypes zadáte v clusteru výsledky do sady škálování virtuálního počítače získávání nastavení. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Další kroky
V tuto chvíli máte zabezpečené cluster pro správu ověřování pomocí certifikátů. Dále [připojit ke clusteru](service-fabric-connect-to-secure-cluster.md) a zjistěte, jak [spravovat tajné klíče aplikace](service-fabric-application-secret-management.md).  Také další informace o [možnosti podpory Service Fabric](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
<!--[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node -->
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
