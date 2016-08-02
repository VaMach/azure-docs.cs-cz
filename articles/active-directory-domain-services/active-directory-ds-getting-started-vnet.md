<properties
    pageTitle="Služba Azure AD Domain Services: Vytvoření nebo výběr virtuální sítě | Microsoft Azure"
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
    ms.date="04/25/2016"
    ms.author="maheshu"/>

# Služba Azure AD Domain Services *(ve verzi Preview)* – Vytvoření nebo výběr virtuální sítě

## Pokyny k výběru virtuální sítě Azure
Při výběru virtuální sítě pro použití se službou Azure AD Domain Services mějte na paměti následující pokyny:

- Ujistěte se, že vybíráte virtuální síť v oblasti podporované službou Azure AD Domain Services. Na stránce [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) se dozvíte, pro jaké oblasti je dostupná služba Azure AD Domain Services.

- Chcete-li použít existující virtuální síť, ujistěte se, že jde o místní virtuální síť. Virtuální sítě, které používají starší verzi mechanizmu skupiny vztahů nelze použít se službou Azure AD Domain Services. Budete muset [migrovat starší verze virtuálních sítí do regionálních virtuálních sítí](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

- Chcete-li použít existující virtuální síť, ujistěte se, že pro ni nejsou nastavené žádné vlastní servery DNS. Služba Azure AD Domain Services nepodporuje vlastní servery DNS.

- Chcete-li použít existující virtuální síť, ujistěte se, že na ní nemáte k dispozici existující doménu se stejným názvem. Například předpokládejme, že již máte doménu „contoso.com“ k dispozici na vybrané virtuální síti. Následně se na této síti pokusíte povolit doménu spravovanou službou Azure AD Domain Services se stejným názvem (tedy „contoso.com“). Při pokusu o povolení služby Azure AD Domain Services dojde k chybě. Je to kvůli konfliktu názvů domén ve virtuální síti. V této situaci musíte použít jiný název pro nastavení domény spravované službou Azure AD Domain Services. Případně můžete zrušit existující doménu a poté pokračovat v povolení služby Azure AD Domain Services.

- Vyberte virtuální síť, která právě hostí nebo bude hostit virtuální počítače, které potřebují přístup ke službě Azure AD Domain Services. Až službu Domain Services povolíte, už ji nebudete moci přesunout na jinou virtuální síť.

- Služba Azure AD Domain Services nepodporuje virtuální sítě vytvořené pomocí Azure Resource Manager. Chcete-li používat službu Azure AD Domain Services ve virtuální síti vytvořené pomocí Azure Resource Manager, můžete [připojit klasickou virtuální síť k virtuální síti na bázi ARM](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).


## Úloha 2: Vytvořte virtuální síť Azure
Další úlohou konfigurace je vytvoření virtuální sítě Azure, ve které chcete povolit službu Azure AD Domain Services. Pokud již máte existující virtuální síť, kterou chcete použít, můžete tento krok přeskočit.

> [AZURE.NOTE] Ujistěte se, že virtuální síť Azure, kterou vytváříte nebo si vyberete pro použití se službou Azure AD Domain Services patří do oblasti Azure podporované službou Azure AD Domain Services. Na stránce [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) se dozvíte, pro jaké oblasti je dostupná služba Azure AD Domain Services.

Bude potřeba si poznamenat název virtuální sítě, abyste při povolování služby Azure AD Domain Services v dalším kroku konfigurace vybrali správnou virtuální síť.

Proveďte následující kroky konfigurace pro vytvoření virtuální sítě Azure, ve které chcete povolit službu Azure AD Domain Services.

1. Přejděte do **portálu Azure Classic** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. V levém podokně vyberte uzel **Sítě**.

3. Klikněte na **Nová** v podokně úloh ve spodní části stránky.

    ![Uzel virtuálních sítí](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. V uzlu **Síťové služby** vyberte možnost **Virtuální sít**.

5. Chcete-li vytvořit virtuální síť, klikněte na **Rychle vytvořit**.

    ![Virtuální síť – rychle vytvořit](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Zadejte **Název** vaší virtuální sítě. Pro tuto síť také můžete nakonfigurovat **Adresní prostor** nebo **Maximální počet virtuálních počítačů**. Prozatím můžete ponechat možnost Server DNS na „Žádný“. Toto nastavení bude aktualizováno poté, co povolíte službu Azure AD Domain Services.

7. Ujistěte se, že vybíráte podporovanou oblast Azure z rozevíracího seznamu **Umístění**. Na stránce [Služby Azure podle oblasti](https://azure.microsoft.com/regions/#services/) se dozvíte, pro jaké oblasti je dostupná služba Azure AD Domain Services. Toto je důležitý krok. Pokud vyberete virtuální síť v oblast Azure, která není podporovaná službou Azure AD Domain Services, nebudete moci pro tuto virtuální síť službu povolit.

8. Pro vytvoření svojí virtuální sítě klikněte na tlačítko **Vytvořit virtuální síť**.

    ![Vytvoření virtuální sítě pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet.png)

<br>

## Úloha 3 – Povolení služby Azure AD Domain Services
Další úlohou konfigurace je [povolení služby Azure AD Domain Services](active-directory-ds-getting-started-enableaadds.md).



<!--HONumber=Jun16_HO2-->


