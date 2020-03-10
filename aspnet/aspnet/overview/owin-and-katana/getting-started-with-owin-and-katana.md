---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Prise en main avec OWIN et Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584671"
---
# <a name="getting-started-with-owin-and-katana"></a>Bien démarrer avec OWIN et Katana

par [Mike Wasson](https://github.com/MikeWasson)

[Open Web interface for .net (OWIN)](http://owin.org/) définit une abstraction entre les serveurs Web .net et les applications Web. En découplant le serveur Web de l’application, OWIN facilite la création d’un intergiciel (middleware) pour le développement Web .NET. De plus, OWIN facilite le portage des applications Web vers d’autres&#8212;hôtes, par exemple l’auto-hébergement dans un service Windows ou tout autre processus.

OWIN est une spécification appartenant à la Communauté, et non une implémentation. Le projet Katana est un ensemble de composants OWIN Open source développés par Microsoft. Pour obtenir une vue d’ensemble générale des OWIN et Katana, consultez [vue d’ensemble du projet Katana](an-overview-of-project-katana.md). Dans cet article, je vais passer directement au code pour commencer.

Ce didacticiel utilise [Visual Studio 2013 version Release candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mais vous pouvez également utiliser Visual Studio 2012. Certaines étapes sont différentes dans Visual Studio 2012, comme je l’ai indiqué ci-dessous.

## <a name="host-owin-in-iis"></a>Héberger OWIN dans IIS

Dans cette section, nous allons héberger OWIN dans IIS. Cette option vous offre la flexibilité et la composition d’un pipeline OWIN avec l’ensemble de fonctionnalités matures d’IIS. À l’aide de cette option, l’application OWIN s’exécute dans le pipeline de demande ASP.NET.

Tout d’abord, créez un projet d’application Web ASP.NET. (Dans Visual Studio 2012, utilisez le type de projet d’application Web vide ASP.NET.)

![](getting-started-with-owin-and-katana/_static/image1.png)

Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **vide** .

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Ajouter des paquets NuGet

Ensuite, ajoutez les packages NuGet requis. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre de la console du gestionnaire de package, tapez la commande suivante :

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Ajouter une classe de démarrage

Ensuite, ajoutez une classe de démarrage OWIN. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet et sélectionnez **Ajouter**, puis sélectionnez **nouvel élément**. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **Owin Startup Class**. Pour plus d’informations sur la configuration de la classe de démarrage, consultez détection de la [classe de démarrage OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Ajoutez le code suivant à la méthode `Startup1.Configuration` :

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Ce code ajoute un simple élément d’intergiciel au pipeline OWIN, implémenté comme une fonction qui reçoit une instance **Microsoft. OWIN. IOwinContext** . Lorsque le serveur reçoit une requête HTTP, le pipeline OWIN appelle l’intergiciel (middleware). L’intergiciel définit le type de contenu de la réponse et écrit le corps de la réponse.

> [!NOTE]
> Le modèle de classe de démarrage OWIN est disponible dans Visual Studio 2013. Si vous utilisez Visual Studio 2012, ajoutez simplement une nouvelle classe vide nommée `Startup1`, puis collez le code suivant :

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Exécution de l'application

Appuyez sur F5 pour commencer le débogage. Visual Studio ouvre une fenêtre de navigateur pour `http://localhost:*port*/`. La page doit ressembler à ce qui suit :

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>OWIN de l’auto-hébergement dans une application console

Il est facile de convertir cette application de l’hébergement IIS vers l’auto-hébergement dans un processus personnalisé. Avec l’hébergement IIS, IIS agit à la fois comme serveur HTTP et comme processus qui héberge le service. Avec l’auto-hébergement, votre application crée le processus et utilise la classe **HttpListener** comme serveur http.

Dans Visual Studio, créez une application console. Dans la fenêtre de la console du gestionnaire de package, tapez la commande suivante :

`Install-Package Microsoft.Owin.SelfHost -Pre`

Ajoutez une classe `Startup1` de la partie 1 de ce didacticiel au projet. Vous n’avez pas besoin de modifier cette classe.

Implémentez la méthode `Main` de l’application comme suit.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Lorsque vous exécutez l’application console, le serveur commence à écouter `http://localhost:9000`. Si vous accédez à cette adresse dans un navigateur Web, la page « Hello World » s’affiche.

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Ajouter OWIN Diagnostics

Le package Microsoft. Owin. Diagnostics contient un intergiciel (middleware) qui intercepte les exceptions non gérées et affiche une page HTML avec les détails de l’erreur. Cette page fonctionne de façon très similaire à la page d’erreur ASP.NET, parfois appelée «[écran jaune de décès](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)» (YSOD). Comme YSOD, la page d’erreur Katana est utile lors du développement, mais il est conseillé de la désactiver en mode production.

Pour installer le package de diagnostics dans votre projet, tapez la commande suivante dans la fenêtre console du gestionnaire de package :

`install-package Microsoft.Owin.Diagnostics –Pre`

Modifiez le code de votre méthode `Startup1.Configuration` comme suit :

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Utilisez maintenant CTRL + F5 pour exécuter l’application sans débogage, de sorte que Visual Studio ne s’arrête pas sur l’exception. L’application se comporte de la même manière qu’auparavant, jusqu’à ce que vous naviguiez vers `http://localhost/fail`, à ce stade, l’application lève l’exception. L’intergiciel (middleware) de la page d’erreurs intercepte l’exception et affiche une page HTML contenant des informations sur l’erreur. Vous pouvez cliquer sur les onglets pour afficher la pile, la chaîne de requête, les cookies, l’en-tête de demande et les variables d’environnement OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Étapes suivantes

- [Détection de classe de démarrage OWIN](owin-startup-class-detection.md)
- [Utiliser OWIN pour l’auto-hébergement API Web ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Utiliser OWIN pour l’auto-hébergement Signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
