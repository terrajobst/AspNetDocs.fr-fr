---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Génération et empaquetage des projets d’application Web | Microsoft Docs
author: jrjlee
description: Lorsque vous souhaitez déployer un projet d’application Web dans un environnement de serveur distant, votre première tâche consiste à générer le projet et à générer un pack de déploiement Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 1d0ee0264ce6461d7b0159f1a44de4de31e2d079
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573919"
---
# <a name="building-and-packaging-web-application-projects"></a>Génération et empaquetage des projets d’application web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Lorsque vous souhaitez déployer un projet d’application Web dans un environnement de serveur distant, votre première tâche consiste à générer le projet et à générer un package de déploiement Web. Cette rubrique décrit le fonctionnement du processus de génération pour les projets d’application Web. En particulier, il décrit les éléments suivants :
> 
> - Comment le pipeline de publication Web (WPP) étend le processus de génération pour inclure la fonctionnalité de déploiement.
> - Comment l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS) transforme votre application Web en package de déploiement.
> - Fonctionnement du processus de génération et d’empaquetage et des fichiers qui sont créés.

Dans Visual Studio 2010, le processus de génération et de déploiement pour les projets d’application Web est pris en charge par le WPP. Le WPP fournit un ensemble de cibles Microsoft Build Engine (MSBuild) qui étendent les fonctionnalités de MSBuild et lui permettent de s’intégrer à Web Deploy. Dans Visual Studio, vous pouvez voir cette fonctionnalité étendue sur les pages de propriétés de votre projet d’application Web. La page **Web Package/Publication** , ainsi que la page **Package/Publication SQL** , vous permettent de configurer la façon dont votre projet d’application Web est empaqueté pour le déploiement lorsque le processus de génération est terminé.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Comment le WPP fonctionne-t-il ?

Si vous examinez le fichier projet pour un C#projet d’application Web basé sur, vous pouvez voir qu’il importe deux fichiers. targets.

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]

La première instruction **Import** est commune à tous les C# projets visuels. Ce fichier, *Microsoft. CSharp. targets*, contient des cibles et des tâches spécifiques à C#Visual. Par exemple, la C# tâche de compilateur (**CSC**) est appelée ici. Le fichier *Microsoft. CSharp. targets* importe à son tour le fichier *Microsoft. Common. targets* . Cela définit les cibles qui sont communes à tous les projets, telles que la **génération**, la **régénération**, l' **exécution**, la **compilation**et le **nettoyage**. La deuxième instruction **Import** est spécifique aux projets d’application Web. Le fichier *Microsoft. WebApplication. targets* importe à son tour le fichier *Microsoft. Web. Publishing. targets* . Le fichier *Microsoft. Web. Publishing. targets* *est* essentiellement le WPP. Il définit des cibles, telles que **package** et **MSDeployPublish**, qui appellent Web Deploy pour effectuer diverses tâches de déploiement.

Pour comprendre comment ces cibles supplémentaires sont utilisées, dans l’exemple de solution du gestionnaire de contacts, ouvrez le fichier *Publish. proj* , puis examinez la cible **BuildProjects** .

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]

Cette cible utilise la tâche **MSBuild** pour générer différents projets. Notez les propriétés **DeployOnBuild** et **DeployTarget** :

- La propriété **DeployOnBuild = true** signifie essentiellement « je souhaite exécuter une cible supplémentaire lorsque la build se termine correctement ».
- La propriété **DeployTarget** identifie le nom de la cible que vous souhaitez exécuter lorsque la propriété **DeployOnBuild** est égale à **true**. Dans ce cas, vous spécifiez que vous souhaitez que MSBuild exécute la cible du **package** après la génération du projet.

La cible du **package** est définie dans le fichier *Microsoft. Web. Publishing. targets* . Fondamentalement, cette cible prend la sortie de génération de votre projet d’application Web et la convertit en package de déploiement Web qui peut être publié sur un serveur Web IIS.

