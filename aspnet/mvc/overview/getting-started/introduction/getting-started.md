---
uid: mvc/overview/getting-started/introduction/getting-started
title: Prise en main avec ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: ca39bc37c757c0452cf56624c8e37c04df4b41f2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602766"
---
# <a name="getting-started-with-aspnet-mvc-5"></a>Prise en main de ASP.NET MVC 5

par [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC 5 à l’aide de [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Le code source final du didacticiel se trouve sur [GitHub](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Ce didacticiel a été rédigé [par Scott Guthrie](https://weblogs.asp.net/scottgu/) (Twitter[@scottgu](https://twitter.com/scottgu) ), [scott Hanselman](http://www.hanselman.com/blog/) (Twitter : [@shanselman](https://twitter.com/shanselman) ) et [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )

Vous avez besoin d’un compte Azure pour déployer cette application sur Azure :

- Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : vous recevez des crédits que vous pouvez utiliser pour tester les services Azure payants, et même après leur utilisation, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- Vous pouvez [activer les avantages de l'abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : votre abonnement MSDN vous donne droit chaque mois à des crédits dont vous pouvez vous servir pour les services Azure payants.

## <a name="get-started"></a>Prise en main

Commencez par [installer Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Ouvrez ensuite Visual Studio.

Visual Studio est un environnement de développement intégré ou IDE. Tout comme vous utilisez Microsoft Word pour écrire des documents, vous utiliserez un IDE pour créer des applications. Dans Visual Studio, il existe une liste en bas de la liste qui présente les différentes options disponibles. Il y a également un menu qui offre une autre façon d’effectuer des tâches dans l’IDE. Par exemple, au lieu de sélectionner **nouveau projet** dans la **page de démarrage**, vous pouvez utiliser la barre de menus et sélectionner **fichier** > **nouveau projet**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Créer votre première application

Dans la **page Démarrer**, sélectionnez **nouveau projet**. Dans la boîte de dialogue **nouveau projet** , sélectionnez la catégorie **visuel C#**  sur la gauche, puis **Web**, puis sélectionnez le modèle de projet **application Web ASP.net (.NET Framework)** . Nommez votre projet « MvcMovie », puis choisissez **OK**.

![](getting-started/_static/image2.png)

Dans la boîte de dialogue **nouvelle application Web ASP.net** , choisissez **MVC** , puis choisissez **OK**.

![](getting-started/_static/image3.png)

Visual Studio utilisait un modèle par défaut pour le projet MVC ASP.NET que vous venez de créer, ce qui vous permet de disposer d’une application fonctionnelle sans rien faire ! Il s’agit d’un simple « Hello World ! » et c’est un bon point de départ pour démarrer votre application.

![](getting-started/_static/image4.png)

Appuyez sur **F5** pour démarrer le débogage. Quand vous appuyez sur **F5**, Visual Studio démarre [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application Web. Visual Studio lance ensuite un navigateur et ouvre la page d’hébergement de l’application. Notez que la barre d’adresses du navigateur indique `localhost:port#` et non `example.com`. En effet, `localhost` pointe toujours vers votre propre ordinateur local, ce qui, dans ce cas, exécute l’application que vous venez de créer. Lorsque Visual Studio exécute un projet Web, un port aléatoire est utilisé pour le serveur Web. Dans l’image ci-dessous, le numéro de port est 1234. Lorsque vous exécutez l’application, vous verrez un numéro de port différent.

![](getting-started/_static/image5.png)

Ce modèle par défaut vous donne `Home`, `Contact`et `About` pages. L’image ci-dessous ne montre pas les liens **page d’hébergement**, **à propos**de et **contact** . Selon la taille de la fenêtre de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour afficher ces liens.

![](getting-started/_static/image6.png)

L’application prend également en charge l’inscription et la connexion. L’étape suivante consiste à modifier le fonctionnement de cette application et à en savoir plus sur ASP.NET MVC. Fermez l’application MVC ASP.NET et modifiez du code.

Pour obtenir la liste des didacticiels actuels, consultez [Articles recommandés pour MVC](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Voir cette application en cours d’exécution sur Azure

Voulez-vous que le site terminé s’exécute en tant qu’application Web en direct ? Vous pouvez déployer une version complète de l’application sur votre compte Azure en cliquant simplement sur le bouton suivant.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/dotnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous n’avez pas encore de compte, utilisez l’une des options suivantes pour en créer un :

- [Ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : vous recevez des crédits que vous pouvez utiliser pour tester les services Azure payants, et même après leur utilisation, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages de l’abonné Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) : votre abonnement Visual Studio vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
