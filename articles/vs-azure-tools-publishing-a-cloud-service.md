---
title: "Publikování cloudové služby pomocí nástroje Azure | Microsoft Docs"
description: "Další informace o tom, jak publikovat projekty Azure cloudových služeb pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1a07b6e4-3678-4cbf-b37e-4520b402a3d9
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/14/2017
ms.author: kraigb
ms.openlocfilehash: e617d600dbc8287eea737fc4969833e873365288
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Publikování pomocí nástroje Azure cloudové služby
Pomocí nástrojů Azure pro Microsoft Visual Studio můžete aplikaci Azure publikovat přímo z Visual Studia. Visual Studio podporuje integrované publikování do pracovního nebo produkčního prostředí cloudové služby.

Než můžete publikovat aplikaci Azure, musí mít předplatné Azure. Účet služby a úložiště cloudu potřeba také nastavit a vaše aplikace použít. Můžete nastavit tyto na [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!IMPORTANT]
> Když publikujete, můžete vybrat prostředí nasazení pro cloudovou službu. Také musíte vybrat účet úložiště, který se používá k ukládání balíčku aplikace pro nasazení. Po nasazení odeberou se z účtu úložiště balíčku aplikace.
> 
> 

Při vývoji a testování aplikací Azure, můžete použít nasazení webu k publikování změny přírůstkově pro webové role. Po publikování aplikace do prostředí nasazení, nasazení webu umožňuje nasadit změny přímo na virtuální počítač, který běží webové role. Nemáte balíčku a publikování aplikace celý Azure pokaždé, když chcete aktualizovat vaši webovou roli k otestování změny. S tímto přístupem může mít webové role změny k dispozici v cloudu pro testování bez čekání na vaše aplikace publikována do prostředí nasazení.

K publikování aplikace Azure a k aktualizaci webové role pomocí nástroje nasazení webu pomocí následujících postupů:

* Publikování nebo balíčků aplikací Azure ze sady Visual Studio
* Aktualizovat webovou roli jako součást vývoj a testování cyklu

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publikování nebo balíčků aplikací Azure ze sady Visual Studio
Když publikujete aplikaci Azure, můžete provést jednu z následujících úloh:

