---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Auto-hébergement API Web ASP.NET 1 (C#)-ASP.net 4. x
author: MikeWasson
description: Le didacticiel avec code montre comment héberger une API Web dans une application console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525087"
---
# <a name="self-host-aspnet-web-api-1-c"></a>Auto-hébergement API Web ASP.NET 1 (C#)

par [Mike Wasson](https://github.com/MikeWasson)

> Ce didacticiel montre comment héberger une API Web dans une application console. API Web ASP.NET ne requiert pas IIS. Vous pouvez auto-héberger une API Web dans votre propre processus hôte. 
> 
> **Les nouvelles applications doivent utiliser OWIN pour l’API Web auto-hébergée.** Voir [utiliser OWIN pour l’auto-hébergement API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - API web 1
> - Visual Studio 2012

## <a name="create-the-console-application-project"></a>Créer le projet d’application console

Démarrez Visual Studio et sélectionnez **nouveau projet** dans la page de **démarrage** . Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Windows**. Dans la liste des modèles de projet, sélectionnez **application console**. Nommez le projet &quot;&quot; SelfHost, puis cliquez sur **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Définir la version cible de .NET Framework (Visual Studio 2010)

Si vous utilisez Visual Studio 2010, remplacez la version cible de .NET Framework par .NET Framework 4,0. (Par défaut, le modèle de projet cible le [.NET Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**. Dans la liste déroulante **Framework cible** , remplacez la version cible de .net framework par .NET Framework 4,0. Lorsque vous êtes invité à appliquer la modification, cliquez sur **Oui**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Installer le gestionnaire de package NuGet

Le gestionnaire de package NuGet est le moyen le plus simple d’ajouter les assemblys d’API Web à un projet non-ASP.NET.

Pour vérifier si le gestionnaire de package NuGet est installé, cliquez sur le menu **Outils** dans Visual Studio. Si vous voyez un élément de menu appelé **Gestionnaire de package NuGet**, vous disposez du gestionnaire de package NuGet.

Pour installer le gestionnaire de package NuGet :

1. Démarrez Visual Studio.
2. Dans le menu **Outils**, sélectionnez **Extensions et mises à jour**.
3. Dans la boîte de dialogue **extensions et mises à jour** , sélectionnez **en ligne**.
4. Si vous ne voyez pas « gestionnaire de package NuGet », tapez « gestionnaire de package NuGet » dans la zone de recherche.
5. Sélectionnez le gestionnaire de package NuGet, puis cliquez sur **Télécharger**.
6. Une fois le téléchargement terminé, vous êtes invité à installer.
7. Une fois l’installation terminée, vous pouvez être invité à redémarrer Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Ajouter le package NuGet de l’API Web

Une fois le gestionnaire de package NuGet installé, ajoutez le package d’hébergement d’API Web à votre projet.

1. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**. *Remarque*: Si vous ne voyez pas cet élément de menu, vérifiez que le gestionnaire de package NuGet est correctement installé.
2. Sélectionnez **gérer les packages NuGet pour la solution**
3. Dans la boîte de dialogue **gérer les packages pépite** , sélectionnez **en ligne**.
4. Dans la zone de recherche, tapez &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.
5. Sélectionnez le API Web ASP.NET package auto-hôte, puis cliquez sur **installer**.
6. Une fois le package installé, cliquez sur **Fermer** pour fermer la boîte de dialogue.

> [!NOTE]
> Veillez à installer le package nommé Microsoft. AspNet. WebApi. SelfHost, et non AspNetWebApi. SelfHost.

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Créer le modèle et le contrôleur

Ce didacticiel utilise les mêmes classes de modèle et de contrôleur que le didacticiel [prise en main](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .

Ajoutez une classe publique nommée `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Ajoutez une classe publique nommée `ProductsController`. Dérivez cette classe de **System. Web. http. ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Pour plus d’informations sur le code de ce contrôleur, reportez-vous au didacticiel [prise en main](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) . Ce contrôleur définit trois actions d’extraction :

| URI | Description |
| --- | --- |
| /api/products | Obtenir la liste de tous les produits. |
| *ID* /API/Products/ | Obtenir un produit par ID. |
| /API/products/ ? category =*catégorie* | Obtenir une liste de produits par catégorie. |

## <a name="host-the-web-api"></a>Héberger l’API Web

Ouvrez le fichier Program.cs et ajoutez les instructions using suivantes :

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Ajoutez le code suivant à la classe **Program** .

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>Facultatif Ajouter une réservation d’espace de noms d’URL HTTP

Cette application écoute les `http://localhost:8080/`. Par défaut, l’écoute d’une adresse HTTP particulière requiert des privilèges d’administrateur. Par conséquent, lorsque vous exécutez le didacticiel, vous pouvez recevoir cette erreur : « HTTP n’a pas pu inscrire l’URL http://+:8080/» il existe deux façons d’éviter cette erreur :

- Exécuter Visual Studio avec des autorisations d’administrateur élevées ou
- Utilisez netsh. exe pour accorder à votre compte les autorisations nécessaires pour réserver l’URL.

Pour utiliser Netsh. exe, ouvrez une invite de commandes avec des privilèges d’administrateur, puis entrez la commande suivante :

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

où *ordinateur\nom* est votre compte d’utilisateur.

Une fois l’auto-hébergement terminé, veillez à supprimer la réservation :

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Appeler l’API Web à partir d’une applicationC#cliente ()

Nous allons écrire une application console simple qui appelle l’API Web.

Ajoutez un nouveau projet d’application console à la solution :

- Dans Explorateur de solutions, cliquez avec le bouton droit sur la solution et sélectionnez **Ajouter nouveau projet**.
- Créez une nouvelle application console nommée &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Utilisez le gestionnaire de package NuGet pour ajouter le package de bibliothèques principales API Web ASP.NET :

- Dans le menu Outils, sélectionnez **Gestionnaire de package NuGet**.
- Sélectionnez **gérer les packages NuGet pour la solution**
- Dans la boîte de dialogue **gérer les packages NuGet** , sélectionnez **en ligne**.
- Dans la zone de recherche, tapez &quot;Microsoft. AspNet. WebApi. client&quot;.
- Sélectionnez le package bibliothèques clientes de l’API Web Microsoft ASP.NET, puis cliquez sur **installer**.

Ajoutez une référence dans ClientApp au projet SelfHost :

- Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet ClientApp.
- Sélectionnez **Ajouter une référence**.
- Dans la boîte de dialogue **Gestionnaire de références** , sous **solution**, sélectionnez **projets**.
- Sélectionnez le projet SelfHost.
- Cliquez sur **OK**.

![](self-host-a-web-api/_static/image6.png)

Ouvrez le fichier client/programme. cs. Ajoutez les instructions **using** suivantes :

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Ajoutez une instance **httpclient** statique :

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Ajoutez les méthodes suivantes pour répertorier tous les produits, répertorier un produit par ID et répertorier les produits par catégorie.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Chacune de ces méthodes suit le même modèle :

1. Appelez **httpclient. GetAsync** pour envoyer une requête d’extraction à l’URI approprié.
2. Appelez **HttpResponseMessage. EnsureSuccessStatusCode**. Cette méthode lève une exception si l’état de la réponse HTTP est un code d’erreur.
3. Appelez **ReadAsAsync&lt;t&gt;** pour désérialiser un type CLR à partir de la réponse http. Cette méthode est une méthode d’extension, définie dans **System .net. http. HttpContentExtensions**.

Les méthodes **GetAsync** et **ReadAsAsync** sont toutes deux asynchrones. Elles retournent des objets de **tâche** qui représentent l’opération asynchrone. L’obtention de la propriété **result** bloque le thread jusqu’à ce que l’opération se termine.

Pour plus d’informations sur l’utilisation de HttpClient, notamment sur la façon d’effectuer des appels non bloquants, consultez [appel d’une API Web à partir d’un client .net](../advanced/calling-a-web-api-from-a-net-client.md).

Avant d’appeler ces méthodes, définissez la propriété BaseAddress sur l’instance HttpClient sur «`http://localhost:8080`». Exemple :

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Cela doit générer le résultat suivant. (N’oubliez pas d’exécuter d’abord l’application SelfHost.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
