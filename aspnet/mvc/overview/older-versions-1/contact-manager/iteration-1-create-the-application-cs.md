---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: '#1 d’itération : créer l’applicationC#() | Microsoft Docs'
author: microsoft
description: 'Dans la première itération, nous créons le gestionnaire de contacts de la manière la plus simple possible. Nous ajoutons la prise en charge des opérations de base de données de base : créer, lire, mettre à jour et D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: d3a940308f21a4f87bf80249bd465e8812794f68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582130"
---
# <a name="iteration-1--create-the-application-c"></a>#1 d’itération : créer l’applicationC#()

par [Microsoft](https://github.com/microsoft)

[Télécharger le code](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> Dans la première itération, nous créons le gestionnaire de contacts de la manière la plus simple possible. Nous ajoutons la prise en charge des opérations de base de données de base : créer, lire, mettre à jour et supprimer (CRUD).

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Création d’une application MVC ASP.NET de gestion des contacts (VB)

Dans cette série de didacticiels, nous créons une application de gestion de contacts entière du début à la fin. L’application Gestionnaire de contacts vous permet de stocker les informations de contact, les noms de téléphone et les adresses de messagerie, pour obtenir la liste des personnes.

Nous générons l’application sur plusieurs itérations. À chaque itération, nous améliorons progressivement l’application. L’objectif de cette approche à plusieurs itérations est de vous permettre de comprendre la raison de chaque modification.

- #1 d’itération : créez l’application. Dans la première itération, nous créons le gestionnaire de contacts de la manière la plus simple possible. Nous ajoutons la prise en charge des opérations de base de données de base : créer, lire, mettre à jour et supprimer (CRUD).

- Itération #2 : rendez l’application agréable. Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page maître par défaut de la vue MVC ASP.NET et la feuille de style en cascade.

- Itération #3 : ajouter une validation de formulaire. Dans la troisième itération, nous ajoutons la validation de base du formulaire. Nous empêchons les utilisateurs de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendez l’application faiblement couplée. Dans cette quatrième itération, nous tirant parti de plusieurs modèles de conception de logiciels pour faciliter la gestion et la modification de l’application de gestionnaire de contacts. Par exemple, nous refactorisons notre application pour utiliser le modèle de référentiel et le modèle d’injection de dépendances.

- #5 d’itération-créer des tests unitaires. Dans la cinquième itération, nous rendons notre application plus facile à gérer et à modifier en ajoutant des tests unitaires. Nous imitons nos classes de modèle de données et créons des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6-Utilisez le développement piloté par les tests. Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code sur les tests unitaires. Dans cette itération, nous ajoutons des groupes de contacts.

- #7 d’itération-ajoutez des fonctionnalités AJAX. Dans la septième itération, nous améliorons la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

## <a name="this-iteration"></a>Cette itération

Dans cette première itération, nous générons l’application de base. L’objectif est de créer le gestionnaire de contacts de la manière la plus rapide et la plus simple possible. Dans les itérations ultérieures, nous améliorons la conception de l’application.

L’application du gestionnaire de contacts est une application de base basée sur des bases de données. Vous pouvez utiliser l’application pour créer des contacts, modifier des contacts existants et supprimer des contacts.

Dans cette itération, nous allons effectuer les étapes suivantes :

1. Application ASP.NET MVC
2. Créer une base de données pour stocker nos contacts
3. Générer une classe de modèle pour notre base de données avec le Entity Framework Microsoft
4. Créer une action de contrôleur et une vue qui nous permet de répertorier tous les contacts de la base de données
5. Créer des actions de contrôleur et une vue qui nous permet de créer un contact dans la base de données
6. Créer des actions de contrôleur et une vue qui nous permet de modifier un contact existant dans la base de données
7. Créer des actions de contrôleur et une vue qui nous permet de supprimer un contact existant dans la base de données

## <a name="software-prerequisites"></a>Composants logiciels requis

Dans les applications ASP.NET MVC, Visual Studio 2008 ou Visual Web Developer 2008 doit être installé sur votre ordinateur (Visual Web Developer est une version gratuite de Visual Studio qui n’inclut pas toutes les fonctionnalités avancées de Visual Studio). Vous pouvez télécharger la version d’évaluation de Visual Studio 2008 ou Visual Web Developer à partir de l’adresse suivante :

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Pour les applications ASP.NET MVC avec Visual Web Developer, vous devez avoir installé Visual Web Developer Service Pack 1. Sans Service Pack 1, vous ne pouvez pas créer de projets d’application Web.

Framework MVC ASP.NET. Vous pouvez télécharger l’infrastructure MVC ASP.NET à partir de l’adresse suivante :

[https://www.asp.net/mvc](../../../index.md)

Dans ce didacticiel, nous utilisons le Entity Framework Microsoft pour accéder à une base de données. Le Entity Framework est inclus avec .NET Framework 3,5 Service Pack 1. Vous pouvez télécharger cette Service Pack à partir de l’emplacement suivant :

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

En guise d’alternative à l’exécution de chacun de ces téléchargements, vous pouvez tirer parti du Web Platform Installer (Web PI). Vous pouvez télécharger le Web PI à partir de l’adresse suivante :

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Projet MVC ASP.NET

Projet d’application Web MVC ASP.NET. Lancez Visual Studio et sélectionnez l’option de menu **fichier, nouveau projet**. La boîte de dialogue **nouveau projet** s’affiche (voir figure 1). Sélectionnez le type de projet **Web** et le modèle d' **Application Web MVC ASP.net** . Nommez votre nouveau projet *ContactManager* et cliquez sur le bouton OK.

Assurez-vous que .NET Framework 3,5 est sélectionné dans la liste déroulante en haut à droite de la boîte de dialogue **nouveau projet** . Dans le cas contraire, le modèle d’application Web MVC ASP.NET ne s’affiche pas.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figure 01**: boîte de dialogue Nouveau projet ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image2.png))

Application ASP.NET MVC, la boîte de dialogue **créer un projet de test unitaire** s’affiche. Vous pouvez utiliser cette boîte de dialogue pour indiquer que vous souhaitez créer et ajouter un projet de test unitaire à votre solution lorsque vous créez votre application ASP.NET MVC. Bien que nous ne générions pas de tests unitaires dans cette itération, vous devez sélectionner l’option **Oui, créer un projet de test unitaire** , car nous prévoyons d’ajouter des tests unitaires dans une itération ultérieure. L’ajout d’un projet de test lorsque vous créez pour la première fois un projet MVC ASP.NET est beaucoup plus facile que l’ajout d’un projet de test après la création du projet MVC ASP.NET.

> [!NOTE] 
> 
> Étant donné que Visual Web Developer ne prend pas en charge les projets de test, vous ne recevez pas la boîte de dialogue créer un projet de test unitaire lors de l’utilisation de Visual Web Developer.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figure 02**: boîte de dialogue créer un projet de test unitaire ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image4.png))

