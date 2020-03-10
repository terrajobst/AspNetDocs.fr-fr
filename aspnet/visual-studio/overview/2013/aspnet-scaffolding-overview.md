---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Génération de modèles automatique ASP.NET dans Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: La génération de modèles automatique ASP.NET est une nouvelle fonctionnalité qui est incluse dans Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557980"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a>Génération de modèles automatique ASP.NET dans Visual Studio 2013

par [Tom FitzMacken](https://github.com/tfitzmac)

> La génération de modèles automatique ASP.NET est une nouvelle fonctionnalité qui est incluse dans Visual Studio 2013.

## <a name="overview"></a>Présentation

La génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET. Visual Studio 2013 comprend des générateurs de code préinstallés pour les projets MVC et API Web. Vous ajoutez la génération de modèles automatique à votre projet lorsque vous souhaitez ajouter rapidement du code qui interagit avec les modèles de données. L’utilisation de la génération de modèles automatique peut réduire le temps nécessaire au développement d’opérations de données standard dans votre projet.

Par défaut, Visual Studio 2013 ne prend pas en charge la génération de code pour un projet Web Forms, mais vous pouvez utiliser la génération de modèles automatique avec Web Forms en ajoutant des dépendances MVC au projet ou en installant une extension. Les deux approches sont présentées ci-dessous.

Visual Studio 2013 Update 2 (actuellement RC) offre la possibilité d’étendre la génération de modèles automatique ASP.NET pour répondre aux exigences de votre scénario. Avec cette fonctionnalité, vous pouvez créer un modèle de génération de modèles automatique personnalisé et l’ajouter à la boîte de dialogue Ajouter une nouvelle structure. Dans le modèle personnalisé, vous spécifiez le code généré lors de l’ajout d’un élément de génération de modèles automatique. Pour plus d’informations, consultez [création d’un générateur de modèles personnalisé pour Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Conditions préalables requises

Pour utiliser la génération de modèles automatique ASP.NET, vous devez disposer des éléments suivants :

- Microsoft Visual Studio 2013
- Outils de développement Web (partie de l’installation Visual Studio 2013 par défaut)
- Frameworks et outils Web ASP.NET 2013 (partie de l’installation Visual Studio 2013 par défaut)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Ajouter un élément de génération de modèles automatique à MVC ou à l’API Web

Pour ajouter une structure, cliquez avec le bouton droit sur le projet ou sur un dossier dans le projet, puis sélectionnez **Ajouter** – **nouvel élément**généré automatiquement, comme illustré dans l’image suivante.

![Ajouter un élément de structure](aspnet-scaffolding-overview/_static/image1.png)

Dans la fenêtre **Ajouter une structure** , sélectionnez le type de structure à ajouter.

![Sélectionner le type de structure](aspnet-scaffolding-overview/_static/image2.png)

La fenêtre **Ajouter un contrôleur** vous donne la possibilité de sélectionner des options pour générer le contrôleur, notamment si vous souhaitez utiliser les nouvelles fonctionnalités async de Entity Framework 6.

![Ajouter un contrôleur](aspnet-scaffolding-overview/_static/image3.png)

Les classes et les pages pertinentes sont créées pour votre scénario. Par exemple, l’illustration suivante montre le contrôleur MVC et les vues qui ont été créées à l’aide de la génération de modèles automatique pour une classe de modèle nommée movies.

![Fichiers créés](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Ajouter un élément de génération de modèles automatique à Web Forms

Pour ajouter la génération de modèles automatique qui génère Web Forms code, vous devez installer une extension dans Visual Studio ou ajouter des dépendances MVC. Les deux approches sont présentées ci-dessous, mais vous n’avez besoin d’effectuer qu’une seule de ces approches.

### <a name="web-forms-scaffolding-extension"></a>Extension de génération de modèles automatique Web Forms

Vous pouvez installer une extension Visual Studio qui vous permet d’utiliser la génération de modèles automatique avec un projet de Web Forms. Dans Visual Studio, sélectionnez **Outils** , puis **extensions et mises à jour**. Dans cette boîte de dialogue, recherchez **Web Forms génération de modèles**automatique dans la Galerie Visual Studio.

![installer la génération de modèles automatique Web Forms](aspnet-scaffolding-overview/_static/image5.png)

Pour plus d’informations, consultez [génération de modèles automatique Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dépendances MVC

Pour ajouter des dépendances MVC, sélectionnez **ajouter** - **nouvel élément de génération de modèles**automatique. Dans la fenêtre Ajouter une structure, sélectionnez **dépendances MVC**, comme indiqué ci-dessous.

![Ajouter des dépendances MVC](aspnet-scaffolding-overview/_static/image6.png)

Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète. Si vous sélectionnez minimal, seuls les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option complète, les dépendances minimales sont ajoutées, ainsi que les fichiers de contenu requis pour un projet MVC. Pour utiliser facilement la génération de modèles automatique, sélectionnez dépendances complètes.

![sélectionner les dépendances complètes](aspnet-scaffolding-overview/_static/image7.png)

Après avoir ajouté les dépendances, vous verrez un fichier **Readme. txt** . Suivez attentivement les instructions de ce fichier pour vous assurer que votre projet fonctionne correctement.

Une fois que vous avez effectué les étapes du fichier Readme. txt, vous pouvez ajouter un nouvel élément de génération de modèles automatique comme indiqué dans la section précédente sur MVC et l’API Web. Les vues et le contrôleur générés automatiquement fonctionneront correctement dans votre projet.

## <a name="tutorials"></a>Didacticiels

Pour créer un modèle de modèle personnalisé, consultez [création d’un générateur de modèles personnalisé pour Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Pour personnaliser les fichiers générés, consultez [Comment personnaliser les fichiers générés à partir de la boîte de dialogue Nouvel élément de génération de modèles](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)automatique.

Pour obtenir un exemple d’utilisation de la génération de modèles automatique avec le **développement Database First**, consultez [EF Database First avec ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Pour obtenir un exemple d’utilisation de la génération de modèles automatique dans un projet **MVC** , consultez [prise en main avec ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Pour obtenir un exemple d’utilisation de la génération de modèles automatique dans un projet d' **API Web** , consultez [créer une API REST avec routage d’attributs dans Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
