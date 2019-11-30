---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Partie 1 : vue d’ensemble et création du projet | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600329"
---
# <a name="part-1-overview-and-creating-the-project"></a>Partie 1 : vue d’ensemble et création du projet

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework est une infrastructure de mappage objet/relationnel. Il mappe les objets de domaine dans votre code aux entités d’une base de données relationnelle. Pour l’essentiel, vous n’avez pas à vous soucier de la couche de base de données, car Entity Framework s’en occupe pour vous. Votre code manipule les objets, et les modifications sont conservées dans une base de données.

## <a name="about-the-tutorial"></a>À propos du didacticiel

Dans ce didacticiel, vous allez créer une application Store simple. L’application comporte deux parties principales. Les utilisateurs normaux peuvent afficher les produits et créer des commandes :

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Les administrateurs peuvent créer, supprimer ou modifier des produits :

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Compétences que vous allez apprendre

Voici ce que vous allez apprendre :

- Comment utiliser Entity Framework avec API Web ASP.NET.
- Comment utiliser Knockout. js pour créer une interface utilisateur dynamique du client.
- Comment utiliser l’authentification par formulaire avec l’API Web pour authentifier les utilisateurs.

Bien que ce didacticiel soit autonome, vous pouvez commencer par lire les didacticiels suivants :

- [Votre première API Web ASP.NET](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Création d’une API Web qui prend en charge les opérations CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Une connaissance de [ASP.NET MVC](../../../../mvc/index.md) est également utile.

## <a name="overview"></a>Vue d'ensemble de

À un niveau élevé, voici l’architecture de l’application :

- ASP.NET MVC génère les pages HTML pour le client.
- API Web ASP.NET expose les opérations CRUD sur les données (Products et Orders).
- Entity Framework traduit les modèles utilisés C# par l’API Web en entités de base de données.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Le diagramme suivant montre comment les objets de domaine sont représentés dans les différentes couches de l’application : la couche de base de données, le modèle objet et enfin le format de câble, qui est utilisé pour transmettre des données au client via HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Vous pouvez créer le projet de didacticiel à l’aide de Visual Web Developer Express ou de la version complète de Visual Studio.

Dans la page de **démarrage** , cliquez sur **nouveau projet**.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**. Nommez le projet « ProductStore », puis cliquez sur **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez **application Internet** , puis cliquez sur **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Le modèle « application Internet » crée une application ASP.NET MVC qui prend en charge l’authentification par formulaire. Si vous exécutez l’application maintenant, elle contient déjà certaines fonctionnalités :

- Les nouveaux utilisateurs peuvent s’inscrire en cliquant sur le lien « s’inscrire » dans le coin supérieur droit.
- Les utilisateurs inscrits peuvent se connecter en cliquant sur le lien « connexion ».

Les informations d’appartenance sont conservées dans une base de données qui est créée automatiquement. Pour plus d’informations sur l’authentification par formulaire dans ASP.NET MVC, consultez [procédure pas à pas : utilisation de l’authentification par formulaire dans ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Mettre à jour le fichier CSS

Cette étape est cosmétique, mais elle rendra les pages restituées comme les captures d’écran précédentes.

Dans Explorateur de solutions, développez le dossier Content et ouvrez le fichier nommé site. css. Ajoutez les styles CSS suivants :

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Suivant](using-web-api-with-entity-framework-part-2.md)