L’application ASP.NET MVC s’affiche dans la fenêtre de Explorateur de solutions Visual Studio (voir figure 3). Si vous ne voyez pas la fenêtre de Explorateur de solutions, vous pouvez ouvrir cette fenêtre en sélectionnant la **vue de menu, Explorateur de solutions**. Notez que la solution contient deux projets : le projet MVC ASP.NET et le projet de test. Le projet MVC ASP.NET est nommé ContactManager et le projet de test est nommé ContactManager. tests.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figure 03**: fenêtre Explorateur de solutions ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image6.png))

## <a name="deleting-the-project-sample-files"></a>Suppression des fichiers d’exemple de projet

Le modèle de projet MVC ASP.NET comprend des exemples de fichiers pour les contrôleurs et les vues. Avant de créer une nouvelle application ASP.NET MVC, vous devez supprimer ces fichiers. Vous pouvez supprimer des fichiers et des dossiers dans la fenêtre Explorateur de solutions en cliquant avec le bouton droit sur un fichier ou un dossier et en sélectionnant l’option de menu **supprimer**.

Vous devez supprimer les fichiers suivants du projet MVC ASP.NET :

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Et, vous devez supprimer le fichier suivant du projet de test :

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Création de la base de données

L’application du gestionnaire de contacts est une application Web basée sur la base de données. Nous utilisons une base de données pour stocker les informations de contact.

