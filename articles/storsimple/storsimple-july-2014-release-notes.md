---
title: "Poznámky k verzi zařízení StorSimple 8000 vydání verze | Microsoft Docs"
description: "Popisuje nové funkce, otevřete problémy a k dispozici řešení pro červenec 2014 Microsoft Azure StorSimple verze."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 12f1796e-37c3-42b4-b997-a84fc1950c20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: e6e087ad61ee54fef3552895c00a478fa51e2072
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Poznámky k verzi zařízení StorSimple 8000 řady vydání verze – červenec 2014

> [!NOTE]
> Portálu classic pro StorSimple je zastaralý. Váš správce zařízení StorSimple se automaticky přesune na nový portál Azure podle plánu vyřazení. Obdržíte e-mailu a portálu oznámení pro přesunutí. Tento dokument také vyřadí brzy k dispozici. Dotazy týkající se přechodu, najdete v části [– nejčastější dotazy: přesunout do portálu Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Přehled

Poznámky k verzi postupujte podle identifikovat kritická otevřené problémy pro zařízení StorSimple řady 8000 červenec 2014 obecné dostupnosti (GA) verzi Microsoft Azure StorSimple. Tato verze odpovídá verzi softwaru 6.3.9600.17215.

Pokud není uvedeno jinak, tyto poznámky k verzi platí pro všechny modely zařízení StorSimple. Poznámky k verzi se průběžně aktualizuje; Při zjištění zásadních problémů, které vyžadují řešení, jsou přidány. Před nasazením řešení Microsoft Azure StorSimple, vezměte v úvahu následující informace.

## <a name="known-issues-in-this-release"></a>Známé problémy v této verzi

Následující tabulka obsahuje souhrn známých problémů v této verzi.  

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |V některých případech při provádění obnovení továrního nastavení zařízení StorSimple může být pomalé a zobrazit tato zpráva: **resetovat do výrobního probíhá (fáze 8)**. To se stane, když stisknutím CTRL + C, když probíhá rutinu. |Není stiskněte kombinaci kláves CTRL + C po inicializaci obnovení továrního nastavení. Pokud jste již v tomto stavu, požádejte o další kroky Microsoft Support. |Ano |Ne |
| 2 |Disk kvora |Ve výjimečných případech Pokud jsou většinu disků ve skříni EBOD 8600 zařízení odpojeny, což vede k žádné disk kvora, pak fondu úložiště přejde do režimu offline. Zůstane offline, i když jsou znovu připojeny disky. |Je potřeba restartovat zařízení. Pokud potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 3 |Selhání snímku cloudu |Ve výjimečných případech cloudový snímek může selhat s chybou **zálohování dosažen limit maximální**. K tomu dochází, je-li být delší než 255 online klony na stejném zařízení, ze stejné původního svazku, který byl odstraněn. | |Ano |Ano |
| 4 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení bitovou kopii z uzlu sdílené ID řadič může zobrazují původně jako ID partnera řadiče. Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče. |Ano |Ne |
| 5 |Monitorování grafy zařízení |Ve službě StorSimple Manager grafy monitorování zařízení nefungují, pokud základní nebo je v konfiguraci proxy serveru pro zařízení povolené ověřování protokolem NTLM. |Upravte konfiguraci webového proxy pro toto zařízení zaregistrovali služby StorSimple Manager, že ověřování je nastaven na hodnotu NONE. Chcete-li to provést, spusťte prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. |Ano |Ano |
| 6 |Účty úložiště |Pomocí služby úložiště odstranit účet úložiště se o nepodporovaný scénář. To povede k situaci, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 7 |Navrácení služeb po obnovení |Navrácení služeb po obnovení do 24 hodin zotavení po havárii (DR) není podporována. | |Ano |Ne |
| 8 |Převzetí služeb při selhání zařízení |Více převzetí služeb při selhání kontejner svazků ze stejného zdrojového zařízení do jiné cílové zařízení není podporována. Převzetí služeb při selhání z jednoho neaktivní zařízení do více zařízení bude kontejnery svazků na prvním převzal zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení na portálu Azure classic. | |Ano |Ne |
| 9 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je třeba zadat IP zařízení pro instalaci úspěšně dokončit. | |Ano |Ne |
| 10 |Síťová rozhraní |Síťová rozhraní DATA 2 a DATA 3 měla vzájemně zaměněny v softwaru. |Pokud potřebujete nakonfigurovat tyto rozhraní, obraťte se na Microsoft Support. |Ano |Ne |

