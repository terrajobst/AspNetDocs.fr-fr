---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Mise hors connexion des applications Web avec Web Deploy | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment mettre une application Web hors connexion pendant la durée d’un déploiement automatisé à l’aide de la Dépl Web de l’Internet Information Services (IIS)...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: ba60664a0c3daa0650cd7e7cfc4ab9da08df3440
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526067"
---
# <a name="taking-web-applications-offline-with-web-deploy"></a>Passage d’applications web hors connexion avec Web Deploy

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment mettre une application Web hors connexion pendant la durée d’un déploiement automatisé à l’aide de l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy). Les utilisateurs qui accèdent à l’application Web sont redirigés vers une application *\_fichier. htm hors connexion* jusqu’à ce que le déploiement soit terminé.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Dans de nombreux scénarios, vous souhaiterez mettre une application Web hors connexion lorsque vous apportez des modifications à des composants associés, comme des bases de données ou des services Web. En général, dans IIS et ASP.NET, vous pouvez le faire en plaçant un fichier nommé *App\_offline. htm* dans le dossier racine du site Web ou de l’application Web IIS. L' *application\_fichier offline. htm* est un fichier HTML standard et contient généralement un message simple qui conseille à l’utilisateur que le site est temporairement indisponible en raison d’une opération de maintenance. Alors que l' *application\_fichier. htm hors connexion* se trouve dans le dossier racine du site Web, IIS redirige automatiquement toutes les demandes vers le fichier. Lorsque vous avez terminé d’effectuer des mises à jour, vous supprimez l' *application\_fichier. htm hors connexion* et le site Web reprend les demandes comme d’habitude.

Lorsque vous utilisez Web Deploy pour effectuer des déploiements automatisés ou en une seule étape vers un environnement cible, vous pouvez incorporer l’ajout et la suppression de l' *application\_fichier. htm hors connexion* dans votre processus de déploiement. Pour ce faire, vous devez effectuer ces tâches de haut niveau :

- Dans le fichier projet Microsoft Build Engine (MSBuild) que vous utilisez pour contrôler le processus de déploiement, créez une cible MSBuild qui copie une *application\_fichier. htm hors connexion* au serveur de destination avant le début des tâches de déploiement.
- Ajoutez une autre cible MSBuild qui supprime l' *application\_fichier. htm hors connexion* du serveur de destination lorsque toutes les tâches de déploiement sont terminées.
- Dans le projet d’application Web, créez un fichier *. WPP. targets* qui garantit qu’une *application\_fichier. htm hors connexion* est ajouté au package de déploiement lorsque Web Deploy est appelé.

Cette rubrique vous montre comment effectuer ces procédures. Les tâches et procédures pas à pas de cette rubrique partent du principe que vous avez déjà créé une solution qui contient au moins un projet d’application Web et que vous utilisez un fichier projet personnalisé pour contrôler le processus de déploiement comme décrit dans [déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Vous pouvez également utiliser l’exemple de solution du [Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) pour suivre les exemples de la rubrique.

## <a name="adding-an-app_offline-file-to-a-web-application-project"></a>Ajout d’une application\_fichier hors connexion à un projet d’application Web

La première tâche que vous devez effectuer consiste à ajouter une *application\_fichier hors connexion* à votre projet d’application Web :

- Pour empêcher le fichier d’interférer avec le processus de développement (vous ne voulez pas que votre application soit définitivement hors connexion), vous devez l’appeler autre chose que l' *application\_offline. htm*. Par exemple, vous pouvez nommer l’application de fichier *\_Offline-template. htm*.
- Pour empêcher le déploiement de ce fichier tel quel, vous devez définir l’action de génération sur **aucun**.

**Pour ajouter une application\_fichier hors connexion à un projet d’application Web**

1. Ouvrez votre solution dans Visual Studio 2010.
2. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur votre projet d’application Web, pointez sur **Ajouter**, puis cliquez sur **nouvel élément**.
3. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez **page HTML**.
4. Dans la zone **nom** , tapez **application\_Offline-template. htm**, puis cliquez sur **Ajouter**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Ajoutez du code HTML simple pour informer les utilisateurs que l’application n’est pas disponible, puis enregistrez le fichier. N’incluez pas de balises côté serveur (par exemple, toutes les balises qui sont précédées de « ASP : »). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur le nouveau fichier, puis cliquez sur **Propriétés**.
7. Dans la fenêtre **Propriétés** , dans la ligne **action de génération** , sélectionnez **aucune**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-app_offline-file"></a>Déploiement et suppression d’une application\_fichier hors connexion

L’étape suivante consiste à modifier votre logique de déploiement pour copier le fichier sur le serveur de destination au début du processus de déploiement et à le supprimer à la fin.

> [!NOTE]
> La procédure suivante suppose que vous utilisez un fichier projet MSBuild personnalisé pour contrôler votre processus de déploiement, comme décrit dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Si vous déployez directement à partir de Visual Studio, vous devez utiliser une approche différente. Sayed Ibrahim Hashimi décrit une approche de [la façon de mettre votre application Web hors connexion pendant la publication](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).

Pour déployer une *application\_fichier hors connexion* sur un site Web IIS de destination, vous devez appeler MSDeploy. exe à l’aide du [fournisseur Web Deploy **contentPath** ](https://technet.microsoft.com/library/dd569034(WS.10).aspx). Le fournisseur **contentPath** prend en charge les chemins d’accès de répertoire physique et les chemins d’accès d’application ou de site Web IIS, ce qui en fait le choix idéal pour synchroniser un fichier entre un dossier de projet Visual Studio et une application Web IIS. Pour déployer le fichier, votre commande MSDeploy doit ressembler à ceci :

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]

Pour supprimer le fichier du site de destination à la fin du processus de déploiement, votre commande MSDeploy doit ressembler à ceci :

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]

