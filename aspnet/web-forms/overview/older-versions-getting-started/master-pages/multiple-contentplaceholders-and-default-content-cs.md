---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Plusieurs ContentPlaceHolders et contenu par défautC#() | Microsoft Docs
author: rick-anderson
description: Examine comment ajouter plusieurs espaces réservés de contenu à une page maître et comment spécifier le contenu par défaut dans les espaces réservés de contenu.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: e902bcae05c0e7976a20293f2b01e5f2e2bee13a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630297"
---
# <a name="multiple-contentplaceholders-and-default-content-c"></a>ContentPlaceHolders multiples et contenu par défaut (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Examine comment ajouter plusieurs espaces réservés de contenu à une page maître et comment spécifier le contenu par défaut dans les espaces réservés de contenu.

## <a name="introduction"></a>Introduction

Dans le didacticiel précédent, nous avons examiné comment les pages maîtres permettent aux développeurs ASP.NET de créer une disposition cohérente à l’ensemble du site. Les pages maîtres définissent à la fois les balises communes à toutes les pages de contenu et les régions qui peuvent être personnalisées page par page. Dans le didacticiel précédent, nous avons créé une page maître simple (`Site.master`) et deux pages de contenu (`Default.aspx` et `About.aspx`). Notre page maître était composée de deux ContentPlaceHolders nommés `head` et `MainContent`, qui étaient respectivement situés dans l’élément `<head>` et le formulaire Web. Alors que les pages de contenu ont chacune deux contrôles de contenu, nous avons uniquement spécifié le balisage pour celui correspondant à `MainContent`.

Comme le prouvent les deux contrôles ContentPlaceHolder dans `Site.master`, une page maître peut contenir plusieurs ContentPlaceHolders. De plus, la page maître peut spécifier le balisage par défaut pour les contrôles ContentPlaceHolder. Une page de contenu, ensuite, peut éventuellement spécifier son propre balisage ou utiliser le balisage par défaut. Dans ce didacticiel, nous examinons l’utilisation de plusieurs contrôles de contenu dans la page maître et voyons comment définir le balisage par défaut dans les contrôles ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Étape 1 : ajout de contrôles ContentPlaceHolder supplémentaires à la page maître

De nombreuses conceptions de sites Web contiennent plusieurs zones de l’écran personnalisées page par page. `Site.master`, la page maître que nous avons créée dans le didacticiel précédent, contient un seul ContentPlaceHolder dans le formulaire Web nommé `MainContent`. Plus précisément, ce ContentPlaceHolder se trouve dans l’élément `mainContent` `<div>`.

La figure 1 montre `Default.aspx` affichées dans un navigateur. La région encerclée en rouge est le balisage spécifique à la page correspondant à `MainContent`.

[![la région cerclée affiche la zone actuellement personnalisable page par page.](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Figure 01**: la région cerclée affiche la zone actuellement personnalisable page par page ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))

Imaginez qu’en plus de la région illustrée dans la figure 1, nous devons également ajouter des éléments spécifiques à la page dans la colonne de gauche, sous les sections leçons et actualités. Pour ce faire, nous ajoutons un autre contrôle ContentPlaceHolder à la page maître. Pour effectuer la procédure, ouvrez la page maître `Site.master` dans Visual Web Developer, puis faites glisser un contrôle ContentPlaceHolder de la boîte à outils vers le concepteur après la section Actualités. Définissez le `ID` de ContentPlaceHolder sur `LeftColumnContent`.

[![ajouter un contrôle ContentPlaceHolder à la colonne de gauche de la page maître](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Figure 02**: ajouter un contrôle ContentPlaceHolder à la colonne de gauche de la page maître ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))

Avec l’ajout de l' `LeftColumnContent` ContentPlaceHolder à la page maître, nous pouvons définir le contenu de cette région page par page en incluant un contrôle de contenu dans la page dont `ContentPlaceHolderID` a la valeur `LeftColumnContent`. Nous examinons ce processus à l’étape 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Étape 2 : définition du contenu pour le nouveau ContentPlaceHolder dans les pages de contenu

