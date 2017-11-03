---
title: "Odesílání událostí do centra událostí Azure pomocí jazyka C | Microsoft Docs"
description: "Odesílání událostí do centra událostí Azure pomocí jazyka C"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 25311958314cca049d109ecbe3f46aaaa36b694d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Odesílání událostí do centra událostí Azure pomocí jazyka C

## <a name="introduction"></a>Úvod
Event Hubs je vysoce škálovatelná služba, kterou lze přijímat miliony událostí za sekundu, povolení aplikaci zpracovávat a analyzovat masivní objemy dat vytvářených připojených zařízení a aplikací. Až se shromáždí do centra událostí, můžete transformovat a ukládat data pomocí úložného clusteru nebo všechny zprostředkovatele datové analýzy v reálném čase.

Další informace najdete v tématu [Přehled služby Event Hubs] [Přehled služby Event Hubs].

V tomto kurzu se dozvíte, jak odesílat události do centra událostí pomocí konzolové aplikace v C. Chcete-li přijímat události, klikněte na tlačítko přijímající příslušný jazyk v levé tabulce obsahu.

K dokončení tohoto kurzu potřebujete:

* Prostředí pro vývoj C. V tomto kurzu budeme předpokládat zásobníku RSZ ve virtuálním počítači Azure Linux s Ubuntu 14.04.
* [Sadu Microsoft Visual Studio](https://www.visualstudio.com/).
* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-to-event-hubs"></a>Zasílání zpráv do služby Event Hubs
V této části jsme zápisu aplikace C odesílat události do vašeho centra událostí. Kód používá knihovnu AMQP kanálem z [Apache Qpid projektu](http://qpid.apache.org/). Toto je obdobou pomocí front Service Bus a témat s AMQP z C, jak je znázorněno [zde](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Další informace najdete v tématu [Qpid kanálem dokumentaci](http://qpid.apache.org/proton/index.html).

1. Z [Qpid AMQP Messenger stránky](https://qpid.apache.org/proton/messenger.html), postupujte podle pokynů k instalaci Qpid kanálem, v závislosti na vašem prostředí.
2. Kompilace knihovně kanálem, nainstalujte následující balíčky:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Stažení [Qpid kanálem knihovny](http://qpid.apache.org/proton/index.html)a rozbalte ho, například:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Vytvoření adresáře sestavení, kompilace a instalace:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Pracovní adresář, vytvořte nový soubor s názvem **sender.c** následujícím kódem. Nezapomeňte nahradit hodnoty pro název centra událostí a název oboru názvů. Také je třeba nahradit verzi klíč pro kódovaná adresou URL **SendRule** vytvořili dříve. Můžete kódování URL ho [zde](http://www.w3schools.com/tags/ref_urlencode.asp).
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://{SAS Key Name}:{SAS key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. Kompilace souboru, za předpokladu, že **RSZ**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Tento kód používáme okno s odchozí 1 Vynutit zprávy se co nejdříve. Obecně platí aplikace se pokuste batch zprávy a pokuste se zvýšit propustnost. Najdete v článku [Qpid AMQP Messenger stránky](https://qpid.apache.org/proton/messenger.html) informace o tom, jak v knihovně Qpid kanálem v tomto a dalších prostředích a z platforem, pro které jsou k dispozici vazby (aktuálně Perl, PHP, Python a Ruby).


## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs najdete na následujících odkazech:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md
)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
