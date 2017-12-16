## <a name="deployment-considerations"></a>Aspekty nasazování

* Dostupnost virtuálních počítačů N-series, najdete v části [produkty podle oblasti](https://azure.microsoft.com/en-us/regions/services/).

* N-series virtuálních počítačů lze nasadit pouze v modelu nasazení Resource Manager.

* Při vytváření virtuálních počítačů N-series, pomocí webu Azure portal na **Základy** vyberte **typ disku virtuálního počítače** z **HDD**. Zvolte dostupné velikosti N-series v **velikost** okně klikněte na tlačítko **zobrazit všechny**.

* NC a virtuální počítače vs nepodporují disky virtuálních počítačů, které jsou zajišťované Azure Premium storage.

* Pokud chcete nasadit víc než několik virtuálních počítačů N-series, zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu. Pokud používáte [bezplatný účet Azure](https://azure.microsoft.com/free/), můžete použít pouze omezený počet výpočetních jader Azure.

* Potřebujete může se zvýšit kvótu jader (podle oblasti) ve vašem předplatném Azure a zvýšení samostatné kvóty pro počet jader NC NCv2, ND nebo vs. Požádat o zvýšení kvóty, [otevřete žádosti o podporu online zákazníka](../articles/azure-supportability/how-to-create-azure-support-request.md) zdarma. Výchozí omezení se může lišit v závislosti na vaše předplatné kategorii.

* Můžete nasadit na virtuálních počítačích N-series jeden image virtuálního počítače je [virtuální počítač Azure datové vědy](../articles/machine-learning/data-science-virtual-machine/overview.md). Virtuální počítač datové vědy provede předinstalaci a nakonfiguruje mnoha oblíbených datové vědy a přímý výukové nástroje. Provede také předinstalaci NVIDIA tesla – měrná GPU ovladače.





