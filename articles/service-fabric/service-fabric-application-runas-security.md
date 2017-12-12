---
title: "Informace o zásadách zabezpečení Azure mikroslužeb | Microsoft Docs"
description: "Přehled o tom, jak běžet v rámci systému a účtů místní zabezpečení, včetně SetupEntry bodu, pokud aplikace potřebuje k provedení některých privilegované akce před spuštěním aplikace Service Fabric"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: mfussell
ms.openlocfilehash: b2ff715d8225bd0a9c7f6108f8804cdfa3189cc8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="configure-security-policies-for-your-application"></a>Konfigurace zásad zabezpečení pro aplikaci
Pomocí Azure Service Fabric můžete zabezpečit aplikace, které jsou spuštěny v clusteru v rámci jiné uživatelské účty. Service Fabric také pomáhá zabezpečit prostředky, které jsou používány aplikací v době nasazení podle uživatelských účtů – například soubory, adresářů a certifikáty. Díky spuštěné aplikace, i v prostředí sdílené hostované bezpečnější od sebe navzájem.

Ve výchozím nastavení se aplikace Service Fabric běžet pod účtem, proces Fabric.exe kompatibilní se. Service Fabric taky poskytuje možnost spouštět aplikace pod účtem místního uživatelského účtu nebo účtu local system, který je určený v rámci manifest aplikace. Typy účtů podporovaný místní systém **LocalUser**, **NetworkService**, **LocalService**, a **LocalSystem**.

 Když spouštíte Service Fabric na Windows serveru ve vašem datovém centru pomocí samostatný instalační program systému, můžete účty domény služby Active Directory, včetně skupinové účty spravované služby.

Můžete definovat a vytvořit skupiny uživatelů, aby pro každou skupinu pro správu společně lze přidat jeden nebo více uživatelů. To je užitečné, pokud existuje více uživatelů pro různé služby vstupní body a vyžadují, aby byla určité společné oprávnění, které jsou k dispozici na úrovni skupiny.

## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Konfigurace zásad pro bod služby instalační položka
Jak je popsáno v [služby manifestů aplikace a](service-fabric-application-and-service-manifests.md), instalační program vstupního bodu, **SetupEntryPoint**, je privilegované vstupního bodu, který běží se stejnými pověřeními, jako Service Fabric (obvykle *NetworkService* účtu) před další vstupní bod. Spustitelný soubor, který je zadán **EntryPoint** je obvykle dlouho běžící hostitele služby. Proto nutnosti samostatného instalačního vstupního bodu tomu není nutné spustit spustitelný soubor hostitele služby s vysokou úrovní oprávnění pro dlouhou dobu. Spustitelný soubor, **EntryPoint** určuje běží **SetupEntryPoint** ukončí úspěšně. Výsledný proces monitorovat a restartuje a znovu začíná **SetupEntryPoint** Pokud někdy ukončí nebo dojde k chybě.

Zde je jednoduché služby manifestu příklad, který ukazuje SetupEntryPoint a hlavní vstupní bod pro službu.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
```

### <a name="configure-the-policy-by-using-a-local-account"></a>Nakonfigurujte zásady pomocí místního účtu
Po dokončení konfigurace služby tak, aby měl instalační program vstupního bodu, můžete změnit oprávnění zabezpečení, které běží v části v manifestu aplikace. Následující příklad ukazuje, jak nakonfigurovat službu pro spuštění pod oprávnění uživatelského účtu správce.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
```

Nejprve vytvořte **objekty** část s uživatelským jménem, jako je například SetupAdminUser. To znamená, že uživatel je členem skupiny Administrators systému.

Dále v části **ServiceManifestImport** nakonfigurujte zásadu použít tento objekt zabezpečení na **SetupEntryPoint**. Tato hodnota informuje Service Fabric který po **MySetup.bat** spuštění souboru, měla by být `RunAs` s oprávněními správce. Vzhledem k tomu, že máte *není* použít zásadu na hlavní vstupní bod, kód v **MyServiceHost.exe** běží v rámci systému **NetworkService** účtu. Toto je výchozí účet, který všechny vstupní body služby jsou spustit jako.

Nyní přidejte umožňuje soubor MySetup.bat do projektu sady Visual Studio k testování s oprávněními správce. V sadě Visual Studio klikněte pravým tlačítkem na projekt služby a přidejte nový soubor s názvem MySetup.bat.

