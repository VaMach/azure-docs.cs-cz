---
title: "Zřízení zařízení pomocí Azure IoT Hub zařízení zřizování služby (.NET) | Microsoft Docs"
description: "Zřízení zařízení do jednoho IoT hub pomocí Azure IoT Hub zařízení zřizování služby (.NET)"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6919f962d853faa572ee7ad5d0cb9aeacd3bd2b6
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/20/2018
---
# <a name="enroll-the-device-to-an-iot-hub-using-the-azure-iot-hub-provisioning-service-client-net"></a>Registrace zařízení do služby IoT hub pomocí Azure IoT Hub zřizování služby klienta (.NET)

V tomto kurzu předchozí jste zjistili, jak nastavit zařízení pro připojení k službě zřizování zařízení. V tomto kurzu, můžete další informace o použití této služby zařízení do jedné služby IoT hub, zřídit pomocí obou  **_jednotlivých registrace_**  a  **_registrace skupiny_**. V tomto kurzu získáte informace o následujících postupech:

> [!div class="checklist"]
> * Registrace zařízení
> * Spuštění zařízení
> * Ověřte, zda že je zařízení zaregistrované

## <a name="prerequisites"></a>Požadavky

Než budete pokračovat, ujistěte se, pro konfiguraci zařízení a jeho *modul hardwarového zabezpečení* popsané v tomto kurzu [nastavení zařízení ke zřízení pomocí Azure IoT Hub zařízení zřizování služby](./tutorial-set-up-device.md).

* Visual Studio 2015 nebo Visual Studio 2017

