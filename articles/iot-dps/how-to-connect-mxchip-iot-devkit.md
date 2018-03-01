---
title: "Jak používat MXChip IoT DevKit pro připojení k Azure IoT Hub zařízení zřizování služby | Microsoft Docs"
description: "Jak používat MXChip IoT DevKit pro připojení k Azure IoT Hub zařízení zřizování služby"
services: iot-dps
keywords: 
author: liydu
ms.author: liydu
ms.date: 02/20/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 697196f725f0cb8ad3054bd8336b588c17dddc3d
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-hub-device-provisioning-service"></a>Připojení k zařízení Azure IoT Hub zřizování služby MXChip IoT DevKit

Tento článek popisuje postup konfigurace DevKit aby automatickou registraci do služby IoT Hub pomocí služby zřizování zařízení. V tomto kurzu se naučíte, jak:

* Konfigurace globálních koncový bod distribučních bodů na zařízení
* Použít jedinečný zařízení tajný klíč (UDS) k vygenerování certifikátu X.509
* Registraci jednotlivých zařízení
* Ověřte, zda že je zařízení zaregistrované

## <a name="before-you-begin"></a>Než začnete

Pokud chcete provést kroky v tomto kurzu, budete potřebovat následující:

* Příprava vašeho DevKit s [Příručka Začínáme](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* Upgrade na nejnovější firmware (> = 1.3.0) s [upgrade firmwaru](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) kurzu.
* Vytvoření a propojení IoT Hub s instancí služby zřizování zařízení s [nastavit automatické zřizování](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision).

## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Nastavení konfigurace služby zřizování zařízení na zařízení

Chcete-li povolit DevKit se připojit k instanci služby zřizování zařízení, kterou jste vytvořili:

1. Na portálu Azure vyberte **přehled** služba zřizování zařízení a zapište **koncový bod globální zařízení** a **ID oboru** hodnotu.
  ![Distribučních bodů globální koncový bod a ID oboru](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Ujistěte se, že je na vašem počítači nainstalovaný `git` a že je přidaný do proměnných prostředí, ke kterým má příkazové okno přístup. V tématu [softwaru volnost ochranou životního prostředí na nástrojích klienta Git](https://git-scm.com/download/) mít nainstalovanou nejnovější verzi.

3. Otevřete příkazový řádek. Naklonujte úložiště GitHub pro distribučních bodů ukázkový kód:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Spusťte VS Code a DevKit připojte k počítači, otevřete složku, která obsahuje kód, který jste naklonovali.

5. Otevřete **DevKitDPS.ino**, najít a nahradit `[Global Device Endpoint]` a `[ID Scope]` s hodnotami si poznamenejte právě dolů.
  ![Koncový bod distribučních bodů](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) můžete nechat **registrationId** jako prázdné aplikace vygeneruje jednu založeného na MAC adres a firmware verzi. Pokud chcete přizpůsobit, ID registrace musí použít alfanumerické znaky a malých písmen a dlouhé pomlčku kombinací pouze s maximálně 128 znaků. Další informace najdete v tématu [spravovat registrace zařízení pomocí portálu Azure](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments).

6. Použití **rychlé otevřete** v produktu VS Code (Windows: `Ctrl+P`, systému macOS: `Cmd+P`) a typ **úkolů zařízení – nahrání** sestavit a odeslat do DevKit kód.

7. Sledujte úspěch úlohy v okně výstupu.

## <a name="save-unique-device-secret-on-stsafe-security-chip"></a>Uložení jedinečný tajný klíč zařízení na čip TPM STSAFE zabezpečení

Zařízení zřizování službu lze nakonfigurovat na zařízení na základě jeho [modulu hardwarového zabezpečení (HSM)](https://azure.microsoft.com/en-us/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/). Používá DevKit [zařízení Identity složení modulu (ROZČLENĚNÍ)](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) z [Computing skupiny TCG (Trusted)](https://trustedcomputinggroup.org). A **jedinečný zařízení tajný klíč (UDS)** uložit v zabezpečení STSAFE čip TPM na DevKit slouží ke generování jedinečný zařízení [X.509](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#select-a-hardware-security-module) certifikátu. Certifikát můžete později použít pro proces registrace ve službě zřizování zařízení.

Typické **jedinečný zařízení tajný klíč (UDS)** je 64 znaků dlouhý řetězec. Ukázka UDS je jako níže:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Každé dva znaky se používá jako hodnota šestnáctkově výpočtu zabezpečení. Proto je přeložen na výše uvedené ukázkové UDS: "`0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`".

Na DevKit uložení jedinečný tajný klíč zařízení:

1. Otevřete sériové monitorování pomocí nástroje, například Putty, najdete v části [konfigurace režimu použít](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) podrobnosti.

2. S DevKit připojená k počítači podržte tlačítko A potom push a uvolněním tlačítka resetování zadejte režim konfigurace. Na obrazovce by měl zobrazit DevKit ID a **"Konfigurace"**.

3. Trvat dlouho ukázkový UDS řetězec výše a změnit jeden nebo více znaků, jiné hodnoty mezi `0` a `f`. To slouží jako vlastní UDS.

4. V okně Sledování sériového portu, zadejte **set_dps_uds [your_own_uds_value]** a stiskněte klávesu Enter ji uložit.
  > [!NOTE]
  > Například pokud nastavíte tak, že změníte poslední dva znaky, které mají vlastní UDS `f`, je třeba zadat příkaz jako **set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff**.

5. Bez zavření okna sériové sledování, stiskněte tlačítko Obnovení nastavení na DevKit.

6. Poznamenejte si **adresa MAC DevKit** a **verzi firmwaru DevKit** hodnotu.
  ![Verze firmwaru](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-x509-certificate"></a>Vygenerování certifikátu X.509

### <a name="windows"></a>Windows

1. Otevření souboru explorer a přejděte do složky obsahovat DSP klonovat ukázkový kód, je **.build** složky, najít a zkopírujte **DPS.ino.bin** a **DPS.ino.map** v ní.
  ![Generované soubory](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Pokud jste změnili `built.path` konfigurace pro Arduino do jiné složky. Budete muset najít tyto soubory ve složce, kterou jste nakonfigurovali.

2. Vložte tyto dva soubory do **nástroje** složky na stejné úrovni s **.build** složky.

3. Spustit **dps_cert_gen.exe**, postupujte podle pokynů zadejte vaše **UDS**, **adresa MAC** pro DevKit a **verzi firmwaru** ke generování certifikát X.509.
  ![Run dps-cert-gen.exe](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Sledovat úspěch generace, **.pem** certifikát je uložen ve stejné složce.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Vytvoření položky registrace zařízení ve službě Device Provisioning

1. Na portálu Azure přejděte do služby zřizování. Klikněte na **Správa registrací** a vyberte kartu **Jednotlivé registrace**. ![Jednotlivé registrace](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Klikněte na tlačítko **Add** (Přidat).

3. V **mechanismus**, zvolte **X.509**.
  ![Nahrání certifikátu.](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

4. V **soubor .pem nebo .cer certifikátu**, nahrajte **.pem** právě máte certifikát.

5. Ponechejte zbývající jako výchozí nastavení a klikněte na tlačítko **Uložit**.

## <a name="start-the-devkit"></a>Spuštění DevKit

1. Spusťte VS Code a otevřete nástroj Sledování sériového portu.

2. Stiskněte **resetovat** na vaše DevKit tlačítko.

Měli byste vidět DevKit začínat registrace služby zřizování zařízení.

![Výstupní kód VS](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-the-devkit-is-registered-on-iot-hub"></a>Ověřte, zda že je zaregistrován DevKit na IoT Hub

Jednou vaše jednotlivými spuštěními zařízení následující akce má být provedena:

1. Zařízení odesílá žádost o registraci vašeho zařízení zřizovací služby.
2. Službu zřizování zařízení odesílá zpět registraci výzvy, ke kterému odpovídá zařízení.
3. Na úspěšnou registraci zařízení zřizovací služby odešle že službě IoT hub identifikátor URI, ID zařízení a zašifrovaný klíč zpět do zařízení.
4. IoT Hub klientská aplikace na zařízení pak připojí do vašeho centra.
5. Úspěšné připojení k rozbočovači měli byste vidět zařízení se zobrazí v Průzkumníku zařízení IoT hub.
  ![Zařízení registrovaná](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-device-id"></a>ID zařízení změn

Výchozí ID zařízení zaregistrované v Azure IoT Hub je **AZ3166**. Pokud chcete upravit, postupujte podle [pokynů tady](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problémy a zpětné vazby

Pokud narazíte na problémy, podívejte se na [nejčastější dotazy k](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) nebo se obraťte na nás z následující kanály:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Další postup

Teď, když jste se naučili Příprava DevKit zaregistrovat zařízení do distribučních bodů pomocí ROZČLENĚNÍ, takže může být automaticky zaregistrujte do služby IoT Hub se touch nula. V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Konfigurace globálních koncový bod distribučních bodů na zařízení
> * Použít jedinečný zařízení tajný klíč (UDS) k vygenerování certifikátu X.509
> * Registraci jednotlivých zařízení
> * Ověřte, zda že je zařízení zaregistrované

Zálohy na další kurzy, další informace:

> [!div class="nextstepaction"]
> [Vytvořit a zřídit simulovaného zařízení](./quick-create-simulated-device.md)

