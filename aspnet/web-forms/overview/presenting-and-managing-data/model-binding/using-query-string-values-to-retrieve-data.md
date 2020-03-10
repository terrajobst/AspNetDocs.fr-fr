---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Utilisation de valeurs de chaîne de requête pour filtrer des données avec une liaison de modèle et des Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639096"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Utilisation de valeurs de chaîne de requête pour filtrer des données avec la liaison de modèle et les Web Forms

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.
> 
> Ce didacticiel montre comment passer une valeur dans la chaîne de requête et comment utiliser cette valeur pour récupérer des données via une liaison de modèle.
> 
> Ce didacticiel s’appuie sur le projet créé dans les parties [précédentes](retrieving-data.md) de la série.
> 
> Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.

## <a name="what-youll-build"></a>Ce que vous allez générer

Dans ce didacticiel, vous allez :

1. Ajouter une nouvelle page pour afficher les cours inscrits pour un étudiant
2. Récupérer les cours inscrits pour l’étudiant sélectionné en fonction d’une valeur dans la chaîne de requête
3. Ajouter un lien hypertexte avec une valeur de chaîne de requête de la vue grille à la nouvelle page

Les étapes de ce didacticiel sont assez similaires à celles que vous avez effectuées dans le [didacticiel](sorting-paging-and-filtering-data.md) précédent pour filtrer les étudiants affichés en fonction de la sélection de l’utilisateur dans une liste déroulante. Dans ce didacticiel, vous avez utilisé l’attribut **Control** dans la méthode Select pour spécifier que la valeur du paramètre provient d’un contrôle. Dans ce didacticiel, vous allez utiliser l’attribut **QueryString** dans la méthode Select pour spécifier que la valeur du paramètre provient de la chaîne de requête.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Ajouter une nouvelle page pour afficher les cours d’un étudiant

Ajoutez un nouveau formulaire Web qui utilise la page maître site. Master et nommez les **cours**de la page.

Dans le fichier **courses. aspx** , ajoutez une vue grille pour afficher les cours de l’étudiant sélectionné.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Définir la méthode Select

Dans **courses.aspx.cs**, vous allez ajouter la méthode Select avec le nom que vous avez spécifié dans la propriété **SelectMethod** de la vue de grille. Dans cette méthode, vous allez définir la requête de récupération des cours d’un étudiant et spécifier que le paramètre provient d’une valeur de chaîne de requête portant le même nom que le paramètre.

Tout d’abord, vous devez ajouter les instructions **using** suivantes.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Ensuite, ajoutez le code suivant à Courses.aspx.cs :

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

L’attribut QueryString signifie qu’une valeur de chaîne de requête nommée StudentID est automatiquement assignée au paramètre dans cette méthode.

## <a name="add-hyperlink-with-query-string-value"></a>Ajouter un lien hypertexte avec une valeur de chaîne de requête

Dans l’affichage de grille sur students. aspx, vous allez ajouter un champ de lien hypertexte qui lie à votre nouvelle page de cours. Le lien hypertexte inclut une valeur de chaîne de requête avec l’ID de l’étudiant.

Dans students. aspx, ajoutez le champ suivant aux colonnes de la vue grille juste en dessous du champ pour le nombre total de crédits.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Exécutez l’application et notez que l’affichage de grille comprend désormais le lien cours.

![Ajouter un lien hypertexte](using-query-string-values-to-retrieve-data/_static/image1.png)

Lorsque vous cliquez sur l’un des liens, vous voyez les cours inscrits de l’étudiant.

![afficher les cours](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez ajouté un lien avec une valeur de chaîne de requête. Vous avez utilisé cette valeur de chaîne de requête pour la valeur de paramètre dans la méthode Select.

Dans le [didacticiel](adding-business-logic-layer.md)suivant, vous allez déplacer le code des fichiers code-behind dans une couche de logique métier et une couche d’accès aux données.

> [!div class="step-by-step"]
> [Précédent](integrating-jquery-ui.md)
> [Suivant](adding-business-logic-layer.md)