L’infrastructure MVC ASP.NET avec toute base de données moderne, y compris Microsoft SQL Server, Oracle, MySQL et les bases de données IBM DB2. Dans ce didacticiel, nous utilisons une base de données Microsoft SQL Server. Lorsque vous installez Visual Studio, vous avez la possibilité d’installer Microsoft SQL Server Express qui est une version gratuite de la base de données Microsoft SQL Server.

Pour créer une base de données, cliquez avec le bouton droit sur le dossier de données de l’application\_dans la fenêtre Explorateur de solutions et sélectionnez l’option de menu **Ajouter, nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez la catégorie de **données** et le modèle **de base de données SQL Server** (voir figure 4). Nommez la nouvelle base de données ContactManagerDB. mdf, puis cliquez sur le bouton OK.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figure 04**: création d’une nouvelle base de données Microsoft SQL Server Express ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image8.png))

Une fois la base de données créée, la base de données s’affiche dans le dossier application\_données de la fenêtre Explorateur de solutions. Double-cliquez sur le fichier ContactManager. mdf pour ouvrir la fenêtre de Explorateur de serveurs et connectez-vous à la base de données.

> [!NOTE] 
> 
> La fenêtre Explorateur de serveurs est appelée fenêtre explorateur de base de données dans le cas de Microsoft Visual Web Developer.

Vous pouvez utiliser la fenêtre Explorateur de serveurs pour créer des objets de base de données tels que des tables de base de données, des vues, des déclencheurs et des procédures stockées. Cliquez avec le bouton droit sur le dossier tables et sélectionnez l’option de menu **Ajouter une nouvelle table**. La Concepteur de tables de base de données s’affiche (voir figure 5).

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figure 05**: concepteur de tables de la base de données ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image10.png))

Nous devons créer une table qui contient les colonnes suivantes :

<a id="0.1_table01"></a>

| **Nom de la colonne** | **Type de données** | **Null autorisé** |
| --- | --- | --- |
| Id | int | false |
| FirstName | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Phone | nvarchar(50) | false |
| Messagerie | nvarchar(255) | false |

La première colonne, la colonne ID, est spéciale. Vous devez marquer la colonne ID comme une colonne d’identité et une colonne de clé primaire. Vous indiquez qu’une colonne est une colonne d’identité en développant les propriétés des colonnes (regardez au bas de la figure 6) et en faisant défiler jusqu’à la propriété spécification de l’identité. Affectez à la propriété **(is Identity)** la valeur **Yes**.

Vous marquez une colonne en tant que colonne de clé primaire en sélectionnant la colonne et en cliquant sur le bouton avec l’icône d’une clé. Une fois qu’une colonne est marquée comme colonne de clé primaire, une icône de clé apparaît en regard de la colonne (voir figure 6).

Une fois que vous avez terminé la création de la table, cliquez sur le bouton Enregistrer (le bouton avec une icône de disquette) pour enregistrer la nouvelle table. Donnez à votre nouvelle table le nom *contacts*.

À la fin de la création de la table de base de données contacts, vous devez ajouter des enregistrements à la table. Cliquez avec le bouton droit sur la table contacts dans la fenêtre Explorateur de serveurs et sélectionnez l’option de menu **afficher les données**de la table. Entrez un ou plusieurs contacts dans la grille qui s’affiche.

