---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement de fichiers supplémentaires | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: eaa3141c22980f0c816e2f33b5597ac9fe69c23c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548411"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement de fichiers supplémentaires

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Présentation

Ce didacticiel montre comment étendre le pipeline de publication Web de Visual Studio pour effectuer une tâche supplémentaire pendant le déploiement. La tâche consiste à copier les fichiers supplémentaires qui ne se trouvent pas dans le dossier du projet vers le site Web de destination.

Pour ce didacticiel, vous allez copier un fichier supplémentaire : *robots. txt*. Vous souhaitez déployer ce fichier dans un environnement intermédiaire mais pas dans un environnement de production. Dans [le didacticiel déploiement en production](deploying-to-production.md) , vous avez ajouté ce fichier au projet et configuré le profil de publication de production pour l’exclure. Dans ce didacticiel, vous verrez une méthode alternative pour gérer cette situation, qui sera utile pour tous les fichiers que vous souhaitez déployer mais que vous ne souhaitez pas inclure dans le projet.

## <a name="move-the-robotstxt-file"></a>Déplacer le fichier robots. txt

Pour préparer une méthode différente de gestion de *robots. txt*, dans cette section du didacticiel, vous déplacez le fichier vers un dossier qui n’est pas inclus dans le projet et vous supprimez *robots. txt* de l’environnement intermédiaire. Il est nécessaire de supprimer le fichier de la phase intermédiaire afin que vous puissiez vérifier que votre nouvelle méthode de déploiement du fichier dans cet environnement fonctionne correctement.

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le fichier *robots. txt* , puis cliquez sur **exclure du projet**.
2. À l’aide de l’Explorateur de fichiers Windows, créez un nouveau dossier dans le dossier de la solution et nommez-le *ExtraFiles*.
3. Déplacez le fichier *robots. txt* du dossier du projet *ContosoUniversity* vers le dossier *ExtraFiles* .

    ![Dossier ExtraFiles](deploying-extra-files/_static/image1.png)
4. À l’aide de votre outil FTP, supprimez le fichier *robots. txt* du site Web intermédiaire.

    Vous pouvez également sélectionner **Supprimer les fichiers supplémentaires à la destination** sous options de publication de **fichier** sous l’onglet **paramètres** du profil de publication intermédiaire, puis republier dans l’environnement intermédiaire.

## <a name="update-the-publish-profile-file"></a>Mettre à jour le fichier de profil de publication

Vous avez uniquement besoin de *robots. txt* dans l’environnement intermédiaire. par conséquent, le seul profil de publication que vous devez mettre à jour pour le déployer est intermédiaire.

1. Dans Visual Studio, ouvrez *Staging. pubxml*.
2. À la fin du fichier, avant la balise de fermeture `</Project>`, ajoutez le balisage suivant :

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Ce code crée une nouvelle *cible* qui collectera les fichiers supplémentaires à déployer. Une cible est composée d’une ou de plusieurs tâches que MSBuild exécutera en fonction des conditions que vous spécifiez.

    L’attribut `Include` spécifie que le dossier dans lequel rechercher les fichiers est *ExtraFiles*, situé au même niveau que le dossier du projet. MSBuild collecte tous les fichiers de ce dossier et de manière récursive à partir de tous les sous-dossiers (le double astérisque spécifie les sous-dossiers récursifs). Avec ce code, vous pouvez placer plusieurs fichiers, ainsi que des fichiers dans des sous-dossiers du dossier *ExtraFiles* , et tous seront déployés.

    L’élément `DestinationRelativePath` spécifie que les dossiers et les fichiers doivent être copiés dans le dossier racine du site Web de destination, dans la même structure de fichiers et de dossiers que celle figurant dans le dossier *ExtraFiles* . Si vous souhaitez copier le dossier *ExtraFiles* lui-même, la valeur `DestinationRelativePath` serait *ExtraFiles\%(RecursiveDir)% (fileName)% (extension)* .
3. À la fin du fichier, avant la balise de fermeture `</Project>`, ajoutez le balisage suivant qui spécifie quand exécuter la nouvelle cible.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Ce code provoque l’exécution de la nouvelle cible `CustomCollectFiles` chaque fois que la cible qui copie des fichiers dans le dossier de destination est exécutée. Il existe une cible distincte pour la création de packages de publication et de déploiement, et la nouvelle cible est injectée dans les deux cibles au cas où vous décidiez de la déployer à l’aide d’un package de déploiement au lieu de la publication.

    Le fichier *. pubxml* ressemble maintenant à l’exemple suivant :

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Enregistrez et fermez le fichier *Staging. pubxml* .

## <a name="publish-to-staging"></a>Publier dans un environnement intermédiaire

À l’aide de la publication en un clic ou de la ligne de commande, publiez l’application à l’aide du profil de mise en lots.

Si vous utilisez la publication en un clic, vous pouvez vérifier dans la fenêtre d' **Aperçu** que *robots. txt* sera copié. Dans le cas contraire, utilisez votre outil FTP pour vérifier que le fichier *robots. txt* se trouve dans le dossier racine du site Web après le déploiement.

## <a name="summary"></a>Récapitulatif

Cette série de didacticiels se termine lors du déploiement d’une application Web ASP.NET sur un fournisseur d’hébergement tiers. Pour plus d’informations sur les sujets abordés dans ces didacticiels, consultez le [plan de contenu de déploiement ASP.net](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Informations complémentaires

Si vous savez comment utiliser des fichiers MSBuild, vous pouvez automatiser de nombreuses autres tâches de déploiement en écrivant du code dans les fichiers *. pubxml* (pour les tâches spécifiques au profil) ou le fichier Project *. WPP. targets* (pour les tâches qui s’appliquent à tous les profils). Pour plus d’informations sur les fichiers *. pubxml* et *. WPP. targets* , consultez [procédure : modifier les paramètres de déploiement dans les fichiers de profil de publication (. pubxml) et le fichier. WPP. targets dans les projets Web Visual Studio](https://msdn.microsoft.com/library/ff398069). Pour une présentation de base du code MSBuild, consultez **l’anatomie d’un fichier projet dans une** [série de déploiement d’entreprise : présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Pour savoir comment utiliser des fichiers MSBuild pour effectuer des tâches dans vos propres scénarios, consultez cet ouvrage : à l' [intérieur du Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build](http://msbuildbook.com) par Sayed ibraham Hashimi et William Bartholomew.

## <a name="acknowledgements"></a>Remerciements

Je souhaite remercier les personnes suivantes qui ont apporté des contributions significatives au contenu de cette série de didacticiels :

- [Alberto Poblacion, MVP &amp; MCT, Espagne](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, MVP développement de plate-forme de données, États-Unis
- Mittal dur, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter : [@jongalloway](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike pape, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italie](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter : [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter : [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter : [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbie](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter : [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Précédent](command-line-deployment.md)
> [Suivant](troubleshooting.md)