* Vytvořit balíček služby: Tento balíček a konfigurační soubor služby můžete použít k publikování aplikace do prostředí nasazení z [portálu Azure Classic](http://go.microsoft.com/fwlink/?LinkID=213885).
* Publikování projektu Azure ze sady Visual Studio: pro publikování aplikace přímo do Azure, můžete použít Průvodce publikování. Informace najdete v tématu [Průvodci publikováním aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Chcete-li vytvořit balíček služby ze sady Visual Studio
1. Jakmile budete připraveni k publikování aplikace, otevřete Průzkumníka řešení, otevřete místní nabídku pro Azure projekt, který obsahuje vaše role a volba možnosti publikovat.
2. Chcete-li vytvořit balíček služby pouze, postupujte takto:  
   
   1. V místní nabídce pro projektu Azure, zvolte **balíček**.
   2. V **balíčku aplikace Azure** dialogové okno, zvolte konfigurace služby, pro který chcete vytvořit balíček a potom je konfigurace sestavení.
   3. (volitelné) Chcete-li na vzdálené plochy pro cloudové služby po publikování, vyberte **povolení vzdálené plochy u všech rolí** zaškrtněte políčko a potom vyberte **nastavení** konfigurace vzdálené plochy. Pokud chcete ladit cloudové služby, jakmile ji publikujete, zapnout vzdálené ladění výběrem **povolení vzdáleného ladicího programu u všech rolí**.
      
      Další informace najdete v tématu [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).
   4. Chcete-li vytvořit balíček, klikněte **balíček** odkaz.
      
      Průzkumník souborů ukazuje umístění souboru nově vytvořený balíčku. Toto umístění můžete zkopírovat, aby ho z můžete používat [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
   5. Chcete-li publikovat tento balíček do prostředí nasazení, musíte použít toto umístění jako umístění balíčku, při vytváření cloudové služby a nasazení tohoto balíčku do prostředí s [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
3. (Volitelné) Chcete-li zrušit procesu nasazení v místní nabídce řádku položky v protokolu aktivit, zvolte **zrušit a odeberte**. To zastaví proces nasazení a odstraní prostředí nasazení z Azure.
   
   > [!NOTE]
   > Chcete-li odebrat toto nasazení prostředí poté, co byla nasazena, je nutné použít [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
   > 
   > 
4. (Volitelné) Po spuštění instance role mít Visual Studio automaticky zobrazí prostředí nasazení v **cloudové služby** uzlu v Průzkumníku serveru. Zde se zobrazí stav instance jednotlivých rolí. V tématu [Správa Azure prostředků pomocí Průzkumníka cloudu](vs-azure-tools-resources-managing-with-cloud-explorer.md). Následující obrázek znázorňuje instance rolí v době, kdy jsou stále ve stavu Probíhá inicializace:
   
    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Aktualizovat webovou roli jako součást vývoj a testování cyklu
Pokud je vaše aplikace back-end infrastruktura stabilní, ale webové role třeba více často aktualizovat, můžete použít Web Deploy aktualizovat webovou roli ve vašem projektu. To je užitečné, když nechcete znovu sestavili a nasadili rolí pracovního procesu back-end, nebo pokud máte více webové role a chcete aktualizovat pouze jeden z webové role.

### <a name="requirements"></a>Požadavky
Tady jsou požadavky na službu Web Deploy používat aktualizovat vaši webovou roli:

* **Pouze pro vývoj a testování účely:** provedení změn přímo k virtuálnímu počítači se spuštěným systémem webovou roli. Pokud tento virtuální počítač má recyklace, změny budou ztraceny, protože původní balíček, který jste publikovali se používá k opětovnému vytvoření virtuálního počítače pro roli. Je potřeba publikovat znovu získání nejnovějších změn pro roli webové aplikace.
* **Aktualizovat lze pouze webové role:** rolí pracovního procesu nelze aktualizovat. Kromě toho nelze aktualizovat RoleEntryPoint v web role.cs.
* **Podporuje pouze jednu instanci webové role:** nemůže mít víc instancí žádné webové role v prostředí pro nasazení. Víc webových rolí každý se pouze jedna instance jsou však podporovány.
* **Je nutné povolit připojení ke vzdálené ploše:** to je potřeba, aby se Web Deploy používat uživatel a heslo pro připojení k virtuálnímu počítači k nasazení změny na server, který běží Internetové informační služby (IIS). Kromě toho budete muset připojit k virtuálnímu počítači do IIS přidat důvěryhodný certifikát na tomto virtuálním počítači. (To zajistí, že bude zabezpečeného vzdáleného připojení pro službu IIS, který je používán nasazení webu.)

V následujícím postupu se předpokládá, že používáte **publikování aplikaci Azure** průvodce.

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Chcete-li povolit nasazení webu při publikování aplikace
1. Chcete-li povolit **povolit nasazení webu** pro všechny webové role zaškrtněte políčko, musíte nejdřív nakonfigurovat připojení ke vzdálené ploše. Vyberte **povolení vzdálené plochy** u všech rolí a pak zadejte přihlašovací údaje, které se použije pro připojení vzdáleně v **konfigurace vzdálené plochy** pole, které se zobrazí. V tématu [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md) Další informace.
2. Chcete-li nasazení webu pro všechny webové role v aplikaci, vyberte **povolit nasazení webu pro všechny webové role**.
   
    Žlutý trojúhelník upozornění se zobrazí. Nasazení webu používá certifikát podepsaný svým držitelem, nedůvěryhodné ve výchozím nastavení, která se nedoporučuje pro nahrávání citlivá data. Pokud potřebujete zabezpečit tento proces pro citlivá data, můžete přidat certifikát SSL, který se má použít pro nasazení webu připojení. Tento certifikát musí být důvěryhodný certifikát. Informace o tom, jak to udělat, najdete v části **k zkontrolujte webové nasazení zabezpečit** dál v tomto tématu.
3. Zvolte **Další** zobrazíte **Souhrn** obrazovce a potom vyberte **publikovat** nasazení cloudové služby.
   
    Cloudové služby je publikován. Virtuální počítač, který je vytvořen má povolené pro službu IIS tak, aby nasazení webu, můžete použít k aktualizaci webové role znovu publikovat je vzdálená připojení.
   
   > [!NOTE]
   > Pokud máte více než jednu instanci nakonfigurována pro webovou roli, zobrazí se zpráva upozornění oznamující, že každý webovou roli budou omezený na jednu instanci pouze v balíčku, který se vytvoří pro publikování aplikace. Vyberte **OK** pokračujte. Jak je uvedeno v části požadavky, můžete mít více než jednu webovou roli, ale jenom jednu instanci každé role.
   > 
   > 

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Chcete-li aktualizovat vaši webovou roli pomocí nasazení webu
1. Službu Web Deploy používat, provádět změny kódu do projektu pro některý z vaší webové role v sadě Visual Studio, který chcete publikovat a potom klikněte pravým tlačítkem na tento uzel projektu ve vašem řešení a přejděte na příkaz **publikovat**. **Publikovat Web** zobrazí se dialogové okno.
2. (Volitelné) Pokud jste přidali důvěryhodný certifikát SSL sloužící k připojení ke vzdálené pro službu IIS, můžete zrušit **Povolit nedůvěryhodný certifikát** zaškrtávací políčko. Informace o tom, jak přidat certifikát do nasazení webu zabezpečit, najdete v části **k zkontrolujte webové nasazení zabezpečené** dál v tomto tématu.
3. Službu Web Deploy používat, musí tento mechanismus publikování Uživatelské jméno a heslo, které jste nastavili pro vaše připojení k vzdálené ploše při prvním publikování balíčku.
   
   1. V **uživatelské jméno**, zadejte uživatelské jméno.
   2. V **heslo**, zadejte heslo.
   3. (Volitelné) Pokud chcete uložit toto heslo v tomto profilu, zvolte **uložit heslo**.
4. Chcete-li publikovat změny na vaši webovou roli, zvolte **publikovat**.
   
    Ve stavovém řádku zobrazí **publikovat spuštění**. Po dokončení publikování **publikování bylo úspěšné** se zobrazí. Změny jsou nyní nasazené webovou roli na virtuálním počítači. Nyní můžete spustit aplikaci Azure v prostředí Azure k testování změny.

### <a name="to-make-web-deploy-secure"></a>Zabezpečit webové nasazení
1. Nasazení webu používá certifikát podepsaný svým držitelem, nedůvěryhodné ve výchozím nastavení, která se nedoporučuje pro nahrávání citlivá data. Pokud potřebujete zabezpečit tento proces pro citlivá data, můžete přidat certifikát SSL, který se má použít pro nasazení webu připojení. Tento certifikát musí být důvěryhodný certifikát, který můžete získat od certifikační autority (CA).
   
    Zabezpečit nasazení webu pro každý virtuální počítač pro jednotlivé webové role, musíte nahrát důvěryhodný certifikát, který chcete použít pro web nasadit [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). Tím je zajištěno, že je certifikát přidat k virtuálnímu počítači, který je vytvořen pro webovou roli při publikování aplikace.
2. Pokud chcete přidat do služba IIS používat pro vzdálená připojení důvěryhodná certifikát SSL, postupujte takto:
   
   1. Pro připojení k virtuálnímu počítači, na kterém běží role webové, vyberte instanci webové role v **Průzkumník cloudu** nebo **Průzkumníka serveru**a potom vyberte **připojit pomocí vzdálené plochy**  příkaz. Podrobné pokyny o tom, jak připojit k virtuálnímu počítači, najdete v části [pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).
      
      Váš prohlížeč vás vyzve ke stažení. Soubor RDP.
   2. Chcete-li přidat certifikát SSL, otevřete službu správy ve Správci služby IIS. Ve Správci služby IIS povolit protokol SSL otevřením **vazby** odkaz **akce** podokně. **Přidat vazbu webu** zobrazí se dialogové okno. Zvolte **přidat**a potom vyberte HTTPS v **typ** rozevíracího seznamu. V **certifikát SSL** vyberte certifikát SSL, který měl podepsaný Certifikační autoritou a že jste nahráli do [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). Další informace najdete v tématu [konfigurace nastavení připojení pro službu správy](http://go.microsoft.com/fwlink/?LinkId=215824).
      
      > [!NOTE]
      > Pokud chcete přidat důvěryhodný certifikát SSL, žlutý trojúhelník upozornění se již v **Průvodci publikováním**.
      > 
      > 

## <a name="include-files-in-the-service-package"></a>Vložené soubory v balíčku služby
Možná budete muset zahrnout konkrétní soubory do balíčku služby, aby byly k dispozici na virtuálním počítači, který se vytvoří pro roli. Můžete například chtít přidat .exe nebo soubor .msi, který je používán spouštěcí skript do vašeho balíčku služby. Nebo možná budete muset přidat sestavení, které vyžaduje projekt webové role nebo pracovní role. Vložené soubory je nutné je přidat do řešení pro aplikaci Azure.

### <a name="to-include-files-in-the-service-package"></a>Chcete-li zahrnout soubory v balíčku služby
1. K přidání sestavení do balíčku služby, použijte následující kroky:
   
   1. V **Průzkumníku řešení** otevřete uzel projektu pro projekt, který chybí odkazované sestavení.
   2. Chcete-li do projektu přidejte sestavení, otevřete místní nabídku pro **odkazy** složku a potom vyberte **přidat odkaz na**. Otevře se dialogové okno Přidat odkaz.
   3. Vyberte odkaz, na který chcete přidat a pak zvolte **OK** tlačítko.
      
      Odkaz je přidán do seznamu v části **odkazy** složky.
   4. Otevřete místní nabídku pro sestavení, které jste přidali a zvolte **vlastnosti**. **Vlastnosti** se zobrazí v okně.
      
      Zahrnout toto sestavení v balíčku služby, **kopie místního seznamu** zvolte **True**.
2. V **Průzkumníku řešení** otevřete uzel projektu pro projekt, který chybí odkazované sestavení.
3. Chcete-li do projektu přidejte sestavení, otevřete místní nabídku pro **odkazy** složku a potom vyberte **přidat odkaz na**. **Přidat odkaz na** otevře se dialogové okno.
4. Vyberte odkaz, na který chcete přidat a pak zvolte **OK** tlačítko.
   
    Odkaz je přidán do seznamu v části **odkazy** složky.
5. Otevřete místní nabídku pro sestavení, které jste přidali a zvolte **vlastnosti**. Zobrazí se okno Vlastnosti.
6. Zahrnout toto sestavení v balíčku služby, **místní kopie** vyberte **True**.
7. Chcete-li zahrnout soubory v balíčku služby, které byly přidány do projektu webové role, otevřete místní nabídky souboru a poté zvolte **vlastnosti**. Z **vlastnosti** okně zvolte **obsahu** z **akce sestavení** pole se seznamem.
8. Chcete-li zahrnout soubory v balíčku služby, které byly přidány do projektu role pracovního procesu, otevřete místní nabídky souboru a poté zvolte **vlastnosti**. Z **vlastnosti** okně zvolte **kopírovat, pokud je novější** z **kopírovat do výstupního adresáře** pole se seznamem.

## <a name="next-steps"></a>Další kroky
Další informace o publikování do Azure ze sady Visual Studio najdete v tématu [Průvodci publikováním aplikace Azure](vs-azure-tools-publish-azure-application-wizard.md).

