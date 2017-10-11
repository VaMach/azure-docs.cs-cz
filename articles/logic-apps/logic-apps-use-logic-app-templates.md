---
title: "Vytváření pracovních postupů z šablon - Azure Logic Apps | Microsoft Docs"
description: "Začínáme – rychlé vytváření pracovních postupů pomocí šablony aplikace logiky Azure k připojení aplikace a integrovat data."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 3656acfb-eefd-4e75-b5d2-73da56c424c9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89272869f7dfaa34cbd2ad32d67dca0955e6158b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-workflow-using-a-pre-built-template-or-pattern-to-get-started-quickly"></a>Konfigurace pracovního postupu pomocí předdefinovaných šablon nebo vzor rychle začít

## <a name="what-are-logic-app-templates"></a>Co jsou šablony aplikace logiky
Šablona aplikace logiky je předdefinovaných logiku aplikace, která vám pomůže rychle začít vytvářet své vlastní pracovní postup. 

Tyto šablony jsou vhodný způsob, jak zjistit různých vzorů, které mohou být vytvořeny pomocí aplikací logiky. Můžete použít tyto šablony jako-je nebo upravit tak, aby vyhovovaly vašemu scénáři.

## <a name="overview-of-available-templates"></a>Přehled dostupných šablon
Existuje mnoho dostupných šablon aktuálně publikované v platformě aplikace logiky. Některé kategorie příklad, stejně jako typy konektorů použít v nich, jsou uvedeny níže.

### <a name="enterprise-cloud-templates"></a>Šablony organizace cloudu
Šablony, které se integrují Dynamics CRM Salesforce, pole, objektů Blob v Azure a ostatní konektory pro vaše potřeby enterprise cloud. Některé příklady z co můžete udělat pomocí těchto šablony zahrnují uspořádání zájemců a zálohování dat podnikových souborů.

### <a name="enterprise-integration-pack-templates"></a>Šablony organizace integration pack
Konfigurace VETER (ověřit, extrakce, transformace, zlepšit komunikaci oddělení, směrovat) kanály, přijetí X12 EDI dokumentu přes AS2 a převod do formátu XML, stejně jako zprávy X12 a AS2 zpracování.

### <a name="protocol-pattern-templates"></a>Protokol vzor šablony
Tyto šablony se skládají logic Apps, které obsahují vzory protokolu, jako je požadavků a odpovědí prostřednictvím protokolu HTTP, jakož i integrace napříč FTP a SFTP. Použijte tyto, které jsou, nebo jako základ pro vytváření složitějších vzory protokolu.  

### <a name="personal-productivity-templates"></a>Osobních kancelářských šablony
Vzory k vylepšení osobních kancelářských zahrnují šablony, které nastavit denní připomenutí, zapněte důležité pracovní položky do seznamu úkolů a automatizaci úloh zdlouhavé dolů krok schválení jednoho uživatele.

### <a name="consumer-cloud-templates"></a>Šablony příjemce cloudu
Jednoduché šablony, které integrovat služby sociálních médií jako Twitter, Slack a e-mailu, nakonec schopná posílení sociálních médií marketingové iniciativy. Patří sem také šablony například zakalená kopírování, které může pomoci zvýšit produktivitu uložením čas strávený tradičně opakovaných úloh. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Postup vytvoření aplikace logiky pomocí šablony
Chcete-li začít používat šablonu aplikace logiky, přejděte do návrháře logiku aplikace. Pokud zadáváte návrháře otevřením existující aplikace logiky, aplikaci logiky automaticky načte v návrháře zobrazení. Pokud vytváříte novou aplikaci logiky, ale zobrazí na obrazovce níže.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Prostřednictvím této obrazovky buď můžete začít s prázdnou logiku aplikace nebo předem připravených šablon. Pokud vyberete jednu z šablon, jsou k dispozici další informace. V tomto příkladu používáme *v Dropbox je vytvořen nový soubor, zkopírujte jej do OneDrive* šablony.  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Pokud se rozhodnete použít šablonu, stačí vybrat *tuto šablonu použít* tlačítko. Budete vyzváni k přihlášení do svých účtů na základě na konektory, které využívá šablony. Nebo, pokud jste dříve vytvořili připojení pomocí těchto konektorů, můžete si vybrat pokračovat, jak vidíte níže.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Po navázání připojení a výběrem *pokračovat*, otevře aplikaci logiky v návrháře zobrazení.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

V příkladu nahoře jako je tomu u mnoha šablony, některá pole povinná vlastnost může doplnit v rámci konektory; ale některé může stále vyžadují hodnotu bylo možné správně nasadit aplikaci logiky. Pokud se pokusíte nasadit bez zadávání některá pole chybí, zobrazí se upozornění s chybovou zprávou.

Pokud chcete vrátit do prohlížeče šablony, vyberte *šablony* tlačítko v horním navigačním panelu. Přepnutím zpět do prohlížeče šablony ztratit všechny neuložené průběh. Před přepnout zpět do prohlížeče šablony, se zobrazí upozornění zpráva s upozorněním tohoto objektu.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Postup nasazení aplikace logiky vytvořené ze šablony
Po načtení šablony a všechny požadované změny, vyberte uložení tlačítko v levém horním rohu. Tím se uloží a publikuje aplikace logiky.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Pokud vás zajímají další informace o tom, jak přidat další kroky do stávající šablony aplikace logiky, nebo proveďte úpravy obecně, přečtěte si informace v [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).

