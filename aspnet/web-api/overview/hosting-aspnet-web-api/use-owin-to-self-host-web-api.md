---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Utiliser OWIN pour l’auto-hébergement API Web ASP.NET-ASP.NET 4. x
author: rick-anderson
description: Didacticiel contenant du code illustrant comment héberger des API Web ASP.NET dans une application console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556538"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a>Utiliser OWIN pour l’auto-hébergement API Web ASP.NET 

> Ce didacticiel montre comment héberger des API Web ASP.NET dans une application console, à l’aide de OWIN pour auto-héberger l’infrastructure de l’API Web.
>
> [Open Web interface for .net](http://owin.org) (OWIN) définit une abstraction entre les serveurs Web .net et les applications Web. OWIN découple l’application Web du serveur, ce qui fait de OWIN idéal pour l’auto-hébergement d’une application Web dans votre propre processus, en dehors d’IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - API Web 5.2.7

> [!NOTE]
> Vous trouverez le code source complet de ce didacticiel sur [github.com/ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).

## <a name="create-a-console-application"></a>Créer une application console

Dans le menu **fichier** , cliquez sur **nouveau**, puis sélectionnez **projet**. Dans **installé**, sous **visuel C#** , sélectionnez **Bureau Windows** , puis sélectionnez **application console (.NET Framework)** . Nommez le projet « OwinSelfhostSample » et sélectionnez **OK**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Ajouter l’API Web et les packages OWIN

Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Cette opération installe le package WebAPI OWIN selfHost et tous les packages OWIN requis.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configurer l’API Web pour l’auto-hébergement

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe. Nommez la classe `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Remplacez tout le code réutilisable dans ce fichier par ce qui suit :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Ajouter un contrôleur d’API Web

Ensuite, ajoutez une classe de contrôleur d’API Web. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **ajouter** / **classe** pour ajouter une nouvelle classe. Nommez la classe `ValuesController`.

Remplacez tout le code réutilisable dans ce fichier par ce qui suit :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Démarrer l’hôte OWIN et effectuer une demande avec HttpClient

Remplacez tout le code réutilisable dans le fichier Program.cs par le code suivant :

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Exécuter l'application

Pour exécuter l’application, appuyez sur F5 dans Visual Studio. La sortie doit se présenter comme suit :

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Ressources supplémentaires

[Vue d’ensemble du projet Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[API Web ASP.NET d’hôte dans un rôle de travail Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
