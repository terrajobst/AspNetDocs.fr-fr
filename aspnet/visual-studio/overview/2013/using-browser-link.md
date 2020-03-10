---
uid: visual-studio/overview/2013/using-browser-link
title: Utilisation du lien de navigateur dans Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622905"
---
# <a name="using-browser-link-in-visual-studio-2013"></a>Utilisation du lien de navigateur dans Visual Studio 2013

par [Mike Wasson](https://github.com/MikeWasson)

Le lien du navigateur est une nouvelle fonctionnalité de Visual Studio 2013 qui crée un canal de communication entre l’environnement de développement et un ou plusieurs navigateurs Web. Vous pouvez utiliser le lien du navigateur pour actualiser votre application web dans plusieurs navigateurs à la fois, ce qui est utile pour les tests dans plusieurs navigateurs.

- [Actualisation du navigateur](#browser-refresh)
- [Affichage du tableau de bord des liens de navigateur](#dashboard)
- [Activation du lien de navigateur pour les fichiers HTML statiques](#static-html)
- [Désactivation du lien de navigateur](#disabling)
- [Comment cela fonctionne-t-il ?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Actualisation du navigateur

Avec l’actualisation du navigateur, vous pouvez actualiser plusieurs navigateurs connectés à Visual Studio via le lien du navigateur.

Pour utiliser l’actualisation du navigateur, commencez par créer une application ASP.NET à l’aide de l’un des modèles de projet. Déboguez l’application en appuyant sur F5 ou en cliquant sur l’icône de flèche dans la barre d’outils :

![](using-browser-link/_static/image1.png)

Vous pouvez également utiliser la liste déroulante pour sélectionner un navigateur spécifique à déboguer.

![](using-browser-link/_static/image2.png)

Pour déboguer avec plusieurs navigateurs, sélectionnez **Parcourir avec**. Dans la boîte de dialogue **naviguer avec** , maintenez la touche CTRL enfoncée pour sélectionner plusieurs navigateurs. Cliquez sur **Parcourir** pour déboguer avec les navigateurs sélectionnés. Le lien du navigateur fonctionne également si vous lancez un navigateur en dehors de Visual Studio et accédez à l’URL de l’application.

![](using-browser-link/_static/image3.png)

Les contrôles de lien de navigateur se trouvent dans la liste déroulante avec l’icône de flèche circulaire. L’icône en flèche est le bouton **Actualiser** .

![](using-browser-link/_static/image4.png)

Pour voir quels navigateurs sont connectés, pointez la souris sur le bouton **Actualiser** pendant le débogage. Les navigateurs connectés s’affichent dans une fenêtre d’info-bulle.

![](using-browser-link/_static/image5.png)

Pour actualiser les navigateurs connectés, cliquez sur le bouton **Actualiser** ou appuyez sur Ctrl + Alt + Entrée. Par exemple, la capture d’écran suivante montre un projet ASP.NET, que j’ai créé à l’aide du modèle de projet MVC 5. Vous pouvez voir l’application en cours d’exécution dans deux navigateurs en haut. En bas, le projet est ouvert dans Visual Studio.

![](using-browser-link/_static/image6.png)

Dans Visual Studio, j’ai modifié l’en-tête &lt;H1&gt; pour la page d’hébergement :

![](using-browser-link/_static/image7.png)

Lorsque j’ai cliqué sur le bouton **Actualiser** , la modification apparaît dans les deux fenêtres du navigateur :

![](using-browser-link/_static/image8.png)

**Remarques**

- Pour activer le lien du navigateur, définissez `debug=true` dans l’élément [&gt;de compilation&lt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) dans le fichier Web. config du projet.
- L’application doit être en cours d’exécution sur localhost.
- L’application doit cibler .NET 4,0 ou une version ultérieure.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Affichage du tableau de bord des liens de navigateur

Le tableau de bord du lien de navigateur affiche des informations sur les connexions des liens du navigateur. Pour afficher le tableau de bord, sélectionnez le menu déroulant lien du navigateur (la petite flèche en regard du bouton **Actualiser** ). Cliquez ensuite sur le **tableau de bord lien de navigateur**.

![](using-browser-link/_static/image9.png)

Le tableau de bord répertorie les navigateurs connectés et l’URL vers laquelle chaque navigateur a navigué.

![](using-browser-link/_static/image10.png)

La section **conditions préalables** affiche toutes les étapes nécessaires pour activer le lien du navigateur pour ce projet. Par exemple, la capture d’écran suivante montre un projet où « Debug » a la valeur false dans le fichier Web. config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Activation du lien de navigateur pour les fichiers HTML statiques

Pour activer le lien du navigateur pour les fichiers HTML statiques, ajoutez le code suivant à votre fichier Web. config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Pour des raisons de performances, supprimez ce paramètre lorsque vous publiez votre projet.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Désactivation du lien de navigateur

Le lien du navigateur est activé par défaut. Il existe plusieurs façons de le désactiver :

- Dans le menu déroulant lien du navigateur, décochez la case **activer le lien du navigateur**. 

    ![](using-browser-link/_static/image12.png)
- Dans le fichier Web. config, ajoutez une clé nommée « vs : EnableBrowserLink » avec la valeur « false » dans la section appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Dans le fichier Web. config, définissez debug sur false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Comment cela fonctionne-t-il ?

Le lien du navigateur utilise [signalr](../../../signalr/index.md) pour créer un canal de communication entre Visual Studio et le navigateur. Quand le lien du navigateur est activé, Visual Studio fait office de serveur SignalR auquel plusieurs clients (navigateurs) peuvent se connecter. Le lien du navigateur enregistre également un module HTTP avec ASP.NET. Ce module injecte des références de&gt; de script &lt;spéciales dans chaque demande de page à partir du serveur. Vous pouvez voir les références du script en sélectionnant « Afficher la source » dans le navigateur.

![](using-browser-link/_static/image13.png)

Vos fichiers sources ne sont pas modifiés. Le module HTTP injecte les références de script de manière dynamique.

Étant donné que le code côté navigateur est tout JavaScript, il fonctionne sur tous les navigateurs [pris en charge par signalr](../../../signalr/overview/getting-started/supported-platforms.md), sans nécessiter de plug-in de navigateur.