## <a name="creating-the-data-model"></a>Création du modèle de données

L’application ASP.NET MVC se compose de modèles, de vues et de contrôleurs. Nous commençons par créer une classe de modèle qui représente la table contacts que nous avons créée dans la section précédente.

Dans ce didacticiel, nous utilisons le Entity Framework Microsoft pour générer automatiquement une classe de modèle à partir de la base de données.

> [!NOTE] 
> 
> L’infrastructure MVC ASP.NET n’est pas liée à la Entity Framework Microsoft de quelque manière que ce soit. Vous pouvez utiliser ASP.NET MVC avec d’autres technologies d’accès aux bases de données, notamment NHibernate, LINQ to SQL ou ADO.NET.

Pour créer les classes de modèle de données, procédez comme suit :

1. Cliquez avec le bouton droit sur le dossier modèles dans la fenêtre Explorateur de solutions, puis sélectionnez **Ajouter, nouvel élément**. La boîte de dialogue **Ajouter un nouvel élément** s’affiche (voir figure 6).
2. Sélectionnez la catégorie de **données** et le modèle de **Entity Data Model ADO.net** . Nommez votre modèle de données *ContactManagerModel. edmx* , puis cliquez sur le bouton **Ajouter** . L’Assistant Entity Data Model s’affiche (voir la figure 7).
3. Dans l’étape **choisir le contenu du modèle** , sélectionnez **générer à partir de la base de données** (voir la figure 7).
4. Dans l’étape **choisir votre connexion de données** , sélectionnez la base de données ContactManagerDB. mdf, puis entrez le nom *ContactManagerDBEntities* pour les paramètres de connexion de l’entité (voir figure 8).
5. Dans l’étape **choisir vos objets de base de données** , activez la case à cocher tables (voir figure 9). Le modèle de données inclut toutes les tables contenues dans votre base de données (il n’y en a qu’une, la table contacts). Entrez les *modèles*d’espace de noms. Cliquez sur le bouton Terminer pour terminer l’Assistant.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figure 06**: boîte de dialogue Ajouter un nouvel élément ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image12.png))

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figure 07**: choisir le contenu du modèle ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image14.png))

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figure 08**: choisir votre connexion de données ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image16.png))

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figure 09**: choisir vos objets de base de données ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image18.png))

Une fois que vous avez terminé l’Assistant Entity Data Model, le concepteur de Entity Data Model s’affiche. Le concepteur affiche une classe qui correspond à chaque table en cours de modélisation. Vous devez voir une classe nommée contacts.

L’Assistant Entity Data Model génère des noms de classes basés sur des noms de tables de base de données. Vous devez presque toujours modifier le nom de la classe générée par l’Assistant. Cliquez avec le bouton droit sur la classe contacts dans le concepteur, puis sélectionnez l’option de menu **Renommer**. Remplacez le nom de la classe contacts (pluriel) par contact (singulier). Une fois que vous avez modifié le nom de la classe, la classe doit apparaître comme figure 10.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figure 10**: classe contact ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image20.png))

À ce stade, nous avons créé notre modèle de base de données. Nous pouvons utiliser la classe contact pour représenter un enregistrement de contact particulier dans notre base de données.

## <a name="creating-the-home-controller"></a>Création du contrôleur d’hébergement

L’étape suivante consiste à créer notre contrôleur d’hébergement. Le contrôleur d’hébergement est le contrôleur par défaut appelé dans une application ASP.NET MVC.

Créez la classe de contrôleur d’hébergement en cliquant avec le bouton droit sur le dossier Controllers dans la fenêtre Explorateur de solutions et en sélectionnant l’option de menu **Ajouter, contrôleur** (voir figure 11). Remarquez la case à cocher **Ajouter des méthodes d’action pour les scénarios de création, de mise à jour et de détails**. Assurez-vous que cette case à cocher est activée avant de cliquer sur le bouton **Ajouter** .

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figure 11**: ajout du contrôleur de démarrage ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image22.png))

