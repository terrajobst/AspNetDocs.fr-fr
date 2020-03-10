---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 8 | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications de Web Forms ASP.NET à l’aide de l’Entity Framework. L’exemple d’application est...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: ff6a808dd501df8056c0fb0784a8e42e094d5db7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585910"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 8

par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Utilisation de la fonctionnalité de Dynamic Data pour mettre en forme et valider des données

Dans le didacticiel précédent, vous avez implémenté des procédures stockées. Ce didacticiel vous montre comment Dynamic Data fonctionnalités peuvent offrir les avantages suivants :

- Les champs sont automatiquement mis en forme pour être affichés en fonction de leur type de données.
- Les champs sont validés automatiquement en fonction de leur type de données.
- Vous pouvez ajouter des métadonnées au modèle de données pour personnaliser le comportement de mise en forme et de validation. Dans ce cas, vous pouvez ajouter les règles de mise en forme et de validation à un seul emplacement, et elles sont automatiquement appliquées partout où vous accédez aux champs à l’aide de Dynamic Data contrôles.

Pour voir comment cela fonctionne, vous allez modifier les contrôles que vous utilisez pour afficher et modifier des champs dans la page *students. aspx* existante, et ajouter des métadonnées de mise en forme et de validation aux champs nom et date du type d’entité `Student`.

[![image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Utilisation des contrôles DynamicField et DynamicControl

Ouvrez la page *students. aspx* et dans le contrôle `StudentsGridView` remplacez le **nom** et la **Date d’inscription** `TemplateField` éléments par le balisage suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Ce balisage utilise `DynamicControl` contrôles à la place des contrôles `TextBox` et `Label` dans le champ modèle de nom d’étudiant, et il utilise un contrôle `DynamicField` pour la date d’inscription. Aucune chaîne de format n’est spécifiée.

Ajoutez un contrôle `ValidationSummary` après le contrôle `StudentsGridView`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

Dans le `SearchGridView` contrôle remplace le balisage pour les colonnes **nom** et **Date d’inscription** comme vous l’avez fait dans le contrôle `StudentsGridView`, à l’exception de l’omission de l’élément `EditItemTemplate`. L’élément `Columns` du contrôle `SearchGridView` contient désormais le balisage suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Ouvrez *students.aspx.cs* et ajoutez l’instruction `using` suivante :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Ajoutez un gestionnaire pour l’événement `Init` de la page :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Ce code spécifie que Dynamic Data fournira la mise en forme et la validation dans ces contrôles liés aux données pour les champs de l’entité `Student`. Si vous recevez un message d’erreur semblable à celui de l’exemple suivant lors de l’exécution de la page, cela signifie généralement que vous avez oublié d’appeler la méthode `EnableDynamicData` dans `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Exécutez la page.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

Dans la colonne **Date d’inscription** , l’heure est affichée avec la date, car le type de propriété est `DateTime`. Vous corrigerez cela plus tard.

Pour le moment, remarquez que Dynamic Data fournit automatiquement la validation des données de base. Par exemple, cliquez sur **modifier**, effacez le champ date, cliquez sur **mettre à jour**. vous constatez que Dynamic Data crée automatiquement ce champ obligatoire, car la valeur n’est pas Nullable dans le modèle de données. La page affiche un astérisque après le champ et un message d’erreur dans le contrôle `ValidationSummary` :

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Vous pouvez omettre le contrôle `ValidationSummary`, car vous pouvez également placer le pointeur de la souris sur l’astérisque pour afficher le message d’erreur :

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dynamic Data vérifiera également que les données entrées dans le champ **Date d’inscription** sont valides :

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Comme vous pouvez le voir, il s’agit d’un message d’erreur générique. Dans la section suivante, vous verrez comment personnaliser les messages, ainsi que les règles de validation et de mise en forme.

## <a name="adding-metadata-to-the-data-model"></a>Ajout de métadonnées au modèle de données

En règle générale, vous souhaitez personnaliser les fonctionnalités fournies par Dynamic Data. Par exemple, vous pouvez modifier la façon dont les données sont affichées et le contenu des messages d’erreur. En général, vous personnalisez également les règles de validation des données pour fournir plus de fonctionnalités que ce que Dynamic Data fournit automatiquement en fonction des types de données. Pour ce faire, vous créez des classes partielles qui correspondent à des types d’entités.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoUniversity** , sélectionnez **Ajouter une référence**, puis ajoutez une référence à `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

Dans le dossier *dal* , créez un nouveau fichier de classe, nommez-le *Student.cs*, puis remplacez le code du modèle par le code suivant.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Ce code crée une classe partielle pour l’entité `Student`. L’attribut `MetadataType` appliqué à cette classe partielle identifie la classe que vous utilisez pour spécifier des métadonnées. La classe de métadonnées peut avoir n’importe quel nom, mais l’utilisation du nom de l’entité et des « métadonnées » est une pratique courante.

Les attributs appliqués aux propriétés dans la classe de métadonnées spécifient la mise en forme, la validation, les règles et les messages d’erreur. Les attributs présentés ici auront les résultats suivants :

- `EnrollmentDate` s’affiche sous la forme d’une date (sans heure).
- Les deux champs de nom doivent avoir une longueur inférieure ou égale à 25 caractères, et un message d’erreur personnalisé est fourni.
- Les deux champs de nom sont obligatoires et un message d’erreur personnalisé est fourni.

Exécutez à nouveau la page *students. aspx* et vous voyez que les dates s’affichent désormais sans heure :

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Modifiez une ligne et essayez d’effacer les valeurs dans les champs nom. Les astérisques indiquant les erreurs de champ apparaissent dès que vous laissez un champ, avant de cliquer sur **mettre à jour**. Lorsque vous cliquez sur **mettre à jour**, la page affiche le texte du message d’erreur que vous avez spécifié.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Essayez d’entrer des noms de plus de 25 caractères, cliquez sur **mettre à jour**, et la page affiche le texte du message d’erreur que vous avez spécifié.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Maintenant que vous avez configuré ces règles de mise en forme et de validation dans les métadonnées du modèle de données, elles sont automatiquement appliquées à chaque page qui affiche ou autorise les modifications de ces champs, tant que vous utilisez des contrôles `DynamicControl` ou `DynamicField`. Cela réduit la quantité de code redondant que vous devez écrire, ce qui facilite la programmation et le test, et garantit la cohérence de la mise en forme et de la validation des données dans toute l’application.

## <a name="more-information"></a>Plus d'informations

Cela conclut cette série de didacticiels sur Prise en main avec le Entity Framework. Pour plus d’informations sur l’utilisation de la Entity Framework, poursuivez avec [le premier didacticiel de la prochaine série de didacticiels Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) ou visitez les sites suivants :

- [FAQ Entity Framework](http://www.ef-faq.org/introduction.html)
- [Blog de l’équipe Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework dans MSDN Library](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework dans le centre de développement des données MSDN](https://msdn.microsoft.com/data/ef.aspx)
- [Vue d’ensemble du contrôle serveur Web EntityDataSource dans MSDN Library](https://msdn.microsoft.com/library/cc488502.aspx)
- [Informations de référence sur l’API de contrôle EntityDataSource dans MSDN Library](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Forums Entity Framework sur MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog de Julie Lerman](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-7.md)
