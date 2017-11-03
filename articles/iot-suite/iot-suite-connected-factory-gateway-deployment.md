---
title: "Nasazení brány připojené factory - Azure | Microsoft Docs"
description: "K nasazení brány v systému Windows nebo Linux pro umožnění připojení k připojené objekt pro vytváření předkonfigurovaného řešení."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 9f11d93be49f7d88012b1274ad61495e38a2e9a9
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-a-gateway-on-windows-or-linux-for-the-connected-factory-preconfigured-solution"></a>Nasazení brány v systému Windows nebo Linux připojené objekt pro vytváření předkonfigurovaného řešení

Software, abyste mohli nasadit bránu pro připojené objekt pro vytváření předkonfigurovaného řešení má dvě součásti:

* *OPC Proxy* naváže připojení ke službě IoT Hub. *OPC Proxy* potom počká, než pro příkazy a ovládání zprávy z integrované OPC prohlížeče, který běží na portálu řešení připojených objekt pro vytváření.
* *OPC vydavatele* se připojuje k existující místní OPC UA servery a předává telemetrické zprávy z nich do služby IoT Hub.

Obě součásti jsou open source a jsou k dispozici jako zdroj na Githubu a jako kontejnery Docker:

| GitHubu | DockerHub |
| ------ | --------- |
| [Vydavatel OPC][lnk-publisher-github] | [Vydavatel OPC][lnk-publisher-docker] |
| [OPC Proxy][lnk-proxy-github] | [OPC Proxy][lnk-proxy-docker] |

Žádné veřejnou IP adresu nebo díry v bráně firewall brány jsou požadovány pro žádné komponenty. OPC Proxy a OPC vydavatele používat pouze odchozí porty 443 a 5671, 8883.

