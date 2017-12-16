---
title: "Nastavit zařízení do služby Azure IoT Hub zařízení zřizování | Microsoft Docs"
description: "Nastavení zařízení a zajišťují prostřednictvím zřizování služby zařízení IoT Hub během zařízení výrobní proces"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 835a54f147b9ea543df21e7dfeb226ac42aceda3
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Nastavení zařízení ke zřízení pomocí služby Azure IoT Hub zařízení zřizování Service

V tomto kurzu předchozí jste zjistili, jak nastavit služby Azure IoT Hub zařízení zřizování Service pro automatické zřizování zařízení do služby IoT hub. Tento kurz obsahuje pokyny pro nastavení zařízení během procesu výroby tak, že nakonfigurujete službu zřizování zařízení pro vaše zařízení na základě jeho [modulu hardwarového zabezpečení (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security), a zařízení připojení k službě zřizování zařízení, když ji spustí poprvé. Tento kurz popisuje procesy:

> [!div class="checklist"]
> * Vyberte modul hardwarového zabezpečení.
> * Sestavení pro vybraný modul hardwarového zabezpečení zařízení zřizování Client SDK
> * Extrahování artefakty zabezpečení
> * Nastavení konfigurace služby zřizování zařízení na zařízení

## <a name="prerequisites"></a>Požadavky

Než budete pokračovat, vytvořte instanci služby zřizování zařízení a služby IoT hub, postupujte podle pokynů uvedených v tomto kurzu [nastavení cloudu pro zřizování zařízení](./tutorial-set-up-cloud.md).


## <a name="select-a-hardware-security-module"></a>Vyberte modul hardwarového zabezpečení.

[Klienta zařízení zřizování služby SDK](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client) poskytuje podporu pro dva typy modulů hardwarového zabezpečení (nebo moduly hardwarového zabezpečení): 

- [Trusted Platform Module (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module).
    - Čip TPM je navázáno standard pro většinu platforem zařízení se systémem Windows, jakož i několik Linux/Ubuntu na základě zařízení. Jako výrobce zařízení můžete tento modul hardwarového zabezpečení, pokud máte buď tyto operační systémy běžící na vašem zařízení, a pokud hledáte zavedených standard pro moduly hardwarového zabezpečení. S čipy TPM můžete registrovat pouze každé zařízení jednotlivě, abyste službu zřizování zařízení. Pro účely vývoje můžete použít simulátoru TPM na vývojovém počítači Windows nebo Linux.

- [X.509](https://cryptography.io/en/latest/x509/) na základě moduly hardwarového zabezpečení. 
    - X.509 moduly hromadného zabezpečení jsou relativně novější čipy s pracovní aktuálně stavu zpracování v rámci Microsoftu nepokojů nebo ROZČLENĚNÍ čipy, které implementují třídu certifikáty X.509. S čipy X.509, můžete provést hromadné registrace na portálu. Podporuje také některé jiné - operační systémy jako embedOS. Za účelem vývoj klienta zařízení zřizování služby SDK podporuje simulátor zařízení X.509. 

Jako výrobce zařízení je nutné vybrat hardwaru zabezpečení moduly nebo čipy založená na některý z výše uvedených typů. Jiné typy moduly hardwarového zabezpečení, se v klientovi služby zřizování zařízení SDK aktuálně nepodporují.   


## <a name="build-device-provisioning-client-sdk-for-the-selected-hsm"></a>Sestavení pro vybraný modul hardwarového zabezpečení zařízení zřizování Client SDK

SDK klienta služby pro zřizování zařízení pomůže implementovat vybrané bezpečnostní mechanismus v softwaru. Následující kroky ukazují, jak pomocí sady SDK pro vybrané čip modulu hardwarového zabezpečení:

1. Pokud jste postupovali podle [rychlý start pro vytvoření simulovaného zařízení](./quick-create-simulated-device.md), mají nastavení průvodce je připraven vytvořit sadu SDK. Pokud ne, postupujte podle kroků první čtyři z části s názvem [Příprava vývojového prostředí](./quick-create-simulated-device.md#setupdevbox). Tyto kroky naklonujte úložiště GitHub pro SDK klienta služby pro zřizování zařízení a také nainstalovat `cmake` nástrojem sestavení. 

1. Sestavení sady SDK pro typ modulu hardwarového zabezpečení, které jste vybrali pro vaše zařízení pomocí jedné z následujících příkazů na příkazovém řádku:
    - Pro zařízení s TPM:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

    - Pro TPM simulátoru:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
        ```

    - Pro zařízení X.509 a simulátoru:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

1. Sada SDK poskytuje výchozí podporu pro zařízení se systémem Windows nebo Ubuntu implementace čip TPM a X.509 moduly hardwarového zabezpečení. Tyto podporované moduly hardwarového zabezpečení, přejděte k části s názvem [extrahovat artefakty zabezpečení](#extractsecurity) níže. 
 
## <a name="support-custom-tpm-and-x509-devices"></a>Podpora vlastních zařízení čip TPM a X.509

SDK klienta systému pro zřizování zařízení neposkytuje podporu výchozí pro TPM a X.509 zařízení, která nepoužívají Windows nebo Ubuntu. Pro taková zařízení budete muset psát vlastní kód pro vaše konkrétní čip TPM HSM, jak je znázorněno v následující kroky:

### <a name="develop-your-custom-repository"></a>Vývoj vlastní úložiště

1. Vytvořte knihovnu pro přístup k vaší HSM. Tento projekt je potřeba vytvořit statickou knihovnu pro SDK zřizování zařízení a využívat.
1. Vaše knihovna musí implementovat funkce definované v následující soubor hlaviček:. Pro vlastní TPM implementovat funkcí definovaných v [vlastní HSM dokumentu](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-tpm-api).
    b. Pro vlastní X.509 implementovat funkcí definovaných v [vlastní HSM dokumentu](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-x509-api). 

### <a name="integrate-with-the-device-provisioning-service-client"></a>Integrovat zařízení zřizování klienta služby

Jakmile vaše knihovna úspěšně staví na svůj vlastní, můžete přesunout k IoThub C-SDK a odkaz na knihovnu:

1. Zadejte vlastní úložiště HSM GitHub, danou cestu knihovny a jeho název v následujícím příkazu cmake:
    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=<path_and_name_of_library> <PATH_TO_AZURE_IOT_SDK>
    ```
   
1. Otevřete v sadě visual studio SDK a sestavte jej. 

    - Proces sestavení zkompiluje knihovny sady SDK.
    - Sada SDK se pokusí o propojení pro vlastní modul hardwarového zabezpečení definované v příkazu cmake.

1. Spustit `\azure-iot-sdk-c\provisioning_client\samples\prov_dev_client_ll_sample\prov_dev_client_ll_sample.c` Ukázka ověření, pokud vaše modulu hardwarového zabezpečení je implementovaná správně.

<a id="extractsecurity"></a>
## <a name="extract-the-security-artifacts"></a>Extrahování artefakty zabezpečení

Dalším krokem je pro extrahování artefakty zabezpečení modulu hardwarového zabezpečení na vašem zařízení.

1. U zařízení, TPM, budete muset zjistit **ověřovací klíč** přidružené od výrobce čipu TPM. Můžete odvodit jedinečný **ID registrace** pro vaše zařízení TPM rozdělováním ověřovacího klíče. 
2. Pro zařízení se systémem X.509 musíte získat certifikáty vydané na vašem zařízení - certifikáty pro koncové entity pro registraci jednotlivých zařízení při kořenové certifikáty pro skupiny registrace zařízení.

Tyto artefakty zabezpečení vyžadovaných pro registraci zařízení ke službě zřizování zařízení. Zřizování služby potom počká, než pro některý z těchto zařízení, aby spouštěcí a připojte se k němu kdykoli později v čase. V tématu [jak spravovat registrace zařízení](how-to-manage-enrollments.md) informace o tom, jak pomocí těchto artefaktů zabezpečení můžete vytvořit registrace. 

Když se zařízení spustí poprvé, klient SDK komunikuje s vaší čip TPM k extrakci artefakty zabezpečení ze zařízení a ověřuje registraci služby zřizování zařízení. 


## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Nastavení konfigurace služby zřizování zařízení na zařízení

Poslední krok v zařízení výrobní proces je psát aplikace, které používá služba zřizování zařízení klienta SDK k registraci zařízení se službou. Tato sada SDK obsahuje následující rozhraní API pro vaše aplikace používat:

```C
// Creates a Provisioning Client for communications with the Device Provisioning Client Service
PROV_DEVICE_LL_HANDLE Prov_Device_LL_Create(const char* uri, const char* scope_id, PROV_DEVICE_TRANSPORT_PROVIDER_FUNCTION protocol)

// Disposes of resources allocated by the provisioning Client.
void Prov_Device_LL_Destroy(PROV_DEVICE_LL_HANDLE handle)

// Asynchronous call initiates the registration of a device.
PROV_DEVICE_RESULT Prov_Device_LL_Register_Device(PROV_DEVICE_LL_HANDLE handle, PROV_DEVICE_CLIENT_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, PROV_DEVICE_CLIENT_REGISTER_STATUS_CALLBACK reg_status_cb, void* status_user_ctext)

// Api to be called by user when work (registering device) can be done
void Prov_Device_LL_DoWork(PROV_DEVICE_LL_HANDLE handle)

// API sets a runtime option identified by parameter optionName to a value pointed to by value
PROV_DEVICE_RESULT Prov_Device_LL_SetOption(PROV_DEVICE_LL_HANDLE handle, const char* optionName, const void* value)
```

Mějte na paměti, k chybě při inicializaci proměnné `uri` a `id_scope` jak je uvedeno v [simulovat první pořadí spouštění pro části zařízení této úvodní](./quick-create-simulated-device.md#firstbootsequence), než je použijete. Registrace zařízení zřizování klienta rozhraní API `Prov_Device_LL_Create` připojí ke službě globální zřizování zařízení. *ID oboru* je vygenerované službou a zaručí jedinečnosti. Je neměnné a slouží k jednoznačné identifikaci ID registrace. `iothub_uri` Umožňuje registraci klienta služby IoT Hub rozhraní API `IoTHubClient_LL_CreateFromDeviceAuth` připojit ke správné služby IoT hub. 


Tato rozhraní API pomoci zařízení pro připojení a registrace pomocí služby zřizování zařízení, když ji spustí, získat informace o službě IoT hub a připojte se k němu. Soubor `provisioning_client/samples/prov_client_ll_sample/prov_client_ll_sample.c` ukazuje, jak použijte tato rozhraní API. Obecně platí musíte vytvořit následující rozhraní pro registrace klienta:

```C
static const char* global_uri = "global.azure-devices-provisioning.net";
static const char* id_scope = "[ID scope for your provisioning service]";
...
static void register_callback(DPS_RESULT register_result, const char* iothub_uri, const char* device_id, void* context)
{
    USER_DEFINED_INFO* user_info = (USER_DEFINED_INFO *)user_context;
    ...
    user_info. reg_complete = 1;
}
static void registation_status(DPS_REGISTRATION_STATUS reg_status, void* user_context)
{
}
int main()
{
    ...
    SECURE_DEVICE_TYPE hsm_type;
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    prov_dev_security_init(hsm_type); // initialize your HSM 

    prov_transport = Prov_Device_HTTP_Protocol;
    
    PROV_CLIENT_LL_HANDLE handle = Prov_Device_LL_Create(global_uri, id_scope, prov_transport); // Create your provisioning client

    if (Prov_Client_LL_Register_Device(handle, register_callback, &user_info, register_status, &user_info) == IOTHUB_DPS_OK) {
        do {
        // The register_callback is called when registration is complete or fails
            Prov_Client_LL_DoWork(handle);
        } while (user_info.reg_complete == 0);
    }
    Prov_Client_LL_Destroy(handle); // Clean up the Provisioning client
    ...
    iothub_client = IoTHubClient_LL_CreateFromDeviceAuth(user_info.iothub_uri, user_info.device_id, transport); // Create your IoT hub client and connect to your hub
    ...
}
```

Služba zřizování zařízení klientské registrace aplikace pomocí simulovaného zařízení na první pohled pomocí instalačního programu testovací služby může Upřesnit. Jakmile aplikace funguje v testovacím prostředí, můžete vytvářet pro konkrétní zařízení a zkopírujte spustitelný soubor do bitové kopie zařízení. Nespouštět zařízení, ale budete muset [registraci zařízení se službou zřizování zařízení](./tutorial-provision-device-to-hub.md#enrolldevice) před zahájením zařízení. Najdete v části Další kroky a zjistěte, tento proces. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

V tomto okamžiku jste může nastavili služby zřizování zařízení a služby IoT Hub v portálu. Pokud chcete abandon zařízení zřizování instalační program, nebo zpoždění pomocí kteréhokoli z těchto služeb, doporučujeme je vypíná účtovány zbytečných nákladů.

1. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyberte svou službu Device Provisioning. V horní části okna **Všechny prostředky** klikněte na **Odstranit**.  
1. V nabídce vlevo na webu Azure Portal klikněte na **Všechny prostředky** a vyberte své centrum IoT. V horní části okna **Všechny prostředky** klikněte na **Odstranit**.  


## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vyberte modul hardwarového zabezpečení.
> * Sestavení pro vybraný modul hardwarového zabezpečení zařízení zřizování Client SDK
> * Extrahování artefakty zabezpečení
> * Nastavení konfigurace služby zřizování zařízení na zařízení

Přechodu na v dalším kurzu se dozvíte, jak zřídit zařízení do služby IoT hub tak, že zaregistrujete ve službě Azure IoT Hub zařízení zřizování pro automatické zřizování.

> [!div class="nextstepaction"]
> [Zřízení zařízení do služby IoT hub](tutorial-provision-device-to-hub.md)

