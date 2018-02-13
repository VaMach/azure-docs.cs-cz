---
title: "Zabezpečení Azure container Service Fabric | Microsoft Docs"
description: "Naučte se zabezpečení služby kontejneru."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 7ebec89e1481ccc232403426c04bed0ffd6f4fe7
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="container-security"></a>Kontejner zabezpečení

Service Fabric poskytuje mechanismus pro služby uvnitř kontejneru pro přístup k certifikátu, který je nainstalován na uzly v clusteru systému Windows nebo Linux (verze 5.7 nebo vyšší). Kromě toho Service Fabric také podporuje gMSA (skupinové účty spravované služby) pro Windows kontejnery. 

## <a name="certificate-management-for-containers"></a>Správa certifikátů pro kontejnery

Zadáním certifikát můžete zabezpečit vaše služby kontejneru. Certifikát musí být nainstalovaný do LocalMachine na všech uzlech clusteru. Informace o certifikátu je součástí manifest aplikace v rámci `ContainerHostPolicies` značce jako následující fragment kódu ukazuje:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Modul runtime pro windows clusterů, při spouštění aplikace, čte certifikáty a vygeneruje soubor PFX a heslo pro každý certifikát. Tento soubor PFX a heslo jsou přístupné uvnitř kontejneru pomocí následující proměnné prostředí: 

* **Certificates_ServicePackageName_CodePackageName_CertName_PFX**
* **Certificates_ServicePackageName_CodePackageName_CertName_Password**

Pro Linux clusterů certificates(PEM) jednoduše zkopírují přes z obchodu určeného X509StoreName do kontejneru. Odpovídající proměnné prostředí v systému linux jsou:

* **Certificates_ServicePackageName_CodePackageName_CertName_PEM**
* **Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey**

Případně, pokud už máte ve formuláři požadované certifikáty a jednoduše chcete k němu přístup do kontejneru, můžete vytvořit balíček data uvnitř vaší balíček aplikace a zadejte následující uvnitř manifest aplikace:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
   <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

Službu kontejneru nebo procesu je zodpovědná za importování soubory certifikátu do kontejneru. Chcete-li import certifikátu, můžete použít `setupentrypoint.sh` skriptů nebo spuštění vlastního kódu v rámci procesu kontejneru. Následuje ukázkový kód v jazyce C# pro import souboru PFX:

```csharp
    string certificateFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_PFX");
    string passwordFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_Password");
    X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
    string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
    password = password.Replace("\0", string.Empty);
    X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
    store.Open(OpenFlags.ReadWrite);
    store.Add(cert);
    store.Close();
```
Tento certifikát PFX, který slouží pro ověřování aplikace nebo služby nebo zabezpečenou komunikaci s jinými službami. Ve výchozím nastavení jsou soubory ACLed pouze do systému. Seznam ACL můžete ho na jiné účty podle potřeby službou.


## <a name="set-up-gmsa-for-windows-containers"></a>Nastavit gMSA pro kontejnery Windows

Nastavit gMSA (skupina účty spravované služby), specifikace soubor přihlašovacích údajů (`credspec`) je umístěn na všech uzlech v clusteru. Soubor můžete zkopírovat na všech uzlech pomocí rozšíření virtuálního počítače.  `credspec` Soubor musí obsahovat informace o účtu gMSA. Další informace o `credspec` souborů najdete v tématu [účty služby](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). Specifikace přihlašovacích údajů a `Hostname` značky jsou určené v manifestu aplikace. `Hostname` Značky musí odpovídat názvu účtu gMSA kompatibilní se kontejneru.  `Hostname` Značka umožňuje kontejneru ke svému ověření do dalších služeb v doméně pomocí ověřování protokolem Kerberos.  Ukázka pro zadání `Hostname` a `credspec` v aplikaci se zobrazí manifestu v následující fragment kódu:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a>Další postup

* [Nasazení kontejneru systému Windows pro Service Fabric na Windows Server 2016](service-fabric-get-started-containers.md)
* [Nasadit kontejner Docker do Service Fabric v systému Linux](service-fabric-get-started-containers-linux.md)
