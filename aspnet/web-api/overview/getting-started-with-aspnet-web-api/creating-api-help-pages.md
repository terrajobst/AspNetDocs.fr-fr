---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Création de pages d’aide pour API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Ce didacticiel contenant du code montre comment créer des pages d’aide pour API Web ASP.NET dans ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556874"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a>Création de pages d’aide pour API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Ce didacticiel contenant du code montre comment créer des pages d’aide pour API Web ASP.NET dans ASP.NET 4. x.

Lorsque vous créez une API Web, il est souvent utile de créer une page d’aide afin que d’autres développeurs sachent comment appeler votre API. Vous pouvez créer l’ensemble de la documentation manuellement, mais il est préférable de générer automatiquement le plus possible. Pour faciliter cette tâche, API Web ASP.NET fournit une bibliothèque pour la génération automatique des pages d’aide au moment de l’exécution.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Création des pages d’aide de l’API

Installez [ASP.net et Web Tools mise à jour 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Cette mise à jour intègre les pages d’aide dans le modèle de projet d’API Web.

Ensuite, créez un nouveau projet ASP.NET MVC 4 et sélectionnez le modèle de projet d’API Web. Le modèle de projet crée un exemple de contrôleur d’API nommé `ValuesController`. Le modèle crée également les pages d’aide de l’API. Tous les fichiers de code de la page d’aide sont placés dans le dossier zones du projet.

![](creating-api-help-pages/_static/image2.png)

Lorsque vous exécutez l’application, la page d’hébergement contient un lien vers la page d’aide de l’API. À partir de la page d’hébergement, le chemin d’accès relatif est/help.

![](creating-api-help-pages/_static/image3.png)

Ce lien vous amène à une page de résumé des API.

![](creating-api-help-pages/_static/image4.png)

La vue MVC pour cette page est définie dans Areas/HelpPage/views/Help/index. cshtml. Vous pouvez modifier cette page pour modifier la disposition, l’introduction, le titre, les styles et ainsi de suite.

La partie principale de la page est un tableau d’API, regroupées par contrôleur. Les entrées de table sont générées de manière dynamique, à l’aide de l’interface **IApiExplorer** . (Je parlerai plus en détail de cette interface ultérieurement.) Si vous ajoutez un nouveau contrôleur d’API, la table est automatiquement mise à jour au moment de l’exécution.

La colonne « API » répertorie la méthode HTTP et l’URI relatif. La colonne « Description » contient de la documentation pour chaque API. Au départ, la documentation est simplement un texte d’espace réservé. Dans la section suivante, je vais vous montrer comment ajouter de la documentation à partir de commentaires XML.

Chaque API a un lien vers une page contenant des informations plus détaillées, y compris des exemples de corps de demande et de réponse.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Ajout de pages d’aide à un projet existant

Vous pouvez ajouter des pages d’aide à un projet d’API Web existant à l’aide du gestionnaire de package NuGet. Cette option est utile pour démarrer à partir d’un autre modèle de projet que le modèle d’API Web.

Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre [console du gestionnaire de package](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , tapez l’une des commandes suivantes :

Pour une **C#** application : `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Pour une application **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Il existe deux packages, un pour C# et un pour Visual Basic. Veillez à utiliser celle qui correspond à votre projet.

Cette commande installe les assemblys nécessaires et ajoute les vues MVC pour les pages d’aide (situées dans le dossier Areas/HelpPage). Vous devez ajouter manuellement un lien vers la page d’aide. L’URI est/help. Pour créer un lien dans une vue Razor, ajoutez ce qui suit :

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Veillez également à inscrire les zones. Dans le fichier global. asax, ajoutez le code suivant à l' **Application\_méthode Start** , si elle n’existe pas déjà :

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Ajout de la documentation sur les API

Par défaut, les pages d’aide contiennent des chaînes d’espace réservé pour la documentation. Vous pouvez utiliser des [Commentaires de documentation XML](https://msdn.microsoft.com/library/b2s063f7.aspx) pour créer la documentation. Pour activer cette fonctionnalité, ouvrez le fichier zones/HelpPage/App\_Start/HelpPageConfig. cs et supprimez les marques de commentaire de la ligne suivante :

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Activez à présent la documentation XML. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**. Sélectionnez la page **générer** .

![](creating-api-help-pages/_static/image6.png)

Sous **sortie**, activez la case à cocher **fichier de documentation XML**. Dans la zone d’édition, tapez « App\_Data/XmlDocument. Xml ».

![](creating-api-help-pages/_static/image7.png)

Ensuite, ouvrez le code du contrôleur d’API `ValuesController`, qui est défini dans/Controllers/ValuesController.cs. Ajoutez des commentaires de documentation aux méthodes de contrôleur. Exemple :

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Conseil : Si vous placez le signe insertion sur la ligne au-dessus de la méthode et que vous tapez trois barres obliques, Visual Studio insère automatiquement les éléments XML. Vous pouvez alors remplir les espaces.

Maintenant, générez et exécutez à nouveau l’application, puis accédez aux pages d’aide. Les chaînes de documentation doivent apparaître dans la table des API.

![](creating-api-help-pages/_static/image8.png)

La page d’aide lit les chaînes à partir du fichier XML au moment de l’exécution. (Lorsque vous déployez l’application, veillez à déployer le fichier XML.)

## <a name="under-the-hood"></a>En coulisses

Les pages d’aide s’appuient sur la classe **ApiExplorer** , qui fait partie de l’infrastructure de l’API Web. La classe **ApiExplorer** fournit la documentation brute pour la création d’une page d’aide. Pour chaque API, **ApiExplorer** contient un **ApiDescription** qui décrit l’API. À cet effet, une « API » est définie comme la combinaison de la méthode HTTP et de l’URI relatif. Par exemple, voici quelques API distinctes :

- Obtient/api/Products
- Obtient/api/Products/{id}
- POSTER/api/Products

Si une action de contrôleur prend en charge plusieurs méthodes HTTP, **ApiExplorer** traite chaque méthode comme une API distincte.

Pour masquer une API dans **ApiExplorer**, ajoutez l’attribut **ApiExplorerSettings** à l’action et définissez *IgnoreApi* sur true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Vous pouvez également ajouter cet attribut au contrôleur pour exclure l’ensemble du contrôleur.

La classe ApiExplorer obtient les chaînes de documentation à partir de l’interface **IDocumentationProvider** . Comme vous l’avez vu précédemment, la bibliothèque de pages d’aide fournit un **IDocumentationProvider** qui obtient la documentation des chaînes de documentation XML. Le code se trouve dans/Areas/HelpPage/XmlDocumentationProvider.cs. Vous pouvez vous procurer la documentation à partir d’une autre source en écrivant votre propre **IDocumentationProvider**. Pour l’associer, appelez la méthode d’extension **SetDocumentationProvider** , définie dans **HelpPageConfigurationExtensions**

**ApiExplorer** appelle automatiquement l’interface **IDocumentationProvider** pour obtenir des chaînes de documentation pour chaque API. Elle les stocke dans la propriété **documentation** des objets **ApiDescription** et **ApiParameterDescription** .

## <a name="next-steps"></a>Étapes suivantes

Vous n’êtes pas limité aux pages d’aide indiquées ici. En fait, **ApiExplorer** n’est pas limité à la création de pages d’aide. Yao Huang lin a écrit des billets de blog pour vous aider à réfléchir à la boîte :

- [Ajout d’un client test simple à API Web ASP.NET page d’aide](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Création d’API Web ASP.NET page d’aide sur les services auto-hébergés](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Génération de la page d’aide (ou du client) au moment du design pour API Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personnalisations de la page d’aide avancée](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