Lorsque vous créez le contrôleur d’hébergement, vous recevez la classe dans la liste 1.

**Liste 1-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Liste des contacts

Pour afficher les enregistrements dans la table de base de données de contacts, nous devons créer une action d’index () et une vue d’index.

Le contrôleur d’hébergement contient déjà une action d’index (). Nous devons modifier cette méthode pour qu’elle ressemble à la liste 2.

**Liste 2-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Notez que la classe de contrôleur d’hébergement de la liste 2 contient un champ privé nommé \_entités. Le champ entités \_représente les entités du modèle de données. Nous utilisons le champ entités \_pour communiquer avec la base de données.

La méthode index () renvoie une vue qui représente tous les contacts de la table de base de données contacts. Expression \_entités. ContactSet. ToList () renvoie la liste des contacts sous la forme d’une liste générique.

Maintenant que nous avons créé le contrôleur d’index, nous devons ensuite créer la vue d’index. Avant de créer la vue d’index, compilez votre application en sélectionnant l’option de menu **Générer, générer la solution**. Vous devez toujours compiler votre projet avant d’ajouter une vue pour que la liste des classes de modèle s’affiche dans la boîte de dialogue **Ajouter une vue** .

Pour créer la vue index, cliquez avec le bouton droit sur la méthode index () et sélectionnez l’option de menu **Ajouter une vue** (voir figure 12). La sélection de cette option de menu ouvre la boîte de dialogue **Ajouter une vue** (voir figure 13).

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figure 12**: ajout de la vue d’index ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image24.png))

Dans la boîte de dialogue **Ajouter une vue** , activez la case à cocher **créer une vue fortement typée**. Sélectionnez la classe de données d’affichage ContactManager. Models. contact et la liste afficher le contenu. La sélection de ces options génère une vue qui affiche une liste d’enregistrements de contact.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figure 13**: boîte de dialogue Ajouter une vue ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image26.png))

Lorsque vous cliquez sur le bouton **Ajouter** , la vue index de la liste 3 est générée. Remarquez la directive &lt;% @ page%&gt; qui apparaît en haut du fichier. La vue d’index hérite de la classe de &gt; ViewPage&lt;IEnumerable&lt;ContactManager. Models. contact&gt;. En d’autres termes, la classe Model de la vue représente une liste d’entités contact.

Le corps de la vue index contient une boucle foreach qui itère au sein de chacun des contacts représentés par la classe Model. La valeur de chaque propriété de la classe contact s’affiche dans un tableau HTML.

**Liste 3-Views\Home\Index.aspx (non modifié)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Nous devons apporter une modification à la vue index. Étant donné que nous n’avons pas créé un affichage des détails, nous pouvons supprimer le lien Détails. Recherchez et supprimez le code suivant dans la vue index :

{ID = élément. ID})%&gt;

Après avoir modifié la vue d’index, vous pouvez exécuter l’application du gestionnaire de contacts. Sélectionnez l’option de menu Déboguer, démarrer le débogage ou appuyez simplement sur F5. La première fois que vous exécutez l’application, vous recevez la boîte de dialogue dans la figure 14. Sélectionnez l’option **modifier le fichier Web. config pour activer le débogage** , puis cliquez sur le bouton OK.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figure 14**: activation du débogage ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image28.png))

La vue index est retournée par défaut. Cette vue répertorie toutes les données de la table de base de données de contacts (voir figure 15).

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figure 15**: vue d’index ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image30.png))

Notez que la vue index comprend un lien intitulé créer nouveau en bas de la vue. Dans la section suivante, vous allez apprendre à créer des contacts.

## <a name="creating-new-contacts"></a>Création de nouveaux contacts

