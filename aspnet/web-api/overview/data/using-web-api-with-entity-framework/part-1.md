---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Utilisation de l’API Web 2 avec Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Ce didacticiel vous apprend les bases de la création d’une application Web à l’aide d’un back end API Web ASP.NET. Le didacticiel utilise Entity Framework 6 pour la disposition des données...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622478"
---
# <a name="using-web-api-2-with-entity-framework-6"></a>Utilisation de Web API 2 avec Entity Framework 6

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

> Ce didacticiel vous apprend les bases de la création d’une application Web à l’aide d’un back end API Web ASP.NET. Le didacticiel utilise Entity Framework 6 pour la couche de données et Knockout. js pour l’application JavaScript côté client. Ce didacticiel montre également comment déployer l’application sur Azure App Service Web Apps.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
> - API Web 2,1
> - Visual Studio 2017 (Télécharger Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
> - [Knockout. js](http://knockoutjs.com/) 3,1

Ce didacticiel utilise API Web ASP.NET 2 avec Entity Framework 6 pour créer une application Web qui manipule une base de données principale. Voici une capture d’écran de l’application que vous allez créer.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

L’application utilise une conception d’application à page unique (SPA). « Application à page unique » est le terme général pour une application Web qui charge une seule page HTML, puis met à jour la page dynamiquement, au lieu de charger de nouvelles pages. Après le chargement initial de la page, l’application communique avec le serveur via des demandes AJAX. Les requêtes AJAX retournent des données JSON, que l’application utilise pour mettre à jour l’interface utilisateur.

AJAX n’est pas nouveau, mais aujourd’hui, il existe des infrastructures JavaScript qui facilitent la création et la maintenance d’une application SPA sophistiquée de grande taille. Ce didacticiel utilise [Knockout. js](http://knockoutjs.com/), mais vous pouvez utiliser n’importe quelle infrastructure client JavaScript.

Voici les principaux blocs de construction pour cette application :

- ASP.NET MVC crée la page HTML.
- API Web ASP.NET gère les requêtes AJAX et retourne les données JSON.
- Données Knockout. js-lie les éléments HTML aux données JSON.
- Entity Framework communique avec la base de données.

## <a name="see-this-app-running-on-azure"></a>Voir cette application en cours d’exécution sur Azure

Voulez-vous que le site terminé s’exécute en tant qu’application Web en direct ? Vous pouvez déployer une version complète de l’application sur votre compte Azure en sélectionnant le bouton suivant.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous n’avez pas encore de compte, vous disposez des options suivantes :

- [Ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : vous recevez des crédits que vous pouvez utiliser pour tester les services Azure payants, et même après leur utilisation, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages des abonnés MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

## <a name="create-the-project"></a>Créer le projet

Ouvrez Visual Studio. Dans le menu **fichier** , sélectionnez **nouveau**, puis sélectionnez **projet**. (Ou sélectionnez **nouveau projet** dans la page de démarrage.)

Dans la boîte de dialogue **nouveau projet** , sélectionnez **Web** dans le volet gauche et **application Web ASP.net (.NET Framework)** dans le volet central. Nommez le projet **BookService** et sélectionnez **OK**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **API Web** .

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

Sélectionnez **OK** pour créer le projet.

## <a name="configure-azure-settings-optional"></a>Configurer les paramètres Azure (facultatif)

Après avoir créé le projet, vous pouvez choisir de déployer sur Azure App Service Web Apps à tout moment. 

1. Dans Explorateur de solutions, cliquez avec le bouton droit sur votre projet et sélectionnez **publier**.

2. Dans la fenêtre qui s’affiche, sélectionnez **Démarrer**. La boîte **de dialogue choisir une cible de publication** s’affiche.

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Sélectionnez **Créer un profil**. La boîte de dialogue **Créer App Service** s’affiche.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Acceptez les valeurs par défaut ou entrez des valeurs différentes pour le nom de l’application, le groupe de ressources, le plan d’hébergement, l’abonnement Azure et la région géographique. 

4. Sélectionnez **créer une base de données SQL**. La boîte de dialogue **configurer SQL Server** s’affiche. 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Acceptez les valeurs par défaut ou entrez des valeurs différentes. Entrez un **nom d’utilisateur d’administrateur** et un **mot de passe d’administrateur** pour votre nouvelle base de données. Lorsque vous avez terminé, sélectionnez **OK** . La page **créer App service** réapparaît.

5. Sélectionnez **créer** pour créer votre profil. Un message s’affiche dans l’angle inférieur droit indiquant que le déploiement est en cours. Après quelques instants, la fenêtre de **publication** réapparaît.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    Le profil que vous avez créé pour déployer l’application est maintenant disponible. 

> [!div class="step-by-step"]
> [Next](part-2.md)
