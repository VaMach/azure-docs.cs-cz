## <a name="deployment-considerations"></a>Aspekty nasazování

* Dostupnost virtuálních počítačů N-series, najdete v části [produkty podle oblasti](https://azure.microsoft.com/en-us/regions/services/).

* N-series virtuálních počítačů lze nasadit pouze v modelu nasazení Resource Manager.

* N-series virtuálních počítačů se liší v typ podporují pro jejich disky úložiště Azure. NC a virtuální počítače vs podporují jenom disky virtuálních počítačů, které jsou zajišťované pomocí standardní úložiště disku (HDD). NCv2 ND a NCv3 (Náhled) virtuálních počítačů pouze podporu virtuálních počítačů disky, které jsou zajišťované pomocí disku úložiště Premium (SSD).

* Pokud chcete nasadit víc než několik virtuálních počítačů N-series, zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu. Pokud používáte [bezplatný účet Azure](https://azure.microsoft.com/free/), můžete použít pouze omezený počet výpočetních jader Azure.

* Potřebujete může se zvýšit kvótu jader (podle oblasti) ve vašem předplatném Azure a zvýšení samostatné kvóty pro počet jader NC NCv2, ND nebo vs. Požádat o zvýšení kvóty, [otevřete žádosti o podporu online zákazníka](../articles/azure-supportability/how-to-create-azure-support-request.md) zdarma. Výchozí omezení se může lišit v závislosti na vaše předplatné kategorii.

* Můžete nasadit na virtuálních počítačích N-series jeden image virtuálního počítače je [virtuální počítač Azure datové vědy](../articles/machine-learning/data-science-virtual-machine/overview.md). Virtuální počítač datové vědy provede předinstalaci a nakonfiguruje mnoha oblíbených datové vědy a přímý výukové nástroje. Provede také předinstalaci NVIDIA tesla – měrná GPU ovladače.





