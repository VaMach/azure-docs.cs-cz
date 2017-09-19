---
title: "Úvod k systému Apache Kafka ve službě HDInsight – Azure | Dokumentace Microsoftu"
description: "Přečtěte si o systému Apache Kafka ve službě HDInsight: co to je, co to dělá a kde najít příklady a informace pro začátek."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/07/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 39234ca792983178cfd4304e001271ea30e28ae6
ms.contentlocale: cs-cz
ms.lasthandoff: 09/13/2017

---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>Představení Apache Kafka ve službě HDInsight (Preview)

[Apache Kafka](https://kafka.apache.org) je open source distribuovaná streamovací platforma, kterou lze použít k vytváření aplikací a kanálů pro streamování dat v reálném čase. Kafka také poskytuje funkci pro zprostředkování zpráv podobnou frontě zpráv, ve které můžete publikovat pojmenované datové proudy a přihlásit se k jejich odběru. Kafka ve službě HDInsight představuje spravovanou, vysoce škálovatelnou a vysoce dostupnou službu v cloudu Microsoft Azure.

## <a name="why-use-kafka-on-hdinsight"></a>Proč používat Kafka ve službě HDInsight?

Kafka poskytuje následující funkce:

* Vzorec zasílání zpráv na principu publikování a odběru: Kafka poskytuje rozhraní API pro autory k publikování záznamů v tématech Kafka. Rozhraní API pro příjemce se používá při přihlášení k odběru tématu.

* Zpracování datových proudů: Kafka se často používá společně s Apache Storm nebo Spark ke zpracování datových proudů v reálném čase. Kafka 0.10.0.0 (HDInsight verze 3.5) zavedla rozhraní API pro streamování, které umožňuje vytvářet řešení streamování bez potřeby Stormu nebo Sparku.

* Horizontální škálování: Kafka rozděluje datové proudy mezi uzly v clusteru HDInsight. Procesy příjemců můžou být přidružené k jednotlivým oddílům pro zajištění vyrovnávání zatížení při využívání záznamů.

* Doručení v daném pořadí: V rámci každého oddílu se záznamy ukládají v datovém proudu v pořadí, v jakém byly přijaty. Přidružením jednoho procesu příjemce na oddíl můžete zajistit zpracování záznamů v daném pořadí.

* Odolnost proti chybám: Oddíly je možné replikovat mezi uzly a zajistit tak odolnost proti chybám.

* Integrace se službou Azure Managed Disks: Spravované disky přináší virtuálním počítačům v clusteru HDInsight lepší škálování a vyšší propustnost.

    Pro systém Kafka ve službě HDInsight jsou spravované disky povolené ve výchozím nastavení. Počet použitých disků na uzel je možné nakonfigurovat při vytváření služby HDInsight. Další informace o spravovaných discích najdete v tématu [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md).

    Informace o konfiguraci spravovaných disků v systému Kafka v HDInsight najdete v tématu [Zvýšení škálovatelnosti systému Kafka v prostředí HDInsight](hdinsight-apache-kafka-scalability.md).

## <a name="use-cases"></a>Případy použití

* **Zasílání zpráv:** Protože Kafka podporuje vzorec zasílání zpráv na principu publikování a odběru, často se používá jako zprostředkovatel zpráv.

* **Sledování aktivit:** Protože Kafka poskytuje protokolování záznamů v daném pořadí, je možné ji použít ke sledování a opětovnému vytvoření aktivit. Například akcí uživatelů na webu nebo v aplikaci.

* **Agregace:** Pomocí zpracování datových proudů můžete agregovat a kombinovat informace z různých datových proudů a centralizovat je v podobě provozních dat.

* **Transformace:** Pomocí zpracování datových proudů můžete kombinovat data z více vstupních témat a rozšiřovat je do jednoho nebo několika výstupních témat.

## <a name="architecture"></a>Architektura

![Konfigurace clusteru Kafka](./media/hdinsight-apache-kafka-introduction/kafka-cluster.png)

Tento diagram ukazuje obvyklou konfiguraci Kafka s využitím skupin příjemců, dělení a replikace. Díky tomu nabízí paralelní čtení událostí s odolností proti chybám. Apache ZooKeeper spravuje stav clusteru Kafka a proto je postaven pro souběžné a odolné transakce s nízkou latencí. Kafka ukládá záznamy v *tématech*. Záznamy jsou vytvářeny *producenty* a spotřebovávány *konzumenty*. Producenti načítají záznamy ze *zprostředkovatelů* Kafka. Každý pracovní uzel v clusteru HDInsight je zprostředkovatelem Kafka. Pro každého příjemce se vytvoří jeden oddíl, což umožňuje paralelní zpracování streamovaných dat. K rozprostření oddílů mezi uzly se využívá replikace, která zajišťuje ochranu před výpadky uzlů (zprostředkovatelé). Oddíl s označením *(L)* je vedoucím daného uzlu. Provoz producenta se směruje do vedoucích jednotlivých uzlů pomocí stavu, který spravuje ZooKeeper.

> [!IMPORTANT]
> Kafka nemá žádné informace o podkladovém hardwaru (rack) v datovém centru Azure. Pokud chcete zajistit správné rozmístění oddílů napříč podkladovým hardwarem, přečtěte si téma věnované [konfiguraci vysoké dostupnosti dat (Kafka)](hdinsight-apache-kafka-high-availability.md).

## <a name="next-steps"></a>Další kroky

Následující odkazy popisují používání Apache Kafka ve službě HDInsight:

* [Začínáme s Kafka ve službě HDInsight](hdinsight-apache-kafka-get-started.md)

* [Vytvoření repliky Kafka ve službě HDInsight pomocí MirrorMakeru](hdinsight-apache-kafka-mirroring.md)

* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](hdinsight-apache-storm-with-kafka.md)

* [Použití Apache Sparku se systémem Kafka ve službě HDInsight](hdinsight-apache-spark-with-kafka.md)

* [Připojení k systému Kafka přes virtuální síť Azure](hdinsight-apache-kafka-connect-vpn-gateway.md)