Quand vous ajoutez une nouvelle page de contenu au site Web, Visual Web Developer crée automatiquement un contrôle de contenu dans la page pour chaque ContentPlaceHolder dans la page maître sélectionnée. Après avoir ajouté un `LeftColumnContent` ContentPlaceHolder à notre page maître à l’étape 1, les nouvelles pages ASP.NET auront désormais trois contrôles de contenu.

Pour illustrer cela, ajoutez une nouvelle page de contenu au répertoire racine nommé `MultipleContentPlaceHolders.aspx` qui est lié à la page maître `Site.master`. Visual Web Developer crée cette page avec le balisage déclaratif suivant :

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Entrez du contenu dans le contrôle de contenu référençant le `MainContent` ContentPlaceHolders (`Content2`). Ensuite, ajoutez le balisage suivant au contrôle de contenu `Content3` (qui fait référence à l' `LeftColumnContent` ContentPlaceHolder) :

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Après avoir ajouté cette balise, accédez à la page via un navigateur. Comme le montre la figure 3, la balise placée dans le contrôle de contenu `Content3` s’affiche dans la colonne de gauche sous la section Actualités (entourée d’un cercle rouge). La balise placée dans `Content2` s’affiche dans la partie droite de la page (entourée d’un cercle en bleu).

[![la colonne de gauche comprend maintenant un contenu spécifique à la page sous la section Actualités](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Figure 03**: la colonne de gauche comprend maintenant un contenu spécifique à la page sous la section Actualités ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))

### <a name="defining-content-in-existing-content-pages"></a>Définition du contenu dans les pages de contenu existantes

La création d’une page de contenu incorpore automatiquement le contrôle ContentPlaceHolder que nous avons ajouté à l’étape 1. Toutefois, nos deux pages de contenu existantes, `About.aspx` et `Default.aspx`, n’ont pas de contrôle de contenu pour le `LeftColumnContent` ContentPlaceHolder. Pour spécifier le contenu de ce ContentPlaceHolder dans ces deux pages existantes, nous devons ajouter un contrôle de contenu.

Contrairement à la plupart des contrôles Web ASP.NET, la boîte à outils de Visual Web Developer n’inclut pas d’élément de contrôle de contenu. Nous pouvons manuellement taper le balisage déclaratif du contrôle de contenu dans le mode Source, mais une approche plus simple et plus rapide consiste à utiliser la Mode Création. Ouvrez la page `About.aspx` et basculez vers le Mode Création. Comme illustré à la figure 4, le `LeftColumnContent` ContentPlaceHolder apparaît dans le Mode Création ; Si vous pointez avec la souris dessus, le titre affiché indique « LeftColumnContent (Master) ». L’inclusion de « Master » dans le titre indique qu’aucun contrôle de contenu n’est défini dans la page pour ce ContentPlaceHolder. S’il existe un contrôle de contenu pour ContentPlaceHolder, comme dans le cas de `MainContent`, le titre est lu : «*ContentPlaceHolderID* (Custom) ».

Pour ajouter un contrôle de contenu pour l' `LeftColumnContent` ContentPlaceHolder à `About.aspx`, développez la balise active de ContentPlaceHolder, puis cliquez sur le lien créer un contenu personnalisé.

[![le mode Design pour about. aspx affiche LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Figure 04**: la vue de conception pour `About.aspx` affiche le `LeftColumnContent` ContentPlaceHolder ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))

Le fait de cliquer sur le lien créer un contenu personnalisé génère le contrôle de contenu nécessaire dans la page et définit sa propriété `ContentPlaceHolderID` sur le `ID`de ContentPlaceHolder. Par exemple, si vous cliquez sur le lien créer un contenu personnalisé pour `LeftColumnContent` région dans `About.aspx` ajoutez la balise déclarative suivante à la page :

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Omission de contrôles de contenu

ASP.NET ne requiert pas que toutes les pages de contenu incluent des contrôles de contenu pour chaque ContentPlaceHolder défini dans la page maître. Si un contrôle de contenu est omis, le moteur ASP.NET utilise le balisage défini dans ContentPlaceHolder dans la page maître. Ce balisage est appelé le *contenu par défaut* de ContentPlaceHolder et est utile dans les scénarios où le contenu d’une région est commun parmi la majorité des pages, mais doit être personnalisé pour un petit nombre de pages. L’étape 3 explore la spécification du contenu par défaut dans la page maître.

Actuellement, `Default.aspx` contient deux contrôles de contenu pour le `head` et `MainContent` ContentPlaceHolders ; Il n’a pas de contrôle de contenu pour `LeftColumnContent`. Par conséquent, lorsque `Default.aspx` est restitué, le contenu par défaut de `LeftColumnContent` ContentPlaceHolder est utilisé. Étant donné que nous n’avons pas encore défini de contenu par défaut pour ce ContentPlaceHolder, l’effet net est qu’aucun balisage n’est émis pour cette région. Pour vérifier ce comportement, visitez `Default.aspx` via un navigateur. Comme le montre la figure 5, aucun balisage n’est émis dans la colonne de gauche sous la section Actualités.

[![aucun contenu n’est restitué pour LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Figure 05**: aucun contenu n’est restitué pour le `LeftColumnContent` ContentPlaceHolder ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))

## <a name="step-3-specifying-default-content-in-the-master-page"></a>Étape 3 : spécification du contenu par défaut dans la page maître

Certaines conceptions de sites Web incluent une région dont le contenu est le même pour toutes les pages du site, à l’exception d’une ou deux exceptions. Prenons l’exemple d’un site Web qui prend en charge les comptes d’utilisateur. Un tel site nécessite une page de connexion où les visiteurs peuvent entrer leurs informations d’identification pour se connecter au site. Pour accélérer le processus de connexion, les concepteurs de sites Web peuvent inclure des zones de texte nom d’utilisateur et mot de passe dans le coin supérieur gauche de chaque page pour permettre aux utilisateurs de se connecter sans avoir à accéder explicitement à la page de connexion. Bien que ces zones de texte nom d’utilisateur et mot de passe soient utiles dans la plupart des pages, elles sont redondantes dans la page de connexion, qui contient déjà des zones de texte pour les informations d’identification de l’utilisateur.

Pour implémenter cette conception, vous pouvez créer un contrôle ContentPlaceHolder dans l’angle supérieur gauche de la page maître. Chaque page qui devait afficher les zones de texte nom d’utilisateur et mot de passe dans son coin supérieur gauche créera un contrôle de contenu pour ce ContentPlaceHolder et ajoutera l’interface nécessaire. La page de connexion, en revanche, ometrait soit d’ajouter un contrôle de contenu pour ce ContentPlaceHolder, soit de créer un contrôle de contenu sans balisage défini. L’inconvénient de cette approche est que nous devons penser à ajouter les zones de texte nom d’utilisateur et mot de passe à chaque page que nous ajoutons au site (à l’exception de la page de connexion). Cela demande des problèmes. Nous aurions probablement oublié d’ajouter ces zones de texte à une page ou deux ou, pire encore, nous n’implémenterons pas l’interface correctement (peut-être en ajoutant une seule zone de texte au lieu de deux).

Une meilleure solution consiste à définir les zones de texte nom d’utilisateur et mot de passe comme contenu par défaut de ContentPlaceHolder. En procédant ainsi, il vous suffit de remplacer ce contenu par défaut dans les pages qui n’affichent pas les zones de texte nom d’utilisateur et mot de passe (la page de connexion, par exemple). Pour illustrer la spécification du contenu par défaut pour un contrôle ContentPlaceHolder, nous allons implémenter le scénario abordé précédemment.

> [!NOTE]
> Le reste de ce didacticiel met à jour notre site Web pour inclure une interface de connexion dans la colonne de gauche pour toutes les pages, à l’exception de la page de connexion. Toutefois, ce didacticiel n’examine pas comment configurer le site Web pour prendre en charge les comptes d’utilisateur. Pour plus d’informations sur cette rubrique, reportez-vous à mes [formulaires : didacticiels d’authentification, d’autorisation, de comptes d’utilisateurs et de rôles](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Ajout d’un ContentPlaceHolder et spécification de son contenu par défaut

Ouvrez la page maître `Site.master` et ajoutez le balisage suivant à la colonne de gauche entre les `DateDisplay` section étiquette et leçons :

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Après avoir ajouté cette balise, les Mode Création de votre page maître doivent ressembler à la figure 6.

[![la page maître comprend un contrôle de connexion](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Figure 06**: la page maître comprend un contrôle de connexion ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))

Ce ContentPlaceHolder, `QuickLoginUI`, a un contrôle Web de connexion comme son contenu par défaut. Le contrôle de connexion affiche une interface utilisateur qui invite l’utilisateur à entrer son nom d’utilisateur et son mot de passe, ainsi qu’un bouton de connexion. Lorsque vous cliquez sur le bouton se connecter, le contrôle de connexion valide en interne les informations d’identification de l’utilisateur par rapport à l’API d’appartenance. Pour utiliser ce contrôle de connexion dans la pratique, vous devez configurer votre site pour qu’il utilise l’appartenance. Cette rubrique n’est pas traitée dans ce didacticiel. Pour plus d’informations sur la création d’une application Web qui prend en charge les comptes d’utilisateur, consultez les didacticiels sur [l’authentification par formulaire, l’autorisation, les comptes d’utilisateurs et les rôles](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

N’hésitez pas à personnaliser le comportement ou l’apparence du contrôle de connexion. J’ai défini deux de ses propriétés : `TitleText` et `FailureAction`. La valeur de la propriété `TitleText`, qui a comme valeur par défaut « se connecter », est affichée en haut de l’interface utilisateur du contrôle. J’ai défini cette propriété de sorte qu’elle affiche le texte « Sign in » comme élément `<h3>`. La propriété `FailureAction` indique la procédure à suivre si les informations d’identification de l’utilisateur ne sont pas valides. La valeur par défaut est `Refresh`, ce qui laisse l’utilisateur sur la même page et affiche un message d’échec dans le contrôle de connexion. Je l’ai changé en `RedirectToLoginPage`, qui envoie l’utilisateur à la page de connexion en cas d’informations d’identification non valides. Je préfère envoyer l’utilisateur à la page de connexion lorsqu’un utilisateur tente de se connecter à partir d’une autre page, mais échoue, car la page de connexion peut contenir des instructions et des options supplémentaires qui ne s’adaptent pas facilement à la colonne de gauche. Par exemple, la page de connexion peut inclure des options permettant de récupérer un mot de passe oublié ou de créer un nouveau compte.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Création de la page de connexion et remplacement du contenu par défaut

Une fois la page maître terminée, l’étape suivante consiste à créer la page de connexion. Ajoutez une page ASP.NET au répertoire racine de votre site nommé `Login.aspx`, en la liant à la page maître `Site.master`. Vous créerez ainsi une page avec quatre contrôles de contenu, un pour chacun des ContentPlaceHolders définis dans `Site.master`.

Ajoutez un contrôle de connexion au contrôle de contenu `MainContent`. De même, n’hésitez pas à ajouter du contenu à la région `LeftColumnContent`. Toutefois, veillez à conserver le contrôle de contenu pour le `QuickLoginUI` ContentPlaceHolder vide. Cela permet de s’assurer que le contrôle de connexion n’apparaît pas dans la colonne de gauche de la page de connexion.

Après avoir défini le contenu pour les régions `MainContent` et `LeftColumnContent`, le balisage déclaratif de votre page de connexion doit ressembler à ce qui suit :

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

La figure 7 illustre cette page lorsque vous l’affichez dans un navigateur. Étant donné que cette page spécifie un contrôle de contenu pour le `QuickLoginUI` ContentPlaceHolder, elle remplace le contenu par défaut spécifié dans la page maître. L’effet net est que le contrôle de connexion affiché dans le Mode Création de la page maître (voir la figure 6) n’est pas rendu dans cette page.

[![la page de connexion réappuie sur le contenu par défaut de ContentPlaceHolder QuickLoginUI](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Figure 07**: la page de connexion réappuie sur le contenu par défaut de `QuickLoginUI` ContentPlaceHolder ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))

### <a name="using-the-default-content-in-new-pages"></a>Utilisation du contenu par défaut dans les nouvelles pages

Nous souhaitons afficher le contrôle de connexion dans la colonne de gauche pour toutes les pages, à l’exception de la page de connexion. Pour ce faire, toutes les pages de contenu à l’exception de la page de connexion doivent omettre un contrôle de contenu pour l' `QuickLoginUI` ContentPlaceHolder. En omettant un contrôle de contenu, le contenu par défaut de ContentPlaceHolder sera utilisé à la place.

Nos pages de contenu existantes-`Default.aspx`, `About.aspx`et `MultipleContentPlaceHolders.aspx` : n’incluez pas de contrôle de contenu pour `QuickLoginUI`, car ils ont été créés avant que nous ayons ajouté ce contrôle ContentPlaceHolder à la page maître. Par conséquent, ces pages existantes n’ont pas besoin d’être mises à jour. Toutefois, les nouvelles pages ajoutées au site Web incluent, par défaut, un contrôle de contenu pour le `QuickLoginUI` ContentPlaceHolder. Par conséquent, nous devons nous souvenir de supprimer ces contrôles de contenu chaque fois que nous ajoutons une nouvelle page de contenu (sauf si vous souhaitez remplacer le contenu par défaut de ContentPlaceHolder, comme dans le cas de la page de connexion).

Pour supprimer le contrôle de contenu, vous pouvez supprimer manuellement son balisage déclaratif de la vue source ou, à partir de la Mode Création, choisir le lien de contenu par défaut vers le maître de sa balise active. Les deux approches suppriment le contrôle de contenu de la page et produisent le même effet net.

La figure 8 illustre `Default.aspx` affichées dans un navigateur. Rappelez-vous que `Default.aspx` n’a que deux contrôles de contenu spécifiés dans son balisage déclaratif (un pour `head` et un pour `MainContent`. Par conséquent, le contenu par défaut des `LeftColumnContent` et `QuickLoginUI` ContentPlaceHolders s’affiche.

[![le contenu par défaut des ContentPlaceHolders LeftColumnContent et QuickLoginUI sont affichés](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Figure 08**: le contenu par défaut des `LeftColumnContent` et `QuickLoginUI` ContentPlaceHolders s’affiche ([cliquez pour afficher l’image en taille réelle](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))

## <a name="summary"></a>Récapitulatif

Le modèle de page maître ASP.NET autorise un nombre arbitraire de ContentPlaceHolders dans la page maître. De plus, ContentPlaceHolders inclut le contenu par défaut, qui est émis dans le cas où il n’y a aucun contrôle de contenu correspondant dans la page de contenu. Dans ce didacticiel, nous avons vu comment inclure des contrôles ContentPlaceHolder supplémentaires dans la page maître et comment définir des contrôles de contenu pour ces nouveaux ContentPlaceHolders dans les pages ASP.NET nouvelles et existantes. Nous avons également examiné la spécification du contenu par défaut dans un ContentPlaceHolder, ce qui est utile dans les scénarios où seule une minorité de pages doit personnaliser le contenu autrement standardisé dans une certaine région.

Dans le didacticiel suivant, nous examinerons le `head` ContentPlaceHolder plus en détail, en expliquant comment définir de façon déclarative et par programmation le titre, les balises méta et d’autres en-têtes HTML page par page.

Bonne programmation !

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs ouvrages ASP/ASP. net et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 3,5 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott peut être contacté au [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teli Banerjee. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](creating-a-site-wide-layout-using-master-pages-cs.md)
> [Suivant](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