> [!NOTE]
> Pour afficher un fichier projet (par exemple, <em>ContactManager. Mvc. csproj</em>) dans Visual Studio 2010, vous devez d’abord décharger le projet de votre solution. Dans la fenêtre <strong>Explorateur de solutions</strong> , cliquez avec le bouton droit sur le nœud du projet, puis cliquez sur <strong>décharger le projet</strong>. Cliquez à nouveau avec le bouton droit sur le nœud du projet, puis cliquez sur <strong>modifier</strong><em>[fichier projet]</em>). Le fichier projet s’ouvre dans son formulaire XML brut. N’oubliez pas de recharger le projet une fois que vous avez terminé.  
> Pour plus d’informations sur les cibles, les tâches et les instructions d' <strong>importation</strong> MSBuild, consultez [fonctionnement du fichier projet](understanding-the-project-file.md). Pour une présentation plus approfondie des fichiers projet et du WPP, consultez à l' [intérieur du Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build](http://amzn.com/0735645248) par Sayed Ibrahim Hashimi et William Bartholomew, ISBN : 978-0-7356-4524-0.

## <a name="what-is-a-web-deployment-package"></a>Qu’est-ce qu’un package de déploiement Web ?

Lorsque vous générez et déployez un projet d’application Web, à l’aide de Visual Studio 2010 ou directement à l’aide de MSBuild, le résultat final est généralement un *package de déploiement Web*. Le package de déploiement Web est un fichier. zip. Il contient tout ce dont les services IIS et Web Deploy ont besoin pour recréer votre application Web, notamment :

- La sortie compilée de votre application Web, y compris le contenu, les fichiers de ressources, les fichiers de configuration, les ressources JavaScript et les feuilles de style en cascade (CSS), et ainsi de suite.
- Assemblys pour votre projet d’application Web et pour tous les projets référencés dans votre solution.
- Scripts SQL pour générer les bases de données que vous déployez avec votre application Web.

Une fois que le package de déploiement Web a été généré, vous pouvez le publier sur un serveur Web IIS de différentes façons. Par exemple, vous pouvez le déployer à distance en ciblant le Web Deploy service de l’agent distant ou le gestionnaire de Web Deploy sur le serveur Web de destination, ou vous pouvez utiliser le gestionnaire des services Internet pour importer manuellement le package sur le serveur Web de destination. Pour plus d’informations sur ces approches de déploiement, consultez [choix de l’approche appropriée pour le déploiement Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Comment fonctionne le processus de génération ?

Cela montre ce qui se passe lorsque vous générez et Empaquetez un projet d’application Web :

![](building-and-packaging-web-application-projects/_static/image2.png)

Quand vous générez un projet d’application Web, le processus de génération génère un fichier nommé *[nom du projet]. SourceManifest. xml*. Avec le fichier projet et la sortie de la génération, this *. Le fichier SourceManifest. xml* indique Web Deploy ce qu’il doit inclure dans le package de déploiement Web. À l’aide de ces entrées, Web Deploy génère un package de déploiement Web nommé *[nom du projet]. zip*.

Outre le package de déploiement Web, le processus de génération génère deux fichiers qui peuvent vous aider à utiliser le package :

- Le fichier *. deploy. cmd* comprend un ensemble de commandes Web Deploy paramétrées (MSDeploy. exe) qui publient votre package de déploiement Web sur un serveur Web IIS distant. L’exécution du fichier *. deploy. cmd* , avec les paramètres appropriés, offre généralement une alternative plus rapide et plus facile à la construction manuelle des commandes msdeploy. exe.
- Le fichier *SetParameters. xml* fournit un ensemble de valeurs de paramètre à la commande MSDeploy. exe. Ces valeurs incluent des propriétés telles que le nom de l’application Web IIS vers laquelle vous souhaitez déployer le package, les valeurs des points de terminaison de service et des chaînes de connexion définis dans le fichier *Web. config* , ainsi que les valeurs de propriété de déploiement définies dans les pages de propriétés du projet.

Le fichier *SetParameters. xml* est essentiel pour gérer le processus de déploiement. Ce fichier est généré dynamiquement en fonction du contenu de votre projet d’application Web. Par exemple, si vous ajoutez une chaîne de connexion à votre fichier *Web. config* , le processus de génération détecte automatiquement la chaîne de connexion, paramétre le déploiement en conséquence et crée une entrée dans le fichier *SetParameters. xml* pour vous permettre de modifier la chaîne de connexion dans le cadre du processus de déploiement. La rubrique suivante, [Configuration des paramètres pour le déploiement de packages Web](configuring-parameters-for-web-package-deployment.md), explique le rôle de ce fichier plus en détail et décrit les différentes façons dont vous pouvez le modifier lors de la génération et du déploiement.

> [!NOTE]
> Dans Visual Studio 2010, le WPP ne prend pas en charge la précompilation des pages dans une application Web avant l’empaquetage. La prochaine version de Visual Studio et du WPP inclura la possibilité de précompiler une application Web en tant qu’option de Packaging.

## <a name="conclusion"></a>Conclusion

Cette rubrique a fourni une vue d’ensemble du processus de génération et d’empaquetage pour les projets d’application Web dans Visual Studio 2010. Il a décrit comment l’WPP vous permet d’appeler des commandes Web Deploy à partir de MSBuild, et il a expliqué le fonctionnement du processus de génération et d’empaquetage.

Une fois que vous avez créé un package de déploiement Web, l’étape suivante consiste à le déployer. Pour plus d’informations, consultez [Configuration des paramètres pour le déploiement de packages Web](configuring-parameters-for-web-package-deployment.md) et [déploiement de packages Web](deploying-web-packages.md).

## <a name="further-reading"></a>informations supplémentaires

Les rubriques suivantes de ce didacticiel, la [Configuration des paramètres pour le déploiement de packages Web](configuring-parameters-for-web-package-deployment.md) et le [déploiement de packages Web](deploying-web-packages.md), fournissent des conseils sur l’utilisation du package Web que vous avez créé. Le dernier didacticiel de cette série, le [déploiement Web d’entreprise avancé](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), fournit des conseils sur la façon de personnaliser et résoudre les problèmes liés au processus d’empaquetage.

Pour une présentation plus approfondie des fichiers projet et du WPP, consultez à l' [intérieur du Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build](http://amzn.com/0735645248) par Sayed Ibrahim Hashimi et William Bartholomew, ISBN : 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Précédent](understanding-the-build-process.md)
> [Suivant](configuring-parameters-for-web-package-deployment.md)
