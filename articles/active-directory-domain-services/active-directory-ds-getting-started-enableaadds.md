<properties
    pageTitle="Služba Azure AD Domain Services: Povolení služby Azure AD Domain Services | Microsoft Azure"
    description="Začínáme se službou Azure Active Directory Domain Services (ve verzi Preview)"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/06/2016"
    ms.author="maheshu"/>

# Služba Azure AD Domain Services *(ve verzi Preview)* – Povolení služby Azure AD Domain Services

## Úloha 3: Povolení služby Azure AD Domain Services
V této úloze povolíte službu Azure AD Domain Services pro svůj adresář. Proveďte následující kroky konfigurace pro povolení služby Azure AD Domain Services pro svůj adresář.

1. Přejděte do **portálu Azure Classic** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. V levém podokně vyberte uzel **Active Directory**.

3. Vyberte klienta Azure AD (adresář), pro kterého chcete povolit službu Azure AD Domain Services.

    ![Vyberte adresář služby Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klikněte na kartu **Konfigurace**.

    ![Karta konfigurace adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Posuňte se dolů k oddílu s názvem **služby domény**.

    ![Oddíl konfigurace doménových služeb](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Přepněte možnost s názvem **Povolit doménové služby pro tento adresář** na **Ano**. Na stránce se zobrazí několik dalších možností konfigurace služby Azure AD Domain Services.

    ![Povolení doménových služeb](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Když pro svého klienta povolíte službu Azure AD Domain Services, Azure AD bude generovat a ukládat přihlašovací údaje protokolů Kerberos a NTLM potřebné pro ověřování uživatelů.

7. Zadejte **název domény DNS doménových služeb**.

   - Výchozí název domény adresáře (tedy končící příponou domény **.onmicrosoft.com**) bude vybrán ve výchozím nastavení.

   - Seznam obsahuje všechny domény, které byly nakonfigurované pro váš adresář služby Azure AD – včetně ověřených i neověřených domén, které konfigurujete na kartě „Domény“.

   - Kromě toho můžete přidat vlastní název domény ručním zadáním. V tomto příkladu jsme zadali vlastní název domény „contoso100.com“.

     > [AZURE.WARNING] Ujistěte se, že předpona domény vámi zadaného názvu domény (tedy  „contoso100“ v názvu domény „contoso100.com“) není delší než 15 znaků. Nelze vytvořit doménu služby Azure AD Domain Services s předponou domény delší než 15 znaků.

8. Dalším krokem je výběr virtuální sítě, ve které chcete zpřístupnit službu Azure AD Domain Services. Vyberte virtuální síť, kterou jste právě vytvořili, z rozevírací nabídky s názvem **Připojit doménové služby k této virtuální síti**.

   - Ujistěte se, že vybraná virtuální síť patří do oblasti Azure podporované službou Azure AD Domain Services.

   - Na stránce [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) se dozvíte, pro jaké oblasti je dostupná služba Azure AD Domain Services.

   - Poznámka: virtuální sítě patřící do oblasti, kde není služba Azure AD Domain Services podporovaná, se v rozevíracím seznamu nezobrazí.

   - Podobně se v rozevíracím seznamu nezobrazí ani virtuální sítě vytvořené pomocí Azure Resource Manager (virtuální sítě na bázi ARM). To proto, že virtuální sítě na bázi ARM nejsou v současné době službou Azure AD Domain Services podporované.

9. Ujistěte se, že vámi zvolený název domény DNS pro spravovanou doménu ještě ve virtuální síti neexistuje. To může nastat v některém z následujících scénářů:

   - Pokud již máte ve virtuální síti doménu se stejným názvem domény DNS.

   - Pokud má vámi zvolená virtuální síť připojení VPN s vaší místní sítí, ve které máte doménu se stejným názvem domény DNS.

   - Pokud máte ve virtuální síti existující cloudovou službu s tímto názvem.

10. Když jste hotovi s výběrem výše uvedených možností, klikněte na **Uložit** v podokně úloh v dolní části stránky pro povolení služby Azure AD Domain Services.

11. Na stránce se zobrazí stav „Čeká na vyřízení...“ , zatímco probíhá povolení služby Azure AD Domain Services pro váš adresář.

    ![Povolení doménových služeb – stav Čekání na vyřízení](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Služba Azure AD Domain Services poskytuje vysokou dostupnost vaší spravované domény. Když poprvé povolíte službu Azure AD Domain Services pro svoji doménu, všimnete si, že se jedna po druhé objeví IP adresy, na kterých jsou doménové služby dostupné.  Druhá IP adresa se zobrazí po chvíli, hned jak služba povolí vysokou dostupnost vaší domény.  Když je vysoká dostupnost vaší domény nakonfigurovaná a aktivní, měli byste vidět dvě IP adresy v oddílu **doménové služby** na kartě **Konfigurace**.

12. Po přibližně 20-30 minutách uvidíte v poli **IP adresa** na kartě **Konfigurace** první IP adresu ve vaší virtuální síti, na které jsou dostupné doménové služby. 

    ![Doménové služby povoleny – zajištěna první IP adresa](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Když je vysoká dostupnost vaší domény funkční, uvidíte na stránce dvě IP adresy. Toto jsou IP adresy ve vámi zvolené virtuální síti, na kterých bude dostupná služba Azure AD Domain Services. Poznamenejte si tyto IP adresy, abyste mohli aktualizovat nastavení DNS pro vaši virtuální síť. Tento krok umožňuje virtuálním počítačům ve virtuální síti připojit se k doméně kvůli operacím, jako je například připojení k doméně.

    ![Doménové služby povoleny – zajištěny obě IP adresy](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] V závislosti na velikosti adresáře služby Azure AD (počet uživatelů, skupin, atd.) bude chvíli trvat, než bude obsah adresáře dostupný ve službě Azure AD Domain Services. Proces synchronizace se odehrává na pozadí. Pro velké adresáře s desítkami tisíc objektů může trvat i několik dní, než proběhne synchronizace všech uživatelů, členství ve skupinách a přihlašovacích údajů a jejich zpřístupnění ve službě Azure AD Domain Services.

<br>

## Úloha 4 – Aktualizace nastavení DNS pro virtuální síť Azure
Další úlohou konfigurace je [aktualizace DNS pro virtuální síť Azure](active-directory-ds-getting-started-dns.md).



<!--HONumber=Aug16_HO4-->


