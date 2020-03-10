---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Partie 3 : menu disposition et catégorie | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 3 couvre l’ajout de la disposition et d’un menu de catégorie.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: a223b97fd362ecf73ecde431e141021c1dcc6a6d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639117"
---
# <a name="part-3-layout-and-category-menu"></a><span data-ttu-id="d1784-104">Partie 3 : menu disposition et catégorie</span><span class="sxs-lookup"><span data-stu-id="d1784-104">Part 3: Layout and Category Menu</span></span>

<span data-ttu-id="d1784-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d1784-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="d1784-106">Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="d1784-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="d1784-107">Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="d1784-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="d1784-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="d1784-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="d1784-109">La partie 3 couvre l’ajout de la disposition et d’un menu de catégorie.</span><span class="sxs-lookup"><span data-stu-id="d1784-109">Part 3 covers adding layout and a category menu.</span></span>

## <a id="_Toc260221669"></a><span data-ttu-id="d1784-110">Ajout de certaines mises en page et d’un menu catégorie</span><span class="sxs-lookup"><span data-stu-id="d1784-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="d1784-111">Dans notre page maître du site, nous allons ajouter une balise div pour la colonne du côté gauche qui contiendra notre menu de catégorie de produits.</span><span class="sxs-lookup"><span data-stu-id="d1784-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="d1784-112">Notez que l’alignement souhaité et une autre mise en forme seront fournis par la classe CSS que nous avons ajoutée à notre fichier style. css.</span><span class="sxs-lookup"><span data-stu-id="d1784-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="d1784-113">Le menu catégorie de produit est créé dynamiquement au moment de l’exécution en interrogeant la base de données commerce pour rechercher les catégories de produits existantes et en créant les éléments de menu et les liens correspondants.</span><span class="sxs-lookup"><span data-stu-id="d1784-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="d1784-114">Pour ce faire, nous utilisons deux pages ASP. Contrôles de données puissants de NET.</span><span class="sxs-lookup"><span data-stu-id="d1784-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="d1784-115">Le contrôle « source de données d’entité » et le contrôle « ListView ».</span><span class="sxs-lookup"><span data-stu-id="d1784-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="d1784-116">Passons à la « vue conception » et utilisons les applications d’assistance pour configurer nos contrôles.</span><span class="sxs-lookup"><span data-stu-id="d1784-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="d1784-117">Nous allons affecter à la propriété EntityDataSource ID la valeur EDS\_Category\_menu et cliquer sur configurer la source de données.</span><span class="sxs-lookup"><span data-stu-id="d1784-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="d1784-118">Sélectionnez la connexion CommerceEntities qui a été créée pour nous lors de la création du modèle de source de données d’entité pour notre base de données de commerce, puis cliquez sur « suivant ».</span><span class="sxs-lookup"><span data-stu-id="d1784-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="d1784-119">Sélectionnez le nom du jeu d’entités « Categories » et laissez les autres options par défaut.</span><span class="sxs-lookup"><span data-stu-id="d1784-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="d1784-120">Cliquez sur « Terminer ».</span><span class="sxs-lookup"><span data-stu-id="d1784-120">Click "Finish".</span></span>

<span data-ttu-id="d1784-121">Nous allons maintenant définir la propriété ID de l’instance de contrôle ListView que nous avons placée dans notre page sur ListView\_ProductsMenu et activer son application auxiliaire.</span><span class="sxs-lookup"><span data-stu-id="d1784-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="d1784-122">Bien qu’il soit possible d’utiliser des options de contrôle pour mettre en forme l’affichage et la mise en forme des éléments de données, notre création de menu nécessite uniquement un balisage simple. nous allons donc entrer le code dans la vue source.</span><span class="sxs-lookup"><span data-stu-id="d1784-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="d1784-123">Veuillez noter l’instruction « eval » : &lt;% # Eval (« CategoryName »)%&gt;</span><span class="sxs-lookup"><span data-stu-id="d1784-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="d1784-124">La syntaxe ASP.NET &lt;% #%&gt; est une convention sténographique qui indique au runtime d’exécuter tout ce qui est contenu dans et de générer les résultats « in line ».</span><span class="sxs-lookup"><span data-stu-id="d1784-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="d1784-125">L’instruction eval (« CategoryName ») indique que, pour l’entrée actuelle dans la collection liée d’éléments de données, récupérez la valeur des noms d’élément de modèle d’entité « CategoryName ».</span><span class="sxs-lookup"><span data-stu-id="d1784-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CategoryName".</span></span> <span data-ttu-id="d1784-126">Il s’agit d’une syntaxe concise pour une fonctionnalité très puissante.</span><span class="sxs-lookup"><span data-stu-id="d1784-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="d1784-127">Permet d’exécuter l’application maintenant.</span><span class="sxs-lookup"><span data-stu-id="d1784-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="d1784-128">Notez que notre menu de catégorie de produits s’affiche et lorsque nous pointons sur l’un des éléments de menu catégorie, nous pouvons voir le lien de l’élément de menu pointe vers une page que nous avons encore implémentée nommée ProductsList. aspx et que nous avons créé un argument de chaîne de requête dynamique qui contient le  ID de catégorie.</span><span class="sxs-lookup"><span data-stu-id="d1784-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1784-129">[Précédent](tailspin-spyworks-part-2.md)
> [Suivant](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="d1784-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
