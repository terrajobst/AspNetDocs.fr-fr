---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Créer une application cliente OData v4C#() | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556293"
---
# <a name="create-an-odata-v4-client-app-c"></a>Créer une application cliente OData v4 (C#)

par [Mike Wasson](https://github.com/MikeWasson)

Dans le didacticiel précédent, vous avez créé un service OData de base qui prend en charge les opérations CRUD. Créons maintenant un client pour le service.

Démarrez une nouvelle instance de Visual Studio et créez un nouveau projet d’application console. Dans la boîte de dialogue **nouveau projet** , sélectionnez **installé** &gt; **modèles** &gt; **Visual C#**  &gt; **Bureau Windows**, puis sélectionnez le modèle **application console** . Nommez le projet &quot;&quot;ProductsApp.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Vous pouvez également ajouter l’application console à la même solution Visual Studio qui contient le service OData.

## <a name="install-the-odata-client-code-generator"></a>Installer le générateur de code OData client

Dans le menu **Outils**, sélectionnez **Extensions et mises à jour**. Sélectionnez **en ligne** &gt; **Galerie Visual Studio**. Dans la zone de recherche, recherchez &quot;générateur de code du client OData&quot;. Cliquez sur **Télécharger** pour installer le VSIX. Vous pouvez être invité à redémarrer Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Exécuter le service OData localement

Exécutez le projet ProductService à partir de Visual Studio. Par défaut, Visual Studio lance un navigateur à la racine de l’application. Notez l’URI ; vous en aurez besoin à l’étape suivante. Laissez l'application en cours d'exécution.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Si vous placez les deux projets dans la même solution, assurez-vous d’exécuter le projet ProductService sans débogage. À l’étape suivante, vous devrez garder le service en cours d’exécution pendant que vous modifiez le projet d’application console.

## <a name="generate-the-service-proxy"></a>Générer le proxy de service

Le proxy de service est une classe .NET qui définit des méthodes pour accéder au service OData. Le proxy convertit les appels de méthode en requêtes HTTP. Vous allez créer la classe proxy en exécutant un [modèle T4](https://msdn.microsoft.com/library/bb126445.aspx).

Cliquez avec le bouton droit sur le projet. Sélectionnez **ajouter** &gt; **nouvel élément**.

![](create-an-odata-v4-client-app/_static/image5.png)

Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **éléments visuels C#**  &gt; **code** &gt; **client OData**. Nommez le modèle &quot;&quot;ProductClient.tt. Cliquez sur **Ajouter** , puis cliquez sur l’avertissement de sécurité.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

À ce stade, vous obtenez une erreur, que vous pouvez ignorer. Visual Studio exécute automatiquement le modèle, mais le modèle nécessite d’abord certains paramètres de configuration.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Ouvrez le fichier ProductClient. OData. config. Dans l’élément `Parameter`, collez l’URI du projet ProductService (étape précédente). Exemple :

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Réexécutez le modèle. Dans Explorateur de solutions, cliquez avec le bouton droit sur le fichier ProductClient.tt et sélectionnez **exécuter un outil personnalisé**.

Le modèle crée un fichier de code nommé ProductClient.cs qui définit le proxy. Lorsque vous développez votre application, si vous modifiez le point de terminaison OData, réexécutez le modèle pour mettre à jour le proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Utiliser le proxy de service pour appeler le service OData

Ouvrez le fichier Program.cs et remplacez le code réutilisable par ce qui suit.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Remplacez la valeur de *ServiceUri* par l’URI de service de la version antérieure.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Lorsque vous exécutez l’application, elle doit générer le résultat suivant :

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
