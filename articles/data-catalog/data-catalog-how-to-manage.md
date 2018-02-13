---
title: "Spravovat datové prostředky v Azure Data Catalog | Microsoft Docs"
description: "V článku se dozvíte, jak řídit viditelnost a vlastnictví datových prostředků, které jsou zaregistrované v Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 5a4b2b5734bf8bfbbc45a65b02362d1fa37b1a87
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Spravovat datové prostředky v Azure Data Catalog
## <a name="introduction"></a>Úvod
Azure Data Catalog je určená pro zdroj dat zjišťování, takže můžete snadno vyhledat a pochopit zdroje dat, které budete muset provádět analýzy a rozhodnutí. Tyto funkce zjišťování zajistěte největších dopad při vy a ostatní uživatelé můžete najít a pochopit nejširší řadu dostupných zdrojů dat. S těmito prvky na paměti je výchozí chování katalogu Data Catalog pro všechny zdroje dat registrované být viditelné a zjistitelný všichni uživatelé katalogu.

Data Catalog není umožňují přístup k samotná data. Vlastníkem zdroje dat se řídí přístup k datům. Pomocí katalogu Data Catalog můžete zjistit zdroje dat a zobrazit metadata, která souvisí s zdrojům, které jsou zaregistrovány v katalogu.

Můžou nastat situace, ale kde zdroje dat by měly být jenom viditelné určitým uživatelům nebo do členů určitých skupin. V takových scénářů uživatelé převzít vlastnictví registrované datové prostředky v rámci katalogu a pak řídit viditelnost prostředků, které vlastní.

> [!NOTE]
> Funkce popsané v tomto článku je k dispozici pouze v Azure Data Catalog, Standard Edition. Edice Free nenabízí možnosti pro vlastnictví a omezení viditelnosti datový prostředek.
>
>

## <a name="manage-ownership-of-data-assets"></a>Spravovat vlastnictví datových prostředků
Ve výchozím nastavení jsou bez přiřazeného vlastnictví datových prostředků, které jsou zaregistrovány v katalogu Data Catalog. Každý uživatel s oprávněním pro přístup ke katalogu můžete zjistit a opatřit poznámkami tyto prostředky. Uživatelé mohou převzít vlastnictví bez přiřazeného vlastnictví datových prostředků a pak omezit viditelnost prostředků, které vlastní.

Pokud vlastní datový prostředek v katalogu Data Catalog, jenom uživatelé, kteří jsou autorizovaní vlastníky můžete zjistit asset a zobrazit jeho metadata, a pouze vlastníci můžete odstranit i asset z katalogu.

> [!NOTE]
> Vlastnictví v katalogu Data Catalog ovlivňuje pouze metadata, která je uložené v katalogu. Vlastnictví neuděluje všechna oprávnění v podkladovém zdroji dat.
>
>

### <a name="take-ownership"></a>Převzít vlastnictví
Uživatelé mohou převzít vlastnictví datových prostředků tak, že vyberete **převzít vlastnictví** možnost na portálu katalogu Data Catalog. Žádné speciální oprávnění jsou vyžadována k převzetí vlastnictví prostředek bez přiřazeného vlastnictví data. Každý uživatel může převzít vlastnictví prostředek bez přiřazeného vlastnictví data.

### <a name="add-owners-and-co-owners"></a>Přidat vlastníků a spoluvlastníci
Pokud je již vlastněn datový prostředek, nelze ostatní uživatelé jednoduše převzít vlastnictví. Je nutné přidat jako spoluvlastníci existující vlastník. Všechny vlastníka můžete přidat další uživatele nebo skupiny zabezpečení jako spoluvlastníci.

> [!NOTE]
> Je vhodné mít alespoň dva jednotlivce jako vlastníci pro všechny vlastní datový prostředek.
>
>

### <a name="remove-owners"></a>Odebrat vlastníky
Stejně jako všechny vlastník majetku můžete přidat spoluvlastníky, můžete odebrat všechny vlastník majetku žádné spoluvlastník.

Vlastníka prostředku, který odebere mu nebo samu sebe jako vlastníka již nebude možné spravovat asset. Pokud vlastník majetku odebere mu nebo samu sebe jako vlastníka a neexistují žádné společné vlastníci, asset přejde do stavu bez přiřazeného vlastnictví.

## <a name="control-visibility"></a>Přehled ovládacího prvku
Datový prostředek vlastníky můžete řídit viditelnost datových prostředků, které vlastní. Chcete-li omezit viditelnost jako výchozí, kde všichni uživatelé katalogu Data Catalog můžete vyhledat a zobrazit datový prostředek, můžete vlastník majetku přepnutí viditelnost nastavení z **Everyone** k **vlastníci a tito uživatelé** v vlastnosti pro asset. Vlastníci poté můžete přidat konkrétní uživatele a skupiny zabezpečení.

> [!NOTE]
> Kdykoli je to možné, měli oprávnění vlastnictví a viditelnost asset přiřadit do skupin zabezpečení a ne pro jednotlivé uživatele.
>
>

## <a name="catalog-administrators"></a>Správci katalogu
Správci katalogu dat se implicitně společné vlastníci všechny prostředky v katalogu. Vlastníci Asset nelze odebrat viditelnost od správců, a můžou správci spravovat vlastnictví a viditelnost pro všechny datové prostředky v katalogu.

## <a name="summary"></a>Souhrn
Data Catalog crowdsourcingový model metadata a data zjišťování asset umožňuje všem uživatelům katalogu přispívat a zjišťovat. Standard Edition Data Catalog je určen pro vlastnictví a správu omezit viditelnost a použití konkrétní datové prostředky.