Pour permettre aux utilisateurs de créer des contacts, nous devons ajouter deux actions de création () au contrôleur d’hébergement. Nous devons créer une action Create () qui retourne un formulaire HTML pour la création d’un nouveau contact. Nous devons créer une deuxième action de création () qui effectue l’insertion de base de données réelle du nouveau contact.

Les nouvelles méthodes Create () que nous devons ajouter au contrôleur de démarrage sont contenues dans la liste 4.

**Liste 4-Controllers\HomeController.cs (avec les méthodes Create)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

La première méthode Create () peut être appelée à l’aide d’une requête HTTP d’extraction tandis que la deuxième méthode Create () peut être appelée uniquement par une requête HTTP. En d’autres termes, la deuxième méthode Create () peut être appelée uniquement lors de la publication d’un formulaire HTML. La première méthode Create () retourne simplement une vue qui contient le formulaire HTML pour la création d’un nouveau contact. La deuxième méthode Create () est bien plus intéressante : elle ajoute le nouveau contact à la base de données.

Notez que la deuxième méthode Create () a été modifiée pour accepter une instance de la classe contact. Les valeurs de formulaire publiées à partir du formulaire HTML sont liées automatiquement à cette classe de contact par l’infrastructure MVC ASP.NET. Chaque champ de formulaire du formulaire HTML créer est affecté à une propriété du paramètre contact.

Notez que le paramètre contact est décoré avec un attribut [Bind]. L’attribut [Bind] est utilisé pour exclure la propriété d’ID de contact de la liaison. Étant donné que la propriété ID représente une propriété Identity, nous ne voulons pas définir la propriété ID.

Dans le corps de la méthode Create (), le Entity Framework est utilisé pour insérer le nouveau contact dans la base de données. Le nouveau contact est ajouté à l’ensemble de contacts existant et la méthode SaveChanges () est appelée pour envoyer ces modifications à la base de données sous-jacente.

Vous pouvez générer un formulaire HTML pour créer des contacts en cliquant avec le bouton droit sur l’une des deux méthodes Create () et en sélectionnant l’option de menu **Ajouter une vue** (voir figure 16).

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figure 16**: ajout de la vue Create ([Cliquer pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image32.png))

Dans la boîte de dialogue **Ajouter une vue** , sélectionnez la classe **ContactManager. Models. contact** et l’option **créer** pour afficher le contenu (voir figure 17). Lorsque vous cliquez sur le bouton **Ajouter** , une vue créer est générée automatiquement.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figure 17**: affichage d’une explosion de page ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image34.png))

L’affichage créer contient des champs de formulaire pour chacune des propriétés de la classe contact. Le code de la vue Create est inclus dans la liste 5.

**Liste 5-Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Après avoir modifié les méthodes Create () et ajouté la vue Create, vous pouvez exécuter l’application du gestionnaire de contacts et créer des contacts. Cliquez sur le lien **créer** qui apparaît dans la vue index pour accéder à la vue Create. Vous devez voir la vue dans la figure 18.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figure 18**: vue Create ([Cliquer pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image36.png))

## <a name="editing-contacts"></a>Modification des contacts

L’ajout de la fonctionnalité de modification d’un enregistrement de contact est très similaire à l’ajout de la fonctionnalité de création d’enregistrements de contact. Tout d’abord, nous devons ajouter deux nouvelles méthodes de modification à la classe de contrôleur d’hébergement. Ces nouvelles méthodes Edit () sont contenues dans la liste 6.

**Liste 6-Controllers\HomeController.cs (avec les méthodes d’édition)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

La première méthode Edit () est appelée par une opération HTTP. Un paramètre ID est passé à cette méthode, qui représente l’ID de l’enregistrement de contact en cours de modification. Le Entity Framework est utilisé pour récupérer un contact qui correspond à l’ID. Une vue qui contient un formulaire HTML pour la modification d’un enregistrement est retournée.

