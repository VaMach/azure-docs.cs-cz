---
title: "Přehled Portálu Microsoft Azure"
description: "Další informace o použití portálu Microsoft Azure"
services: 
documentationcenter: 
author: davidwrede
manager: dwrede
editor: jimbe
ms.assetid: 53cb9df1-c96a-4f4e-b022-18336cd3d697
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/16/2015
ms.author: dwrede
translationtype: Human Translation
ms.sourcegitcommit: 6ab02779d65dfc1519b457486678ace5fafc9fb5
ms.openlocfilehash: 60733f973e7b88389816c1ea7cc15532ba967693


---
# <a name="microsoft-azure-portal-overview"></a>Přehled Portálu Microsoft Azure
Portál Microsoft Azure je centrálním místem, kde můžete zřizovat a spravovat prostředky Azure.  V tomto kurzu se seznámíme s Portálem a ukážeme si, jak používat některé z těchto klíčových funkcí:

* **Komplexní marketplace**, ve kterém najdete tisíce položek od společnosti Microsoft a jiných dodavatelů, které je možné zakoupit nebo zřídit.
* **Jednotné a škálovatelné prostředí procházení obsahu** umožňující snadno najít prostředky, které vás zajímají, a provádět různé operace správy.
* **Konzistentní stránky (okna) správy**, které vám umožní spravovat širokou škálu služeb Azure s konzistentním zobrazování nastavení, akcí, fakturačních informací, monitorování stavu, dat o využití a spousty dalších věcí.
* **Možnosti přizpůsobení**, které vám umožňují vytvořit vlastní úvodní obrazovku s informacemi, které chcete, aby se zobrazily vždy, když se přihlásíte.  Můžete si také přizpůsobit kterékoli z oken obsahujících dlaždice.
  
  ![Orientace v uživatelském rozhraní webu Azure Portal][UIOrientation]

## <a name="before-you-get-started"></a>Než začnete
Abyste mohli absolvovat tento kurz, budete potřebovat platné předplatné Azure.  Pokud žádné nemáte, můžete si hned [zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).  Až budete předplatné mít, dostanete se na portál na adrese [https://portal.azure.com].

## <a name="how-to-create-a-resource"></a>Jak vytvářet prostředky
Azure má Marketplace s tisíci položkami, které můžete vytvořit z jednoho místa.  Řekněme, že chcete vytvořit nový virtuální počítač s Windows Serverem 2012.  Vašim vstupním bodem do kurátorované sady vybraných kategorií z Marketplace je centrum +NOVÝ.  Každá kategorie obsahuje menší skupinu vybraných položek spolu s odkazem na kompletní nabídku Marketplace se všemi kategoriemi a vyhledáváním. Když budete chtít vytvořit nový virtuální počítač s Windows Serverem 2012, udělejte toto:  

1. Windows Server 2012 je mezi vybranými položkami, takže ho můžete vybrat z kategorie Compute.  
2. Vyplňte ve formuláři základní údaje.
3. Klikněte na Vytvořit. Virtuální počítač se začne okamžitě zřizovat.

Jakmile se prostředek vytvoří, zobrazí se upozornění v centru oznámení a otevře se okno správy (prostředky pak můžete procházet i kdykoli později).

![Kategorie portálu][PortalCategories]

## <a name="how-to-find-your-resources"></a>Jak vyhledávat svoje prostředky
Často používané prostředky si můžete kdykoli připnout na Úvodní panel. Někdy ale můžete potřebovat najít něco, co zase tak často nepoužíváte.  Ke všem svým prostředkům se dostanete pomocí centra procházení, které vidíte na obrázku níže.  Můžete je filtrovat podle předplatného, můžete zvolit sloupce a měnit jejich velikost a kliknutím na jednotlivé položky přejít na okna pro správu.

![Centrum procházení][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Jak spravovat a delegovat přístup k prostředkům
Z tohoto okna se můžete připojit k virtuálnímu počítači pomocí vzdálené plochy, monitorovat klíčové metriky výkonu, řídit přístup k tomuto virtuálnímu počítači pomocí přístupu na základě rolí (RBAC), konfigurovat virtuální počítače a provádět další důležité úlohy správy.  Delegování přístupu na základě role je důležité pro správu ve velkém měřítku.  Kliknutím [sem](active-directory/role-based-access-control-configure.md) si o tom můžete přečíst více. Když budete chtít udělit přístup k prostředku, udělejte toto:

1. Vyhledejte prostředek.
2. Klikněte na Všechna nastavení v části Základní údaje.
3. V seznamu nastavení klikněte na Uživatelé.
4. Na panelu příkazů klikněte na Přidat.
5. Zvolte uživatele a roli.

![Správa prostředků][ManageResource]

## <a name="how-to-get-help"></a>Jak získat nápovědu
Pokud byste někdy měli s něčím nějaký problém, jsme tu pro vás.  Portál obsahuje stránku nápovědy a podpory, která vás navede správným směrem.  V závislosti na tom, jaký máte [plán podpory](https://azure.microsoft.com/support/plans/), můžete také přímo na portálu vytvářet lístky žádostí o podporu.  Po vytvoření lístku můžete spravovat lístek v celém jeho životním cyklu přímo z portálu. Na stránku nápovědy a podpory se dostanete tak, že přejdete na Procházet -> Nápověda a podpora.  

![Nápověda a podpora][HelpSupport]

## <a name="summary"></a>Souhrn
Pojďme si shrnout, co jste se v tomto kurzu naučili:

* Naučili jste se, jak se zaregistrovat, získat předplatné a procházet portál.
* Zorientovali jste se v uživatelském rozhraní portálu a zjistili jste, jak můžete vytvářet a procházet prostředky.
* Dozvěděli jste se, jak vytvářet a procházet prostředky.
* Seznámili jste se se strukturou a okny pro správu a zjistili jste, jak můžete konzistentně spravovat různé typy prostředků.
* Naučili jste se, jak si portál přizpůsobit, abyste měli na očích informace, které jsou pro vás nejdůležitější.
* Zjistili jste, jak řídit přístup k prostředkům pomocí přístupu na základě rolí (RBAC).
* Dozvěděli jste se, jak získat nápovědu a podporu.

Portál Microsoft Azure radikálně zjednodušuje vytváření a správu aplikací v cloudu.  Snažíme se [naslouchat názorům uživatelů](https://feedback.azure.com/forums/223579-azure-preview-portal/) a stále něco vylepšovat. Doporučujeme proto sledovat [blog správy](https://azure.microsoft.com/blog/topics/management/), abyste měli vždy informace o tom, co je nového.  Dalším skvělým místem, kde najdete všechny aktuality ohledně Azure, je [blog uživatele ScottGu](http://weblogs.asp.net/scottgu).

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png



<!--HONumber=Dec16_HO2-->


