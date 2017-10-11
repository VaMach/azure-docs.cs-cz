---
title: "Nastavte průběžnou integraci pro Azure mikroslužeb | Microsoft Docs"
description: "Získáte přehled o tom, jak nastavit průběžnou integraci a nasazení pro aplikace Service Fabric pomocí Visual Studio Team Services (VSTS)."
services: service-fabric
documentationcenter: na
author: mthalman-msft
manager: timlt
editor: 
ms.assetid: 3e8c2290-9e7a-456a-9b2c-db44d1b3988d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2016
ms.author: mthalman;mikhegn
ms.openlocfilehash: 76a1e013e824910c7a489e345b6563ae3951378f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-service-fabric-continuous-integration-and-deployment-with-visual-studio-team-services"></a>Nastavit Service Fabric průběžnou integraci a nasazení s Visual Studio Team Services
Tento článek popisuje postup nastavení průběžnou integraci a nasazení pro aplikaci Azure Service Fabric pomocí Visual Studio Team Services (VSTS).

Tento dokument odráží aktuální procedury a očekává se časem změnit.

## <a name="prerequisites"></a>Požadavky
Abyste mohli začít, postupujte takto:

1. Zajistěte, abyste měli přístup k účtu Team Services nebo [vytvořit](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) sami.
2. Ujistěte se, zda máte přístup k týmovému projektu Team Services nebo [vytvořit](https://www.visualstudio.com/docs/setup-admin/create-team-project) sami.
3. Ujistěte se, že máte cluster Service Fabric, která můžete nasadit aplikace nebo vytvořte jednu pomocí [portál Azure](service-fabric-cluster-creation-via-portal.md), [šablony Azure Resource Manageru](service-fabric-cluster-creation-via-arm.md), nebo [sadyVisualStudio](service-fabric-cluster-creation-via-visual-studio.md).
4. Ujistěte se, že jste již vytvořili projekt aplikace Service Fabric (.sfproj). Musíte mít projekt, který byl vytvořen nebo upgradovat pomocí Service Fabric SDK 2.1 nebo vyšší (.sfproj soubor by měl obsahovat hodnotu vlastnosti ProjectVersion 1.1 nebo vyšší).

> [!NOTE]
> Vlastní sestavovací agentů se už nevyžadují. Týmové služby hostované agentů je dodávána předinstalované software pro správu clusteru Service Fabric, povolení pro nasazení aplikací přímo z těchto agentů.
> 
> 

## <a name="configure-and-share-your-source-files"></a>Konfigurace a sdílet zdrojové soubory
První věc, kterou chcete provést je připravit profil publikování pro použití procesu nasazení, který spouští v rámci Team Services.  Profil publikování by měl být nakonfigurovaný pro cluster, který jste připravili dříve:

1. Vyberte profil publikování v rámci projektu aplikace, kterou chcete použít pro pracovní postup nepřetržité integrace. Postupujte podle [publikovat pokyny](service-fabric-publish-app-remote-cluster.md) o tom, jak publikovat aplikaci do vzdáleného clusteru. Nemusíte ve skutečnosti Přestože publikování aplikace. Můžete kliknout na **Uložit** hypertextový odkaz v dialogovém okně publikování, když jste správně nakonfigurovali věcí.
2. Pokud chcete, aby vaše aplikace k upgradu pro každé nasazení, který se vyskytuje v Team Services, budete chtít nakonfigurovat profil publikování povolit upgrade. Ve stejném dialogové okno publikování použitým v kroku 1, ujistěte se, že **upgradu aplikace** je zaškrtnuté políčko.  Další informace o [konfigurace dalších nastavení upgradu](service-fabric-visualstudio-configure-upgrade.md). Klikněte **Uložit** hypertextový odkaz na Uložit nastavení do profilu publikování.
3. Ujistěte se, že jste uložili změny pro profil publikování a zrušení dialogové okno publikování.
4. Nyní je čas [sdílet zdrojové soubory vašeho projektu aplikace](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) s Team Services. Jakmile zdrojové soubory jsou přístupné v Team Services, nyní se můžete přesunout další krok generování sestavení. 

## <a name="create-a-build-definition"></a>Vytvořit definici sestavení
Definice sestavení Team Services popisuje pracovní postup, který se skládá ze sady kroky sestavení, které jsou prováděny postupně. K vytvoření balíčku aplikace Service Fabric a artefaktů, které lze použít k nasazení aplikace je cílem definici sestavení, kterou vytváříte. Další informace o Team Services [sestavení definice](https://www.visualstudio.com/docs/build/define/create).

### <a name="create-a-definition-from-the-build-template"></a>Vytvoření definice z šablony sestavení
1. Otevřete váš týmový projekt v sadě Visual Studio Team Services.
2. Vyberte **sestavení** kartě.
3. Vyberte zeleným  **+**  přihlásit k vytvoření nové definice sestavení.
4. V dialogovém okně, které se otevře, vyberte **aplikace Azure Service Fabric** v rámci **sestavení** kategorie šablony.
5. Vyberte **Další**.
6. Vyberte úložiště a větve přidružená aplikace Service Fabric.
7. Vyberte fronty agenta, kterou chcete použít. Hostovaní agenti jsou podporovány.
8. Vyberte **Vytvořit**.
9. Uložit definici sestavení a zadejte název.
10. Následující odstavce je uveden popis kroků vygenerované šablony:

| Krok sestavení | Popis |
| --- | --- |
| Obnovení NuGet |Obnoví balíčky NuGet pro řešení. |
| Vytvoření řešení \*.sln |Sestaví celé řešení. |
| Vytvoření řešení \*.sfproj |Generuje balíček aplikace Service Fabric, který se používá k nasazení aplikace. Musí být v adresáři artefaktů sestavení je zadat umístění balíčku aplikace. |
| Aktualizace verze aplikace Service Fabric |Aktualizace hodnoty verze součástí souborů manifestu balíčku aplikace umožňující podpora upgradu. Najdete v článku [stránky dokumentace, která úloha](https://go.microsoft.com/fwlink/?LinkId=820529) Další informace. |
| Kopírování souborů |Zkopíruje soubory parametry profilu a aplikaci publikovat do artefakty sestavení, který se má používat pro nasazení. |
| Publikování artefaktů |Publikuje sestavení artefaktů. Umožňuje definici verze artefakty sestavení využívat. |

### <a name="verify-the-default-set-of-tasks"></a>Ověřte výchozí skupinu úlohy
1. Ověřte **řešení** vstupní pole pro **obnovení NuGet** a **sestavení řešení** kroky sestavení.  Ve výchozím nastavení, tyto kroky sestavení spuštění na všechny soubory řešení, které jsou obsaženy v úložišti přidružené.  Pokud chcete pouze definici sestavení pracovat na jednom z těchto souborů řešení, budete muset explicitně aktualizujte cestu k souboru.
2. Ověřte **řešení** vstupní pole pro **balíčku aplikace** kroku sestavení.  Ve výchozím nastavení tento krok sestavení předpokládá, že existuje jenom jedna aplikace Service Fabric projektu (.sfproj) v úložišti.  Pokud máte více tyto soubory do úložiště a chcete cílit pouze jedna je pro tuto definici sestavení, budete muset explicitně aktualizujte cestu k souboru.  Pokud chcete balíček více projektů aplikace v úložišti, budete muset vytvořit další **Visual Studio sestavení** kroky v definici sestavení, každý cílí projekt aplikace.  By pak také musíte aktualizovat **argumenty MSBuild** pole pro každý z nich kroky sestavení tak, aby umístění balíčku je jedinečný pro každý z nich.
3. Ověřte nastavení správy verzí definované v **verze aktualizace Service Fabric aplikace** kroku sestavení.  Ve výchozím nastavení tento krok sestavení připojí číslo sestavení pro všechny verze hodnoty v souborech manifestu balíčku aplikace. Najdete v článku [stránky dokumentace, která úloha](https://go.microsoft.com/fwlink/?LinkId=820529) Další informace. To je užitečné pro podporu upgradu vaší aplikace, vzhledem k tomu, že každé nasazení upgradu vyžaduje jinou verzi hodnoty z předchozí nasazení. Pokud nejsou hodláte použít upgradu aplikace v pracovním postupu, zvažte zákaz tento krok sestavení. Musí být zakázáno, pokud chcete vytvořit sestavení, které je možné přepsat stávající aplikace Service Fabric. Nasazení se nezdaří, pokud verze aplikace vyprodukované sestavení neodpovídá verzi aplikace v clusteru.
4. Pokud vaše řešení obsahuje projektu .NET Core, je nutné zajistit, že vaše definice sestavení obsahuje krok sestavení, která obnoví závislosti:
   1. Vyberte **krok sestavení přidat...** .
   2. Vyhledejte **příkazového řádku** v rámci kartě nástrojů a klikněte na jeho tlačítko Přidat.
   3. Úkolu vstupní pole, použijte následující hodnoty:
   4. Nástroj: dotnet.
   5. Argumenty: obnovení
   6. Přetáhněte úlohu tak, aby se okamžitě po **obnovení NuGet** krok.
5. Uložte změny, které jste udělali na definici sestavení.

### <a name="try-it"></a>Vyzkoušet
Vyberte **fronty sestavení** ruční spuštění sestavení. Vytvoří také aktivační události nabízené nebo vrácení se změnami. Jakmile se ujistíte, že sestavení úspěšně spouští, nyní se můžete přesunout definování definicí verze, která nasadí aplikaci do clusteru.

## <a name="create-a-release-definition"></a>Vytvoření definice verze
Verze definice Team Services popisuje pracovní postup, který se skládá z sadu úloh, které jsou prováděny postupně. Cílem definice verze, kterou vytváříte je trvat balíček aplikace a nasadíte ho do clusteru. Při použití společně, definici sestavení a verze definice můžete provést celého pracovního postupu z počínaje zdrojové soubory k konče spuštěné aplikace v clusteru. Další informace o Team Services [verze definice](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-definition-from-the-release-template"></a>Vytvoření definice z šablony verze
1. Otevřete projekt v sadě Visual Studio Team Services.
2. Vyberte **verze** kartě.
3. Vyberte zeleným  **+**  přihlásit a vytvořte novou definici verze vyberte **vytvořit verze definice** v nabídce.
4. V dialogovém okně, které se otevře, vyberte **Azure Service Fabric nasazení** v rámci **nasazení** kategorie šablony.
5. Vyberte **Další**.
6. Vyberte definici sestavení, které chcete použít jako zdroj této verze definice.  Verze definice odkazuje artefaktů, které byly vytvořeny podle definice vybrané sestavení.
7. Zkontrolujte **průběžné nasazování** zaškrtávací políčko, pokud chcete mít Team Services automaticky vytvořit novou verzi a nasadit aplikace Service Fabric vždy, když sestavení se dokončí.
8. Vyberte fronty agenta, kterou chcete použít. Hostovaní agenti jsou podporovány.
9. Vyberte **Vytvořit**.
10. Název definice upravte kliknutím na ikonu tužky v horní části stránky.
11. Vyberte cluster, do které by měly být aplikace nasazeny z **připojení clusteru** vstupní pole úlohy. Připojení clusteru poskytuje potřebné informace, které umožňuje úloha nasazení se připojit ke clusteru. Pokud ještě nemáte připojení clusteru pro cluster, vyberte **spravovat** hypertextový odkaz vedle pole, které chcete přidat. Na stránce, které se otevře proveďte následující kroky:
    1. Vyberte **nový koncový bod služby** a pak vyberte **Azure Service Fabric** z nabídky.
    2. Vyberte typ ověřování používá cílem tento koncový bod clusteru.
    3. Definujte název připojení v **název připojení** pole.  Obvykle byste použili název clusteru.
    4. Zadejte adresu URL koncového bodu připojení klienta v **koncový bod clusteru** pole.  Příklad: tcp://contoso.westus.cloudapp.azure.com:19000.
    5. Zadání přihlašovacích údajů Azure Active Directory, zadejte pověření, kterou chcete použít pro připojení k clusteru, **uživatelské jméno** a **heslo** pole.
    6. Pro ověřování pomocí certifikátu, zadejte kódování Base64 souboru certifikátu klienta v **klientský certifikát** pole.  Naleznete v nápovědě automaticky otevírané okno na toto pole informace o tom, jak získat tuto hodnotu.  Pokud váš certifikát je chráněný heslem, zadejte heslo do **heslo** pole.
    7. Potvrďte změny kliknutím **OK**. Po přechodu zpět do vaší definice vydání, kliknutím na ikonu aktualizace **připojení clusteru** pole zobrazíte koncový bod, který jste právě přidali.
12. Uložte definici verze.

> [!NOTE]
> Accounts Microsoft (například @hotmail.com nebo @outlook.com) nejsou podporovány pomocí ověřování Azure Active Directory.
> 
> 

Definice, který se vytvoří se skládá z úloh typu **nasazení aplikace Service Fabric**. Najdete v článku [stránky dokumentace, která úloha](https://go.microsoft.com/fwlink/?LinkId=820528) Další informace o této úloze.

### <a name="verify-the-template-defaults"></a>Zkontrolujte výchozí nastavení šablony
1. Ověřte **profil publikování** vstupní pole pro **nasazení aplikace Service Fabric** úloh. Ve výchozím nastavení odkazuje na toto pole s názvem Cloud.xml, které jsou součástí sestavení artefaktů profil publikování. Pokud chcete k odkazování profil publikování jiný nebo pokud sestavení obsahuje několik balíčků aplikace v některé jeho artefakty, budete muset aktualizovat cestu správně.
2. Ověřte **balíčku aplikace** vstupní pole pro **nasazení aplikace Service Fabric** úloh. Ve výchozím nastavení odkazuje toto pole na výchozí cestu balíčku aplikace použité v šabloně definice sestavení.  Pokud jste změnili výchozí cestu balíčku aplikace v definici sestavení, budete muset aktualizovat cestu správně zde také.

### <a name="try-it"></a>Vyzkoušet
Vyberte **vytvořit verzi** z **verze** tlačítko nabídky můžete ručně vytvořit verze. V dialogovém okně, které se otevře, vyberte sestavení, který chcete na základní verze a potom klikněte na **vytvořit**. Pokud jste povolili průběžné nasazování, verze se vytvoří automaticky po dokončení sestavení definice přidružené sestavení.

## <a name="next-steps"></a>Další kroky
Další informace o průběžnou integraci s aplikací Service Fabric, najdete v následujících článcích:

* [Dokumentace služby Team domácí](https://www.visualstudio.com/docs/overview)
* [Sestavení správy v nástroji Team Services](https://www.visualstudio.com/docs/build/overview)
* [Správy verzí v Team Services](https://www.visualstudio.com/docs/release/overview)

