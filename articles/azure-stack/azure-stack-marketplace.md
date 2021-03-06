---
title: "Publikovat vlastní marketplace položky v Azure zásobníku (operátor cloudu) | Microsoft Docs"
description: "Jako operátor zásobník Azure zjistěte, jak publikovat vlastní marketplace položky v Azure zásobníku."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: c791708e11b7e9e8bbe046f06233d948d4632c90
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="the-azure-stack-marketplace-overview"></a>Přehled Azure Marketplace zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Na webu Marketplace je kolekce služeb, aplikací a prostředky, které jsou přizpůsobené pro zásobník Azure, jako jsou sítě, virtuální počítače, úložiště a tak dále. Uživatelé se zde k vytvoření nových prostředků a nasazení nové aplikace. Považujte ho za nákupní katalogu, kde uživatelé mohou procházet a vyberte položky, které chtějí používat. Pokud chcete použít položku Marketplace, musí uživatelé přihlásit k nabídka, která jim udělí přístup k položce.

Jako operátor zásobník Azure, rozhodnete položek, které chcete přidat (publikovat) na webu Marketplace. Můžete publikovat třeba databáze, aplikační služby a tak dále. Díky tomu je viditelná pro všechny uživatele. Můžete publikovat vlastní položky, které vytvoříte. Můžete také publikovat položky z rozšiřujících se [seznam položek Azure Marketplace](azure-stack-marketplace-azure-items.md). Při publikování položky Marketplace, uživatelé mohou vidět do pěti minut.

Chcete-li spustit nástroj na webu Marketplace, klikněte na tlačítko **nový**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Položky Marketplace
Položka Azure Marketplace zásobníku je služby, aplikace nebo prostředku, který vaši uživatelé můžete stáhnout a použít. Všechny položky Marketplace zásobník Azure jsou viditelné pro všechny uživatele.

Každá položka Marketplace má:

* Šablonu Azure Resource Manageru pro zřizování prostředků
* Metadata, jako jsou řetězce, ikony a další marketingové nástroje
* Formátování informace o položce v portálu

Každá položka publikovaných na Marketplace s cílem používá formát názvem balíčku Galerie Azure (azpkg). Přidat nasazení nebo modul runtime prostředky (jako je kód, soubory zip s softwaru nebo bitové kopie virtuálních počítačů) do zásobník Azure samostatně, nikoli jako součást položku Marketplace. 

## <a name="next-steps"></a>Další postup
[Vytvoření a publikování položku marketplace.](azure-stack-create-and-publish-marketplace-item.md)

