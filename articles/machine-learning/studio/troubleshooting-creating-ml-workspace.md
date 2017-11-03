---
title: "Řešení potíží: Vytvořte a připojte se k pracovní prostor Machine Learning | Microsoft Docs"
description: "Řešení pro běžné problémy při vytvoření a připojení k pracovní prostor služby Azure Machine Learning"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: cdccd4ce7b87f47d21578076653bde901748188b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Průvodce odstraňováním potíží: Vytvoření pracovního prostoru Machine Learning a připojení k němu
Tato příručka poskytuje řešení pro některé nejčastější problémy při se nastavování pracovních prostorů Azure Machine Learning.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Vlastník pracovní prostor
Otevřete pracovní prostor v nástroji Machine Learning Studio, musíte být přihlášeni k Microsoft Account jste použili k vytvoření pracovního prostoru, nebo je potřeba přijmout pozvánku od vlastníka pro připojení k pracovním prostoru. Z portálu Azure můžete spravovat pracovní prostor, který zahrnuje možnost konfigurace přístupu.

Další informace o pracovním prostoru Správa, najdete v části [spravovat pracovní prostor služby Azure Machine Learning].

[spravovat pracovní prostor služby Azure Machine Learning]: manage-workspace.md

## <a name="allowed-regions"></a>Povolených oblastí
Machine Learning je aktuálně k dispozici pro omezený počet oblastí. Pokud vaše předplatné neobsahuje jednu z těchto oblastí, mohou se zobrazit chybová zpráva, "V povolených oblastí používat žádná předplatná."

Chcete-li požádat o, oblast přidat do vašeho předplatného, z portálu Azure vytvořit novou žádost o podporu společnosti Microsoft, zvolte **fakturace** jako typ problému a postupujte podle pokynů a odešlete žádost.

## <a name="storage-account"></a>Účet úložiště
Služba Machine Learning potřebuje účet úložiště pro ukládání dat. Můžete použít existující účet úložiště, nebo můžete vytvořit nový účet úložiště, když vytvoříte nový pracovní prostor Machine Learning (Pokud máte kvóty pro vytvoření nového účtu úložiště).

Po vytvoření nového pracovního prostoru Machine Learning se můžete přihlásit Machine Learning Studio pomocí účtu Microsoft, který jste použili k vytvoření pracovního prostoru. Pokud narazíte na chybová zpráva "Prostoru nebyl nalezen" (podobně jako na následujícím snímku obrazovky), použijte následující postup odstranit soubory cookie prohlížeče.

![Pracovní prostor nebyl nalezen][screen3]

**Chcete-li odstranit soubory cookie prohlížeče**

1. Pokud používáte Internet Explorer, klikněte **nástroje** tlačítko v pravém horním rohu a vyberte **Možnosti Internetu**.  

![Možnosti Internetu][screen4]

2. V části **Obecné** , klikněte na **odstranit...**

![Karta Obecné][screen5]

3. V **odstranit historii procházení** dialogové okno zkontrolujte, zda **soubory cookie a data webové stránky** je vybrána a klikněte na tlačítko **odstranit**.

![Odstraňte soubory cookie][screen6]

Po odstranění souborů cookie se znovu spustit prohlížeč a přejděte ke [Microsoft Azure Machine Learning](https://studio.azureml.net) stránky. Po zobrazení výzvy k zadání uživatelského jména a hesla, zadejte stejný účet Microsoft, které jste použili k vytvoření pracovního prostoru.

## <a name="comments"></a>Komentáře

Naším cílem je zajistit nejblíže jako bezproblémové prostředí Machine Learning. Položit všechny komentáře a problémy v [fórum Azure Machine Learning](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) a pomoci tak můžete poskytovat lepší.

[screen1]:media/troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/troubleshooting-creating-ml-workspace/screen6.png
