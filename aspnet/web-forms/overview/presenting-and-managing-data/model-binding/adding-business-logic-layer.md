---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Ajout d’une couche de logique métier à un projet qui utilise la liaison de modèle et les Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634833"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Ajout d’une couche de logique métier à un projet qui utilise la liaison de modèle et les Web Forms

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.
> 
> Ce didacticiel montre comment utiliser la liaison de modèle avec une couche de logique métier. Vous allez définir le membre OnCallingDataMethods pour spécifier qu’un objet autre que la page actuelle est utilisé pour appeler les méthodes de données.
> 
> Ce didacticiel s’appuie sur le projet créé dans les parties [précédentes](retrieving-data.md) de la série.
> 
> Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.

## <a name="what-youll-build"></a>Ce que vous allez générer

La liaison de modèle vous permet de placer votre code d’interaction de données dans le fichier code-behind d’une page Web ou dans une classe de logique métier distincte. Les didacticiels précédents ont montré comment utiliser les fichiers code-behind pour le code d’interaction des données. Cette approche fonctionne pour les petits sites, mais elle peut entraîner une répétition du code et une plus grande difficulté lors de la maintenance d’un site de grande taille. Il peut également être très difficile de tester par programmation le code qui réside dans les fichiers code-behind, car il n’existe aucune couche d’abstraction.

Pour centraliser le code d’interaction des données, vous pouvez créer une couche de logique métier qui contient toute la logique permettant d’interagir avec les données. Vous appelez ensuite la couche de logique métier à partir de vos pages Web. Ce didacticiel montre comment déplacer tout le code que vous avez écrit dans les didacticiels précédents dans une couche de logique métier, puis comment utiliser ce code à partir des pages.

Dans ce didacticiel, vous allez :

1. Déplacer le code des fichiers code-behind vers une couche de logique métier
2. Modifier vos contrôles liés aux données pour appeler les méthodes dans la couche de logique métier

## <a name="create-business-logic-layer"></a>Créer une couche de logique métier

À présent, vous allez créer la classe qui est appelée à partir des pages Web. Les méthodes de cette classe sont similaires aux méthodes que vous avez utilisées dans les didacticiels précédents et incluent les attributs du fournisseur de valeurs.

Tout d’abord, ajoutez un nouveau dossier appelé **BLL**.

![Ajouter un dossier](adding-business-logic-layer/_static/image1.png)

Dans le dossier BLL, créez une nouvelle classe nommée **SchoolBL.cs**. Elle contient toutes les opérations de données qui se trouvaient à l’origine dans les fichiers code-behind. Les méthodes sont presque les mêmes que celles du fichier code-behind, mais elles incluent des modifications.

La modification la plus importante à noter est que vous n’exécutez plus le code à partir d’une instance de la classe **page** . La classe page contient la méthode **TryUpdateModel** et la propriété **ModelState** . Quand ce code est déplacé vers une couche de logique métier, vous n’avez plus d’instance de la classe page pour appeler ces membres. Pour contourner ce problème, vous devez ajouter un paramètre **ModelMethodContext** à toute méthode qui accède à TryUpdateModel ou ModelState. Vous utilisez ce paramètre ModelMethodContext pour appeler TryUpdateModel ou récupérer ModelState. Vous n’avez pas besoin de modifier quoi que ce soit dans la page Web pour tenir compte de ce nouveau paramètre.

Remplacez le code dans SchoolBL.cs par le code suivant.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Réviser les pages existantes pour récupérer des données de la couche de logique métier

Enfin, vous allez convertir les pages students. aspx, AddStudent. aspx et courses. aspx en utilisant des requêtes dans le fichier code-behind pour utiliser la couche de logique métier.

Dans les fichiers code-behind pour les élèves, les AddStudent et les cours, supprimez ou commentez les méthodes de requête suivantes :

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Vous ne devriez à présent avoir aucun code dans le fichier code-behind qui se rapporte aux opérations de données.

Le gestionnaire d’événements **OnCallingDataMethods** vous permet de spécifier un objet à utiliser pour les méthodes de données. Dans students. aspx, ajoutez une valeur pour ce gestionnaire d’événements et remplacez les noms des méthodes de données par les noms des méthodes de la classe de logique métier.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Dans le fichier code-behind pour students. aspx, définissez le gestionnaire d’événements pour l’événement CallingDataMethods. Dans ce gestionnaire d’événements, vous spécifiez la classe de logique métier pour les opérations de données.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

Dans AddStudent. aspx, apportez des modifications similaires.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Dans courses. aspx, apportez des modifications similaires.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Exécutez l’application et remarquez que toutes les pages fonctionnent comme auparavant. La logique de validation fonctionne également correctement.

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez restructuré votre application pour qu’elle utilise une couche d’accès aux données et une couche de logique métier. Vous avez spécifié que les contrôles de données utilisent un objet qui n’est pas la page actuelle pour les opérations de données.

> [!div class="step-by-step"]
> [Précédent](using-query-string-values-to-retrieve-data.md)
