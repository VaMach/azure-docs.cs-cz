<properties
   pageTitle="Nasměrování internetové domény společnosti na doménu Traffic Manageru | Microsoft Azure"
   description="Tento článek vám pomůže nasměrovat název domény společnosti na název domény Traffic Manageru."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />


# Nasměrování internetové domény společnosti na doménu Azure Traffic Manageru

Chcete-li název domény vaší společnosti nasměrovat na název domény Traffic Manageru, upravte záznamu prostředku DNS na serveru DNS pro internet tak, aby používal typ záznamu CNAME, který mapuje název domény vaší společnosti na název domény vašeho profilu Traffic Manageru. Název domény Traffic Manageru se zobrazuje v části **Obecné** na stránce Konfigurace pro příslušný profil Traffic Manageru.

Pokud byste například chtěli nasměrovat název domény společnosti www.contoso.com na název domény Traffic Manageru contoso.trafficmanager.net, aktualizovali byste svůj záznam prostředku DNS do této podoby:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Veškeré požadavky na provoz pro název *www.contoso.com* teď budou směrovány na název *contoso.trafficmanager.net*.

>[AZURE.IMPORTANT] Doménu druhé úrovně, například *contoso.com*, nelze nasměrovat na doménu Traffic Manageru. Jedná se o omezení protokolu DNS, které neumožňuje používání záznamů CNAME pro názvy domén druhé úrovně.

## Další kroky

[Metody směrování Traffic Manageru](traffic-manager-routing-methods.md)

[Traffic Manager – Zakázání, povolení nebo odstranění profilu](disable-enable-or-delete-a-profile.md)

[Traffic Manager – Zakázání nebo povolení koncového bodu](disable-or-enable-an-endpoint.md)



<!--HONumber=Sep16_HO3-->