Pour automatiser ces commandes dans le cadre d’un processus de génération et de déploiement, vous devez les intégrer dans votre fichier projet MSBuild personnalisé. La procédure suivante décrit la procédure à suivre.

**Pour déployer et supprimer une application\_fichier hors connexion**

1. Dans Visual Studio 2010, ouvrez le fichier projet MSBuild qui contrôle votre processus de déploiement. Dans l’exemple de solution du [Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , il s’agit du fichier *Publish. proj* .
2. Dans l’élément de **projet** racine, créez un nouvel élément **PropertyGroup** pour stocker les variables de l’application\_le déploiement *hors connexion* :

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. La propriété **SourceRoot** est définie ailleurs dans le fichier *Publish. proj* . Il indique l’emplacement du dossier racine pour le contenu source par rapport au chemin d’accès&#x2014;actuel en d’autres termes, par rapport à l’emplacement du fichier *Publish. proj* .
4. Le fournisseur **contentPath** n’accepte pas les chemins d’accès relatifs aux fichiers. vous devez donc disposer d’un chemin d’accès absolu à votre fichier source pour pouvoir le déployer. Pour ce faire, vous pouvez utiliser la tâche [ConvertToAbsolutePath,](https://msdn.microsoft.com/library/bb882668.aspx) .
5. Ajoutez un nouvel élément **target** nommé **GetAppOfflineAbsolutePath**. Dans cette cible, utilisez la tâche **ConvertToAbsolutePath,** pour récupérer un chemin d’accès absolu à l' *application\_fichier de modèle hors connexion* dans votre dossier de projet.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Cette cible prend le chemin d’accès relatif à l' *application\_fichier de modèle hors connexion* dans votre dossier de projet et l’enregistre dans une nouvelle propriété en tant que chemin d’accès de fichier absolu. L’attribut **BeforeTargets** spécifie que vous souhaitez que cette cible s’exécute avant la cible **DeployAppOffline** , que vous allez créer à l’étape suivante.
7. Ajoutez une nouvelle cible nommée **DeployAppOffline**. Dans cette cible, appelez la commande MSDeploy. exe qui déploie votre *application\_fichier hors connexion* sur le serveur Web de destination.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. Dans cet exemple, la propriété **ContactManagerIisPath** est définie ailleurs dans le fichier projet. Il s’agit simplement d’un chemin d’application IIS, sous la forme *[nom du site Web IIS]/[nom de l’application]* . L’inclusion d’une condition dans la cible permet aux utilisateurs de basculer l' *application\_le déploiement hors connexion* en modifiant une valeur de propriété ou en fournissant un paramètre de ligne de commande.
9. Ajoutez une nouvelle cible nommée **DeleteAppOffline**. Dans cette cible, appelez la commande MSDeploy. exe qui supprime votre *application\_fichier hors connexion* du serveur Web de destination.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. La dernière tâche consiste à appeler ces nouvelles cibles aux points appropriés au cours de l’exécution de votre fichier projet. Vous pouvez effectuer cette opération de différentes façons. Par exemple, dans le fichier *Publish. proj* , la propriété **FullPublishDependsOn** spécifie une liste de cibles qui doivent être exécutées dans l’ordre où la cible par défaut **FullPublish** est appelée.
11. Modifiez votre fichier projet MSBuild pour appeler les cibles **DeployAppOffline** et **DeleteAppOffline** aux points appropriés dans le processus de publication.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Quand vous exécutez votre fichier projet MSBuild personnalisé, l' *application\_fichier hors connexion* est déployé sur le serveur immédiatement après une génération réussie. Elle est ensuite supprimée du serveur une fois que toutes les tâches de déploiement sont terminées.

## <a name="adding-an-app_offline-file-to-deployment-packages"></a>Ajout d’une application\_fichier hors connexion aux packages de déploiement

Selon la façon dont vous configurez votre déploiement, tout contenu existant dans l’application&#x2014;Web IIS de destination, comme l’application\_fichier &#x2014;. htm hors connexion, peut être supprimé automatiquement lorsque vous déployez un package Web vers la destination. Pour vous assurer que l' *application\_fichier. htm hors connexion* reste en place pendant la durée du déploiement, vous devez inclure le fichier dans le package de déploiement Web lui-même en plus de déployer le fichier directement au début du processus de déploiement.

- Si vous avez suivi les tâches précédentes de cette rubrique, vous avez ajouté l' *application\_fichier offline. htm* à votre projet d’application Web sous un nom de fichier différent (nous avons utilisé l' *application\_Offline-template. htm*) et vous avez défini l’action de génération sur **aucun**. Ces modifications sont nécessaires pour empêcher le fichier d’interférer avec le développement et le débogage. Par conséquent, vous devez personnaliser le processus d’empaquetage pour vous assurer que l' *application\_fichier. htm hors connexion* est inclus dans le package de déploiement Web.

Le pipeline de publication Web (WPP) utilise une liste d’éléments nommée **FilesForPackagingFromProject** pour créer une liste des fichiers qui doivent être inclus dans le package de déploiement Web. Vous pouvez personnaliser le contenu de vos packages Web en ajoutant vos propres éléments à cette liste. Pour ce faire, vous devez effectuer ces étapes de haut niveau :

1. Créez un fichier projet personnalisé nommé *[nom du projet]. WPP. targets* dans le même dossier que votre fichier projet.

    > [!NOTE]
    > Le fichier *. WPP. targets* doit se trouver dans le même dossier que votre fichier&#x2014;projet d’application Web, par exemple *ContactManager. Mvc. csproj*&#x2014;plutôt que dans le même dossier que les fichiers projet personnalisés que vous utilisez pour contrôler le processus de génération et de déploiement.
2. Dans le fichier *. WPP. targets* , créez une cible MSBuild qui s’exécute *avant* la cible **CopyAllFilesToSingleFolderForPackage** . Il s’agit de la cible WPP qui crée une liste des éléments à inclure dans le package.
3. Dans la nouvelle cible, créez un élément **ItemGroup** .
4. Dans l’élément **ItemGroup** , ajoutez un élément **FilesForPackagingFromProject** et spécifiez l' *application\_fichier offline. htm* .

Le fichier *. WPP. targets* doit ressembler à ceci :

[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]

Voici les points clés de la note dans cet exemple :

- L’attribut **BeforeTargets** insère cette cible dans le WPP en spécifiant qu’elle doit être exécutée immédiatement avant la cible **CopyAllFilesToSingleFolderForPackage** .
- L’élément **FilesForPackagingFromProject** utilise la valeur de métadonnées **DestinationRelativePath** pour renommer le fichier de l' *application\_offline-template. htm* en *app\_offline. htm* , car il est ajouté à la liste.

La procédure suivante vous montre comment ajouter ce fichier *. WPP. targets* à un projet d’application Web.

**Pour ajouter un fichier. WPP. targets à un package de déploiement Web**

1. Ouvrez votre solution dans Visual Studio 2010.
2. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur le nœud de votre projet d’application Web (par exemple, **ContactManager. Mvc**), pointez sur **Ajouter**, puis cliquez sur **nouvel élément**.
3. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le modèle de **fichier XML** .
4. Dans la zone **nom** , tapez *[nom du projet] * * *. WPP. targets** (par exemple, **ContactManager. Mvc. WPP. targets**), puis cliquez sur **Ajouter**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Si vous ajoutez un nouvel élément au nœud racine d’un projet, le fichier est créé dans le même dossier que le fichier projet. Vous pouvez le vérifier en ouvrant le dossier dans l’Explorateur Windows.
5. Dans le fichier, ajoutez le balisage MSBuild décrit précédemment.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Enregistrez et fermez le fichier *[nom du projet]. WPP. targets* .

La prochaine fois que vous générez et empaquetez votre projet d’application Web, le WPP détectera automatiquement le fichier *. WPP. targets* . L' *application\_fichier Offline-template. htm* sera inclus dans le package de déploiement Web résultant en tant qu' *application\_offline. htm*.

> [!NOTE]
> Si votre déploiement échoue, l’application *\_fichier. htm hors connexion* reste en place et votre application reste hors connexion. C’est généralement le comportement souhaité. Pour remettre votre application en ligne, vous pouvez supprimer l' *application\_fichier offline. htm* de votre serveur Web. Sinon, si vous corrigez des erreurs et que vous exécutez un déploiement réussi, l' *application\_fichier. htm hors connexion* est supprimé.

## <a name="conclusion"></a>Conclusion

Cette rubrique explique comment mettre une application Web hors connexion pendant la durée d’un déploiement, en publiant une application *\_fichier. htm hors connexion* sur le serveur de destination au début du processus de déploiement et en la supprimant à la fin. Elle a également expliqué comment inclure une *application\_fichier. htm hors connexion* dans un package de déploiement Web.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur le processus d’empaquetage et de déploiement, consultez [génération et empaquetage de projets d’application Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configuration des paramètres pour le déploiement de packages Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)et [déploiement de packages Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Si vous publiez vos applications Web directement à partir de Visual Studio, au lieu d’utiliser l’approche de fichier de projet MSBuild personnalisée décrite dans ces didacticiels, vous devez utiliser une approche légèrement différente pour mettre votre application hors connexion pendant la publication. traiter.

> [!div class="step-by-step"]
> [Précédent](excluding-files-and-folders-from-deployment.md)
> [Suivant](running-windows-powershell-scripts-from-msbuild-project-files.md)