> [!NOTE]
> Visual Studio se nevyžaduje. Instalace [.NET](https://www.microsoft.com/net) stačí a vývojáři mohou použít jejich upřednostňovaný editor v systému Windows nebo Linux.  

V tomto kurzu simuluje období, během nebo hned po hardwaru výrobní proces, při přidání informací o zařízení ke službě zřizování. Tento kód obvykle běží na počítač nebo objekt pro vytváření zařízení, které můžete spustit kód .NET a by neměly být přidávány se zařízeními.


## <a name="enroll-the-device"></a>Registrace zařízení

Tento krok zahrnuje přidání artefakty jedinečný zabezpečení zařízení ke službě zřizování zařízení. Tyto artefakty zabezpečení jsou následující:

- Pro zařízení s založené na čipu TPM:
    - *Ověřovací klíč* , je jedinečný pro každý čip TPM nebo simulace. Pro čtení [pochopit ověřovacího klíče TPM](https://technet.microsoft.com/library/cc770443.aspx) Další informace.
    - *ID registrace* který slouží k jednoznačné identifikaci zařízení v oboru názvů nebo oboru. To může nebo nemusí být stejný jako identifikátor zařízení. ID je povinné pro každé zařízení. Pro zařízení, založené na čipu TPM může být ID registrace odvozen od čipu TPM samostatně, například algoritmus hash SHA-256 ověřovacího klíče čipu TPM.

- X.509 na základě zařízení:
    - [X.509 certifikát vydaný pro zařízení](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx), ve formě buď *.pem* nebo *.cer* souboru. Pro jednotlivé registrace, budete muset použít *listu certifikát* pro váš systém X.509, zatímco pro skupiny registrace, budete muset použít *kořenový certifikát* nebo ekvivalentní *podepisující osoba certifikát*.
    - *ID registrace* který slouží k jednoznačné identifikaci zařízení v oboru názvů nebo oboru. To může nebo nemusí být stejný jako identifikátor zařízení. ID je povinné pro každé zařízení. Pro zařízení, na základě X.509 je ID registrace odvozená od certifikátu běžný název (CN). Další informace o těchto požadavků naleznete v části [zařízení koncepty](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-device).

Existují dva způsoby, jak zaregistrovat zařízení ke službě zřizování zařízení:

- **Jednotlivé registrace** reprezentuje položku pro jedno zařízení, která může zaregistrovat službu zřizování zařízení. Jednotlivé registrace používat certifikáty X.509 nebo tokeny SAS (ve fyzických nebo virtuálních čipu TPM), jak mechanismů ověření. Doporučujeme používat jednotlivé registrace pro zařízení, které vyžadují jedinečné počáteční konfigurace, nebo pro zařízení, která lze použít pouze tokeny SAS pomocí čipu TPM jako mechanismus ověření. Jednotlivé registrace může mít požadovaný IoT hub ID zařízení v zadané.

- **Registrace skupiny** reprezentuje skupinu zařízení, které sdílejí mechanismus konkrétní ověření. Doporučujeme používat skupinu registrace pro velký počet zařízení, které sdílejí požadované počáteční konfigurace, nebo pro zařízení všechny má stejné klienta. Registrace skupiny jsou X.509 jenom a všechny sdílené složky podpisový certifikát v řetězu certifikátů jejich X.509.

### <a name="enroll-the-device-using-individual-enrollments"></a>Registrace zařízení pomocí jednotlivých registrace

1. V sadě Visual Studio, vytvoření projektu Visual C# Konzolová aplikace pomocí **konzolovou aplikaci** šablona projektu. Název projektu **DeviceProvisioning**.
    
1. V Průzkumníku řešení klikněte pravým tlačítkem myši **DeviceProvisioning** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .

1. V **Správce balíčků NuGet** vyberte **Procházet** a vyhledejte **microsoft.azure.devices.provisioning.service**. Vyberte položku a klikněte na tlačítko **nainstalovat** k instalaci **Microsoft.Azure.Devices.Provisioning.Service** balíček a přijměte podmínky použití. Tento postup stáhne, nainstaluje a přidá odkaz na [zařízení Azure IoT zřizování sady SDK služby](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet balíček a jeho závislé součásti.

1. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
   
    ```csharp
    using Microsoft.Azure.Devices.Provisioning.Service;
    ```

1. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu připojovacím řetězcem distribučních bodů uvedených v předchozí části.
   
    ```csharp
    static readonly string ServiceConnectionString = "{DPS connection string}";

    private const string SampleRegistrationId = "sample-individual-csharp";
    private const string SampleTpmEndorsementKey =
            "AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAxsj2gUS" +
            "cTk1UjuioeTlfGYZrrimExB+bScH75adUMRIi2UOMxG1kw4y+9RW/IVoMl4e620VxZad0ARX2gUqVjYO7KPVt3d" +
            "yKhZS3dkcvfBisBhP1XH9B33VqHG9SHnbnQXdBUaCgKAfxome8UmBKfe+naTsE5fkvjb/do3/dD6l4sGBwFCnKR" +
            "dln4XpM03zLpoHFao8zOwt8l/uP3qUIxmCYv9A7m69Ms+5/pCkTu/rK4mRDsfhZ0QLfbzVI6zQFOKF/rwsfBtFe" +
            "WlWtcuJMKlXdD8TXWElTzgh7JS4qhFzreL0c1mI0GCj+Aws0usZh7dLIVPnlgZcBhgy1SSDQMQ==";
    private const string OptionalDeviceId = "myCSharpDevice";
    private const ProvisioningStatus OptionalProvisioningStatus = ProvisioningStatus.Enabled;
    ```

1. Přidejte následující implementovat registrace zařízení:

    ```csharp
    static async Task SetRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetRegistrationData");

        Attestation attestation = new TpmAttestation(SampleTpmEndorsementKey);

        IndividualEnrollment individualEnrollment = new IndividualEnrollment(SampleRegistrationId, attestation);

        individualEnrollment.DeviceId = OptionalDeviceId;
        individualEnrollment.ProvisioningStatus = OptionalProvisioningStatus;

        Console.WriteLine("\nAdding new individualEnrollment...");
        var serviceClient = ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString);

        IndividualEnrollment individualEnrollmentResult =
            await serviceClient.CreateOrUpdateIndividualEnrollmentAsync(individualEnrollment).ConfigureAwait(false);

        Console.WriteLine("\nIndividualEnrollment created with success.");
        Console.WriteLine(individualEnrollmentResult);
    }
    ```

1. Nakonec přidejte následující kód, který **hlavní** metoda otevřete připojení ke službě IoT hub a začněte registrace:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Provisioning example");

        SetRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **nastavit projekty po spuštění...** . Vyberte **jeden projekt po spuštění**a pak vyberte **DeviceProvisioning** projekt v rozevírací nabídce.  

1. Spuštění aplikace .NET zařízení **DeviceProvisiong**. Měli nastavit zřizování pro zařízení: 

    ![Jednotlivé registrace spustit](./media/tutorial-net-provision-device-to-hub/individual.png)

Když je zařízení úspěšně zaregistrované, měli byste vidět se zobrazovat na portálu jako následující:

   ![Úspěšné registrace na portálu](./media/tutorial-net-provision-device-to-hub/individual-portal.png)

### <a name="enroll-the-device-using-enrollment-groups"></a>Registrace zařízení pomocí zápisu skupin

> [!NOTE]
> Ukázka skupiny registrace vyžaduje certifikát X.509.

1. V Průzkumníku řešení Visual Studio, otevřete **DeviceProvisioning** projektu vytvořili výše. 

1. Do horní části souboru **Program.cs** přidejte následující příkazy `using`:
    
    ```csharp
    using System.Security.Cryptography.X509Certificates;
    ```

1. Do třídy **Program** přidejte následující pole. Nahraďte hodnotu zástupného symbolu X509 umístění certifikátu.
   
    ```csharp
    private const string X509RootCertPathVar = "{X509 Certificate Location}";
    private const string SampleEnrollmentGroupId = "sample-group-csharp";
    ```

1. Přidejte následující **Program.cs** implementovat registrace pro skupinu:

    ```csharp
    public static async Task SetGroupRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetGroupRegistrationData");

        using (ProvisioningServiceClient provisioningServiceClient =
                ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString))
        {
            Console.WriteLine("\nCreating a new enrollmentGroup...");

            var certificate = new X509Certificate2(X509RootCertPathVar);

            Attestation attestation = X509Attestation.CreateFromRootCertificates(certificate);

            EnrollmentGroup enrollmentGroup = new EnrollmentGroup(SampleEnrollmentGroupId, attestation);

            Console.WriteLine(enrollmentGroup);
            Console.WriteLine("\nAdding new enrollmentGroup...");

            EnrollmentGroup enrollmentGroupResult =
                await provisioningServiceClient.CreateOrUpdateEnrollmentGroupAsync(enrollmentGroup).ConfigureAwait(false);

            Console.WriteLine("\nEnrollmentGroup created with success.");
            Console.WriteLine(enrollmentGroupResult);
        }
    }
    ```

1. Nakonec nahradit následující kód, který **hlavní** metoda otevřete připojení ke službě IoT hub a začněte registrace skupiny:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Group Provisioning example");

        SetGroupRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

1. Spuštění aplikace .NET zařízení **DeviceProvisiong**. Měli nastavit zřizování skupiny zařízení: 

    ![Registrace skupiny spustit](./media/tutorial-net-provision-device-to-hub/group.png)

    Pokud je skupina zařízení úspěšně zaregistrované, měli byste vidět se zobrazovat na portálu jako následující:

   ![Registrace úspěšná skupiny na portálu](./media/tutorial-net-provision-device-to-hub/group-portal.png)


## <a name="start-the-device"></a>Spuštění zařízení

V tomto okamžiku následující instalační program je připraven pro registraci zařízení:

1. Skupiny zařízení nebo zařízení jsou zaregistrovaná k službě zřizování zařízení a 
2. Zařízení je připraven s zabezpečení nakonfigurované a dostupné prostřednictvím aplikace pomocí klienta zařízení zřizování služby SDK.

Spusťte zařízení a povolit klientské aplikaci začínat registrace služby zřizování zařízení.  


## <a name="verify-the-device-is-registered"></a>Ověřte, zda že je zařízení zaregistrované

Jednou vaše jednotlivými spuštěními zařízení následující akce má být provedena. V tématu TPM simulátoru ukázkovou aplikaci [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) další podrobnosti. 

1. Zařízení odesílá žádost o registraci vašeho zařízení zřizovací služby.
2. Pro zařízení s čipem TPM odešle služba zřizování zařízení zpět registraci výzvy, ke kterému zařízení reaguje. 
3. Na úspěšnou registraci zařízení zřizovací služby odešle že službě IoT hub identifikátor URI, ID zařízení a zašifrovaný klíč zpět do zařízení. 
4. IoT Hub klientská aplikace na zařízení pak připojí do vašeho centra. 
5. Úspěšné připojení k rozbočovači, měli byste vidět zařízení objeví ve službě IoT hub **Explorer zařízení**. 

    ![Úspěšné připojení k rozbočovači na portálu](./media/tutorial-net-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Další postup
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Registrace zařízení
> * Spuštění zařízení
> * Ověřte, zda že je zařízení zaregistrované

Přechodu na v dalším kurzu se dozvíte, jak zřídit víc zařízení přes centra Vyrovnávání zatížení sítě. 

> [!div class="nextstepaction"]
> [Zřízení zařízení napříč Vyrovnávání zatížení sítě centra IoT](./tutorial-provision-multiple-hubs.md)