La deuxième méthode Edit () effectue la mise à jour réelle de la base de données. Cette méthode accepte une instance de la classe contact comme paramètre. L’infrastructure MVC ASP.NET lie automatiquement les champs de formulaire du formulaire de modification à cette classe. Notez que vous ne pouvez pas inclure l’attribut [Bind] lors de la modification d’un contact (nous avons besoin de la valeur de la propriété ID).

Le Entity Framework est utilisé pour enregistrer le contact modifié dans la base de données. Le contact d’origine doit d’abord être récupéré de la base de données. Ensuite, la méthode Entity Framework ApplyPropertyChanges () est appelée pour enregistrer les modifications apportées au contact. Enfin, la méthode Entity Framework SaveChanges () est appelée pour rendre persistantes les modifications apportées à la base de données sous-jacente.

Vous pouvez générer la vue qui contient le formulaire de modification en cliquant avec le bouton droit sur la méthode Edit () et en sélectionnant l’option de menu Ajouter une vue. Dans la boîte de dialogue Ajouter une vue, sélectionnez la classe **ContactManager. Models. contact** et le contenu de la vue **Edit** (voir figure 19).

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figure 19**: ajout d’une vue de modification ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image38.png))

Lorsque vous cliquez sur le bouton Ajouter, une nouvelle vue Edition est générée automatiquement. Le formulaire HTML généré contient des champs qui correspondent à chacune des propriétés de la classe contact (voir la liste 7).

**Liste 7-Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Suppression de contacts

Si vous souhaitez supprimer des contacts, vous devez ajouter deux actions de suppression () à la classe de contrôleur d’hébergement. La première action de suppression () affiche un formulaire de confirmation de suppression. La deuxième action DELETE () effectue la suppression réelle.

> [!NOTE] 
> 
> Plus tard, dans #7 d’itération, nous modifions le gestionnaire de contacts afin qu’il prenne en charge une seule étape de la suppression Ajax.

Les deux nouvelles méthodes Delete () sont contenues dans la liste 8.

**Liste 8-Controllers\HomeController.cs (méthodes Delete)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

La première méthode Delete () renvoie un formulaire de confirmation pour la suppression d’un enregistrement de contact de la base de données (voir Figure20). La deuxième méthode Delete () effectue l’opération de suppression proprement dite sur la base de données. Une fois le contact d’origine récupéré de la base de données, les méthodes Entity Framework SupprimerObjet () et SaveChanges () sont appelées pour effectuer la suppression de la base de données.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figure 20**: vue de confirmation de la suppression ([cliquez pour afficher l’image en plein écran](iteration-1-create-the-application-cs/_static/image40.png))

Nous devons modifier la vue index afin qu’elle contienne un lien permettant de supprimer les enregistrements de contact (voir figure 21). Vous devez ajouter le code suivant à la même cellule de table qui contient le lien modifier :

Html. ActionLink ({ID = élément. ID})%&gt;

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figure 21**: vue d’index avec modification du lien ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image42.png))

Ensuite, nous devons créer la vue de confirmation de suppression. Cliquez avec le bouton droit sur la méthode Delete () dans la classe du contrôleur d’hébergement et sélectionnez l’option de menu Ajouter une vue. La boîte de dialogue Ajouter une vue s’affiche (voir figure 22).

Contrairement à ce qui se passe dans le cas des vues List, Create et Edit, la boîte de dialogue Ajouter une vue ne contient pas d’option permettant de créer une vue Delete. Au lieu de cela, sélectionnez la classe de données **ContactManager. Models. contact** et le contenu de la vue **vide** . Si vous sélectionnez l’option Afficher le contenu vide, nous vous demanderons de créer la vue nous-même.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Figure 22**: ajout de la vue de confirmation de la suppression ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image44.png))

Le contenu de la vue Delete est contenu dans la liste 9. Cette vue contient un formulaire qui confirme si un contact particulier doit être supprimé (voir figure 21).

