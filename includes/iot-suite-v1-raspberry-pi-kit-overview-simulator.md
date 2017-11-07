## <a name="overview"></a>Přehled

V tomto kurzu je provést následující kroky:

- Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure. Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.
- Nastavte zařízení pro komunikaci s vaším počítačem a řešení vzdáleného monitorování.
- Aktualizujte zařízení ukázkový kód pro připojení k řešení vzdáleného monitorování a odeslat simulovanou telemetrii, která můžete zobrazit na řídicí panel řešení.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu potřebujete mít aktivní předplatné služby Azure.

> [!NOTE]
> Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].

### <a name="required-software"></a>Požadovaný software

Musíte klient SSH na umožňují vzdálený přístup na příkazovém řádku pí malin stolního počítače.

- Windows nezahrnuje klientem SSH. Doporučujeme používat [PuTTY](http://www.putty.org/).
- Většina Linuxových distribucích a Mac OS, obsahují nástroj příkazového řádku SSH. Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-hardware"></a>Požadovaný hardware

Stolní počítač, abyste mohli vzdáleně připojit k na příkazovém řádku malin pí.

[Microsoft IoT Starter Kit malin pí 3] [ lnk-starter-kits] nebo ekvivalentní součásti. V tomto kurzu používá následující položky ze sady kit:

- Malinová pí 3
- Karta MicroSD (s NOOBS)
- Kabelu USB malé
- Kabel Ethernet

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/