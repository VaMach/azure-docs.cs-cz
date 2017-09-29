---
title: "Adaptivní řízení aplikací v Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže použít adaptivní řízení aplikací v Azure Security Center k přidávání aplikací spuštěných na virtuálních počítačích Azure na seznam povolených."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 890acae2aebf7684e567b9b49377ca7b6da95245
ms.openlocfilehash: 9c3a9a7255bbbdab8f4c356eb07022d7f1d242d7
ms.contentlocale: cs-cz
ms.lasthandoff: 09/20/2017

---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Adaptivní řízení aplikací v Azure Security Center (Preview)
Pomocí tohoto názorného postupu zjistíte, jak nakonfigurovat řízení aplikací v Azure Security Center.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Co je adaptivní řízení aplikací ve službě Security Center?
Adaptivní řízení aplikací pomáhá řídit, které aplikace se můžou spouštět na vašich virtuálních počítačích v Azure, což kromě dalších výhod posiluje ochranu virtuálních počítačů před malwarem. Služba Security Center pomocí strojového učení analyzuje procesy spuštěné na virtuálním počítači a pomáhá s aplikováním pravidel přidávání na seznam povolených na základě těchto informací. Tato funkce výrazně zjednodušuje proces konfigurace a správy seznamů povolených aplikací a umožňuje:

- Blokovat nebo upozorňovat na pokusy o spuštění škodlivých aplikací, a to včetně aplikací, které by antimalwarová řešení jinak mohla vynechat.
- Dodržovat zásady zabezpečení vaší organizace, které vyžadují používání pouze licencovaného softwaru.
- Zamezit používání nežádoucího softwaru ve vašem prostředí.
- Zamezit spouštění starých a nepodporovaných aplikací.
- Zabránit používání konkrétních softwarových nástrojů, které nejsou ve vaší organizaci povolené.
- Umožnit IT oddělení řídit přístup k citlivým datům prostřednictvím používání aplikace.

> [!NOTE]
> Adaptivní řízení aplikací je dostupné jako omezená verze Public Preview pro zákazníky využívající Azure Security Center Standard. Pokud se chcete připojit k verzi Preview, zašlete [nám](mailto:ASC_appcontrol@microsoft.com) e-mail s ID vašich předplatných.

## <a name="how-to-enable-adaptive-application-controls"></a>Jak povolit adaptivní řízení aplikací?
Adaptivní řízení aplikací pomáhá definovat sadu aplikací, které mají povoleno spouštění v nakonfigurovaných skupinách prostředků. Tato funkce je dostupná pouze pro počítače s Windows (všechny verze, Classic nebo Azure Resource Manager). Pokud chcete ve službě Security Center nakonfigurovat přidávání aplikací na seznam povolených, postupujte podle následujících kroků:

1.  Otevřete řídicí panel **Security Center** a klikněte na **Přehled**.
2.  V části **Pokročilá obrana cloudu** se na dlaždici **Adaptivní řízení aplikací** zobrazí počet už kontrolovaných virtuálních počítačů v porovnání se všemi virtuálními počítači. Zobrazuje také počet nalezených problémů za poslední týden: 

    ![Adaptivní řízení aplikací](./media/security-center-adaptive-application\security-center-adaptive-application-fig1.png)

