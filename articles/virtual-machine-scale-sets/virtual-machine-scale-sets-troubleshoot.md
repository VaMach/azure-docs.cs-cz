---
title: "Řešení potíží s škálování s sady škálování virtuálního počítače | Microsoft Docs"
description: "Řešení potíží s škálování s sady škálování virtuálního počítače. Pochopení typické problémy vzniklé a způsob jejich řešení."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: guybo
ms.openlocfilehash: bd45a0fb99a77851aa7b91d23bd4b830b6f5cc7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Řešení potíží s škálování s sady škálování virtuálního počítače
**Problém** – jste vytvořili automatické škálování infrastruktury v správce Azure Resource Manager pomocí sady škálování virtuálního počítače – například nasazením šablonu takto: https://github.com/Azure/azure-quickstart-templates/tree/master/201- vmss bottle škálování – máte definované pravidel škálování a vyhovující postup, s tím rozdílem, že bez ohledu na to, kolik zatížení vložíte na virtuálních počítačích, se nebude škálování.

## <a name="troubleshooting-steps"></a>Řešení potíží
Některé věci vzít v úvahu, patří:

* Kolik jader každý virtuální počítač nemá a jsou načítání každý jádra?
  Příklad šablony Azure Quickstart výše má do_work.php skript, který načte jediného jádra. Pokud používáte více než jedním jádrem velikost virtuálního počítače jako Standard_A1 nebo D1 virtuálního počítače je nutné ke spuštění této zatížení vícekrát. Zkontrolujte, kolik virtuálních počítačů cores kontrolou [velikosti pro systém Windows virtuálních počítačů v Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Tom, kolik virtuálních počítačů v sadě virtuálních počítačů škálování, je pracujeme na každé z nich?
  
    Horizontální navýšení kapacity událostí bude trvat jenom při umístit průměrné využití procesoru mezi **všechny** virtuální počítače ve škálovací sadě překračuje prahovou hodnotu, časem interní definované v pravidlech automatického škálování.
* Vynechali jste žádné události škálování?
  
    Zkontrolujte, zda že protokolů auditu na portálu Azure pro škálování události. Možná se škálování nahoru a nebyla provedena vertikálně. Můžete filtrovat podle "Škálování"...
  
    ![Protokoly auditu][audit]
* Jsou vaše škálování in a škálování prahové hodnoty dostatečně neliší?
  
    Předpokládejme, že nastavíte pravidlo pro horizontální rozšíření kapacity, když průměrné využití procesoru je větší než 50 % za 5 minut a možnost škálování v Pokud průměrné využití procesoru je menší než 50 %. To by způsobilo "flapping" problém, při využití procesoru je blízko této prahové hodnoty s akcí škálování neustále zvýšení a snížení velikosti sady. Z toho důvodu škálování služby se pokusí zabránit "netřepotá", který můžete manifest jako není škálování. Proto zkontrolujte, zda že se Škálováním na více systémů a škálování v prahové hodnoty dostatečně neliší umožňující nějaké místo mezi škálování.
* Můžete psát vlastní šablonu JSON?
  
    Je snadné uděláte chyby, takže spuštění pomocí šablony jako ten, nad kterou je ověřené k práci a ujistěte se, malé přírůstkové změny. 
* Můžete je ručně škálovat, příchozí nebo odchozí?
  
    Zkuste to znovu nasazení prostředků Škálovací sady virtuálního počítače s nastavením různých "kapacitou" ručně změnit počet virtuálních počítačů. Šablonu příklad k tomu je tady: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – budete muset upravit šablonu a ujistit se, že má stejnou velikost počítače jako sady škálování používá. Pokud počet virtuálních počítačů můžete úspěšně ručně změnit, pak víte, že problém je izolovaný chcete používat automatické škálování.
* Zkontrolujte vaše Microsoft.Compute/virtualMachineScaleSet a Microsoft.Insights prostředky v [Průzkumníka prostředků Azure](https://resources.azure.com/)
  
    To je nezbytné nástroj řešení potíží se zobrazí stav svých prostředků Azure Resource Manager. Klikněte na vaše předplatné a podívejte se na skupinu prostředků, řešení potíží. V rámci zprostředkovatele prostředků výpočetního podívejte se na sady škálování virtuálního počítače jste vytvořili a zkontrolujte zobrazení Instance, která ukazuje stav nasazení. Také zkontrolujte zobrazení instance virtuální počítače ve Škálovací sadě virtuálních počítačů. Pak přejděte do zprostředkovatele prostředků Microsoft.Insights a zkontrolujte, zda že pravidla škálování vypadat dobře.
* Je rozšíření diagnostiky práce a generování údaje o výkonu?
  
    **Aktualizace:** Azure automatického škálování je vylepšená tak, aby pomocí kanálu hostitele na základě metriky, které už vyžaduje rozšíření diagnostiky k instalaci. To znamená další, které několik odstavců přestanou platit, pokud vytvoříte automatické škálování aplikace pomocí nový kanál. Příklad šablony Azure, které byly převedeny na použití kanálu hostitele je: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Použití hostitele na základě metriky pro škálování je lepší z následujících důvodů:
  
  * Méně přesunutí části jako žádná rozšíření diagnostiky je potřeba nainstalovat.
  * Jednodušší šablony. Stačí přidáte pravidla škálování Statistika do stávající šablony sady škálování.
  * Vytváření sestav spolehlivější a rychlejší spouštění nové virtuální počítače.
    
    Pouze z důvodů, které že můžete dál používat příponu diagnostiky by, pokud je třeba paměť diagnostiky reporting/škálování. Metriky hostitele na základě neoznamuje paměti.
    
    Si uvědomit pouze podle zbývající části tohoto článku Pokud stále používáte rozšíření diagnostiky pro vaše automatické škálování.
    
    Škálování ve službě Správce prostředků Azure můžete pracovat (ale už má k) prostřednictvím virtuálního počítače rozšíření nazývá rozšíření diagnostiky. Ho vysílá údaje o výkonu pro účet úložiště, který definujete v šabloně. Tato data se shromažďují potom pomocí služby Azure monitorování.
    
    Pokud služba statistika nelze číst data z virtuálních počítačů, by měla odeslat e-mailu – například pokud virtuální počítače byly dolů, proto zkontrolujte e-mailu (ten, který jste zadali při vytváření účtu Azure).
    
    Můžete také přejít a prohlížet data. Podívejte se na účtu úložiště Azure pomocí Průzkumníku cloudu. Například při použití [cloudu Průzkumníka Visual Studio](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), přihlaste se a vyberte předplatné Azure, které používáte a název účtu úložiště diagnostiky odkazuje v definici rozšíření diagnostiky v šabloně nasazení...
    
    ![Průzkumník cloudu][explorer]
    
    Zde se zobrazí ve svazku tabulek, které ukládají data z jednotlivých virtuálních počítačů. Převádět Linux a metriku procesoru jako příklad, podívejte se na poslední řádky. Průzkumník cloudu Visual Studio podporuje dotazovací jazyk, takže je možné spustit dotaz jako "časové razítko gt data a času: 2016-02-02T21:20:00Z'" a ujistěte se, získat nejnovější události (považovat za čas v UTC). Podporuje dat, které vidíte, že existuje odpovídají pravidlům škálování při nastavování? V následujícím příkladu procesoru pro počítač 20 spuštění, zvýšit na 100 % za posledních 5 minut...
    
    ![Úložiště tabulek][tables]
    
    Pokud není data, pak předpokládají, že je problém s příponou diagnostiky spuštěna ve virtuálních počítačích. Pokud data existuje, znamená to, jestli existuje problém s vaší pravidel škálování, nebo se službou Statistika. Zkontrolujte [Azure stav](https://azure.microsoft.com/status/).
    
    Jakmile jste byl pomocí těchto kroků, pokud máte pořád problémy škálování může vyzkoušejte ve fórech na [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp), nebo [přetečení zásobníku](http://stackoverflow.com/questions/tagged/azure), nebo protokolu volání podpory. Připravte se na sdílet šablony a zobrazení dat výkonu.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