**Liste 9-Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Modification du nom du contrôleur par défaut

Il se peut que le nom de notre classe de contrôleur pour l’utilisation des contacts soit nommé la classe HomeController. Le contrôleur ne doit-il pas être nommé ContactController ?

Ce problème est assez facile à corriger. Tout d’abord, nous devons Refactoriser le nom du contrôleur d’hébergement. Ouvrez la classe HomeController dans l’éditeur de Visual Studio Code, cliquez avec le bouton droit sur le nom de la classe et sélectionnez l’option de menu **Refactoriser, renommer**. La sélection de cette option de menu ouvre la boîte de dialogue Renommer.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figure 23**: refactorisation d’un nom de contrôleur ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image46.png))

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figure 24**: utilisation de la boîte de dialogue Renommer ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image48.png))

Si vous renommez votre classe de contrôleur, Visual Studio met également à jour le nom du dossier dans le dossier views. Visual Studio renomme le dossier \Views\Home dans le dossier \Views\Contact

Une fois cette modification apportée, votre application n’a plus de contrôleur d’hébergement. Lorsque vous exécutez votre application, vous obtenez la page d’erreur dans la figure 25.

[![la boîte de dialogue Nouveau projet](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figure 25**: aucun contrôleur par défaut ([cliquez pour afficher l’image en taille réelle](iteration-1-create-the-application-cs/_static/image50.png))

Nous devons mettre à jour l’itinéraire par défaut dans le fichier global. asax pour utiliser le contrôleur de contact au lieu du contrôleur d’hébergement. Ouvrez le fichier global. asax et modifiez le contrôleur par défaut utilisé par l’itinéraire par défaut (voir le Listing 10).

**Liste 10-Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Une fois ces modifications effectuées, le gestionnaire de contacts s’exécutera correctement. À présent, il utilise la classe de contrôleur de contact comme contrôleur par défaut.

## <a name="summary"></a>Récapitulatif

Dans cette première itération, nous avons créé une application de gestionnaire de contacts de base de la manière la plus rapide possible. Nous avons tiré parti de Visual Studio pour générer automatiquement le code initial de nos contrôleurs et vues. Nous avons également tiré parti de la Entity Framework pour générer automatiquement nos classes de modèle de base de données.

Actuellement, nous pouvons répertorier, créer, modifier et supprimer des enregistrements de contact à l’aide de l’application de gestionnaire de contacts. En d’autres termes, nous pouvons effectuer toutes les opérations de base de données requises par une application Web basée sur la base de données.

Malheureusement, notre application rencontre des problèmes. Tout d’abord, j’hésite à admettre cela, l’application du gestionnaire de contacts n’est pas l’application la plus intéressante. Un travail de conception est nécessaire. Dans l’itération suivante, nous allons voir comment vous pouvez modifier la page maître et la feuille de style en cascade par défaut pour améliorer l’apparence de l’application.

Deuxièmement, nous n’avons implémenté aucune validation de formulaire. Par exemple, rien ne vous empêche de soumettre le formulaire créer un contact sans entrer de valeurs pour les champs de formulaire. En outre, vous pouvez entrer des numéros de téléphone et des adresses de messagerie non valides. Nous commençons à aborder le problème de la validation de formulaire dans l' #3 des itérations.

Enfin, et plus important encore, l’itération actuelle de l’application du gestionnaire de contacts ne peut pas être facilement modifiée ou gérée. Par exemple, la logique d’accès à la base de données est intégrée aux actions du contrôleur. Cela signifie que nous ne pouvons pas modifier notre code d’accès aux données sans modifier nos contrôleurs. Dans les itérations ultérieures, nous explorons les modèles de conception de logiciels que nous pouvons implémenter pour améliorer la résistance du gestionnaire de contacts à la modification.

> [!div class="step-by-step"]
> [Next](iteration-2-make-the-application-look-nice-cs.md)
