---
title: "Služba koncepty ve službě Azure IoT Hub zařízení zřizování Service | Microsoft Docs"
description: "Popisuje zřizování koncepty služby specifické pro zařízení s distribučních bodů a IoT Hub"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 10/03/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 96c63e5d0379150ea619dbbe912a21e373f808af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>Koncepty služby zřizování zařízení IoT Hub

Služba zřizování zařízení IoT Hub je služba Pomocník pro IoT Hub, který použijete ke konfiguraci zařízení nula touch zřizování do zadané služby IoT hub. Se službou zřizování zařízení můžete zřídit miliony zařízení zabezpečené a škálovatelné způsobem.

Zřizování zařízení je proces dvě části. První část je vytvoření počátečního připojení mezi zařízením a řešení IoT podle *registrace* zařízení. Druhá část je použití správné *konfigurace* na zařízení, v závislosti na konkrétních požadavcích řešení. Po dokončení obou kroků, budeme moct, aby zařízení bylo plně *zřízený*. Služba zřizování zařízení automatizuje oba kroky a poskytuje bezproblémové prostředí zřizování pro zařízení.

Tento článek poskytuje přehled zřizování konceptů nejvíce pro správu *služby*. Tento článek je nejdůležitější osoby, které se účastní [cloudu kroku](about-iot-dps.md#cloud-setup-step) z Příprava zařízení pro nasazení.

## <a name="service-operations-endpoint"></a>Koncový bod služby operations

Koncový bod služby operations je koncový bod pro správu nastavení služby a údržbu seznamu registrace. Tento koncový bod je použita pouze správcem služby; není využíván zařízení.

## <a name="device-provisioning-endpoint"></a>Koncový bod zřizování zařízení

Zařízení zřizování koncový bod je centrální koncovým bodem, který všechna zařízení komunikují s pro zřizování. Adresa URL je stejný pro všechny zřizování služby eliminovat potřebu reflash v řetězu scénáře dodávky zařízení s nové informace o připojení. [ID oboru](#id-scope) zajišťuje izolaci klientů.

## <a name="linked-iot-hubs"></a>Propojené centra IoT

Služba zřizování zařízení můžete zřídit jenom zařízení do centra IoT, které byly spojeny s ho. Propojení služby IoT hub službě zřizování zařízení poskytuje služby oprávnění pro čtení a zápis do registru zařízení služby IoT hub; s odkazem může služba zřizování zařízení zaregistrovat ID zařízení a nastavte počáteční konfigurace v dvojče zařízení. Propojené služby IoT hubs může být v libovolné oblasti Azure. Centra v jiných předplatných může propojit zřizování služby.

## <a name="allocation-policy"></a>Přidělení zásad

Úrovně služeb, nastavení, která určuje, jak služba zřizování zařízení přiřadí zařízení do služby IoT hub. Existují tři podporované přidělení zásady:
* **Rovnoměrně váha distribuce**: propojené centra IoT jsou stejně může mít zřízení k nim zařízení. Výchozí nastavení. Pokud jsou zřizování zařízení, aby pouze jeden Centrum IoT, můžete ponechat toto nastavení.
* **Nejnižší latenci**: zřízení zařízení do služby IoT hub s nejnižší latencí do zařízení. Více propojené služby IoT hubs by poskytovat stejné nejnižší latenci, zřizování služby hashuje zařízení v těchto rozbočovače
* **Konfigurace statické prostřednictvím seznamu registrace**: specifikace požadované služby IoT hub v seznamu registrace přednost zásada přidělování úrovně služeb.

## <a name="enrollment"></a>Registrace

Zápisu je záznam zařízení nebo skupin zařízení, která může na některých bodu registrace. Záznam zápisu obsahuje informace o zařízení nebo skupině zařízení, včetně metodu ověření pro zařízení a volitelně počáteční požadované konfigurace požadovaných IoT hub a požadovaného zařízení. Existují dva typy registrace podporován službou zřizování zařízení.

### <a name="enrollment-group"></a>Registrace skupiny

Skupinu registrace je skupina zařízení, které sdílejí mechanismus konkrétní ověření. Všechna zařízení ve skupině registrace k dispozici certifikáty X.509, které byly podepsány stejnou kořenovou certifikační Autoritu. Registrace skupiny lze použít pouze mechanismus ověření X.509. Název skupiny pro zápis a název certifikátu musí být alfanumerické znaky a malých písmen a může obsahovat pomlčky.

> [!TIP]
> Doporučujeme používat skupinu registrace pro velký počet zařízení, které sdílejí požadované počáteční konfigurace, nebo pro zařízení všechny má stejné klienta.

### <a name="individual-enrollment"></a>Jednotlivé registrace

Jednotlivé zápisu je záznam pro jedno zařízení, která může zaregistrovat. Jednotlivé registrace používat certifikáty X.509 nebo tokeny SAS (ve fyzických nebo virtuálních čipu TPM), jak mechanismů ověření. ID registrace v jednotlivých zápisu je alfanumerické znaky a malých písmen a může obsahovat pomlčky. Jednotlivé registrace může mít požadovaný IoT hub ID zařízení v zadané.

> [!TIP]
> Doporučujeme používat jednotlivé registrace zařízení, které vyžadují jedinečné počáteční konfigurace, nebo pro zařízení, která lze použít pouze tokeny SAS prostřednictvím TPM nebo virtuální čipu TPM jako mechanismus ověření.

## <a name="registration"></a>Registrace

Registrace je záznam zařízení úspěšně registrace nebo zřizování do služby IoT Hub prostřednictvím služby zřizování zařízení. Záznamy registrace jsou vytvořeny automaticky. můžete je odstranit, ale nelze aktualizovat.

## <a name="operations"></a>Operace

Operace jsou fakturace jednotka služby zřizování zařízení. Jedna operace je úspěšné dokončení jedné instrukce ke službě. Operace zahrnují registraci a opakovanou registraci zařízení a také změny na straně služby, jako je přidání položek seznamu registrací a jejich aktualizace.