Dále se ujistěte, že soubor MySetup.bat je součástí balíčku služby. Ve výchozím nastavení není. Vyberte soubor, klikněte pravým tlačítkem na získat v místní nabídce a zvolte **vlastnosti**. V dialogovém okně Vlastnosti ověřte, že **kopírovat do výstupního adresáře** je nastaven na **kopírovat, pokud je novější**. Viz následující snímek obrazovky.

![Visual Studio CopyToOutput pro SetupEntryPoint dávkového souboru][image1]

Nyní otevřete soubor MySetup.bat a přidejte následující příkazy:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

V dalším kroku sestavení a nasadit řešení místního vývojového clusteru. Po spuštění služby, jak je znázorněno v Service Fabric Exploreru, uvidíte, že MySetup.bat souboru bylo úspěšné dvěma způsoby. Otevřete příkazový řádek prostředí PowerShell a zadejte:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Poznamenejte si název uzlu, kde služba byla nasazená a spustit v Service Fabric Exploreru – například uzlu 2. Dále přejděte do složky pracovní instance aplikace out.txt soubor, který se zobrazuje hodnota **TestVariable**. Například pokud tato služba byla nasazena na uzlu 2, potom můžete přejít na tuto cestu pro **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Nakonfigurujte zásady pomocí místní systémové účty
Často je vhodnější pro spuštění skriptu spuštění pomocí účtu local system, nikoli účet správce. Spuštění zásad RunAs jako člen skupiny Administrators obvykle nefunguje správně, protože počítače mají přístup k řízení Uživatelských účtů ve výchozím nastavení povolené. V takových případech **doporučujeme spustit SetupEntryPoint pod účtem LocalSystem, ne jako místní uživatel přidán do skupiny Administrators**. Následující příklad ukazuje nastavení SetupEntryPoint běží pod účtem LocalSystem:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

Pro clustery Linux ke spuštění služby nebo nastavení vstupního bodu jako **kořenové**, můžete zadat **AccountType** jako **LocalSystem**.

## <a name="start-powershell-commands-from-a-setup-entry-point"></a>Spusťte příkazy prostředí PowerShell ze vstupního bodu instalační program
Spustit prostředí PowerShell z **SetupEntryPoint** bodu, můžete spustit **PowerShell.exe** v dávkovém souboru, který odkazuje na soubor prostředí PowerShell. Nejprve přidejte soubor prostředí PowerShell služby projektu – například **MySetup.ps1**. Nezapomeňte nastavit *kopírovat, pokud je novější* vlastnost tak, aby soubor jsou také obsaženy v balíčku služby. Následující příklad ukazuje dávkový soubor začínající soubor prostředí PowerShell s názvem MySetup.ps1, která nastavuje proměnnou prostředí systému s názvem **TestVariable**.

MySetup.bat spustit soubor prostředí PowerShell:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

V prostředí PowerShell souboru přidejte následující nastavení proměnné prostředí systému:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Ve výchozím nastavení, když dávkový soubor spouští, vypadá to, v aplikaci složku s názvem **pracovní** pro soubory. V takovém případě při spuštění MySetup.bat chceme najít soubor MySetup.ps1 ve stejné složce, která je aplikace **balíček kódu** složky. Chcete-li změnit tato složka, nastavte pracovní složku:
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="use-console-redirection-for-local-debugging"></a>Použití konzole přesměrování pro místní ladění
V některých případech je užitečné, pokud chcete zobrazit výstup konzoly z spuštění skriptu pro účely ladění. K tomuto účelu můžete nastavit zásadu přesměrování konzoly, který zapisuje výstup do souboru. Soubor výstup zapsán do aplikace složku s názvem **protokolu** na uzlu, kde je aplikace nasazena a spustit. (Viz kde najít to v předchozím příkladu).

> [!WARNING]
> Nikdy nepoužívejte konzolu Zásady přesměrování v aplikaci, která je nasazena v produkčním prostředí, protože to může mít vliv převzetí služeb při selhání aplikaci. *Pouze* používejte pro místní vývoj a účely ladění.  
> 
> 

Následující příklad ukazuje nastavení přesměrování konzoly s hodnotou FileRetentionCount:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Pokud změníte teď MySetup.ps1 souboru pro zápis **Echo** příkaz, to bude zapisovat do výstupního souboru pro účely ladění:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

**Po ladění skriptu, okamžitě odebrat tuto zásadu přesměrování konzoly**.