Kroky v tomto článku ukazují, jak nasadit bránu pomocí Docker na buď [Windows](#windows-deployment) nebo [Linux](#linux-deployment). Brána umožňuje připojení k připojené objekt pro vytváření předkonfigurovaného řešení.

> [!NOTE]
> Software brány, který běží v kontejner Docker [Azure IoT Edge].

## <a name="windows-deployment"></a>Nasazení systému Windows

> [!NOTE]
> Pokud ještě nemáte zařízení brány, Microsoft doporučuje že zakoupit komerční brány na jednom z našich partnerů. Přejděte [katalogu zařízení Azure IoT] seznam zařízení brány, které jsou kompatibilní s řešením připojené objekt pro vytváření. Postupujte podle pokynů, které jsou součástí zařízení nastavit bránu. Alternativně použijte následující pokyny k ruční nastavení jedné z vašich bran existující.

### <a name="install-docker"></a>Nainstalujte Docker

Nainstalujte [Docker pro systém Windows] na zařízení brány se systémem Windows. Během instalace systému Windows Docker vyberte jednotku na počítači hostitele sdílet s Docker. Následující snímek obrazovky ukazuje sdílení diskovou jednotku d v systému Windows:

![Nainstalujte Docker][img-install-docker]

Pak vytvořte složku s názvem **docker** v kořenovém adresáři sdílené jednotky.
Můžete také provést tento krok po instalaci dockerem **nastavení** nabídky.

### <a name="configure-the-gateway"></a>Konfigurace brány

1. Je nutné **iothubowner** připojovací řetězec sady Azure IoT Suite připojen objekt pro vytváření nasazení k dokončení nasazení brány. V [portál Azure], přejděte do služby IoT Hub ve skupině prostředků vytvořili při nasazení řešení připojených objekt pro vytváření. Klikněte na tlačítko **zásady sdíleného přístupu** k přístupu **iothubowner** připojovací řetězec:

    ![Najděte připojovací řetězec služby IoT Hub][img-hub-connection]

    Kopírování **připojovací řetězec – primární klíč** hodnotu.

1. Konfigurace brány pro službu IoT Hub spuštěním moduly dvě brány **po** z příkazového řádku pomocí:

    `docker run -it --rm -h <ApplicationName> -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  je název vaší OPC UA vydavatele ve formátu **vydavatele.&lt; váš plně kvalifikovaný název domény&gt;**. Například, pokud objekt pro vytváření sítě se nazývá **myfactorynetwork.com**, **ApplicationName** hodnota je **publisher.myfactorynetwork.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  je **iothubowner** připojovacího řetězce, které jste zkopírovali v předchozím kroku. Tento připojovací řetězec se používá pouze v tomto kroku, nebudete ho potřebovat v následujících krocích:

    Použití namapované D:\\docker složky ( `-v` argument) později k uchování dva certifikáty X.509, které používají moduly brány.

### <a name="run-the-gateway"></a>Spusťte bránu

1. Restartujte bránu pomocí následujících příkazů:

    `docker run -it --rm -h <ApplicationName> --expose 62222 -p 62222:62222 -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v //D/docker:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v //D/docker:/shared -v //D/docker:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `docker run -it --rm -v //D/docker:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Z bezpečnostních důvodů dva certifikáty X.509 uchovávané v D:\\docker složka obsahovat privátní klíč. Omezit přístup do této složky na přihlašovací údaje (obvykle **správci**) můžete použít ke spuštění kontejner Docker. Klikněte pravým tlačítkem myši D:\\docker složky, vyberte **vlastnosti**, pak **zabezpečení**a potom **upravit**. Poskytnout **správci** úplné řízení a odeberte všechny ostatní:

    ![Udělení oprávnění ke sdílené složce Docker][img-docker-share]

1. Ověřte připojení k síti. Z příkazového řádku, zadejte příkaz `ping publisher.<your fully qualified domain name>` na příkaz ping bránu. Pokud cíl nedostupný, přidejte IP adresu a název své brány do souboru hostitelů na bránu. Soubor hostitele je umístěn ve **Windows\\System32\\ovladače\\atd** složky.

1. Další pokuste se připojit k vydavateli pomocí místní OPC UA klienta se systémem na bráně. Adresa URL je koncový bod OPC UA `opc.tcp://publisher.<your fully qualified domain name>:62222`. Pokud nemáte klientem OPC UA, můžete stáhnout a použít [open-source OPC UA klienta].

1. Když byly úspěšně dokončeny tyto místní testy, přejdete **připojit vlastní OPC UA Server** na portálu řešení připojených objekt pro vytváření. Zadejte adresu URL koncového bodu vydavatele (`tcp://publisher.<your fully qualified domain name>:62222`) a klikněte na tlačítko **Connect**. Získat varování týkající se certifikátu a potom klikněte na **pokračovat.** Další dojde k chybě vydavatele nebude důvěřovat webovému klientovi uživatelský Agent. Chcete-li tuto chybu vyřešit, zkopírujte **uživatelský Agent klienta webové** certifikátu z **D:\\docker\\odmítl certifikáty\\certifikátů** složku pro **D: \\docker\\uživatelský Agent aplikace\\certifikátů** složky na bráně. Není nutné restartovat brány. Tento krok opakujte. Již se můžete připojit k bráně z cloudu a jste připraveni přidat servery OPC UA k řešení.

### <a name="add-your-opc-ua-servers"></a>Přidání serverů OPC UA

1. Vyhledejte **připojit vlastní OPC UA Server** na portálu řešení připojených objekt pro vytváření. Postupujte stejným způsobem jako v předchozí části k vybudování důvěry mezi portál připojené objekt pro vytváření a OPC UA server. Tento krok vytváří vzájemné vztah důvěryhodnosti z portálu připojené objekt pro vytváření a serverem OPC UA certifikátů a vytvoří připojení.

1. Procházet uzlů stromu OPC UA OPC UA serveru, klikněte pravým tlačítkem na OPC uzly a vyberte **publikování**. Pro publikování pro práci tímto způsobem, server OPC UA a vydavatele musí být ve stejné síti. Jinými slovy Pokud je název plně kvalifikované domény vydavatele **publisher.mydomain.com** musí být plně kvalifikovaný název domény serveru OPC UA, například **myopcuaserver.mydomain.com**. Pokud vaše instalace se liší, můžete ručně přidat uzly do souboru publishesnodes.json v nalezen **D:\\docker** složky. Soubor publishesnodes.json se automaticky vygeneroval na prvním úspěšném publikování OPC uzlu.

1. Telemetrická data jsou nyní z zařízení brány. Můžete zobrazit telemetrii v **umístění objektu pro vytváření** zobrazení portálu připojené factory pod **nový objekt pro vytváření**.

## <a name="linux-deployment"></a>Linux nasazení

> [!NOTE]
> Pokud ještě nemáte zařízení brány, Microsoft doporučuje že zakoupit komerční brány na jednom z našich partnerů. Přejděte [katalogu zařízení Azure IoT] seznam zařízení brány, které jsou kompatibilní s řešením připojené objekt pro vytváření. Postupujte podle pokynů, které jsou součástí zařízení nastavit bránu. Alternativně použijte následující pokyny k ruční nastavení jedné z vašich bran existující.

[Nainstalujte Docker] na vaše zařízení brány Linux.

### <a name="configure-the-gateway"></a>Konfigurace brány

1. Je nutné **iothubowner** připojovací řetězec sady Azure IoT Suite připojen objekt pro vytváření nasazení k dokončení nasazení brány. V [portál Azure], přejděte do služby IoT Hub ve skupině prostředků vytvořili při nasazení řešení připojených objekt pro vytváření. Klikněte na tlačítko **zásady sdíleného přístupu** k přístupu **iothubowner** připojovací řetězec:

    ![Najděte připojovací řetězec služby IoT Hub][img-hub-connection]

    Kopírování **připojovací řetězec – primární klíč** hodnotu.

1. Konfigurace brány pro službu IoT Hub spuštěním moduly dvě brány **po** z prostředí s:

    `sudo docker run -it --rm -h <ApplicationName> -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/ -v /shared:/root/.dotnet/corefx/cryptography/x509stores microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName> "<IoTHubOwnerConnectionString>"`

    `sudo docker run --rm -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -i -c "<IoTHubOwnerConnectionString>" -D /mapped/cs.db`

    * **&lt;ApplicationName&gt;**  je název brány vytvoří ve formátu aplikace OPC UA **vydavatele.&lt; váš plně kvalifikovaný název domény&gt;**. Například **publisher.microsoft.com**.
    * **&lt;IoTHubOwnerConnectionString&gt;**  je **iothubowner** připojovacího řetězce, které jste zkopírovali v předchozím kroku. Tento připojovací řetězec se používá pouze v tomto kroku, nebudete ho potřebovat v následujících krocích:

    Můžete použít **/ sdílené** složky ( `-v` argument) později k uchování dva certifikáty X.509, které používají moduly brány.

### <a name="run-the-gateway"></a>Spusťte bránu

1. Restartujte bránu pomocí následujících příkazů:

    `sudo docker run -it -h <ApplicationName> --expose 62222 -p 62222:62222 –-rm -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/Logs -v /shared:/build/src/GatewayApp.NetCore/bin/Debug/netcoreapp1.0/publish/CertificateStores -v /shared:/shared -v /shared:/root/.dotnet/corefx/cryptography/x509stores -e _GW_PNFP="/shared/publishednodes.JSON" microsoft/iot-gateway-opc-ua:1.0.0 <ApplicationName>`

    `sudo docker run -it -v /shared:/mapped microsoft/iot-gateway-opc-ua-proxy:0.1.3 -D /mapped/cs.db`

1. Z bezpečnostních důvodů dva certifikáty X.509 uchovávané v **/ sdílené** složka obsahovat privátní klíč. Omezte přístup do této složky na přihlašovacích údajů, které používáte ke spouštění kontejner Docker. Nastavení oprávnění pro **kořenové** výhradně `chmod` prostředí shell příkaz na složku.

1. Ověřte připojení k síti. Z prostředí, zadejte příkaz `ping publisher.<your fully qualified domain name>` na příkaz ping bránu. Pokud cíl nedostupný, přidejte IP adresu a název své brány do souboru hostitelů na bránu. Soubor hostitele je umístěn ve **/atd** složky.

1. Další pokuste se připojit k vydavateli pomocí místní OPC UA klienta se systémem na bráně. Adresa URL je koncový bod OPC UA `opc.tcp://publisher.<your fully qualified domain name>:62222`. Pokud nemáte klientem OPC UA, můžete stáhnout a použít [open-source OPC UA klienta].

1. Když byly úspěšně dokončeny tyto místní testy, přejdete **připojit vlastní OPC UA Server** na portálu řešení připojených objekt pro vytváření. Zadejte adresu URL koncového bodu vydavatele (`tcp://publisher.<your fully qualified domain name>:62222`) a klikněte na tlačítko **Connect**. Získat varování týkající se certifikátu a potom klikněte na **pokračovat.** Další dojde k chybě vydavatele nebude důvěřovat webovému klientovi uživatelský Agent. Chcete-li tuto chybu vyřešit, zkopírujte **uživatelský Agent webového klienta** certifikátu z **/sdílené/zamítnutý certifikáty nebo certifikáty** složku pro **/shared/UA aplikací/certifikátů** složky na bránu. Není nutné restartovat brány. Tento krok opakujte. Již se můžete připojit k bráně z cloudu a jste připraveni přidat servery OPC UA k řešení.

### <a name="add-your-opc-ua-servers"></a>Přidání serverů OPC UA

1. Vyhledejte **připojit vlastní OPC UA Server** na portálu řešení připojených objekt pro vytváření. Postupujte stejným způsobem jako v předchozí části k vybudování důvěry mezi portál připojené objekt pro vytváření a OPC UA server. Tento krok vytváří vzájemné vztah důvěryhodnosti z portálu připojené objekt pro vytváření a serverem OPC UA certifikátů a vytvoří připojení.

1. Procházet uzlů stromu OPC UA OPC UA serveru, klikněte pravým tlačítkem na OPC uzly a vyberte **publikování**. Pro publikování pro práci tímto způsobem, server OPC UA a vydavatele musí být ve stejné síti. Jinými slovy Pokud je název plně kvalifikované domény vydavatele **publisher.mydomain.com** musí být plně kvalifikovaný název domény serveru OPC UA, například **myopcuaserver.mydomain.com**. Pokud vaše instalace se liší, můžete ručně přidat uzly do souboru publishesnodes.json v nalezen **/ sdílené** složky. Publishesnodes.json se automaticky vygeneroval na prvním úspěšném publikování OPC uzlu.

1. Telemetrická data jsou nyní z zařízení brány. Můžete zobrazit telemetrii v **umístění objektu pro vytváření** zobrazení portálu připojené factory pod **nový objekt pro vytváření**.

## <a name="next-steps"></a>Další kroky

Další informace o architektuře připojené objekt pro vytváření předkonfigurovaného řešení najdete v tématu [připojené factory návod pro předkonfigurované řešení][lnk-walkthrough].

Další informace o [OPC vydavatele odkaz na implementaci](iot-suite-connected-factory-publisher.md).

[img-install-docker]: ./media/iot-suite-connected-factory-gateway-deployment/image1.png
[img-hub-connection]: ./media/iot-suite-connected-factory-gateway-deployment/image2.png
[img-docker-share]: ./media/iot-suite-connected-factory-gateway-deployment/image3.png

[Docker pro systém Windows]: https://www.docker.com/docker-windows
[katalogu zařízení Azure IoT]: https://catalog.azureiotsuite.com/?q=opc
[portál Azure]: http://portal.azure.com/
[open-source OPC UA klienta]: https://github.com/OPCFoundation/UA-.NETStandardLibrary/tree/master/SampleApplications/Samples/Client.Net4
[Nainstalujte Docker]: https://www.docker.com/community-edition#/download
[lnk-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[Azure IoT Edge]: https://github.com/Azure/iot-edge

[lnk-publisher-github]: https://github.com/Azure/iot-edge-opc-publisher
[lnk-publisher-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua
[lnk-proxy-github]: https://github.com/Azure/iot-edge-opc-proxy
[lnk-proxy-docker]: https://hub.docker.com/r/microsoft/iot-gateway-opc-ua-proxy