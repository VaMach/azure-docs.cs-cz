---
title: "Vytvoření zařízení brány s hranou Azure IoT | Microsoft Docs"
description: "Použít k vytvoření zařízení brány, který dokáže zpracovat informace pro více zařízení Azure IoT Edge"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: c9f71a7e95ea8c1b2cbd9b74ef20f9b0342d00f8
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="create-an-iot-edge-gateway-device-to-process-data-from-other-iot-devices---preview"></a>Vytvořit IoT hraniční zařízení brány pro zpracování dat z jiných zařízení IoT – náhled

Existují dva způsoby, jak používat zařízení IoT okraj jako brány:

* Připojení podřízené zařízení, které používají [zařízení Azure IoT SDK][lnk-devicesdk];
* Připojení zařízení, které nepoužívají protokol IoT Hub.

Při použití IoT hraniční zařízení jako brána získáte následující výhody:

* **Připojení multiplexní**. Všechna zařízení připojují ke službě IoT Hub pomocí IoT hraniční zařízení bude používat stejné základní připojení.
* **Provoz vyhlazování**. IoT hraniční zařízení bude automaticky provádět exponenciálního omezení rychlosti v případě služby IoT Hub omezení při zachování zprávy v místním počítači. Odolné vaše řešení nebude nadále k špičky v provozu.
* **Omezenou podporu offline**. Zařízení brány, které se uloží místně zprávy, které nelze doručit do služby IoT Hub.

V tomto článku jsme zavolá *IoT hraniční brána* IoT hraniční zařízení používá jako brána.

>[!NOTE]
>Aktuálně:
> * Podřízené zařízení nejsou moci ověřit u brány, pokud brána je odpojená ze služby IoT Hub; a
> * IoT hraniční zařízení nemůže připojit k IoT hraniční brány.

## <a name="use-the-azure-iot-device-sdk"></a>Použití zařízení Azure IoT SDK

Hraniční rozbočovače, které jsou nainstalovány ve všech IoT hraniční zařízení zveřejňuje následující primitivních elementů do podřízené zařízení:

* zprávy typu zařízení cloud a cloud zařízení;
* přímé metody; a
* operace twin zařízení.

>[!NOTE]
>Podřízené zařízení jsou v současné době není moci používat nahrávání souborů při připojení prostřednictvím IoT vstupní brána.

Při připojení zařízení k IoT vstupní brána pomocí zařízení Azure IoT SDK, budete muset:

* Nastavit podřízené zařízení s připojovacím řetězcem odkazující na název hostitele brány zařízení; a
* Ujistěte se, že podřízené zařízení důvěřuje certifikátu použít tak, aby přijímal připojení pomocí zařízení brány.

Při instalaci modulu runtime Azure IoT Edge použijte skript ovládacího prvku certifikát je vytvořen Edge rozbočovače, jako jste to udělali v kurzu nainstalovat IoT okraj na simulované zařízení na [Windows] [ lnk-tutorial1-win] a [Linux][lnk-tutorial1-lin]. Tento certifikát je používaný rozbočovačem Edge tak, aby přijímal příchozí připojení protokolu TLS a musí být důvěryhodný podřízené zařízení při připojení k zařízení brány.

Můžete vytvořit všechny infrastrukturu certifikátů, která umožňuje důvěryhodnosti vyžadované pro topologii zařízení brány. V tomto článku předpokládáme nastavení stejný certifikát, který byste použili k povolení [zabezpečení certifikační Autority X.509] [ lnk-iothub-x509] IoT hub, která zahrnuje certifikát X.509 Certifikační autority, který je přidružený ke konkrétní Centrum IoT (  *IoT hub vlastníka certifikační Autority*) a řadu certifikáty podepsané s touto certifikační Autoritou, nainstalovaný v zařízení IoT okraj.

>[!IMPORTANT]
>V současné době IoT hraniční zařízení a podřízené zařízení lze použít pouze [tokeny SAS] [ lnk-iothub-tokens] k ověření službou IoT Hub. Certifikáty slouží pouze k ověření připojení TLS mezi zařízením listu a brány.

Naše konfigurace používá **IoT hub vlastníka certifikační Autority** jako:
* podpisový certifikát pro instalaci modulu runtime IoT Edge na všechna zařízení IoT okraj a jako
* certifikát veřejného klíče nainstalované v podřízené zařízení.

Tato akce způsobí řešení, které umožňuje všem zařízením používat zařízení IoT okraj jako brána, tak dlouho, dokud jsou připojeny ke stejné IoT hub.

