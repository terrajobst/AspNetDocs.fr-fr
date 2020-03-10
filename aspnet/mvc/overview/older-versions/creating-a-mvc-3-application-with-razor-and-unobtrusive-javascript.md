---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Création d’une application MVC 3 avec Razor et JavaScript discret | Microsoft Docs
author: microsoft
description: L’exemple d’application Web User list montre à quel degré il est simple de créer des applications ASP.NET MVC 3 à l’aide du moteur de vue Razor. L’exemple d’application...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540984"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Création d’une application MVC 3 Application avec Razor et JavaScript non obstrusif

par [Microsoft](https://github.com/microsoft)

> L’exemple d’application Web User list montre à quel degré il est simple de créer des applications ASP.NET MVC 3 à l’aide du moteur de vue Razor. L’exemple d’application montre comment utiliser le nouveau moteur d’affichage Razor avec ASP.NET MVC version 3 et Visual Studio 2010 pour créer un site Web de liste d’utilisateurs fictifs qui comprend des fonctionnalités telles que la création, l’affichage, la modification et la suppression d’utilisateurs.
> 
> Ce didacticiel décrit les étapes qui ont été effectuées pour créer l’exemple d’application ASP.NET MVC 3 de la liste d’utilisateurs. Un projet Visual Studio avec C# le code source vb et est disponible pour accompagner cette rubrique : [Télécharger](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Si vous avez des questions sur ce didacticiel, envoyez-les sur le [Forum MVC](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Présentation

L’application que vous allez générer est un site Web de liste d’utilisateurs simple. Les utilisateurs peuvent entrer, afficher et mettre à jour les informations utilisateur.

![Exemple de site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Vous pouvez télécharger le projet VB C# et le projet terminé [ici](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Création de l’application Web

Pour démarrer le didacticiel, ouvrez Visual Studio 2010 et créez un nouveau projet à l’aide du modèle d' *application Web ASP.NET MVC 3* . Nommez l’application &quot;&quot;Mvc3Razor.

[![un nouveau projet MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Dans la boîte de dialogue **nouveau projet ASP.NET MVC 3** , sélectionnez **application Internet**, sélectionnez le moteur d’affichage Razor, puis cliquez sur **OK**.

![Boîte de dialogue Nouveau projet ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

Dans ce didacticiel, vous n’utiliserez pas le fournisseur d’appartenances ASP.NET. vous pouvez donc supprimer tous les fichiers associés à l’ouverture de session et à l’appartenance. Dans **Explorateur de solutions**, supprimez les fichiers et répertoires suivants :

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (et tous les fichiers de ce répertoire)

![Soln exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Modifiez le fichier <em>\_Layout. cshtml</em> et remplacez le balisage à l’intérieur de l’élément `<div>` nommé `logindisplay` par le message <em>&quot;</em>connexion désactivée&quot;. L’exemple suivant montre le nouveau balisage :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Ajout du modèle

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *modèles* , sélectionnez **Ajouter**, puis cliquez sur **classe**.

![Nouvelle classe de l’utilisateur MDL](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nommez la classe `UserModel`. Remplacez le contenu du fichier *UserModel* par le code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

La classe `UserModel` représente des utilisateurs. Chaque membre de la classe est annoté avec l’attribut [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) de l’espace de noms [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) . Les attributs de l’espace de noms [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fournissent une validation automatique côté client et côté serveur pour les applications Web.

Ouvrez la classe `HomeController` et ajoutez une directive `using` pour pouvoir accéder aux classes `UserModel` et `Users` :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Juste après la déclaration de `HomeController`, ajoutez le commentaire suivant et la référence à une classe `Users` :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

La classe `Users` est un magasin de données en mémoire simplifié que vous allez utiliser dans ce didacticiel. Dans une application réelle, vous utilisez une base de données pour stocker les informations utilisateur. Les premières lignes du fichier `HomeController` sont présentées dans l’exemple suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Générez l’application afin que le modèle utilisateur soit disponible pour l’Assistant de génération de modèles automatique à l’étape suivante.

## <a name="creating-the-default-view"></a>Création de la vue par défaut

L’étape suivante consiste à ajouter une méthode d’action et une vue pour afficher les utilisateurs.

Supprimez le fichier *Views\Home\Index* existant. Vous allez créer un nouveau fichier d' *index* pour afficher les utilisateurs.

Dans la classe `HomeController`, remplacez le contenu de la méthode `Index` par le code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Cliquez avec le bouton droit à l’intérieur de la méthode `Index`, puis cliquez sur **Ajouter une vue**.

![Ajouter une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Sélectionnez l’option **créer un affichage fortement typé** . Pour la **classe de données d’affichage**, sélectionnez **Mvc3Razor. Models. UserModel**. (Si vous ne voyez pas **Mvc3Razor. Models. UserModel** dans la zone **afficher la classe de données** , vous devez générer le projet.) Assurez-vous que le moteur d’affichage est défini sur **Razor**. Définissez **afficher le contenu** sur **liste** , puis cliquez sur **Ajouter**.

![Ajouter une vue d’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nouvelle vue génère automatiquement une structure automatique des données utilisateur passées à la vue `Index`. Examinez le fichier *Views\Home\Index* que vous venez de générer. Les liens **créer**, **modifier**, **Détails**et **supprimer** ne fonctionnent pas, mais le reste de la page est fonctionnel. Exécutez la page. La liste des utilisateurs s’affiche.

![Page d’index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Ouvrez le fichier *index. cshtml* et remplacez le balisage `ActionLink` pour **modifier**, **Détails**et **supprimer** par le code suivant :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Le nom d’utilisateur est utilisé comme ID pour Rechercher l’enregistrement sélectionné dans les liens **modifier**, **Détails**et **supprimer** .

## <a name="creating-the-details-view"></a>Création du mode Détails

L’étape suivante consiste à ajouter une `Details` méthode et une vue d’action pour afficher les détails de l’utilisateur.

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Ajoutez la méthode `Details` suivante au contrôleur d’hébergement :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Cliquez avec le bouton droit à l’intérieur de la méthode `Details`, puis sélectionnez <strong>Ajouter une vue</strong>. Vérifiez que la zone <strong>afficher la classe de données</strong> contient <strong>Mvc3Razor. Models. UserModel</strong><em>.</em> Définissez <strong>afficher le contenu</strong> sur <strong>Détails</strong> , puis cliquez sur <strong>Ajouter</strong>.

![Ajouter un affichage des détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Exécutez l’application et sélectionnez un lien Détails. La génération de modèles automatique affiche chaque propriété dans le modèle.

![Détails](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Création de la vue Edit

Ajoutez la méthode `Edit` suivante au contrôleur d’hébergement.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Ajoutez une vue comme dans les étapes précédentes, mais définissez **afficher le contenu** à **modifier**.

![Ajouter un affichage de modification](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Exécutez l’application et modifiez le prénom et le nom de l’un des utilisateurs. Si vous ne respectez pas les contraintes de `DataAnnotation` qui ont été appliquées à la classe `UserModel`, lorsque vous envoyez le formulaire, vous verrez des erreurs de validation générées par le code serveur. Par exemple, si vous modifiez le prénom &quot;Ann&quot; à &quot;un&quot;, lorsque vous envoyez le formulaire, l’erreur suivante s’affiche sur le formulaire :

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

Dans ce didacticiel, vous traitez le nom d’utilisateur en tant que clé primaire. Par conséquent, la propriété nom d’utilisateur ne peut pas être modifiée. Dans le fichier *Edit. cshtml* , juste après l’instruction `Html.BeginForm`, définissez le nom d’utilisateur en tant que champ masqué. La propriété est alors passée dans le modèle. Le fragment de code suivant montre l’emplacement de l’instruction `Hidden` :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Remplacez le balisage `TextBoxFor` et `ValidationMessageFor` pour le nom d’utilisateur par un appel de `DisplayFor`. La méthode `DisplayFor` affiche la propriété sous la forme d’un élément en lecture seule. L'exemple suivant montre le balisage complet. Les appels de `TextBoxFor` et d' `ValidationMessageFor` d’origine sont commentés avec les caractères de début et de fin Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Activation de la validation côté client

Pour activer la validation côté client dans ASP.NET MVC 3, vous devez définir deux indicateurs et vous devez inclure trois fichiers JavaScript.

Ouvrez le fichier *Web. config* de l’application. Vérifiez `that ClientValidationEnabled` et `UnobtrusiveJavaScriptEnabled` avez la valeur true dans les paramètres de l’application. Le fragment suivant du fichier *Web. config* racine affiche les paramètres corrects :

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

La définition de `UnobtrusiveJavaScriptEnabled` sur true active Ajax discrète et la validation client discrète. Lorsque vous utilisez la validation discrète, les règles de validation sont converties en attributs HTML5. Les noms d’attributs HTML5 ne peuvent comporter que des lettres minuscules, des chiffres et des tirets.

L’affectation de la valeur true à `ClientValidationEnabled` active la validation côté client. En définissant ces clés dans le fichier *Web. config* de l’application, vous activez la validation du client et le JavaScript discret pour l’application entière. Vous pouvez également activer ou désactiver ces paramètres dans des vues individuelles ou dans des méthodes de contrôleur à l’aide du code suivant :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Vous devez également inclure plusieurs fichiers JavaScript dans l’affichage rendu. Un moyen simple d’inclure le JavaScript dans toutes les vues consiste à les ajouter au fichier *Views\Shared\\_Layout. cshtml* . Remplacez l’élément `<head>` du fichier *\_Layout. cshtml* par le code suivant :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Les deux premiers scripts jQuery sont hébergés par le CDN (Content Delivery Network) de Microsoft Ajax. En tirant parti du CDN Microsoft Ajax, vous pouvez améliorer considérablement les performances de vos applications.

Exécutez l’application et cliquez sur un lien modifier. Affichez la source de la page dans le navigateur. La source du navigateur affiche de nombreux attributs sous la forme `data-val` (pour la validation des données). Lorsque la validation du client et le JavaScript discret sont activés, les champs d’entrée avec une règle de validation du client contiennent l’attribut `data-val="true"` pour déclencher une validation client discrète. Par exemple, le champ `City` du modèle a été décoré avec l’attribut [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) , ce qui donne le code html illustré dans l’exemple suivant :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Pour chaque règle de validation client, un attribut ayant la forme `data-val-rulename="message"`est ajouté. À l’aide de l’exemple de champ `City` indiqué plus haut, la règle de validation client requise génère l’attribut `data-val-required` et le message &quot;le champ City est obligatoire&quot;. Exécutez l’application, modifiez l’un des utilisateurs et désactivez le champ `City`. Lorsque vous quittez le champ, un message d’erreur de validation côté client s’affiche.

![Ville obligatoire](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

De même, pour chaque paramètre de la règle de validation client, un attribut qui a la forme `data-val-rulename-paramname=paramvalue`est ajouté. Par exemple, la propriété `FirstName` est annotée avec l’attribut [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) et spécifie une longueur minimale de 3 et une longueur maximale de 8. La règle de validation des données nommée `length` a le nom de paramètre `max` et la valeur de paramètre 8. L’exemple suivant montre le code HTML généré pour le champ `FirstName` lorsque vous modifiez l’un des utilisateurs :

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Pour plus d’informations sur la validation client discrète, consultez l’entrée « [validation client discrète » dans ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) dans le blog de Brad Wilson.

> [!NOTE]
> Dans ASP.NET MVC 3 Beta, vous devez parfois envoyer le formulaire afin de démarrer la validation côté client. Cela peut être modifié pour la version finale.

## <a name="creating-the-create-view"></a>Création de la vue Create

L’étape suivante consiste à ajouter une `Create` méthode et une vue d’action pour permettre à l’utilisateur de créer un nouvel utilisateur. Ajoutez la méthode `Create` suivante au contrôleur d’hébergement :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Ajoutez une vue comme dans les étapes précédentes, mais définissez **afficher le contenu** à **créer**.

![Créer une vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Exécutez l’application, sélectionnez le lien **créer** , puis ajoutez un nouvel utilisateur. La méthode `Create` tire automatiquement parti de la validation côté client et côté serveur. Essayez d’entrer un nom d’utilisateur qui contient un espace blanc, par exemple &quot;&quot;Ben X. Quand vous quittez le champ nom d’utilisateur, une erreur de validation côté client (`White space is not allowed`) s’affiche.

## <a name="add-the-delete-method"></a>Ajouter la méthode Delete

Pour suivre le didacticiel, ajoutez la méthode `Delete` suivante au contrôleur d’hébergement :

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Ajoutez une `Delete` vue comme dans les étapes précédentes, en affectant la valeur **supprimer**au contenu de la **vue** .

![Supprimer la vue](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Vous disposez maintenant d’une application ASP.NET MVC 3 simple mais entièrement fonctionnelle avec validation.
