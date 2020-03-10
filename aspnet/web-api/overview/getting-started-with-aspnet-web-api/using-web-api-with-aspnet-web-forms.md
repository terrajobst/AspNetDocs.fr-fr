---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Utilisation de l’API Web avec ASP.NET Web Forms-ASP.NET 4. x
author: MikeWasson
description: Didacticiel avec code étape par étape pour ajouter une API Web à une application ASP.NET Forms pour ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556776"
---
# <a name="using-web-api-with-aspnet-web-forms"></a>Utilisation de l’API web avec ASP.NET Web Forms

par [Mike Wasson](https://github.com/MikeWasson)

Ce didacticiel vous guide tout au long des étapes d’ajout de l’API Web à une application ASP.NET Web Forms traditionnelle dans ASP.NET 4. x. 

## <a name="overview"></a>Présentation

Bien que API Web ASP.NET soit empaqueté avec ASP.NET MVC, il est facile d’ajouter l’API Web à une application ASP.NET Web Forms traditionnelle.

Pour utiliser l’API Web dans une application Web Forms, il y a deux étapes principales :

- Ajoutez un contrôleur d’API Web dérivé de la classe **ApiController** .
- Ajoutez une table de routage à l' **Application\_méthode Start** .

## <a name="create-a-web-forms-project"></a>Créer un projet Web Forms

Démarrez Visual Studio et sélectionnez **nouveau projet** dans la page de **démarrage** . Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **ASP.NET Web Forms application**. Entrez un nom pour le projet, puis cliquez sur **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Créer le modèle et le contrôleur

Ce didacticiel utilise les mêmes classes de modèle et de contrôleur que le didacticiel [prise en main](tutorial-your-first-web-api.md) .

Tout d’abord, ajoutez une classe de modèle. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Ajouter une classe**. Nommez la classe Product, puis ajoutez l’implémentation suivante :

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Ensuite, ajoutez un contrôleur d’API Web au projet., un *contrôleur* est l’objet qui gère les requêtes http pour l’API Web.

Dans l’**Explorateur de solutions**, cliquez avec le bouton droit sur le projet. Sélectionnez **Ajouter un nouvel élément**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

Sous **modèles installés**, développez **visuel C#**  et sélectionnez **Web**. Ensuite, dans la liste des modèles, sélectionnez **classe de contrôleur d’API Web**. Nommez le contrôleur « ProductsController », puis cliquez sur **Ajouter**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

L’Assistant **Ajouter un nouvel élément** va créer un fichier nommé ProductsController.cs. Supprimez les méthodes incluses dans l’Assistant et ajoutez les méthodes suivantes :

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Pour plus d’informations sur le code de ce contrôleur, reportez-vous au didacticiel [prise en main](tutorial-your-first-web-api.md) .

## <a name="add-routing-information"></a>Ajouter des informations de routage

Ensuite, nous allons ajouter un itinéraire URI afin que les URI de la forme &quot;/API/Products/&quot; soient acheminés vers le contrôleur.

Dans **Explorateur de solutions**, double-cliquez sur global. asax pour ouvrir le fichier code-behind global.asax.cs. Ajoutez l’instruction **using** suivante.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Ajoutez ensuite le code suivant à l' **Application\_méthode Start** :

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Pour plus d’informations sur les tables de routage, consultez [routage dans API Web ASP.net](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Ajouter AJAX côté client

C’est tout ce dont vous avez besoin pour créer une API Web à laquelle les clients peuvent accéder. À présent, nous allons ajouter une page HTML qui utilise jQuery pour appeler l’API.

Assurez-vous que votre page maître (par exemple, *site. Master*) comprend une `ContentPlaceHolder` avec `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Ouvrez le fichier default. aspx. Remplacez le texte réutilisable qui se trouve dans la section de contenu principale, comme indiqué ci-dessous :

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Ensuite, ajoutez une référence au fichier source jQuery dans la section `HeaderContent` :

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Remarque : vous pouvez facilement ajouter la référence de script en faisant glisser le fichier de **Explorateur de solutions** vers la fenêtre de l’éditeur de code.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Sous la balise de script jQuery, ajoutez le bloc de script suivant :

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Lors du chargement du document, ce script effectue une demande AJAX à &quot;&quot;des API/produits. La requête retourne une liste de produits au format JSON. Le script ajoute les informations sur le produit à la table HTML.

Quand vous exécutez l’application, elle doit ressembler à ceci :

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
