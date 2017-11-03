---
title: "Ukázková data v tabulkách Azure HDInsight Hive | Microsoft Docs"
description: "Dolů vzorkování dat v Azure HDInsight (Hadopop) tabulek Hive"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 357307a034b277e8c37e99bda1ed6a9a76e13f41
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Ukázková data v tabulkách Azure HDInsight Hive
V tomto článku jsme popisují, jak nižší sample data uložená v Azure HDInsight Hive tabulky pomocí dotazů Hive. Nemůžeme popisuje tři metody vzorkování často se používá použít:

* Uniform náhodné vzorkování
* Náhodné vzorkování podle skupin
* Vrstveného vzorkování

Následující **nabídky** odkazy na témata, které popisují, jak ukázková data z různých prostředích úložiště.

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Proč ukázková data?**
Pokud je velké datové sady, které chcete analyzovat, je obvykle vhodné nižší ukázková data, která mají snížit velikost menší, ale reprezentativní a lepší správu bitlockeru. To usnadňuje pochopení dat, zkoumání a funkce inženýrství. Jeho role v procesu Team datové vědy je umožnit rychlé vytváření prototypů zpracování dat funkcí a modelů strojového učení.

Tato úloha vzorkování je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-to-submit-hive-queries"></a>Postup odesílání dotazů Hive
Z konzoly Hadoop příkazový řádek z hlavního uzlu clusteru Hadoop lze odesílat dotazy Hive. K tomu, přihlaste se k hlavnímu uzlu clusteru Hadoop, otevřete konzolu nástroje příkazového řádku Hadoop a odesílání dotazů Hive z ní. Pokyny k odesílání dotazů Hive v konzole nástroje příkazového řádku Hadoop, najdete v části [postup odesílání dotazů Hive](move-hive-tables.md#submit).

## <a name="uniform"></a>Uniform náhodné vzorkování
Uniform náhodné vzorkování znamená, že každý řádek v sadě dat má stejnou šanci se vzorků. To lze provést přidáním další pole rand() se sada dat v informacích o vnitřní dotaz "Vyberte" a v vnější "Vyberte" dotaz tuto podmínku na tomto náhodných poli.

Tady je příklad dotazu:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Zde `<sample rate, 0-1>` Určuje poměr záznamy, které uživatelé má zkusit.

## <a name="group"></a>Náhodné vzorkování podle skupin
Když vzorkování kategorizovaná data, můžete zahrnout nebo vyloučit všechny instance určité konkrétní hodnoty kategorií proměnné. Toto je pojmy "vzorkování skupinou".
Například pokud máte kategorií proměnné "Stavu", která obsahuje hodnoty NY, MA, certifikační Autority, ni, PA atd, chcete záznamů stejného stavu, vždy se společně, jestli jsou vzorkovat nebo ne.

Tady je příklad dotazu této ukázky podle skupiny:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Vrstveného vzorkování
Náhodné vzorkování je si s ohledem na proměnnou kategorií Pokud ukázky získat hodnoty, kategorií, jsou ve stejné poměr jako nadřazené naplnění, ze kterého byly získány ukázky. Pomocí příkladě jako výš, Předpokládejme, že data obsahují dílčí plnění státy, můžete ni má 100 připomínky, NY má 60 připomínky a WA má 300 připomínky. Pokud zadáte rychlost vrstveného vzorkování být 0,5, pak ukázka získat by měl mít přibližně 50, 30 a 150 připomínky ni, NY a WA v uvedeném pořadí.

Tady je příklad dotazu:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Informace pro pokročilejší metody vzorkování, které jsou k dispozici v Hive naleznete v tématu [LanguageManual vzorkování](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

