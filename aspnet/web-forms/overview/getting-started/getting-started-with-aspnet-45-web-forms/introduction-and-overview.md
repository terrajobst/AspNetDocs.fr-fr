---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Prise en main avec ASP.NET 4,7 Web Forms et Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Cette série de didacticiels pas à pas vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,7 et Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568221"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Prise en main avec ASP.NET 4,5 Web Forms et Visual Studio 2017

[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Cette série de didacticiels vous montre comment créer une application ASP.NET Web Forms avec ASP.NET 4,5 et Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Introduction

Cette série de didacticiels vous guide tout au long de la création d’une application ASP.NET Web Forms à l’aide de Visual Studio 2017 et ASP.NET 4,5. Vous allez créer une application nommée **Wingtip Toys** : site Web vitrine simplifié vendre des éléments en ligne. Au cours de la série, les nouvelles fonctionnalités ASP.NET 4,5 sont mises en surbrillance.

### <a name="target-audience"></a>Public cible

Les développeurs qui débutent avec ASP.NET Web Forms sont le public cible de cette série de didacticiels.

Vous devez avoir des connaissances dans les domaines suivants :

- Programmation orientée objet (OOP) et langues
- Développement Web (HTML, CSS, JavaScript)
- Bases de données relationnelles
- Architecture multiniveau

Pour passer en revue ces domaines, pensez à étudier le contenu suivant :

- [Prise en main avec VisualC#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Développement Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, php, jQuery](http://w3schools.com/)
- [Base de données relationnelle](http://en.wikipedia.org/wiki/Relational_database)
- [Architecture à plusieurs niveaux](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Fonctionnalités de l’application

Les fonctionnalités de Web Form ASP.NET présentées dans cette série sont les suivantes :

- Projet d’application Web (pas un projet de site Web)
- Web Forms
- Pages maîtres, configuration
- Bootstrap
- Entity Framework Code First, base de données locale
- Validation de la demande
- Contrôles de données fortement typés
- Liaison de modèle
- Annotations de données
- Fournisseurs de valeurs
- SSL et OAuth
- ASP.NET Identity, configuration et autorisation
- Validation discrète
- Routage
- Gestion des erreurs ASP.NET

### <a name="application-scenarios-and-tasks"></a>Scénarios et tâches d’application

Les tâches de la série de didacticiels sont les suivantes :

- Création, examen et exécution d’un nouveau projet
- Création d’une structure de base de données
- Initialisation et amorçage d’une base de données
- Personnalisation de l’interface utilisateur avec des styles, des graphiques et une page maître
- Ajout de pages et de navigation
- Affichage des détails des menus et des données sur les produits
- Création d’un panier d’achat
- Ajout de la prise en charge de SSL et OAuth
- Ajout d’un mode de paiement
- Inclusion d’un rôle d’administrateur et d’un utilisateur à l’application
- Restriction de l’accès à des pages et des dossiers spécifiques
- Chargement d’un fichier dans l’application Web
- Implémentation de la validation des entrées
- Inscription des itinéraires pour l’application Web
- Implémentation de la gestion des erreurs et de la journalisation des erreurs

## <a name="overview"></a>Présentation

Cette série de didacticiels s’adresse aux personnes connaissant les concepts de programmation, mais qui sont nouvelles dans ASP.NET Web Forms. Si vous êtes déjà familiarisé avec ASP.NET Web Forms, cette série peut encore vous aider à en savoir plus sur les nouvelles fonctionnalités de ASP.NET 4,5. Pour les lecteurs qui ne sont pas familiarisés avec les concepts de programmation et les Web Forms ASP.NET, consultez les didacticiels de Web Forms supplémentaires fournis dans la section [prise en main](../../../index.md) sur le site Web ASP.net.

Le ASP.NET 4,5 fourni dans cette série de didacticiels comprend les fonctionnalités suivantes :

- Interface utilisateur simple pour la création de projets qui offre la [prise en charge de nombreuses infrastructures ASP.net](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC et API Web).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), mise en page, thèmes et infrastructure de conception réactive.
- [ASP.net Identity](../../../../identity/index.md), un nouveau système d’appartenance ASP.net qui fonctionne de la même façon dans toutes les infrastructures ASP.net et fonctionne avec les logiciels d’hébergement Web autres qu’IIS.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Une mise à jour de la Entity Framework vous permettant d’effectuer les opérations suivantes :
  - Récupérer et manipuler des données en tant qu’objets fortement typés
  - Accéder aux données de manière asynchrone
  - Gérer les erreurs temporaires de connexion
  - Journaux des instructions SQL

Pour obtenir une liste complète des fonctionnalités ASP.NET 4,5, consultez [ASP.net et Web Tools pour Visual Studio 2013 notes de publication](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Exemple d’application Wingtip Toys

Les captures d’écran suivantes proviennent de l’application ASP.NET Web Forms que vous créez dans cette série de didacticiels. Quand vous exécutez l’application dans Visual Studio, la page d’hébergement Web suivante s’affiche.

![Wingtip Toys-page par défaut](introduction-and-overview/_static/image1.png)

Vous pouvez vous inscrire en tant que nouvel utilisateur ou vous connecter en tant qu’utilisateur existant. Le volet de navigation supérieur contient des liens vers les catégories de produits et leurs produits à partir de la base de données.

Si vous sélectionnez **produits**, tous les produits disponibles s’affichent. 

![Wingtip Toys-produits](introduction-and-overview/_static/image2.png)

Si vous sélectionnez un produit spécifique, les détails du produit s’affichent.

![Wingtip Toys-détails sur le produit](introduction-and-overview/_static/image3.png)

En tant qu’utilisateur, vous pouvez vous inscrire et vous connecter avec Web Forms fonctionnalité par défaut du modèle. Ce didacticiel explique également comment se connecter à l’aide d’un compte Gmail existant. En outre, vous pouvez vous connecter en tant qu’administrateur pour ajouter et supprimer des produits dans la base de données.

![Wingtip Toys-connexion](introduction-and-overview/_static/image4.png)

Une fois que vous êtes connecté en tant qu’utilisateur, vous pouvez ajouter des produits au panier d’achat et le valider avec PayPal. L’exemple d’application est conçu pour fonctionner dans le bac à sable (sandbox) du développeur PayPal. Aucune transaction financière réelle n’a lieu.

![Wingtip Toys-panier d’achat](introduction-and-overview/_static/image5.png)

PayPal confirme vos informations de compte, de commande et de paiement.

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

Après avoir retourné PayPal, vous pouvez passer en revue et terminer votre commande.

![Wingtip Toys-revue des commandes](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Conditions préalables requises

Avant de commencer, assurez-vous que les logiciels suivants sont installés sur votre ordinateur :

- [Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

Le .NET Framework est installé automatiquement.

Cette série de didacticiels utilise Microsoft Visual Studio Community 2017. Vous pouvez utiliser ce ou Microsoft Visual Studio 2017 pour suivre cette série de didacticiels.

Notez les points suivants à propos de Visual Studio :

* Microsoft Visual Studio 2017 et Microsoft Visual Studio Community 2017 sont connus sous le terme de *Visual Studio* dans cette série de didacticiels.

* Visual Studio 2017 est installé en regard des anciennes versions déjà installées. Les sites créés dans des versions antérieures peuvent être ouverts dans Visual Studio 2017 et continuer à s’ouvrir dans les versions précédentes.

* La première fois que vous démarrez Visual Studio, il est supposé que vous avez sélectionné les paramètres de *développement Web* . Pour plus d’informations, consultez [Comment : sélectionner les paramètres de l’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).

Après avoir installé les composants requis, vous êtes prêt à commencer la création du projet Web présenté dans cette série de didacticiels.

## <a name="download-the-sample-application"></a>Téléchargement de l'exemple d'application

 Vous pouvez télécharger l’exemple d’application terminé à tout moment à partir du site d’exemples MSDN :

[Prise en main avec ASP.NET 4,5 Web Forms et Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Ce téléchargement contient les éléments suivants :

- L’exemple d’application dans le dossier *WingtipToys* .
- Les ressources utilisées pour créer l’exemple d’application dans le dossier *wingtiptoys-biens* du dossier *wingtiptoys* .

Le téléchargement est un fichier *. zip* . Pour voir le projet terminé créé par cette série de didacticiels, recherchez et *C#* sélectionnez le dossier dans le fichier. zip. Enregistrez le C# dossier dans le dossier que vous utilisez pour travailler avec les projets Visual Studio. Par défaut, le dossier de projets Visual Studio 2017 est :

<strong>C:\Users&#92;</strong>  <strong><em>&lt;nom d’utilisateur&gt;</em></strong> <strong>\source\repos</strong>

Renommez ***C#*** le dossier en ***WingtipToys***.

> [!NOTE]
> Si vous avez déjà un dossier nommé *WingtipToys* dans votre dossier de projets, renommez temporairement ce dossier existant avant de *C#* renommer le dossier *wingtiptoys*.

Pour exécuter le projet terminé, ouvrez le dossier *wingtiptoys* et double-cliquez sur le fichier *wingtiptoys. sln* . Visual Studio 2017 ouvre le projet. Ensuite, cliquez avec le bouton droit sur le fichier *default. aspx* dans **Explorateur de solutions** , puis sélectionnez **afficher dans le navigateur**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Participez à un ASP.NET Web Forms pour consulter le contenu

À l’issue de la série de didacticiels, participez à un quiz pour tester vos connaissances et renforcer les concepts clés. Chaque question fournit une explication et des liens vers des conseils supplémentaires.

* [ASP.NET Web Forms quiz](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Prise en charge des didacticiels et commentaires

Pour les questions et les commentaires, utilisez la section Q et une section incluse dans le [prise en main avec la page d’exemple ASP.net 4,5 Web Forms et Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).

Les commentaires de cette série de didacticiels sont les bienvenus. Lors de la mise à jour de cette série de didacticiels, il convient de prendre en compte les corrections ou suggestions pour les améliorations.

Si une erreur se produit, les messages d’erreur correspondants peuvent prêter à confusion, sans bonne explication sur la façon de résoudre le problème. Pour obtenir de l’aide, vous pouvez consulter les [forums ASP.net](https://forums.asp.net/). Une autre bonne source est la section Q et une section du [prise en main avec la page d’exemple ASP.net 4,5 Web Forms et Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#). 

> [!div class="step-by-step"]
> [Next](create-the-project.md)