### <a name="create-the-certificates-for-test-scenarios"></a>Vytvoření certifikáty pro scénáře testování

Můžete použít ukázkové prostředí Powershell a skriptů Bash popsané v [Správa ukázka certifikát certifikační Autority] [ lnk-ca-scripts] vygenerovat certifikát podepsaný svým držitelem **IoT hub vlastníka certifikační Autority** a certifikáty zařízení podepsané s ním.

1. Postupujte podle kroku 1 postupu [Správa ukázka certifikát certifikační Autority] [ lnk-ca-scripts] k instalaci skripty. Klonujte z `modules-preview` větev:
                
                git clone -b modules-preview https://github.com/Azure/azure-iot-sdk-c.git 
2. Postupujte podle kroku 2, pokud chcete generovat **IoT hub vlastníka certifikační Autority**, tento soubor se použije podřízené zařízení k ověření připojení.

Postupujte podle následujících pokynů pro vytvoření certifikátu pro vaše zařízení brány.

#### <a name="bash"></a>Bash

* Spustit `./certGen.sh create_edge_device_certificate myGateway` k vytvoření nového certifikátu zařízení.  
  Tím se vytvoří.\certs\new-edge-device.* soubory obsahující veřejný klíč a PFX a.\private\new-edge-device.key.pem obsahující privátní klíč zařízení.  
* V `certs` directory spustit `cat ./new-edge-device.cert.pem ./azure-iot-test-only.intermediate.cert.pem ./azure-iot-test-only.root.ca.cert.pem > ./new-edge-device-full-chain.cert.pem` získat úplný řetěz veřejného klíče zařízení.
* `./private/new-edge-device.cert.pem`obsahuje privátní klíč zařízení.

#### <a name="powershell"></a>PowerShell

* Spustit `New-CACertsEdgeDevice myGateway` k vytvoření nového certifikátu zařízení.
  Tím se vytvoří soubory myEdgeDevice * obsahující veřejný klíč, privátní klíč a PFX tohoto certifikátu.  Po zobrazení výzvy k zadání hesla během procesu podepisování, zadejte "1234".


>[!IMPORTANT]
>Tato ukázka je určená jenom pro účely testování. Produkčních scénářích najdete v části [zabezpečit vaše nasazení IoT] [ lnk-iothub-secure-deployment] Azure IoT pokyny o tom, jak zabezpečit řešení IoT a odpovídajícím způsobem zřídit svůj certifikát.

### <a name="configure-an-iot-edge-device-as-a-gateway"></a>Nakonfigurovat nastavení zařízení IoT okraj jako brána

Chcete-li nakonfigurovat zařízení IoT okraj jako brána stačí nakonfigurovat na použití certifikátu zařízení vytvořené v předchozí části.

Předpokládáme následující názvy souborů z výše uvedené ukázkové skripty:

| Výstup | Bash skriptu | PowerShell |
| ------ | ----------- | ---------- |
| Certifikát pro zařízení | `certs/new-edge-device.cert.pem` | `certs/new-edge-device.cert.pem` |
| Privátní klíč zařízení | `private/new-edge-device.cert.pem` | `private/new-edge-device.cert.pem` |
| Zařízení řetěz certifikátů | `certs/new-edge-device-full-chain.cert.pem` | `certs/new-edge-device-full-chain.cert.pem` |
| IoT hub vlastníka certifikační Autority | `certs/azure-iot-test-only.root.ca.cert.pem` | `RootCA.pem` |

 Analogicky na instalaci popsané v nasazení Azure IoT Edge v simulovaném zařízení v [Windows] [ lnk-tutorial1-win] nebo [Linux][lnk-tutorial1-lin], budete muset Poskytněte výše uvedené informace pro modul runtime IoT okraj. 
 
 V systému Linux:

        sudo iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/certs/azure-iot-test-only.root.ca.cert.pem

V systému Windows:

        iotedgectl setup --connection-string {device connection string}
            --edge-hostname {gateway hostname, e.g. mygateway.contoso.com}
            --device-ca-cert-file {full path}/certs/new-edge-device.cert.pem
            --device-ca-chain-cert-file {full path}/certs/new-edge-device-full-chain.cert.pem
            --device-ca-private-key-file {full path}/private/new-edge-device.key.pem
            --owner-ca-cert-file {full path}/RootCA.pem

Ve výchozím nastavení ukázkové skripty nenastavujte přístupové heslo soukromého klíče zařízení. Pokud jste nastavili heslo, přidejte následující parametr:

        --device-ca-passphrase {passphrase}

