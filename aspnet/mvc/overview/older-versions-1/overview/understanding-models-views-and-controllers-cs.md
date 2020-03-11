---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Présentation des modèles, des vues et desC#contrôleurs () | Microsoft Docs
author: StephenWalther
description: Vous ne vous inquiétez pas des modèles, des vues et des contrôleurs ? Dans ce didacticiel, Stephen Walther vous présente les différentes parties d’une application ASP.NET MVC.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 57dc82d02d38adc2514aa2c02c6f156ed0fb88a6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600589"
---
# <a name="understanding-models-views-and-controllers-c"></a>Présentation des modèles, des vues et des contrôleurs (C#)

par [Stephen Walther](https://github.com/StephenWalther)

> Vous ne vous inquiétez pas des modèles, des vues et des contrôleurs ? Dans ce didacticiel, Stephen Walther vous présente les différentes parties d’une application ASP.NET MVC.

Ce didacticiel vous fournit une vue d’ensemble des modèles, des vues et des contrôleurs MVC ASP.NET. En d’autres termes, il explique M', V’et C’dans ASP.NET MVC.

Après avoir lu ce didacticiel, vous devez comprendre comment les différentes parties d’une application ASP.NET MVC fonctionnent ensemble. Vous devez également comprendre comment l’architecture d’une application ASP.NET MVC diffère d’une application ASP.NET Web Forms ou d’une application Active Server pages.

## <a name="the-sample-aspnet-mvc-application"></a>Exemple d’application MVC ASP.NET

Le modèle Visual Studio par défaut pour la création d’applications Web ASP.NET MVC comprend un exemple d’application extrêmement simple qui peut être utilisé pour comprendre les différentes parties d’une application ASP.NET MVC. Nous tirant parti de cette application simple dans ce didacticiel.

Vous créez une application MVC ASP.NET avec le modèle MVC en lançant Visual Studio 2008 et en sélectionnant l’option de menu fichier, nouveau projet (voir figure 1). Dans la boîte de dialogue Nouveau projet, sélectionnez votre langage de programmation préféré sous types de C#projets (Visual Basic ou), puis sélectionnez **application Web ASP.NET MVC** sous modèles. Cliquez sur le bouton OK.

[![boîte de dialogue Nouveau projet](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Figure 01**: boîte de dialogue Nouveau projet ([cliquez pour afficher l’image en taille réelle](understanding-models-views-and-controllers-cs/_static/image2.png))

Lorsque vous créez une application ASP.NET MVC, la boîte de dialogue **créer un projet de test unitaire** s’affiche (voir figure 2). Cette boîte de dialogue vous permet de créer un projet distinct dans votre solution pour tester votre application ASP.NET MVC. Sélectionnez l’option **non, ne pas créer de projet de test unitaire** , puis cliquez sur le bouton **OK** .

[![boîte de dialogue créer un test unitaire](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Figure 02**: boîte de dialogue créer un test unitaire ([cliquez pour afficher l’image en taille réelle](understanding-models-views-and-controllers-cs/_static/image4.png))

Après la création de la nouvelle application MVC ASP.NET. Vous verrez plusieurs dossiers et fichiers dans la fenêtre Explorateur de solutions. En particulier, vous verrez trois dossiers nommés modèles, vues et contrôleurs. Comme vous pouvez le deviner dans les noms de dossiers, ces dossiers contiennent les fichiers pour l’implémentation des modèles, des vues et des contrôleurs.

Si vous développez le dossier Controllers, vous devriez voir un fichier nommé AccountController.cs et un fichier nommé HomeController.cs. Si vous développez le dossier views, vous devez voir trois sous-dossiers nommés Account, orig et Shared. Si vous développez le dossier de démarrage, vous verrez deux fichiers supplémentaires nommés about. aspx et index. aspx (voir figure 3). Ces fichiers composent l’exemple d’application inclus avec le modèle ASP.NET MVC par défaut.

[![la fenêtre de Explorateur de solutions](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Figure 03**: fenêtre Explorateur de solutions ([cliquez pour afficher l’image en taille réelle](understanding-models-views-and-controllers-cs/_static/image6.png))

Vous pouvez exécuter l’exemple d’application en sélectionnant l’option de menu **Déboguer, démarrer le débogage**. Vous pouvez également appuyer sur la touche F5.

Lorsque vous exécutez pour la première fois une application ASP.NET, la boîte de dialogue de la figure 4 s’affiche et vous recommande d’activer le mode débogage. Cliquez sur le bouton OK pour que l’application s’exécute.

[boîte de dialogue ![débogage non activé](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Figure 04**: boîte de dialogue Débogage non activé ([cliquez pour afficher l’image en taille réelle](understanding-models-views-and-controllers-cs/_static/image8.png))

Quand vous exécutez une application ASP.NET MVC, Visual Studio lance l’application dans votre navigateur Web. L’exemple d’application se compose uniquement de deux pages : la page d’index et la page about. Lorsque l’application démarre pour la première fois, la page index s’affiche (voir figure 5). Vous pouvez accéder à la page à propos de en cliquant sur le lien du menu situé en haut à droite de l’application.

[![la page d’index](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Figure 05**: page d’index ([cliquez pour afficher l’image en taille réelle](understanding-models-views-and-controllers-cs/_static/image11.png))

Notez les URL dans la barre d’adresses de votre navigateur. Par exemple, lorsque vous cliquez sur le lien du menu à propos, l’URL dans la barre d’adresses du navigateur devient **/Home/about**.

Si vous fermez la fenêtre du navigateur et revenez à Visual Studio, vous ne pourrez pas trouver un fichier avec le chemin d’accès page d’accès à distance/à propos de. Les fichiers n’existent pas. Comment est-ce possible ?

## <a name="a-url-does-not-equal-a-page"></a>Une URL n’est pas égale à une page

Lorsque vous générez une application ASP.NET Web Forms traditionnelle ou une application Active Server pages, il existe une correspondance un-à-un entre une URL et une page. Si vous demandez une page nommée SomePage. aspx à partir du serveur, il existait mieux une page sur le disque nommée SomePage. aspx. Si le fichier SomePage. aspx n’existe pas, vous recevez une erreur **de page Introuvable 404** .

En revanche, lors de la création d’une application MVC ASP.NET, il n’y a aucune correspondance entre l’URL que vous tapez dans la barre d’adresse de votre navigateur et les fichiers que vous trouvez dans votre application. Dans une application MVC ASP.NET, une URL correspond à une action de contrôleur au lieu d’une page sur le disque.

Dans une application ASP.NET ou ASP traditionnelle, les demandes de navigateur sont mappées à des pages. Dans une application MVC ASP.NET, en revanche, les demandes de navigateur sont mappées à des actions de contrôleur. Une application Web Forms ASP.NET est centrée sur le contenu. Une application ASP.NET MVC, en revanche, est centrée sur la logique d’application.

## <a name="understanding-aspnet-routing"></a>Fonctionnement du routage ASP.NET

Une requête de navigateur est mappée à une action de contrôleur via une fonctionnalité de l’infrastructure ASP.NET appelée *routage ASP.net*. Le routage ASP.NET est utilisé par l’infrastructure ASP.NET MVC pour *acheminer* les demandes entrantes vers les actions du contrôleur.

Le routage ASP.NET utilise une table d’itinéraires pour gérer les demandes entrantes. Cette table de routage est créée lors du premier démarrage de votre application Web. La table de routage est configurée dans le fichier global. asax. Le fichier global. asax MVC par défaut est contenu dans la liste 1.

**Liste 1-global. asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Quand une application ASP.NET démarre pour la première fois, l’application\_méthode Start () est appelée. Dans la liste 1, cette méthode appelle la méthode RegisterRoutes () et la méthode RegisterRoutes () crée la table de routage par défaut.

La table de routage par défaut est constituée d’un seul itinéraire. Cet itinéraire par défaut divise toutes les demandes entrantes en trois segments (un segment d’URL est tout ce qui se trouve entre les barres obliques). Le premier segment est mappé à un nom de contrôleur, le deuxième segment est mappé à un nom d’action et le segment final est mappé à un paramètre passé à l’action nommée ID.

Considérez par exemple l’URL suivante :

/Product/Details/3

Cette URL est analysée en trois paramètres, comme suit :

Controller = produit

Action = Détails

Id = 3

L’itinéraire par défaut défini dans le fichier global. asax contient des valeurs par défaut pour les trois paramètres. Le contrôleur par défaut est démarrage, l’action par défaut est index et l’ID par défaut est une chaîne vide. Avec ces valeurs par défaut à l’esprit, réfléchissez à la façon dont l’URL suivante est analysée :

/Employee

Cette URL est analysée en trois paramètres, comme suit :

Controller = employé

action = index

Id = ��

Enfin, si vous ouvrez une application MVC ASP.NET sans fournir d’URL (par exemple, `http://localhost`), l’URL est analysée comme suit :

contrôleur = page d’hébergement

action = index

Id = ��

La demande est routée vers l’action index () sur la classe HomeController.

## <a name="understanding-controllers"></a>Fonctionnement des contrôleurs

Un contrôleur est chargé de contrôler la façon dont un utilisateur interagit avec une application MVC. Un contrôleur contient la logique de contrôle de Flow pour une application MVC ASP.NET. Un contrôleur détermine la réponse à renvoyer à un utilisateur lorsqu’un utilisateur effectue une requête de navigateur.

Un contrôleur est simplement une classe (par exemple, une Visual Basic ou C# une classe). L’exemple d’application ASP.NET MVC comprend un contrôleur nommé HomeController.cs, situé dans le dossier Controllers. Le contenu du fichier HomeController.cs est reproduit dans la liste 2.

**Liste 2-HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Notez que le HomeController a deux méthodes nommées index () et about (). Ces deux méthodes correspondent aux deux actions exposées par le contrôleur. L’URL/Home/Index appelle la méthode HomeController. index () et l’URL/Home/About appelle la méthode HomeController. about ().

Toute méthode publique d’un contrôleur est exposée en tant qu’action de contrôleur. Vous devez faire attention à ce sujet. Cela signifie que toute méthode publique contenue dans un contrôleur peut être appelée par toute personne ayant accès à Internet en entrant l’URL appropriée dans un navigateur.

## <a name="understanding-views"></a>Présentation des vues

Les deux actions de contrôleur exposées par la classe HomeController, index () et about (), retournent toutes deux une vue. Une vue contient le balisage HTML et le contenu qui est envoyé au navigateur. Une vue est l’équivalent d’une page lors de l’utilisation d’une application MVC ASP.NET.

Vous devez créer vos vues à l’emplacement approprié. L’action HomeController. index () renvoie une vue située à l’emplacement suivant :

\Views\Home\Index.aspx

L’action HomeController. about () retourne une vue située à l’emplacement suivant :

\Views\Home\About.aspx

En général, si vous souhaitez retourner une vue pour une action de contrôleur, vous devez créer un sous-dossier dans le dossier views portant le même nom que votre contrôleur. Dans le sous-dossier, vous devez créer un fichier. aspx portant le même nom que l’action du contrôleur.

Le fichier figurant dans la liste 3 contient la vue about. aspx.

**Liste 3-à propos de. aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Si vous ignorez la première ligne de la liste 3, la plupart du reste de la vue se compose du code HTML standard. Vous pouvez modifier le contenu de la vue en entrant le code HTML de votre choix ici.

Une vue est très similaire à une page dans Active Server pages ou ASP.NET Web Forms. Une vue peut contenir du contenu HTML et des scripts. Vous pouvez écrire les scripts dans votre langage de programmation .NET préféré (par exemple C# , ou Visual Basic .net). Vous utilisez des scripts pour afficher du contenu dynamique tel que des données de base de données.

## <a name="understanding-models"></a>Fonctionnement des modèles

Nous avons abordé les contrôleurs et nous avons abordé les vues. La dernière rubrique que nous devons aborder est Models. Qu’est-ce qu’un modèle MVC ?

Un modèle MVC contient toute la logique de votre application qui n’est pas contenue dans une vue ou un contrôleur. Le modèle doit contenir l’ensemble de la logique métier de votre application, la logique de validation et la logique d’accès à la base de données. Par exemple, si vous utilisez l’Entity Framework Microsoft pour accéder à votre base de données, vous créez vos classes Entity Framework (votre fichier. edmx) dans le dossier Models.

Une vue doit contenir uniquement une logique liée à la génération de l’interface utilisateur. Un contrôleur doit contenir uniquement le minimum de logique requis pour retourner la vue de droite ou rediriger l’utilisateur vers une autre action (contrôle de Flow). Tout le reste doit être contenu dans le modèle.

En général, vous devez vous efforcer des modèles FAT et des contrôleurs d’apparence. Vos méthodes de contrôleur ne doivent contenir que quelques lignes de code. Si une action de contrôleur devient trop grasse, vous devez envisager de déplacer la logique vers une nouvelle classe dans le dossier Models.

## <a name="summary"></a>Récapitulatif

Ce didacticiel vous a fourni une vue d’ensemble détaillée des différentes parties d’une application Web ASP.NET MVC. Vous avez appris comment le routage ASP.NET mappe les demandes de navigateur entrantes à des actions de contrôleur particulières. Vous avez appris comment les contrôleurs orchestrent le mode de retour des vues dans le navigateur. Enfin, vous avez appris comment les modèles contiennent une logique d’accès à l’entreprise, à la validation et à la base de données d’application.
