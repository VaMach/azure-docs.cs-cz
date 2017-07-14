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
ms.date: 06/15/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: f6a49be148ded1a90a642b83d3d70a83e572f4e4
ms.contentlocale: cs-cz
ms.lasthandoff: 06/17/2017

---
# Představení Apache Kafka ve službě HDInsight (Preview)
<a id="introducing-apache-kafka-on-hdinsight-preview" class="xliff"></a>

[Apache Kafka](https://kafka.apache.org) je open source distribuovaná streamovací platforma, kterou lze použít k vytváření aplikací a kanálů pro streamování dat v reálném čase. Kafka také poskytuje funkci pro zprostředkování zpráv podobnou frontě zpráv, ve které můžete publikovat pojmenované datové proudy a přihlásit se k jejich odběru. Kafka ve službě HDInsight představuje spravovanou, vysoce škálovatelnou a vysoce dostupnou službu v cloudu Microsoft Azure.

## Proč používat Kafka ve službě HDInsight?
<a id="why-use-kafka-on-hdinsight" class="xliff"></a>

Kafka poskytuje následující funkce:

* Vzorec zasílání zpráv na principu publikování a odběru: Kafka poskytuje rozhraní API pro autory k publikování záznamů v tématech Kafka. Rozhraní API pro příjemce se používá při přihlášení k odběru tématu.

* Zpracování datových proudů: Kafka se často používá společně s Apache Storm nebo Spark ke zpracování datových proudů v reálném čase. Kafka 0.10.0.0 (HDInsight verze 3.5) zavedla rozhraní API pro streamování, které umožňuje vytvářet řešení streamování bez potřeby Stormu nebo Sparku.

* Horizontální škálování: Kafka rozděluje datové proudy mezi uzly v clusteru HDInsight. Procesy příjemců můžou být přidružené k jednotlivým oddílům pro zajištění vyrovnávání zatížení při využívání záznamů.

* Doručení v daném pořadí: V rámci každého oddílu se záznamy ukládají v datovém proudu v pořadí, v jakém byly přijaty. Přidružením jednoho procesu příjemce na oddíl můžete zajistit zpracování záznamů v daném pořadí.

* Odolnost proti chybám: Oddíly je možné replikovat mezi uzly a zajistit tak odolnost proti chybám.

* Integrace se službou Azure Managed Disks: Spravované disky přináší virtuálním počítačům v clusteru HDInsight lepší škálování a vyšší propustnost.

    Správa disků je v systému Kafka v HDInsight ve výchozím nastavení zapnutá a počet disků v jednotlivých uzlech můžete nastavit při vytváření prostředí HDInsight. Další informace o spravovaných discích najdete v tématu [Azure Managed Disks](../storage/storage-managed-disks-overview.md).

    Informace o konfiguraci spravovaných disků v systému Kafka v HDInsight najdete v tématu [Zvýšení škálovatelnosti systému Kafka v prostředí HDInsight](hdinsight-apache-kafka-scalability.md).

## Případy použití
<a id="use-cases" class="xliff"></a>

* **Zasílání zpráv:** Protože Kafka podporuje vzorec zasílání zpráv na principu publikování a odběru, často se používá jako zprostředkovatel zpráv.

* **Sledování aktivit:** Protože Kafka poskytuje protokolování záznamů v daném pořadí, je možné ji použít ke sledování a opětovnému vytvoření aktivit. Například akcí uživatelů na webu nebo v aplikaci.

* **Agregace:** Pomocí zpracování datových proudů můžete agregovat a kombinovat informace z různých datových proudů a centralizovat je v podobě provozních dat.

* **Transformace:** Pomocí zpracování datových proudů můžete kombinovat data z více vstupních témat a rozšiřovat je do jednoho nebo několika výstupních témat.

## Další kroky
<a id="next-steps" class="xliff"></a>

Následující odkazy popisují používání Apache Kafka ve službě HDInsight:

* [Začínáme s Kafka ve službě HDInsight](hdinsight-apache-kafka-get-started.md)

* [Vytvoření repliky Kafka ve službě HDInsight pomocí MirrorMakeru](hdinsight-apache-kafka-mirroring.md)

* [Použití Apache Stormu se systémem Kafka ve službě HDInsight](hdinsight-apache-storm-with-kafka.md)

* [Použití Apache Sparku se systémem Kafka ve službě HDInsight](hdinsight-apache-spark-with-kafka.md)

* [Připojení k systému Kafka přes virtuální síť Azure](hdinsight-apache-kafka-connect-vpn-gateway.md)
