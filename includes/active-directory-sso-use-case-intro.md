Organizace používá více [Software jako služba (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) aplikací pro produktivitu protože cloudové technologie a nástroje jsou čím dál víc snadno dostupné. S růstem počet aplikací SaaS stane náročné správcům spravovat účty a oprávnění a pro uživatele pamatovat jiné hesla. Správu těchto aplikací jednotlivě vytvoří další práci a je méně bezpečné.

* Zaměstnanci, kteří mají ke sledování mnoho hesla zpravidla použití méně bezpečných metod pamatovat, zápis dolů hesla nebo přihlašovali stejnými hesly napříč mnoha účty.
* Když dorazí nového zaměstnance nebo jeden opustí, všechny své účty musí být jednotlivě zřízený nebo zrušte zřízený.
* Kromě toho zaměstnanci mohou začít používat aplikace SaaS pro práci bez průchodu přes IT, což znamená, budou v systémech, které správci IT neschválili nejsou monitorování a vytváření svoje vlastní účty.  

Jednotné přihlašování (SSO) je řešení pro všechny tyto problémy. Je nejjednodušší způsob, jak spravovat víc aplikací a poskytněte uživatelům konzistentní prostředí přihlášení. Azure Active Directory (Azure AD) poskytuje robustní řešení jednotné přihlašování a má mnoho k dispozici předem integrovaných aplikací, s kurzy pro správce rychle nastavit nové aplikace a spustit zřizování uživatelů.

## <a name="how-does-azure-active-directory-integrate-apps"></a>Jak Azure Active Directory integraci aplikace?
Azure AD umožňuje integraci se vaše aplikace a zřízené účty. To lze provést prostřednictvím buď dva přístupy.

* Pokud je aplikace předem integrovaných v aplikaci Galerie, můžete přejít prostřednictvím tohoto portálu k nastavení aplikace a konfigurace nastavení pro povolení jednotného přihlašování. Pro žádné Galerie aplikace, můžete začít používat podle podle pokynů jednoduché podrobné uvedené v galerii aplikací a na webu Azure portal povolit jednotné přihlašování.
* Pokud aplikace není v galerii, může stále nastavíte většinu aplikací ve službě Azure AD jako vlastní aplikaci. To vyžaduje trochu další technické znalosti ke konfiguraci. Přidáním jakékoli aplikace, který podporuje SAML 2.0 jako federované aplikace nebo jakékoli aplikace, která má HTML na přihlašovací stránku jako heslo jednotného přihlašování k aplikaci.

V případě, kde uživatelé vytvořili svoje vlastní účty pro aplikace SaaS, které nejsou spravovány IT, [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) nástroj poskytuje řešení. Tento nástroj monitoruje webové přenosy identifikovat, které aplikace se používají v rámci organizace a kolik uživatelů pomocí každého z nich. IT můžete použít tyto informace se dozvíte, jaké aplikace uživatelé dáváte přednost a rozhodnout, která integruje do Azure AD pro jednotné přihlašování.  

Při integraci aplikace do Azure AD, můžete namapovat identit uživatelů zavedené aplikace na jejich odpovídající identit Azure AD.  