## <a name="configure-a-policy-for-service-code-packages"></a>Nakonfigurujte zásady pro balíčky služeb kódu
V předchozích krocích jste viděli, jak použít zásadu RunAs SetupEntryPoint. O tom, jak vytvořit různé objekty, které mohou být použity jako zásady služby Podívejme trochu podrobněji.

### <a name="create-local-user-groups"></a>Vytvořit místní skupiny uživatelů
Můžete definovat a vytvořit skupiny uživatelů, které umožňují jeden nebo více uživatelů, který se má přidat do skupiny. To je užitečné, pokud existuje více uživatelů pro různé služby vstupní body a vyžadují, aby byla určité společné oprávnění, které jsou k dispozici na úrovni skupiny. Následující příklad ukazuje místní skupinu s názvem **LocalAdminGroup** s oprávněními správce. Dva uživatelé, Customer1 a Customer2, stanou členy této místní skupiny.

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

### <a name="create-local-users"></a>Vytvořit místní uživatele
Můžete vytvořit místní uživatele, který slouží k zajištění služby v rámci aplikace. Když **LocalUser** typ účtu je definováno v sekci objekty manifestu aplikace Service Fabric vytvoří místní uživatelské účty na počítačích, kde je aplikace nasazená. Ve výchozím nastavení tyto účty nemají stejné názvy jako platformám zadaným v manifest aplikace (například Customer3 v následující ukázce). Místo toho se dynamicky generují a mít náhodných hesla.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Pokud aplikace vyžaduje, aby uživatelský účet a heslo být stejná ve všech počítačích (například pro povolení ověřování NTLM), v manifestu clusteru musí nastavit NTLMAuthenticationEnabled na hodnotu true. V manifestu clusteru musíte zadat také NTLMAuthenticationPasswordSecret, který se používá ke generování stejné heslo ve všech počítačích.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-to-the-service-code-packages"></a>Přiřadit zásady do balíčků kódu služby
**RunAsPolicy** část **ServiceManifestImport** Určuje účet z části objekty zabezpečení, který se má použít ke spuštění balíček kódu. Také přidruží kód balíčky service manifest s uživatelskými účty v části objekty zabezpečení. Je to zadané pro instalaci nebo hlavní vstupních bodů, nebo můžete zadat `All` chcete použít pro obojí. Následující příklad ukazuje použití různých zásad:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Pokud **entrypointtype typu** není zadán, ve výchozím nastavení je entrypointtype typu = "Hlavní". Určení **SetupEntryPoint** je obzvláště užitečné, když chcete spustit určité operace instalace s vysokou úrovní oprávnění v rámci účtu system. Aktuální služby kód může být spuštěn pod účtem nižší oprávnění.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Použít výchozí zásady na všechny balíčky kódu služby
Můžete použít **DefaultRunAsPolicy** části zadejte výchozí účet uživatele pro všechny kód balíčky, které nemají konkrétní **RunAsPolicy** definované. Pokud většinu kódu balíčky, které jsou určené v service manifest používaný aplikací k potřebovali pro spuštění pod stejného uživatele, můžete aplikaci právě definovat výchozí zásady RunAs se s uživatelským účtem. Následující příklad určuje, že pokud balíček kódu neobsahuje **RunAsPolicy** zadán, balíček kódu by měl běžet pod **MyDefaultAccount** zadané v části objekty zabezpečení.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a>Použít skupiny domény služby Active Directory nebo uživatele
Pro instance Service Fabric, která byla nainstalována v systému Windows Server pomocí samostatný instalační program systému můžete spustit službu s pověřeními pro uživatele služby Active Directory nebo účet skupiny. Toto je služby Active Directory místně ve vaší doméně a není v Azure Active Directory (Azure AD). Pomocí účtem uživatele domény nebo skupiny můžete pak přístup k jiným prostředkům v doméně (například sdílené složky), kterým bylo uděleno oprávnění.

Následující příklad ukazuje uživatele služby Active Directory názvem *TestUser* s jejich domény heslo šifrované pomocí certifikátu nazývá *MyCert*. Můžete použít `Invoke-ServiceFabricEncryptText` příkaz prostředí PowerShell k vytvoření tajný šifrovaný text. V tématu [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md) podrobnosti.

