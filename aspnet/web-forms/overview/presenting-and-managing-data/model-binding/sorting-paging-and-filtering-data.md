---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Tri, pagination et filtrage des données avec la liaison de modèle et les Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548061"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Tri, pagination et filtrage des données avec la liaison de modèle et les Web Forms

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.
> 
> Ce didacticiel montre comment ajouter le tri, la pagination et le filtrage des données par le biais de la liaison de modèle.
> 
> Ce didacticiel s’appuie sur le projet créé dans la première [partie](retrieving-data.md) de la série.
> 
> Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.

## <a name="what-youll-build"></a>Ce que vous allez générer

Dans ce didacticiel, vous allez :

1. Activer le tri et la pagination des données
2. Activer le filtrage des données en fonction d’une sélection par l’utilisateur

## <a name="add-sorting"></a>Ajouter la fonctionnalité de tri

L’activation du tri dans le GridView est très facile. Dans le fichier Student. aspx, affectez simplement la **valeur true** à **AllowSorting** dans le GridView. Vous n’avez pas besoin de définir une valeur **SortExpression** pour chaque colonne, car DataField est utilisé automatiquement. Le GridView modifie la requête pour inclure le classement des données par la valeur sélectionnée. Le code en surbrillance ci-dessous montre l’ajout que vous devez effectuer pour activer le tri.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Exécutez l’application Web, puis testez les enregistrements d’étudiants de tri en fonction des valeurs figurant dans des colonnes différentes.

![Trier les élèves](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Ajouter la fonctionnalité de pagination

L’activation de la pagination est également très facile. Dans le GridView, affectez la valeur **true** à la propriété **AllowPaging** et définissez la propriété **pageSize** sur le nombre d’enregistrements que vous souhaitez afficher sur chaque page. Dans ce didacticiel, vous pouvez lui affecter la valeur 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Exécutez l’application Web et remarquez que les enregistrements sont désormais répartis sur plusieurs pages, avec un maximum de 4 enregistrements affichés sur une seule page.

![Ajouter la pagination](sorting-paging-and-filtering-data/_static/image4.png)

L’exécution différée des requêtes améliore l’efficacité de l’application. Au lieu de récupérer l’ensemble du jeu de données, le contrôle GridView modifie la requête pour récupérer uniquement les enregistrements de la page actuelle.

## <a name="filter-records-by-user-selection"></a>Filtrer les enregistrements par sélection d’utilisateur

La liaison de modèle ajoute plusieurs attributs qui vous permettent de spécifier comment définir la valeur d’un paramètre dans une méthode de liaison de modèle. Ces attributs se trouvent dans l’espace de noms **System. Web. ModelBinding** . Ils comprennent :

- Contrôle
- Cookie
- Formulaire
- Profil
- QueryString
- RouteData
- Session
- Profils
- ViewState

Dans ce didacticiel, vous allez utiliser la valeur d’un contrôle pour filtrer les enregistrements affichés dans le GridView. Vous allez ajouter l’attribut de **contrôle** à la méthode de requête que vous avez créée précédemment. Dans un didacticiel [ultérieur](using-query-string-values-to-retrieve-data.md) , vous allez appliquer l’attribut **QueryString** à un paramètre pour spécifier que la valeur du paramètre provient d’une valeur de chaîne de requête.

Tout d’abord, au-dessus du ValidationSummary, ajoutez une liste déroulante pour filtrer les élèves affichés.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Dans le fichier code-behind, modifiez la méthode Select pour recevoir une valeur du contrôle, puis définissez le nom du paramètre sur le nom du contrôle qui fournit la valeur.

Vous devez ajouter une instruction **using** pour l’espace de noms **System. Web. ModelBinding** pour résoudre l’attribut de contrôle.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Le code suivant montre que la méthode Select a été retravaillée pour filtrer les données retournées en fonction de la valeur de la liste déroulante. L’ajout d’un attribut de contrôle avant un paramètre spécifie que la valeur de ce paramètre provient d’un contrôle portant le même nom.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Exécutez l’application Web et sélectionnez des valeurs différentes dans la liste déroulante pour filtrer la liste des étudiants.

![filtrer les élèves](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez activé le tri et la pagination des données. Vous avez également activé le filtrage des données par la valeur d’un contrôle.

Dans le [didacticiel](integrating-jquery-ui.md) suivant, vous allez améliorer l’interface utilisateur en intégrant un widget d’interface utilisateur jQuery dans le modèle Dynamic Data.

> [!div class="step-by-step"]
> [Précédent](updating-deleting-and-creating-data.md)
> [Suivant](integrating-jquery-ui.md)
