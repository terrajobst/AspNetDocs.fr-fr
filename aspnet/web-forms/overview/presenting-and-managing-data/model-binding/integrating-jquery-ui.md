---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Intégration de JQuery UI DatePicker avec la liaison de modèle et Web Forms | Microsoft Docs
author: Rick-Anderson
description: Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642477"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Intégration de JQuery UI DatePicker avec la liaison de modèle et les Web Forms

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette série de didacticiels présente les aspects de base de l’utilisation de la liaison de modèle avec un projet de Web Forms ASP.NET. La liaison de modèle rend l’interaction des données plus simple que le traitement des objets de source de données (tels que ObjectDataSource ou SqlDataSource). Cette série commence par une documentation introductive et passe à des concepts plus avancés dans des didacticiels ultérieurs.
> 
> Ce didacticiel montre comment ajouter le [widget DatePicker](http://jqueryui.com/datepicker/) de l’interface utilisateur jQuery à un formulaire Web et comment utiliser la liaison de modèle pour mettre à jour la base de données avec la valeur sélectionnée.
> 
> Ce didacticiel s’appuie sur le projet créé dans la [première](retrieving-data.md) et la [deuxième](updating-deleting-and-creating-data.md) partie de la série.
> 
> Vous pouvez [Télécharger](https://go.microsoft.com/fwlink/?LinkId=286116) le projet complet dans C# ou VB. Le code téléchargeable fonctionne avec Visual Studio 2012 ou Visual Studio 2013. Elle utilise le modèle Visual Studio 2012, qui est légèrement différent du modèle Visual Studio 2013 présenté dans ce didacticiel.

## <a name="what-youll-build"></a>Ce que vous allez générer

Dans ce didacticiel, vous allez :

1. Ajouter une propriété à votre modèle pour enregistrer la date d’inscription de l’étudiant
2. Permettre à l’utilisateur de sélectionner la date d’inscription à l’aide du widget de DatePicker UI de JQuery
3. Appliquer les règles de validation pour la date d’inscription

Le widget de DatePicker de l’interface utilisateur JQuery permet aux utilisateurs de sélectionner facilement une date dans un calendrier qui s’affiche lorsque l’utilisateur interagit avec le champ. L’utilisation de ce widget peut être plus pratique pour les utilisateurs que de taper manuellement une date. L’intégration du widget DatePicker dans une page qui utilise la liaison de modèle pour les opérations de données nécessite uniquement une petite quantité de travail supplémentaire.

## <a name="add-a-new-property-to-the-model"></a>Ajouter une nouvelle propriété au modèle

Tout d’abord, vous allez ajouter une propriété **DateTime** à votre modèle d’étudiant et migrer cette modification vers la base de données. Ouvrez **UniversityModels.cs**, puis ajoutez le code en surbrillance au modèle Student.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

Le **RangeAttribute** est inclus pour appliquer des règles de validation pour la propriété. Pour ce didacticiel, nous partons du principe que Contoso University a été créé le 1er janvier 2013 et que les dates d’inscription antérieures ne sont donc pas valides.

Dans la fenêtre Package Management, ajoutez une migration en exécutant la commande **Add-migration AddEnrollmentDate**. Notez que le code de migration ajoute la nouvelle colonne DateTime à la table Student. Pour correspondre à la valeur que vous avez spécifiée dans RangeAttribute, ajoutez une valeur par défaut pour la nouvelle colonne, comme indiqué dans le code en surbrillance ci-dessous.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Enregistrez vos modifications dans le fichier de migration.

Vous n’avez pas besoin de réamorcer les données. Par conséquent, ouvrez **Configuration.cs** dans le dossier migrations, puis supprimez ou commentez le code dans la méthode **Seed** . Enregistrez et fermez le fichier.

À présent, exécutez la commande **Update-Database**. Notez que la colonne existe désormais dans la base de données et que tous les enregistrements existants ont la valeur par défaut pour EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Ajouter des contrôles dynamiques pour la date d’inscription

Vous allez maintenant ajouter des contrôles pour afficher et modifier la date d’inscription. À ce stade, la valeur est modifiée à l’aide d’une zone de texte. Plus loin dans ce didacticiel, vous allez remplacer la zone de texte par le widget de DatePicker JQuery.

Tout d’abord, il est important de noter que vous n’avez pas besoin d’apporter des modifications au fichier **AddStudent. aspx** . Le contrôle DynamicEntity affiche automatiquement la nouvelle propriété.

Ouvrez **students. aspx**, puis ajoutez le code en surbrillance suivant.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Exécutez l’application et notez que vous pouvez définir la valeur de la date d’inscription en tapant une date. Quand vous ajoutez un nouvel étudiant :

![définir la date](integrating-jquery-ui/_static/image1.png)

Ou modifiez une valeur existante :

![modifier la date](integrating-jquery-ui/_static/image2.png)

La saisie de la date fonctionne, mais elle peut ne pas être l’expérience du client que vous souhaitez fournir. Dans la section suivante, vous allez activer la sélection d’une date dans un calendrier.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Installer le package NuGet pour fonctionner avec JQuery UI

Le package NuGet de l' **interface utilisateur de jus** permet d’intégrer facilement les widgets jQuery UI à votre application Web. Pour utiliser ce package, installez-le via NuGet.

![Ajouter une interface utilisateur de jus](integrating-jquery-ui/_static/image3.png)

La version de l’interface utilisateur de jus que vous installez peut être en conflit avec la version de JQuery dans votre application. Avant de poursuivre ce didacticiel, essayez d’exécuter votre application. Si vous rencontrez une erreur JavaScript, vous devez rapprocher la version de JQuery. Vous pouvez ajouter la version attendue de JQuery à votre dossier de scripts (version 1.8.2 au moment de la rédaction de ce didacticiel), ou dans site. Master, spécifiez le chemin d’accès au fichier JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personnaliser le modèle DateTime pour inclure le widget DatePicker

Vous allez ajouter le widget DatePicker au modèle Dynamic Data pour modifier une valeur DateTime. En ajoutant le widget au modèle, il est automatiquement restitué dans le formulaire d’ajout d’un nouvel étudiant et dans la vue grille pour la modification des étudiants. Ouvrez **DateTime\_Edit. ascx**, puis ajoutez le code en surbrillance suivant.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

Dans le fichier code-behind, vous allez définir les dates minimale et maximale du DatePicker. En définissant ces valeurs, vous empêchez les utilisateurs de naviguer vers des dates non valides. Vous allez récupérer les valeurs minimales et maximales de **RangeAttribute** sur la propriété DateTime, si une valeur est fournie. Ouvrez **DateTime\_Edit.ascx.cs**, puis ajoutez le code en surbrillance suivant à la page\_méthode Load.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Exécutez l’application Web et accédez à la page AddStudent. Fournissez des valeurs pour les champs et remarquez que lorsque vous cliquez sur la zone de texte pour la date d’inscription, le calendrier est affiché.

![sélecteur de dates](integrating-jquery-ui/_static/image4.png)

Choisissez une date, puis cliquez sur **Insérer**. RangeAttribute applique la validation sur le serveur. En définissant la propriété MinDate sur le DatePicker, vous appliquez également la validation sur le client. Le calendrier ne permet pas à l’utilisateur d’accéder à une date antérieure à la valeur de MinDate.

Lorsque vous modifiez un enregistrement dans la vue grille, le calendrier est également affiché.

![DatePicker dans GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez appris à incorporer un widget JQuery dans un formulaire Web qui utilise la liaison de modèle.

Dans le [didacticiel](using-query-string-values-to-retrieve-data.md)suivant, vous allez utiliser une valeur de chaîne de requête lors de la sélection de données.

> [!div class="step-by-step"]
> [Précédent](sorting-paging-and-filtering-data.md)
> [Suivant](using-query-string-values-to-retrieve-data.md)