3. Pokud chcete zobrazit další možnosti, klikněte na dlaždici **Adaptivní řízení aplikací**.

    ![ovládací prvky](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

4. Část Skupiny prostředků obsahuje tři karty:
    * **Doporučené:** Seznam skupin prostředků, pro které se doporučuje řízení aplikací. Služba Security Center pomocí strojového učení identifikuje virtuální počítače, které jsou vhodnými kandidáty pro řízení aplikací, na základě toho, jestli virtuální počítače spouštějí stále stejné aplikace.
    * **Nakonfigurované:** Seznam skupin prostředků obsahujících virtuální počítače s nakonfigurovaným řízením aplikací. 
    * **Žádné doporučení:** Seznam skupin prostředků obsahujících virtuální počítače bez jakýchkoli doporučených řízení aplikací. Například virtuální počítače, na kterých se neustále mění aplikace a které se ještě nedostaly do stabilního stavu.

Následující části se budou podrobněji zabývat jednotlivými možnostmi a jejich použitím.

### <a name="configure-a-new-application-control-policy"></a>Konfigurace nové zásady řízení aplikací
Kliknutím na kartu **Doporučené** zobrazte seznam skupin prostředků s doporučenými řízeními aplikací:

![Doporučené](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

Seznam obsahuje:
- **NÁZEV:** Název předplatného a skupiny prostředků.
- **VIRTUÁLNÍ POČÍTAČE:** Počet virtuálních počítačů ve skupině prostředků.
- **STAV:** Stav doporučení, který ve většině případů bude otevřený.
- **ZÁVAŽNOST:** Úroveň závažnosti doporučení.

Výběrem skupiny prostředků otevřete možnost **Vytvořit pravidla řízení aplikací**:

![Pravidla řízení aplikací](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

V části **Vybrat virtuální počítače** zkontrolujte seznam doporučených virtuálních počítačů a zrušte zaškrtnutí těch, na které nechcete použít řízení aplikací. V části **Vybrat procesy pro pravidla přidávání na seznam povolených** zkontrolujte seznam doporučených aplikací a zrušte zaškrtnutí těch, které nechcete použít. Seznam obsahuje:

- **NÁZEV:** Úplná cesta aplikace.
- **PROCESY:** Počet aplikací, které se nacházejí v jednotlivých cestách.
- **BĚŽNÉ:** Hodnota true značí, že se tyto procesy spustily na většině virtuálních počítačů v této skupině prostředků.
- **ZNEUŽITELNÉ:** Ikona upozornění bude značit, jestli by útočník mohl aplikace využít k obejití seznamu povolených aplikací. Důrazně se doporučuje takové aplikace před schválením zkontrolovat. 

Jakmile budete s výběry hotovi, klikněte na tlačítko **Vytvořit**. Služba Security Center ve výchozím nastavení vždy povoluje řízení aplikací v režimu *Audit*. Až zkontrolujete, že seznam povolených aplikací nemá žádný nepříznivý vliv na vaši sadu funkcí, můžete režim změnit na *Vynucení*.

> [!NOTE]
> Jako osvědčený postup zabezpečení se služba Security Center vždy pokusí pro aplikace, které by se měly přidat na seznam povolených, vytvořit pravidlo vydavatele, a pouze v případě, že aplikace neobsahuje informace o vydavateli (tj. není podepsaná), se vytvoří pravidlo cesty pro úplnou cestu konkrétního souboru EXE.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Úpravy a monitorování skupiny s nakonfigurovaným řízením aplikací

Pokud chcete upravit nebo konfigurovat skupinu s nakonfigurovaným řízením aplikací, klikněte na **NAKONFIGUROVANÉ** v části **Skupiny prostředků**:

![Skupiny prostředků](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

Seznam obsahuje:

- **NÁZEV:** Název předplatného a skupiny prostředků.
- **VIRTUÁLNÍ POČÍTAČE:** Počet virtuálních počítačů ve skupině prostředků.
- **REŽIM:** V režimu Audit se budou protokolovat pokusy o spuštění aplikací, které nejsou na seznamu povolených. Režim Blokování neumožní spouštění aplikací, které nejsou na seznamu povolených.
- **ZÁVAŽNOST:** Úroveň závažnosti doporučení.

Vyberte skupinu prostředků, abyste mohli provést změny na stránce **Upravit zásadu řízení aplikací**.

![Ochrana](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

V části **Režim ochrany** máte na výběr mezi následujícími možnostmi:
- **Audit:** V tomto režimu nebude řešení řízení aplikací vynucovat pravidla a bude pouze auditovat aktivitu na chráněných virtuálních počítačích. Tato možnost se doporučuje pro scénáře, kdy chcete nejprve sledovat celkové chování před tím, než zablokujete spouštění aplikace na cílovém virtuálním počítači.
- **Vynucení:** V tomto režimu bude řešení řízení aplikací vynucovat pravidla a zajistí zablokování aplikací, které nemají povoleno spouštění. 

Jak už jsme zmínili, ve výchozím nastavení se nová zásada řízení aplikací vždy nakonfiguruje v režimu *Audit*. V části **Rozšíření zásady** můžete přidat vlastní cesty aplikací, které chcete přidat na seznam povolených. Po přidání těchto cest služba Security Center pro tyto aplikace vytvoří patřičná pravidla a přidá je k již existujícím pravidlům. V části **Problémy** jsou uvedena všechna aktuální porušení pravidel.

![Problémy](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

Tento seznam obsahuje:

- **PROBLÉMY:** Všechna zaprotokolovaná porušení pravidel, která můžou zahrnovat následující:
    - **Zablokovaná porušení:** Když je řešení spuštěné v režimu Vynucení a o spuštění se pokusí aplikace, která není na seznamu povolených.
    - **Auditovaná porušení:** Když je řešení spuštěné v režimu Audit a spustí se aplikace, která není na seznamu povolených.
    - **Ručně porušená pravidla:** Když se uživatel pokusil ručně konfigurovat pravidla na virtuálním počítači, a ne přes portál pro správu ASC.
- **POČET VIRTUÁLNÍCH POČÍTAČŮ:** Počet virtuálních počítačů s tímto typem problému.

Pokud kliknete na jakýkoli z těchto řádků, budete přesměrování na stránku [Protokol aktivit Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), kde se zobrazí informace o všech virtuálních počítačích s daným typem porušení pravidel. Pokud kliknete na tři tečky na konci každého řádku, budete moct příslušnou položku odstranit. V části **Nakonfigurované virtuální počítače** jsou uvedené virtuální počítače, na které se tato pravidla vztahují. 

![Nakonfigurované virtuální počítače](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

**Pravidla přidávání vydavatele na seznam povolených** uvádí aplikace, pro které se vytvořilo pravidlo vydavatele na základě informací o certifikátu nalezeného pro každou aplikaci. Další informace najdete v tématu [Principy pravidel vydavatele v AppLockeru](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker).

![Pravidla přidávání na seznam povolených](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

Pokud kliknete na tři tečky na konci každého řádku, budete moct příslušné pravidlo odstranit. **Pravidla přidávání cest na seznam povolených** uvádí úplnou cestu aplikace (včetně spustitelného souboru) pro aplikace, které nejsou podepsané digitálním certifikátem, ale stále jsou aktuální v pravidlech přidávání na seznam povolených. 

> [!NOTE]
> Jako osvědčený postup zabezpečení se služba Security Center ve výchozím nastavení vždy pokusí pro soubory EXE, které by se měly přidat na seznam povolených, vytvořit pravidlo vydavatele, a pouze v případě, že soubor EXE neobsahuje informace o vydavateli (tj. není podepsaný), se vytvoří pravidlo cesty pro úplnou cestu konkrétního souboru EXE.

![Pravidla přidávání cest na seznam povolených](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

Seznam obsahuje:
- **NÁZEV:** Úplná cesta spustitelného souboru.
- **ZNEUŽITELNÉ:** Hodnota true značí, že by útočník mohl aplikaci využít k obejití seznamu povolených aplikací.  

Pokud kliknete na tři tečky na konci každého řádku, budete moct příslušné pravidlo odstranit. Po provedení změn můžete kliknout na tlačítko **Uložit**. Pokud se však rozhodnete změny nepoužít, klikněte na **Zahodit**.

### <a name="not-recommended-list"></a>Seznam nedoporučených

Služba Security Center doporučí přidávání aplikací na seznam povolených pouze pro virtuální počítače, na kterých se spouští stabilní sada aplikací. Pokud se aplikace na přidružených virtuálních počítačích neustále mění, doporučení se nevytvoří. 

![Doporučení](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Seznam obsahuje:
- **NÁZEV:** Název předplatného a skupiny prostředků.
- **VIRTUÁLNÍ POČÍTAČE:** Počet virtuálních počítačů ve skupině prostředků.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak použít adaptivní řízení aplikací v Azure Security Center k přidávání aplikací spuštěných na virtuálních počítačích Azure na seznam povolených aplikací. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Zjistěte, jak spravovat výstrahy a reagovat na incidenty zabezpečení ve službě Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Principy výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Seznamte se s dalšími typy výstrah zabezpečení.
* [Průvodce odstraňováním potíží pro službu Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Zjistěte, jak řešit běžné problémy ve službě Security Center. 
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.


