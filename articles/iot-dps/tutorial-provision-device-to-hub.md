---
title: "Zřízení zařízení pomocí Azure IoT Hub zařízení zřizování služby | Microsoft Docs"
description: "Zřízení zařízení do jednoho IoT hub pomocí služby Azure IoT Hub zařízení zřizování Service"
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
ms.openlocfilehash: bf50699d2dc67294d554ba15713254a8b88d8ade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Zřízení zařízení do služby IoT hub pomocí služby Azure IoT Hub zařízení zřizování Service

V tomto kurzu předchozí jste zjistili, jak nastavit zařízení pro připojení k službě zřizování zařízení. V tomto kurzu se dozvíte, jak tuto službu využívat ke zřízení zařízení do jednoho IoT hub pomocí  **_registrace seznamy_**. V tomto kurzu získáte informace o následujících postupech:

> [!div class="checklist"]
> * Registrace zařízení
> * Spuštění zařízení
> * Ověřte, zda že je zařízení zaregistrované

## <a name="prerequisites"></a>Požadavky

Než budete pokračovat, ujistěte se, pro konfiguraci zařízení a jeho *modul hardwarového zabezpečení* popsané v tomto kurzu [nastavení zařízení ke zřízení pomocí Azure IoT Hub zařízení zřizování služby](./tutorial-set-up-device.md).


<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Registrace zařízení

Tento krok zahrnuje přidání artefakty jedinečný zabezpečení zařízení ke službě zřizování zařízení. Tyto artefakty zabezpečení jsou následující:

- Pro zařízení s založené na čipu TPM:
    - *Ověřovací klíč* , je jedinečný pro každý čip TPM nebo simulace. Pro čtení [pochopit ověřovacího klíče TPM](https://technet.microsoft.com/library/cc770443.aspx) Další informace.
    - *ID registrace* který slouží k jednoznačné identifikaci zařízení v oboru názvů nebo oboru. To může nebo nemusí být stejný jako identifikátor zařízení. ID je povinné pro každé zařízení. Pro zařízení, založené na čipu TPM může být ID registrace odvozen od čipu TPM samostatně, například algoritmus hash SHA-256 ověřovacího klíče čipu TPM.

    ![Informace o registraci pro TPM v portálu](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)

- X.509 na základě zařízení:
    - [Certifikát vydaný pro X.509](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx) čip TPM nebo simulace ve formě buď *.pem* nebo *.cer* souboru. Pro jednotlivé registrace, budete muset použít *certifikát podepisující osoba* pro váš systém X.509, zatímco pro skupiny registrace, budete muset použít *kořenový certifikát*.

    ![Informace o registraci pro X.509 na portálu](./media/tutorial-provision-device-to-hub/x509-device-enrollment.png)


Existují dva způsoby, jak zaregistrovat zařízení ke službě zřizování zařízení:

- **Registrace skupiny** reprezentuje skupinu zařízení, které sdílejí mechanismus konkrétní ověření. Doporučujeme používat skupinu registrace pro velký počet zařízení, které sdílejí požadované počáteční konfigurace, nebo pro zařízení všechny má stejné klienta.

    ![Registrace skupiny pro X.509 na portálu](./media/tutorial-provision-device-to-hub/x509-enrollment-groups.png)

- **Jednotlivé registrace** reprezentuje položku pro jedno zařízení, která může zaregistrovat službu zřizování zařízení. Jednotlivé registrace může použít buď x509 certifikáty nebo SAS tokeny (ve fyzických nebo virtuálních čipu TPM) jako mechanismů ověření. Doporučujeme používat jednotlivé registrace zařízení, které vyžadují jedinečné počáteční konfigurace, nebo pro zařízení, které lze použít pouze tokeny SAS prostřednictvím TPM nebo virtuální čipu TPM jako mechanismus ověření. Jednotlivé registrace může mít požadovaný IoT hub ID zařízení v zadané.

Následující kroky k registraci zařízení na portálu:

1. Všimněte si artefakty zabezpečení pro modul hardwarového zabezpečení na vašem zařízení. Možná budete muset použít rozhraní API uvedených v části s názvem [extrahovat zabezpečení artefakty](./tutorial-set-up-device.md#extractsecurity) předchozí kurzu ve vývojovém prostředí.

1. Přihlaste se k webu Azure Portal, v nabídce vlevo klikněte na tlačítko **Všechny prostředky** a otevřete svou službu Device Provisioning.

1. V okně s přehledem služby Device Provisioning vyberte **Správa registrací**. Vyberte buď **jednotlivých registrace** kartě nebo **registrace skupiny** karta podle vašeho nastavení zařízení. Klikněte **přidat** tlačítka v horní části. Vyberte **TPM** nebo **X.509** jako ověření identity *mechanismus*a zadejte příslušná bezpečnostní artefakty, jak jsme vysvětlili výše. Můžete zadat nový **ID zařízení IoT Hub**. Jakmile budete hotovi, klikněte na tlačítko **Uložit**. 

1. Když je zařízení úspěšně zaregistrované, měli byste vidět se zobrazovat na portálu jako následující:

    ![Úspěšné TPM registrace na portálu](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)


## <a name="start-the-device"></a>Spuštění zařízení

V tomto okamžiku následující instalační program je připraven pro registraci zařízení:

1. Skupiny zařízení nebo zařízení jsou zaregistrovaná k službě zřizování zařízení, a 
2. Zařízení je připraven s čipovou HSM nakonfigurované a dostupné prostřednictvím aplikace pomocí klienta zařízení zřizování služby SDK.

Spusťte zařízení a povolit klientské aplikaci začínat registrace služby zřizování zařízení.  


## <a name="verify-the-device-is-registered"></a>Ověřte, zda že je zařízení zaregistrované

Jednou vaše jednotlivými spuštěními zařízení následující akce má být provedena. V tématu TPM simulátoru ukázkovou aplikaci [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) další podrobnosti. 

1. Zařízení odesílá žádost o registraci vašeho zařízení zřizovací služby.
2. Pro zařízení s čipem TPM odešle služba zřizování zařízení zpět registraci výzvy, ke kterému zařízení reaguje. 
3. Na úspěšnou registraci zařízení zřizovací služby odešle že IOT hub identifikátor URI, ID zařízení a zašifrovaný klíč zpět do zařízení. 
4. IoT Hub klientská aplikace na zařízení pak připojí do vašeho centra. 
5. Úspěšné připojení k rozbočovači, měli byste vidět zařízení objeví ve službě IoT hub **Explorer zařízení**. 

    ![Úspěšné připojení k rozbočovači na portálu](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Registrace zařízení
> * Spuštění zařízení
> * Ověřte, zda že je zařízení zaregistrované

Přechodu na v dalším kurzu se dozvíte, jak zřídit víc zařízení přes centra Vyrovnávání zatížení sítě. 

> [!div class="nextstepaction"]
> [Zřízení zařízení napříč Vyrovnávání zatížení sítě centra IoT](./tutorial-provision-multiple-hubs.md)
