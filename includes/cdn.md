# <a name="using-cdn-for-azure"></a>Pomocí Azure CDN
Sítě doručování obsahu (CDN) Azure nabízí vývojářům globální řešení pro doručování širokopásmového obsahu pomocí ukládání do mezipaměti objektů BLOB a statického obsahu výpočetní instance na fyzických uzlech v USA, Evropa, Asie, Austrálie a Jižní Ameriky. Aktuální seznam umístění uzlů CDN, naleznete v části [umístění uzlů CDN Azure].

Tato úloha zahrnuje následující kroky:

* [Krok 1: Vytvoření účtu úložiště](#Step1)
* [Krok 2: Vytvoření nového koncového bodu CDN pro váš účet úložiště](#Step2)
* [Krok 3: Přístup k vašemu obsahu CDN](#Step3)
* [Krok 4: Odebrání obsahu CDN](#Step4)

Mezi výhody používání CDN Azure data do mezipaměti patří:

* Lepší výkon a uživatelské prostředí pro koncové uživatele, kteří jsou daleko od zdroj obsahu a používají aplikace, kde jsou k načtení obsahu vyžadují mnoho internet cest
* Velkých distribuovaných škálování pro lepší zvládání náhlého vysokého zatížení, můžete na začátku události například spuštění produktu

Stávající zákazníky služby CDN teď můžete použít Azure CDN v [portál Azure classic]. CDN je funkce rozšíření k vašemu předplatnému a má samostatné [fakturační plán].

<a id="Step1"> </a>

<h2>Krok 1: Vytvoření účtu úložiště</h2>

Použijte následující postup k vytvoření nového účtu úložiště pro předplatné Azure. Účet úložiště poskytuje přístup ke službám úložiště Azure. Účet úložiště představuje nejvyšší úroveň oboru názvů pro přístup k jednotlivých součástí služby Azure storage: objektu Blob služby, služba fronty a tabulky služby. Další informace o službách Azure storage najdete v tématu [pomocí služby Azure Storage](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Pokud chcete vytvořit účet úložiště, musí být buď správce služby správce nebo spolusprávce pro předplatné přidružený.

> [!NOTE]
> Informace o provedení této operace pomocí Azure Service Management API najdete v tématu [vytvořit účet úložiště](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) referenční téma.
> 
> 

**Chcete-li vytvořit účet úložiště pro předplatné Azure**

1. Přihlaste se na [portál Azure classic].
2. V levém dolním rohu klikněte na **nový**. V **nový** vyberte **datové služby**, pak klikněte na tlačítko **úložiště**, pak **rychle vytvořit**.
   
   **Vytvořit účet úložiště** otevře se dialogové okno.
   
   ![Vytvořit účet úložiště][create-new-storage-account]
3. V **URL** pole, zadejte název subdomény. Tato položka může obsahovat od 3 až 24 malých písmen a číslic.
   
    Tato hodnota se změní na název hostitele v rámci identifikátoru URI, který slouží k adresování objektů Blob, fronty nebo tabulky prostředky pro předplatné. Chcete-li vyřešit prostředek kontejneru ve službě Blob, použijte identifikátor URI v následujícím formátu, kde  *&lt;StorageAccountLabel&gt;*  odkazuje na hodnotu, která jste zadali v **zadejte adresu URL**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;můj_kontejner&gt;*
   
    **Důležité:** adresy URL popisku forms subdoméně účtu úložiště URI a musí být jedinečný mezi všechny hostované služby v Azure.
   
    Tato hodnota se používá jako název tohoto účtu úložiště na portálu, nebo při přístupu k tomuto účtu prostřednictvím kódu programu.
4. Z **oblast/skupině vztahů** rozevíracím seznamu vyberte oblast nebo vztahů skupiny pro účet úložiště. Vyberte skupinu vztahů místo oblast, pokud chcete vaší služby úložiště ve stejném datovém centru s jinými službami Windows Azure, které používáte. Tím lze vylepšit výkon a žádné poplatky se vám neúčtují odchozí.  
   
   **Poznámka:** Pokud chcete vytvořit skupinu vztahů, otevřete **nastavení** oblasti portálu pro správu, klikněte na možnost **skupiny vztahů**a pak klikněte buď **přidat skupinu vztahů** nebo **přidat**. Můžete také vytvořit a spravovat skupiny vztahů pomocí rozhraní API správy služby Windows Azure. Další informace najdete v tématu [operace pro skupiny vztahů].
5. Z **předplatné** rozevíracího seznamu vyberte odběr, který se použije účet úložiště s.
6. Klikněte na **Vytvořit účet úložiště**. Proces vytvoření účtu úložiště může trvat několik minut na dokončení.
7. Pokud chcete ověřit, zda byl úspěšně vytvořen účet úložiště, ověřte, že účet zobrazuje v položkách uvedené pro **úložiště** se stavem **Online**.

<a id="Step2"> </a>

<h2>Krok 2: Vytvoření nového koncového bodu CDN pro váš účet úložiště</h2>

Jakmile povolíte CDN přístup k účtu úložiště nebo hostované služby, jsou způsobilé pro ukládání do mezipaměti CDN edge všechny veřejně dostupné objekty. Pokud upravíte objekt, který je aktuálně uložené do mezipaměti v CDN, nový obsah nebudete mít k dispozici prostřednictvím CDN, dokud CDN aktualizuje jeho obsah, když vyprší platnost mezipaměti obsahu time to live období.

**Chcete-li vytvořit nový koncový bod CDN pro váš účet úložiště**

1. V [portál Azure classic], v navigačním podokně klikněte na tlačítko **CDN**.
2. Na pásu karet klikněte na tlačítko **nový**. V **nový** dialogovém okně, vyberte **App Services**, pak **CDN**, pak **rychle vytvořit**.
3. V **doménu původu** rozevíracího seznamu, vyberte úložiště účet, kterou jste vytvořili v předchozí části ze seznamu účtů úložiště k dispozici. 
4. Klikněte **vytvořit** tlačítko vytvořte nový koncový bod.
5. Po vytvoření koncového bodu se zobrazí v seznamu koncových bodů pro předplatné. Zobrazení seznamu zobrazuje adresu URL, kterou je nutné použít k přístupu k obsahu v mezipaměti, a také doménu původu. 
   
    Doménu původu je umístění, ze kterého CDN ukládá do mezipaměti obsah. Doménu původu může být účet úložiště nebo Cloudová služba; účet úložiště se používá pro účely tohoto příkladu. Úložiště obsahu do mezipaměti. to servery edge podle nastavení cache-control, které určíte, nebo výchozí heuristiky ukládání do mezipaměti sítě. 

    > [AZURE.NOTE]Konfigurace vytvořili pro koncový bod nebude hned dostupný; může trvat až 60 minut, než se registrace rozšíří v síti CDN. Uživatelé, kteří se pokusí použít název domény CDN okamžitě obdržet stavový kód 400 (Chybný požadavek), dokud nebude obsah k dispozici prostřednictvím CDN.

<a id="Step3"> </a>

<h2>Krok 3: Přístup k obsahu CDN</h2> 

Chcete-li získat přístup k obsahu v mezipaměti na CDN, použijte CDN adresy URL poskytnuté na portálu. Adresa pro uložené v mezipaměti objektů blob bude podobný následujícímu:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>

<h2>Krok 4: Odebrat obsah z CDN</h2>

Pokud již nechcete ukládat do mezipaměti objekt v Content Delivery Network (CDN) Azure, můžete provést jednu z následujících kroků:

* Pro Azure blob můžete odstranit objekt blob z veřejného kontejneru.
* Můžete provést kontejner privátní místo veřejné. V tématu [omezit přístup ke kontejnerům a objektům blob](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) Další informace.
* Můžete zakázat nebo odstranit koncový bod CDN pomocí portálu pro správu.
* Můžete upravit vaší hostované služby už reagovat na požadavky pro objekt.

Objekt již uložené v mezipaměti v CDN zůstane v mezipaměti, dokud neuplyne období time to live pro objekt. Po vypršení doby time to live CDN bude zkontrolujte, jestli je stále platný koncový bod CDN a objekt anonymně přístupné. Pokud není, bude mezipaměti už tento objekt.

Možnost hned vymazat obsah není aktuálně podporována na portálu pro správu Azure. Obraťte se na [podporu Azure](https://azure.microsoft.com/support/options/) Pokud potřebujete hned vymazat obsah. 

## <a name="additional-resources"></a>Další zdroje
* [Jak vytvořit skupinu vztahů v Azure]
* [Postupy: Správa účtů úložiště pro předplatné Azure]
* [O rozhraní API správy služby]
* [Postup mapování obsahu CDN do vlastní domény]

[Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
[umístění uzlů CDN Azure]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
[portál Azure classic]: https://manage.windowsazure.com/
[fakturační plán]: /pricing/calculator/?scenario=full
[Jak vytvořit skupinu vztahů v Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
[Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
[O rozhraní API správy služby]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
[Postup mapování obsahu CDN do vlastní domény]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