Je nutné nasadit privátní klíč certifikátu dešifrovat heslo k místnímu počítači pomocí metody out-of-band (v Azure, je to prostřednictvím Správce Azure Resource Manager). Potom při Service Fabric nasadí balíček služby k počítači, je možné dešifrovat tajný klíč a (spolu s uživatelské jméno) ověření pomocí služby Active Directory pro spuštění pod tyto přihlašovací údaje.

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```
### <a name="use-a-group-managed-service-account"></a>Použijte skupinu účet spravované služby.
Pro instance Service Fabric, která byla nainstalována v systému Windows Server pomocí samostatný instalační program systému můžete spustit službu jako skupinový účet spravované služby (gMSA). Upozorňujeme, že služby Active Directory v místě ve vaší doméně a není v Azure Active Directory (Azure AD). Pomocí gMSA neexistuje žádné heslo nebo zašifrované heslo uložené v `Application Manifest`.

Následující příklad ukazuje, jak vytvořit účet gMSA s názvem *svc Test$*; pro nasazení tohoto účtu spravované služby pro uzly clusteru; a jak nakonfigurovat hlavní název uživatele.

##### <a name="prerequisites"></a>Požadavky.
- Doména musí kořenový klíč služby KDS.
- Musí být v systému Windows Server 2012 nebo novější úroveň funkčnosti domény.

##### <a name="example"></a>Příklad
1. Požádejte správce domény služby active directory, vytvoření účtu služby skupina spravované pomocí `New-ADServiceAccount` příkaz a ujistěte se, že `PrincipalsAllowedToRetrieveManagedPassword` zahrnuje všechny uzly clusteru service fabric. Všimněte si, že `AccountName`, `DnsHostName`, a `ServicePrincipalName` musí být jedinečný.
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. Na všech uzlech clusteru service fabric (například `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), instalace a testování gMSA.
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. Nakonfigurovat hlavní název uživatele a nakonfigurovat RunAsPolicy tak, aby odkazovaly uživatele.
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
      </Policies>
   </ServiceManifestImport>
  <Principals>
    <Users>
      <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Přiřadit zásady zabezpečení přístupu pro koncové body HTTP a HTTPS
Pokud použijete RunAs zásady pro služby a service manifest deklaruje koncový bod prostředků pomocí protokolu HTTP, je nutné zadat **SecurityAccessPolicy** zajistit, že porty přidělené s těmito koncovými body jsou správně řízení přístupu pro uživatelský účet Spustit jako, který se spouští služba uvedené. V opačném **http.sys** nemá přístup ke službě, a získat selhání pomocí volání z klienta. Následující příklad se týká účet Customer1 koncový bod názvem **EndpointName**, což dává ho úplná přístupová práva.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Pro koncový bod HTTPS máte také označení názvu certifikátu se vraťte do klienta. Můžete to provést pomocí **EndpointBindingPolicy**, s certifikátem definovaný v oddílu certifikáty v manifestu aplikace.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a>Upgrade více aplikací s koncovými body https
Budete muset pečlivě nepoužívat **stejný port** pro různé instance stejné aplikace při používání protokolu http**s**. Důvodem je, že Service Fabric, nebude možné upgradovat certifikátu pro jednu z instancí aplikace. Například pokud aplikace 1 nebo aplikace 2 obou chcete upgradovat jejich cert 1 cert 2. Při upgradu se stane, Service Fabric může mít vyčistit cert 1 registrace pomocí ovladače http.sys i když se stále používá jiná aplikace. Chcete-li tomu zabránit, Service Fabric zjistí, že se už používá jiná instance aplikace zaregistrované na portu s certifikátem (z důvodu http.sys) a operaci se nezdaří.

Proto Service Fabric nepodporuje upgrade dvě různé služby pomocí **stejný port** v případech jinou aplikaci. Jinými slovy nelze použít stejný certifikát na různé služby na stejném portu. Pokud potřebujete mít sdílené certifikát na stejném portu, je třeba zajistit, že služby jsou umístěny na různých počítačích s omezeními umístění. Nebo zvažte, pokud je to možné pomocí Service Fabric dynamické porty pro každou službu v každé instanci aplikace. 

Pokud se zobrazí upgradu služeb při selhání s protokolem https, chybu upozornění oznamující "Rozhraní API systému Windows HTTP serveru nepodporuje víc certifikátů pro aplikace, které sdílejí port."

## <a name="a-complete-application-manifest-example"></a>V příkladu manifestu aplikace dokončena
Následující manifest aplikace se zobrazuje řadu různých nastavení:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
* [Pochopení aplikačního modelu](service-fabric-application-model.md)
* [Zadejte prostředky v service manifest](service-fabric-service-manifest-resources.md)
* [Nasazení aplikace](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
