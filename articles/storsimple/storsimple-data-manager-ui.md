---
title: "Uživatelské rozhraní aplikace Microsoft Azure StorSimple Manager dat | Microsoft Docs"
description: "Popisuje způsob použití služby StorSimple Data Manager uživatelského rozhraní (soukromém náhledu)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a>Spravovat pomocí služby StorSimple Data Manager uživatelského rozhraní (soukromém náhledu).

Tento článek vysvětluje, jak můžete pomocí uživatelského rozhraní StorSimple Manager dat k transformaci dat na data uložená na řadu zařízení StorSimple 8000. Transformovaná data mohou být spotřebovávána pak jinými službami Azure, například Azure Media Services, Azure HDInsight, Azure Machine Learning a Azure Search. 


## <a name="use-storsimple-data-transformation"></a>Transformace dat StorSimple použít

Data Manager zařízení StorSimple je prostředků, ve kterém transformaci dat se dá vytvořit instance. Transformace dat service umožňuje přesun dat z vašeho místního zařízení StorSimple na objekty BLOB v úložišti Azure. Proto v pracovním postupu budete muset zadat podrobnosti o zařízení StorSimple a data zájmu, které chcete přesunout k účtu úložiště.

### <a name="create-a-storsimple-data-manager-service"></a>Vytvoření služby StorSimple Manager dat

Proveďte následující kroky k vytvoření služby StorSimple Data Manager.

1. Chcete-li vytvořit službu StorSimple Manager dat, přejděte na [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)

2. Klikněte  **+**  ikonu a vyhledávání pro StorSimple Data Manager. Klikněte na tlačítko služby StorSimple Manager dat a pak klikněte na **vytvořit**.

3. Pokud je vaše předplatné povolená pro vytvoření této služby, zobrazí se následující okno.

    ![Vytvořte prostředek StorSimple Data správců](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Zadejte vstupy a klikněte na **vytvořit**. Zadané umístění musí být ten, který je umístěno účty úložiště a služby StorSimple Manager. V současné době jsou podporovány pouze oblasti západní USA a západní Evropa. Proto služby StorSimple Manager, service Data Manager a přidruženého účtu úložiště by měl být v předchozím podporovaných oblastí. Vytvořte službu trvá několik minut.

### <a name="create-a-data-transformation-job-definition"></a>Vytvoření definice úlohy transformace dat

V rámci služby StorSimple Manager dat budete muset vytvořit definici úlohy transformace data. Definice úlohy Určuje podrobné informace o datech, která vás zajímá Přesun do účtu úložiště v nativním formátu. 

Proveďte následující kroky k vytvoření nové definice úlohy transformace dat.

1.  Přejděte ke službě, kterou jste vytvořili. Klikněte na tlačítko **+ úlohy definice**.

    ![Klikněte na tlačítko + definice úlohy](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. Otevře nové okno Definice úlohy. Pojmenujte svou definici úlohy a klikněte na tlačítko **zdroj**. V **zdroj dat konfigurace** okno, zadejte podrobnosti o zařízení StorSimple a požadovaná data.

    ![Vytvořit definici úlohy](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Vzhledem k tomu, že toto je nová služba Data Manager, jsou nakonfigurovány žádné datové úložiště. Chcete-li přidat StorSimple Manager jako úložiště dat, klikněte na tlačítko **přidat nový** rozevírací úložiště dat a pak klikněte na **úložiště dat přidat**.

4. Zvolte **řady StorSimple 8000 Manager** jako úložiště zadejte a zadejte vlastnosti vaší **StorSimple Manager**. Pro **Id prostředku** pole, je nutné zadat číslo před **:** v registračního klíče služby StorSimple manager.

    ![Vytvoření zdroje dat](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Klikněte na tlačítko **OK** po dokončení. To umožňuje ušetřit úložiště dat a tato StorSimple Manager lze opětovně použít v jiné definice úlohy bez opětovného zadávání tyto parametry. Jak dlouho trvá několik sekund, po kliknutí na tlačítko **OK** pro StorSimple Manager objeví v rozevírací nabídce.

6.  V **zdroj dat konfigurace** okno, zadejte název zařízení a název svazku, která obsahuje vaše data týkající se.

7.  V **filtru** pododdílu, zadejte kořenový adresář, který obsahuje vaše data týkající se (v tomto poli by měla začínat znakem `\`). Můžete také přidat všechny souboru filtry.

8.  Služba transformaci dat funguje na data, která vložena do Azure pomocí snímků. Při spuštění této úlohy je možné provést zálohu při každém spuštění této úlohy (pro práci na nejnovější data), nebo chcete použít poslední existující zálohy v cloudu (Pokud pracujete na některé Archivovaná data).

    ![Podrobnosti nové zdroje dat](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. V dalším kroku nastavení Target je potřeba nakonfigurovat ji tak. Existují 2 typy podporované cíle – účty Azure Storage a účty služby Azure Media Services. Vyberte účty úložiště pro soubory umístit do objektů BLOB v daném účtu. Zvolte media services účet ukládat soubory do prostředky v daném účtu. Znovu musíme přidejte úložiště. V rozevírací nabídce, a vyberte **přidat nový** a potom **nakonfigurovat nastavení**.

    ![Vytvoření jímku dat](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. Tady můžete vybrat typ úložiště, které chcete přidat a ostatní parametry související s úložištěm. V obou případech se vytvoří frontu úložiště spuštění úlohy. Tato fronta je naplňována zprávami o transformovaných objektech blob, jakmile jsou připravené. Název této fronty je stejný jako název definice úlohy. Pokud vyberete **Media Services** jako typ úložiště, pak můžete také zadat přihlašovací údaje účtu úložiště kde je vytvářena fronta.

    ![Nová data jímky podrobnosti](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. Po přidání úložiště dat (která má několik sekund), bude najít v rozevírací nabídce v úložišti **název cílového účtu**.  Vyberte cíl, který potřebujete.

12. Klikněte na tlačítko **OK** k vytvoření definice úlohy. Vaše definice úlohy je teď nastavený. Můžete použít tuto definici úlohy několikrát prostřednictvím uživatelského rozhraní.

    ![Přidat nové definice úlohy](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a>Spustit definici úlohy

Kdykoli budete potřebovat pro přesun dat z StorSimple k účtu úložiště, který jste zadali v definici úlohy, musíte ji volat. Není určitou volnost v každém vyvolání úlohy Změna parametrů. Kroky jsou následující:

1. Vybrat služby StorSimple Manager dat, přejděte na **monitorování**. Klikněte na tlačítko **spustit nyní**.

    ![Definice úlohy aktivační události](./media/storsimple-data-manager-ui/run-now.png)

2. Zvolte definici úlohy, který chcete spustit. Klikněte na tlačítko **spustit nastavení** změnit nastavení, které můžete chtít změnit pro tuto úlohu spustit.

    ![Nastavení úloh spustit](./media/storsimple-data-manager-ui/run-settings.png)

3. Klikněte na tlačítko **OK** a pak klikněte na **spustit** spustit úlohu. Ke sledování této úlohy, přejděte na **úlohy** stránky vaše Data Manager zařízení StorSimple.

    ![Seznam úloh a stav](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. Kromě monitorování v **úlohy** okno, můžete také poslouchat na fronty úložiště, kde se zpráva přidá pokaždé, když se soubor přesune ze zařízení StorSimple účet úložiště.


## <a name="next-steps"></a>Další kroky

[Spuštění úlohy StorSimple Manager dat pomocí .NET SDK](storsimple-data-manager-dotnet-jobs.md).