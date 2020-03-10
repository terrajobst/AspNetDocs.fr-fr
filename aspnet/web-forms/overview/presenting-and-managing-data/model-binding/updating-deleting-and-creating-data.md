---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Mise à jour, suppression et création de données avec la liaison de modèle et les Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586645"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Mise à jour, suppression et création de données avec la liaison de modèle et les Web Forms

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.
> 
> Ce didacticiel montre comment créer, mettre à jour et supprimer des données avec la liaison de modèle. Vous allez définir les propriétés suivantes :
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Ces propriétés reçoivent le nom de la méthode qui gère l’opération correspondante. Dans cette méthode, vous fournissez la logique permettant d’interagir avec les données.
> 
> Ce didacticiel s’appuie sur le projet créé dans la première [partie](retrieving-data.md) de la série.
> 
> Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.

## <a name="what-youll-build"></a>Ce que vous allez générer

Dans ce didacticiel, vous allez :

1. Ajouter des modèles de données dynamiques
2. Activer la mise à jour et la suppression des données par le biais de méthodes de liaison de modèle
3. Appliquer des règles de validation des données-activer la création d’un enregistrement dans la base de données

## <a name="add-dynamic-data-templates"></a>Ajouter des modèles de données dynamiques

Pour offrir la meilleure expérience utilisateur et réduire la répétition du code, vous allez utiliser des modèles de données dynamiques. Vous pouvez facilement intégrer des modèles de données dynamiques prédéfinis dans votre site existant en installant un package NuGet.

Dans **gérer les packages NuGet**, installez **DynamicDataTemplatesCS**.

![modèles Dynamic Data](updating-deleting-and-creating-data/_static/image1.png)

Notez que votre projet comprend maintenant un dossier nommé **DynamicData**. Dans ce dossier, vous trouverez les modèles qui sont appliqués automatiquement aux contrôles dynamiques dans vos Web Forms.

![dossier Dynamic Data](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Activer la mise à jour et la suppression

La possibilité pour les utilisateurs de mettre à jour et de supprimer des enregistrements dans la base de données est très similaire au processus de récupération des données. Dans les propriétés **UpdateMethod** et **DeleteMethod** , vous spécifiez les noms des méthodes qui effectuent ces opérations. Avec un contrôle GridView, vous pouvez également spécifier la génération automatique des boutons modifier et supprimer. Le code en surbrillance suivant montre les ajouts apportés au code GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

Dans le fichier code-behind, ajoutez une instruction using pour **System. Data. Entity. infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Ensuite, ajoutez les méthodes Update et Delete suivantes.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

La méthode **TryUpdateModel** applique les valeurs liées aux données correspondantes du formulaire Web à l’élément de données. L’élément de données est récupéré en fonction de la valeur du paramètre ID.

## <a name="enforce-validation-requirements"></a>Appliquer les exigences de validation

Les attributs de validation que vous avez appliqués aux propriétés FirstName, LastName et Year de la classe Student sont automatiquement appliqués lors de la mise à jour des données. Les contrôles DynamicField ajoutent des validateurs de client et de serveur basés sur les attributs de validation. Les propriétés FirstName et LastName sont toutes deux requises. FirstName ne peut pas dépasser 20 caractères et LastName ne peut pas dépasser 40 caractères. L’année doit être une valeur valide pour l’énumération AcademicYear.

Si l’utilisateur ne respecte pas l’une des exigences de validation, la mise à jour ne se poursuit pas. Pour afficher le message d’erreur, ajoutez un contrôle ValidationSummary au-dessus de GridView. Pour afficher les erreurs de validation de la liaison de modèle, affectez la valeur **true**à la propriété **ShowModelStateErrors** . 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Exécutez l’application Web, puis mettez à jour et supprimez tous les enregistrements.

![mettre à jour les données](updating-deleting-and-creating-data/_static/image3.png)

Notez que dans le mode d’édition, la valeur de la propriété Year est automatiquement rendue sous la forme d’une liste déroulante. La propriété Year est une valeur d’énumération, et le modèle Dynamic Data pour une valeur d’énumération spécifie une liste déroulante pour la modification. Vous pouvez trouver ce modèle en ouvrant l' **énumération\_fichier Edit. ascx** dans le dossier **DynamicData**/**FieldTemplates** .

Si vous fournissez des valeurs valides, la mise à jour se termine correctement. Si vous ne respectez pas l’une des exigences de validation, la mise à jour ne se poursuit pas et un message d’erreur s’affiche au-dessus de la grille.

![message d’erreur](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Ajouter de nouveaux enregistrements

Le contrôle GridView n’inclut pas la propriété **InsertMethod** et ne peut donc pas être utilisé pour ajouter un nouvel enregistrement avec une liaison de modèle. Vous pouvez trouver la propriété InsertMethod dans les contrôles **FormView**, **DetailsView**ou **ListView** . Dans ce didacticiel, vous allez utiliser un contrôle FormView pour ajouter un nouvel enregistrement.

Tout d’abord, ajoutez un lien vers la nouvelle page que vous allez créer pour ajouter un nouvel enregistrement. Au-dessus du ValidationSummary, ajoutez :

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Le nouveau lien s’affiche en haut du contenu de la page des étudiants.

![nouveau lien](updating-deleting-and-creating-data/_static/image5.png)

Ajoutez ensuite un nouveau formulaire Web à l’aide d’une page maître, puis nommez-le **AddStudent**. Sélectionnez site. Master comme page maître.

Vous allez afficher les champs pour ajouter un nouvel étudiant à l’aide d’un contrôle **DynamicEntity** . Le contrôle DynamicEntity restitue les propriétés modifiables dans la classe spécifiée dans la propriété ItemType. La propriété StudentID a été marquée avec l’attribut **[ScaffoldColumn (false)]** de sorte qu’elle ne soit pas rendue. Dans l’espace réservé MainContent de la page AddStudent, ajoutez le code suivant.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Dans le fichier code-behind (AddStudent.aspx.cs), ajoutez une instruction **using** pour l’espace de noms **ContosoUniversityModelBinding. Models** .

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Ensuite, ajoutez les méthodes suivantes pour spécifier comment insérer un nouvel enregistrement et un gestionnaire d’événements pour le bouton Annuler.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Enregistrez toutes les modifications.

Exécutez l’application Web et créez un nouveau Student.

![Ajouter un nouvel étudiant](updating-deleting-and-creating-data/_static/image6.png)

Cliquez sur **Insérer** et notez que le nouvel étudiant a été créé.

![afficher un nouvel étudiant](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez activé la mise à jour, la suppression et la création de données. Vous avez vérifié que les règles de validation sont appliquées lors de l’interaction avec les données.

Dans le [didacticiel](sorting-paging-and-filtering-data.md) suivant de cette série, vous allez activer le tri, la pagination et le filtrage des données.

> [!div class="step-by-step"]
> [Précédent](retrieving-data.md)
> [Suivant](sorting-paging-and-filtering-data.md)
