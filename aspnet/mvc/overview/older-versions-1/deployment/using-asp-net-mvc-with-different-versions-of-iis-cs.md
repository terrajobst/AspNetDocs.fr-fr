---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Utilisation de ASP.NET MVC avec différentes versions d’IISC#() | Microsoft Docs
author: microsoft
description: Dans ce didacticiel, vous allez apprendre à utiliser ASP.NET MVC et le routage d’URL avec différentes versions de Internet Information Services. Vous apprenez différentes stratégies...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: 3b0a9509c0600f3598fd1218a7b383430548d4c0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601100"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Utilisation d’ASP.NET MVC avec différentes versions d’IIS (C#)

par [Microsoft](https://github.com/microsoft)

> Dans ce didacticiel, vous allez apprendre à utiliser ASP.NET MVC et le routage d’URL avec différentes versions de Internet Information Services. Vous apprenez différentes stratégies pour l’utilisation de ASP.NET MVC avec IIS 7,0 (mode classique), IIS 6,0 et les versions antérieures d’IIS.

L’infrastructure MVC ASP.NET dépend du routage ASP.NET pour acheminer les demandes de navigateur vers les actions de contrôleur. Pour tirer parti du routage ASP.NET, vous devrez peut-être effectuer des étapes de configuration supplémentaires sur votre serveur Web. Tout dépend de la version de Internet Information Services (IIS) et du mode de traitement des demandes pour votre application.

Voici un résumé des différentes versions d’IIS :

- IIS 7,0 (mode intégré) : aucune configuration spéciale n’est nécessaire pour utiliser le routage ASP.NET.
- IIS 7,0 (mode classique) : vous devez effectuer une configuration spéciale pour utiliser le routage ASP.NET.
- IIS 6,0 ou versions antérieures : vous devez effectuer une configuration spéciale pour utiliser le routage ASP.NET.

La dernière version d’IIS est la version 7,5 (sur Win7). IIS 7 d’IIS est inclus avec Windows Server 2008 et VISTA/SP1 et versions ultérieures. Vous pouvez également installer IIS 7,0 sur n’importe quelle version du système d’exploitation Vista, à l’exception de la version édition personnelle (voir [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7,0 prend en charge deux modes de traitement des demandes. Vous pouvez utiliser le mode intégré ou le mode classique. Vous n’avez pas besoin d’effectuer d’étapes de configuration spéciales lors de l’utilisation d’IIS 7,0 en mode intégré. Toutefois, vous devez effectuer une configuration supplémentaire lors de l’utilisation d’IIS 7,0 en mode classique.

Microsoft Windows Server 2003 comprend IIS 6,0. Vous ne pouvez pas mettre à niveau IIS 6,0 vers IIS 7,0 quand vous utilisez le système d’exploitation Windows Server 2003. Vous devez effectuer des étapes de configuration supplémentaires lors de l’utilisation d’IIS 6,0.

Microsoft Windows XP professionnel comprend IIS 5,1. Vous devez effectuer des étapes de configuration supplémentaires lors de l’utilisation d’IIS 5,1.

Enfin, Microsoft Windows 2000 et Microsoft Windows 2000 Professional incluent IIS 5,0. Vous devez effectuer des étapes de configuration supplémentaires lors de l’utilisation d’IIS 5,0.

## <a name="integrated-versus-classic-mode"></a>Mode intégré et mode classique

IIS 7,0 peut traiter les demandes à l’aide de deux modes de traitement de requête différents : intégré et classique. Le mode intégré offre de meilleures performances et davantage de fonctionnalités. Le mode classique est inclus à des fins de compatibilité descendante avec les versions antérieures d’IIS.

Le mode de traitement des demandes est déterminé par le pool d’applications. Vous pouvez déterminer quel mode de traitement est utilisé par une application Web particulière en déterminant le pool d’applications associé à l’application. Procédez comme suit :

1. Lancer le gestionnaire de Internet Information Services
2. Dans la fenêtre Connexions, sélectionnez une application.
3. Dans la fenêtre actions, cliquez sur le lien **paramètres de base** pour ouvrir la boîte de dialogue Modifier l’application (voir figure 1).
4. Notez le pool d’applications sélectionné.

Par défaut, IIS est configuré pour prendre en charge deux pools d’applications : **DefaultAppPool** et **.net AppPool classique**. Si DefaultAppPool est sélectionné, votre application s’exécute en mode de traitement de demande intégré. Si le pool d’applications .NET Classic est sélectionné, votre application s’exécute en mode de traitement de requête classique.

[![la boîte de dialogue Nouveau projet](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Figure 1**: détection du mode de traitement des demandes ([cliquez pour afficher l’image en taille réelle](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Notez que vous pouvez modifier le mode de traitement des demandes dans la boîte de dialogue Modifier l’application. Cliquez sur le bouton Sélectionner et modifiez le pool d’applications associé à l’application. Sachez qu’il existe des problèmes de compatibilité lors de la modification d’une application ASP.NET du mode classique en mode intégré. Pour plus d’informations, consultez les articles suivants :

- Mise à niveau de ASP.NET 1,1 vers IIS 7,0 sur Windows Vista et Windows Server 2008-- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Intégration de ASP.NET à IIS 7,0- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Si une application ASP.NET utilise DefaultAppPool, vous n’avez pas besoin d’effectuer d’autres étapes pour obtenir le routage ASP.NET (et par conséquent ASP.NET MVC). Toutefois, si l’application ASP.NET est configurée pour utiliser le pool d’applications .NET Classic, poursuivez la lecture, vous avez plus de travail à effectuer.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Utilisation de ASP.NET MVC avec des versions antérieures d’IIS

Si vous devez utiliser ASP.NET MVC avec une version antérieure d’IIS par rapport à IIS 7,0, ou si vous devez utiliser IIS 7,0 en mode classique, vous avez deux options. Tout d’abord, vous pouvez modifier la table de routage pour utiliser des extensions de fichier. Par exemple, au lieu de demander une URL telle que/Store/Details, vous devez demander une URL telle que/Store.aspx/Details.

La deuxième option consiste à créer un nom de *mappage de script générique*. Un mappage de scripts générique vous permet de mapper chaque requête dans l’infrastructure ASP.NET.

Si vous n’avez pas accès à votre serveur Web (par exemple, votre application ASP.NET MVC est hébergée par un fournisseur de services Internet), vous devez utiliser la première option. Si vous ne souhaitez pas modifier l’apparence de vos URL et que vous avez accès à votre serveur Web, vous pouvez utiliser la deuxième option.

Nous explorons chaque option en détail dans les sections suivantes.

## <a name="adding-extensions-to-the-route-table"></a>Ajout d’extensions à la table de routage

Le moyen le plus simple de faire fonctionner le routage ASP.NET avec des versions antérieures d’IIS consiste à modifier votre table de routage dans le fichier global. asax. Le fichier global. asax par défaut et non modifié dans la liste 1 configure un itinéraire nommé Itinéraire par défaut.

**Liste 1-global. asax (non modifié)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

L’itinéraire par défaut configuré dans la liste 1 vous permet d’acheminer les URL qui ressemblent à ceci :

/Home/Index

/Product/Details/3

/Product

Malheureusement, les versions antérieures d’IIS ne transmettent pas ces requêtes à l’infrastructure ASP.NET. Par conséquent, ces requêtes ne sont pas acheminées vers un contrôleur. Par exemple, si vous effectuez une demande de navigateur pour l’URL/Home/Index, vous obtiendrez la page d’erreur dans la figure 2.

[![la boîte de dialogue Nouveau projet](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Figure 2**: erreur de réception d’une erreur 404 introuvable ([cliquez pour afficher l’image en taille réelle](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Les versions antérieures d’IIS mappent uniquement certaines demandes à l’infrastructure ASP.NET. La demande doit être pour une URL avec l’extension de fichier appropriée. Par exemple, une demande de/SomePage.aspx est mappée à l’infrastructure ASP.NET. Toutefois, une requête pour/SomePage.htm ne le fait pas.

Par conséquent, pour que le routage ASP.NET fonctionne, nous devons modifier l’itinéraire par défaut afin qu’il inclue une extension de fichier qui est mappée à l’infrastructure ASP.NET.

Pour ce faire, utilisez un script nommé `registermvc.wsf`. Il a été inclus avec la version ASP.NET MVC 1 dans `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, mais à partir de ASP.NET 2, ce script a été déplacé vers ASP.NET futures, disponible sur [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

L’exécution de ce script enregistre une nouvelle extension. Mvc avec IIS. Après avoir inscrit l’extension. Mvc, vous pouvez modifier vos itinéraires dans le fichier global. asax afin que les itinéraires utilisent l’extension. Mvc.

Le fichier global. asax modifié dans le Listing 2 fonctionne avec les versions antérieures d’IIS.

**Liste 2-global. asax (modifié avec des extensions)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Important**: n’oubliez pas de générer à nouveau votre application ASP.NET MVC après avoir modifié le fichier global. asax.

Deux modifications importantes ont été apportées au fichier global. asax dans la liste 2. Il existe désormais deux itinéraires définis dans global. asax. Le modèle d’URL pour l’itinéraire par défaut, le premier itinéraire, se présente comme suit :

{Controller}. Mvc/{action}/{ID}

L’ajout de l’extension. Mvc modifie le type de fichiers que le module de routage ASP.NET intercepte. Avec cette modification, l’application ASP.NET MVC achemine désormais les requêtes comme suit :

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

Le deuxième itinéraire, l’itinéraire racine, est nouveau. Ce modèle d’URL pour l’itinéraire racine est une chaîne vide. Cet itinéraire est nécessaire pour faire correspondre les demandes effectuées par rapport à la racine de votre application. Par exemple, l’itinéraire racine correspond à une demande similaire à celle-ci :

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Après avoir apporté ces modifications à votre table de routage, vous devez vous assurer que tous les liens de votre application sont compatibles avec ces nouveaux modèles d’URL. En d’autres termes, assurez-vous que tous vos liens incluent l’extension. Mvc. Si vous utilisez la méthode d’assistance HTML. ActionLink () pour générer vos liens, vous n’avez pas besoin d’apporter des modifications.

Au lieu d’utiliser le script registermvc. WCF, vous pouvez ajouter à la main une nouvelle extension à IIS qui est mappée à l’infrastructure ASP.NET. Lorsque vous ajoutez vous-même une nouvelle extension, assurez-vous que la case à cocher **vérifier que le fichier existe** n’est pas activée.

## <a name="hosted-server"></a>Serveur hébergé

Vous n’avez pas toujours accès à votre serveur Web. Par exemple, si vous hébergez votre application ASP.NET MVC à l’aide d’un fournisseur d’hébergement Internet, vous n’aurez pas nécessairement accès à IIS.

Dans ce cas, vous devez utiliser l’une des extensions de fichier existantes qui sont mappées à l’infrastructure ASP.NET. Les extensions. aspx,. axd et. ashx sont des exemples d’extensions de fichier mappées à ASP.NET.

Par exemple, le fichier global. asax modifié dans le Listing 3 utilise l’extension. aspx au lieu de l’extension. Mvc.

**Liste 3-global. asax (modifié avec les extensions. aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Le fichier global. asax de la liste 3 est exactement le même que le fichier global. asax précédent, à l’exception du fait qu’il utilise l’extension. aspx au lieu de l’extension. Mvc. Vous n’avez pas besoin d’effectuer d’installation sur votre serveur Web distant pour utiliser l’extension. aspx.

## <a name="creating-a-wildcard-script-map"></a>Création d’un mappage de scripts générique

Si vous ne souhaitez pas modifier les URL de votre application ASP.NET MVC et que vous avez accès à votre serveur Web, vous disposez d’une option supplémentaire. Vous pouvez créer un mappage de scripts générique qui mappe toutes les demandes au serveur Web à l’infrastructure ASP.NET. De cette façon, vous pouvez utiliser la table d’itinéraires ASP.NET MVC par défaut avec IIS 7,0 (en mode classique) ou IIS 6,0.

N’oubliez pas que cette option entraîne l’interception par IIS de chaque demande effectuée sur le serveur Web. Cela comprend les demandes d’images, les pages ASP classiques et les pages HTML. Par conséquent, l’activation d’un mappage de script générique sur ASP.NET a des répercussions sur les performances.

Voici comment activer un mappage de scripts générique pour IIS 7,0 :

1. Sélectionnez votre application dans la fenêtre connexions
2. Assurez-vous que l’affichage des **fonctionnalités** est sélectionné
3. Double-cliquez sur le bouton **mappages de gestionnaires**
4. Cliquez sur le lien **Ajouter un mappage de scripts générique** (voir figure 3).
5. Entrez le chemin d’accès au fichier Aspnet\_ISAPI. dll (vous pouvez copier ce chemin d’accès à partir du mappage de script que PageHandlerFactory)
6. Entrez le nom MVC
7. Cliquez sur le bouton **OK**

[![la boîte de dialogue Nouveau projet](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Figure 3**: création d’un mappage de scripts générique avec IIS 7,0 ([cliquez pour afficher l’image en taille réelle](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Procédez comme suit pour créer un mappage de scripts générique avec IIS 6,0 :

1. Cliquez avec le bouton droit sur un site Web et sélectionnez Propriétés.
2. Sélectionnez l’onglet **Répertoire de démarrage**
3. Cliquez sur le bouton **configuration**
4. Sélectionnez l’onglet **mappages**
5. Cliquez sur le bouton **Insérer** (voir figure 4).
6. Collez le chemin d’accès au fichier Aspnet\_ISAPI. dll dans le champ exécutable (vous pouvez copier ce chemin d’accès à partir du mappage de script pour les fichiers. aspx)
7. Désactivez la case à cocher **vérifier que le fichier existe**
8. Cliquez sur le bouton **OK**

[![la boîte de dialogue Nouveau projet](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Figure 4**: création d’un mappage de scripts générique avec IIS 6,0 ([cliquez pour afficher l’image en taille réelle](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Une fois que vous avez activé les mappages de scripts génériques, vous devez modifier la table d’itinéraires dans le fichier global. asax afin qu’elle comprenne un itinéraire racine. Dans le cas contraire, vous obtiendrez la page d’erreur dans la figure 5 lorsque vous effectuez une requête pour la page racine de votre application. Vous pouvez utiliser le fichier global. asax modifié dans le Listing 4.

[![la boîte de dialogue Nouveau projet](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Figure 5**: erreur d’itinéraire racine manquant ([cliquez pour afficher l’image en taille réelle](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Liste 4-global. asax (modifié avec l’itinéraire racine)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Une fois que vous avez activé un mappage de scripts générique pour IIS 7,0 ou IIS 6,0, vous pouvez effectuer des demandes qui fonctionnent avec la table de routage par défaut qui ressemble à ceci :

/

/Home/Index

/Product/Details/3

/Product

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était d’expliquer comment vous pouvez utiliser ASP.NET MVC lors de l’utilisation d’une version antérieure d’IIS (ou IIS 7,0 en mode classique). Nous avons abordé deux méthodes permettant d’obtenir le routage ASP.NET pour fonctionner avec des versions antérieures d’IIS : modifiez la table de routage par défaut ou créez un mappage de scripts générique.

La première option vous oblige à modifier les URL utilisées dans votre application ASP.NET MVC. L’un des avantages majeurs de cette première option est que vous n’avez pas besoin d’accéder à un serveur Web pour modifier la table de routage. Cela signifie que vous pouvez utiliser cette première option même lorsque vous hébergez votre application ASP.NET MVC dans une société d’hébergement Internet.

La deuxième option consiste à créer un mappage de scripts générique. L’avantage de cette deuxième option est que vous n’avez pas besoin de modifier vos URL. L’inconvénient de cette deuxième option est qu’elle peut avoir un impact sur les performances de votre application ASP.NET MVC.

> [!div class="step-by-step"]
> [Next](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