Skript zobrazí výzvu k nastavit heslo pro certifikát agenta okraj.
Budete muset restartovat runtime IoT Edge po tento příkaz.

### <a name="configure-an-iot-hub-device-application-as-a-downstream-device"></a>Konfigurace aplikace zařízení IoT Hub jako podřízené zařízení

Podřízené zařízení může být z jakékoliv aplikace pomocí [zařízení Azure IoT SDK][lnk-devicesdk], tak jak je popsáno jednoduché jeden v [připojení zařízení do služby IoT hub pomocí rozhraní .NET] [ lnk-iothub-getstarted].

První, má aplikace podřízené zařízení tak, aby důvěřoval **IoT hub vlastníka certifikační Autority** certifikát k ověřování TLS připojení k zařízení brány. Tento krok můžete obvykle provést dvěma způsoby: na úrovni operačního systému, nebo (pro určité jazyky) na úrovni aplikace.

Například pro aplikace .NET, můžete přidat následující fragment kódu důvěřovat certifikátu ve formátu PEM uložené v cestě `certPath`. Pokud používáte skript výše, cesta bude odkazovat `certs/azure-iot-test-only.root.ca.cert.pem` (Bash), nebo `RootCA.pem` (Powershell).

        using System.Security.Cryptography.X509Certificates;
        
        ...

        X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
        store.Open(OpenFlags.ReadWrite);
        store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
        store.Close();

Provedením tohoto kroku na úrovni operačního systému se liší mezi Windows a mezi distribucí Linux.

Druhým krokem je k chybě při inicializaci sady sdk zařízení IoT Hub s připojovacím řetězcem odkazující na název hostitele zařízení brány.
To se provádí přidáním `GatewayHostName` vlastnost, která má připojovací řetězec zařízení. Pro instanci zde je ukázka zařízení připojovací řetězec pro zařízení, na kterou jsme připojí `GatewayHostName` vlastnost:

        HostName=yourHub.azure-devices-int.net;DeviceId=yourDevice;SharedAccessKey=2BUaYca45uBS/O1AsawsuQslH4GX+SPkrytydWNdFxc=;GatewayHostName=mygateway.contoso.com

Tyto dva kroky povolí aplikaci zařízení pro připojení k zařízení brány.

## <a name="use-other-protocols"></a>Použít jiné protokoly

Jedním z hlavních funkcí zařízení brány je interpretace protokolů podřízené zařízení. Připojení zařízení, které nepoužívají protokol Centrum IoT, můžete vyvíjet modul IoT okraj, který provádí tento překlad a připojí jménem podřízené zařízení do služby IoT Hub.

Můžete se rozhodnout vytvořit *transparentní* nebo *neprůhledného* brány:

| &nbsp; | Transparentní brány | Neprůhledné brány|
|--------|-------------|--------|
| Identity, které jsou uložené v registru identit služby IoT Hub | Identity všech připojených zařízeních | Pouze identity zařízení brány |
| Dvojče zařízení | Každého připojeného zařízení má vlastní dvojče zařízení | Jenom brána má dvojčata zařízení a modulu |
| Přímé metody a zprávy typu cloud zařízení | Cloud jednotlivě adres každého připojeného zařízení | Cloudu můžete vyřešit pouze zařízení brány |
| [IoT Hub omezení a kvóty][lnk-iothub-throttles-quotas] | Platí pro každé zařízení | Platí pro zařízení brány |

Při použití všechna zařízení, připojení prostřednictvím této brány vzor neprůhledného brány sdílet stejnou frontou cloud zařízení, která může obsahovat maximálně 50 zprávy. Vyplývá, že vzoru neprůhledného brány měli použít pouze v případě, že se velmi málo zařízení připojují prostřednictvím brány každé pole, a je malý provoz jejich cloud zařízení.

Při implementaci neprůhledného brány, používá modul překlad protokolu modulu připojovací řetězec.

Při implementaci bránu transparentní, modul vytvoří několik instancí klienta zařízení IoT Hub pomocí připojovací řetězce pro příjem dat zařízení.

[Azure IoT Edge Modbus modulu] [ lnk-modbus-module] je otevřené implementace modulu protokolu adaptéru neprůhledného brány.

## <a name="next-steps"></a>Další kroky

- [Pochopení požadavků a nástrojů pro vývoj modulů IoT Edge][lnk-module-dev].

[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens 
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/modules-preview/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus