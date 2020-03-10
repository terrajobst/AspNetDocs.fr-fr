---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Exclusion de fichiers et de dossiers du déploiement | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment vous pouvez exclure des fichiers et des dossiers d’un package de déploiement Web quand vous générez et Empaquetez un projet d’application Web.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: a262ce43d7199fb1015d54d0b7c213857c360946
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544974"
---
# <a name="excluding-files-and-folders-from-deployment"></a>Exclusion de fichiers et de dossiers pour le déploiement

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment vous pouvez exclure des fichiers et des dossiers d’un package de déploiement Web quand vous générez et Empaquetez un projet d’application Web.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="overview"></a>Présentation

Quand vous générez un projet d’application Web dans Visual Studio 2010, le pipeline de publication Web (WPP) vous permet d’étendre ce processus de génération en empaquetant votre application Web compilée dans un package Web déployable. Vous pouvez ensuite utiliser l’outil de déploiement Web de Internet Information Services (IIS) (Web Deploy) pour déployer ce package Web sur un serveur Web IIS distant ou importer le package Web manuellement via le gestionnaire des services Internet. Ce processus d’empaquetage est expliqué dans [génération et empaquetage de projets d’application Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Comment contrôler ce qui est inclus dans votre package Web ? Les paramètres de projet dans Visual Studio, via le fichier projet sous-jacent, fournissent un contrôle suffisant pour un grand nombre de scénarios. Toutefois, dans certains cas, vous souhaiterez peut-être adapter le contenu de votre package Web à des environnements de destination spécifiques. Par exemple, vous souhaiterez peut-être inclure un dossier pour les fichiers journaux lorsque vous déployez votre application dans un environnement de test, mais excluez le dossier quand vous déployez l’application dans un environnement intermédiaire ou de production. Cette rubrique vous indique comment procéder.

## <a name="what-gets-included-by-default"></a>Qu’est-ce qui est inclus par défaut ?

Quand vous configurez vos propriétés de projet d’application Web dans Visual Studio, la liste **éléments à déployer** sur la page **Web Package/Publication** vous permet de spécifier ce que vous souhaitez inclure dans votre package de déploiement Web. Par défaut, ce paramètre est défini sur **uniquement les fichiers nécessaires à l’exécution de cette application**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Lorsque vous choisissez **uniquement les fichiers nécessaires à l’exécution de cette application**, l’WPP tente de déterminer les fichiers qui doivent être ajoutés au package Web. Cela concerne :

- Toutes les sorties de génération pour le projet.
- Tous les fichiers marqués avec une action de génération de **contenu**.

> [!NOTE]
> La logique qui détermine les fichiers à inclure est contenue dans ce fichier :   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft. Web. Publishing. OnlyFilesToRunTheApp. targets*

## <a name="excluding-specific-files-and-folders"></a>Exclusion de fichiers et dossiers spécifiques

Dans certains cas, vous aurez besoin d’un contrôle plus précis sur les fichiers et dossiers déployés. Si vous savez quels fichiers vous souhaitez exclure de l’avance et que l’exclusion s’applique à tous les environnements de destination, vous pouvez simplement définir l' **action de génération** de chaque fichier sur **aucun**.

**Pour exclure des fichiers spécifiques du déploiement**

1. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur le fichier, puis cliquez sur **Propriétés**.
2. Dans la fenêtre **Propriétés** , dans la ligne **action de génération** , sélectionnez **aucune**.

Toutefois, cette approche n’est pas toujours pratique. Par exemple, vous pouvez choisir de faire varier les fichiers et les dossiers qui sont inclus en fonction de votre environnement de destination et en dehors de Visual Studio. Par exemple, dans l’exemple de solution du gestionnaire de contacts, jetez un coup d’œil au contenu du projet ContactManager. Mvc :

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Le dossier interne contient certains scripts SQL que le développeur utilise pour créer, supprimer et remplir des bases de données locales à des fins de développement. Rien dans ce dossier ne doit être déployé dans un environnement intermédiaire ou de production.
- Le dossier scripts contient plusieurs fichiers JavaScript. Un grand nombre de ces fichiers sont uniquement inclus pour prendre en charge le débogage ou pour fournir IntelliSense dans Visual Studio. Certains de ces fichiers ne doivent pas être déployés dans des environnements intermédiaires ou de production. Toutefois, vous souhaiterez peut-être les déployer dans un environnement de test de développeur pour faciliter la résolution des problèmes.

Bien que vous puissiez manipuler vos fichiers projet pour exclure des fichiers et des dossiers spécifiques, il existe un moyen plus simple. Le WPP inclut un mécanisme permettant d’exclure des fichiers et des dossiers en générant des listes d’éléments nommées **ExcludeFromPackageFolders** et **ExcludeFromPackageFiles**. Vous pouvez étendre ce mécanisme en ajoutant vos propres éléments à ces listes. Pour ce faire, vous devez effectuer ces étapes de haut niveau :

1. Créez un fichier projet personnalisé nommé *[nom du projet]. WPP. targets* dans le même dossier que votre fichier projet.

    > [!NOTE]
    > Le fichier *. WPP. targets* doit se trouver dans le même dossier que votre fichier&#x2014;projet d’application Web, par exemple *ContactManager. Mvc. csproj*&#x2014;plutôt que dans le même dossier que les fichiers projet personnalisés que vous utilisez pour contrôler le processus de génération et de déploiement.
2. Dans le fichier *. WPP. targets* , ajoutez un élément **ItemGroup** .
3. Dans l’élément **ItemGroup** , ajoutez des éléments **ExcludeFromPackageFolders** et **ExcludeFromPackageFiles** pour exclure des fichiers et des dossiers spécifiques selon les besoins.

Il s’agit de la structure de base de ce fichier *. WPP. targets* :

[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]

Notez que chaque élément comprend un élément de métadonnées d’élément nommé **FromTarget**. Il s’agit d’une valeur facultative qui n’affecte pas le processus de génération. Il sert simplement à indiquer pourquoi des fichiers ou dossiers particuliers ont été omis si une personne révise les journaux de génération.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Exclusion de fichiers et de dossiers d’un package Web

La procédure suivante vous montre comment ajouter un fichier *. WPP. targets* à un projet d’application Web et comment utiliser le fichier pour exclure des fichiers et des dossiers spécifiques du package Web lorsque vous générez votre projet.

**Pour exclure des fichiers et des dossiers d’un package de déploiement Web**

1. Ouvrez votre solution dans Visual Studio 2010.
2. Dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur le nœud de votre projet d’application Web (par exemple, **ContactManager. Mvc**), pointez sur **Ajouter**, puis cliquez sur **nouvel élément**.
3. Dans la boîte de dialogue **Ajouter un nouvel élément** , sélectionnez le modèle de **fichier XML** .
4. Dans la zone **nom** , tapez *[nom du projet] * * *. WPP. targets** (par exemple, **ContactManager. Mvc. WPP. targets**), puis cliquez sur **Ajouter**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Si vous ajoutez un nouvel élément au nœud racine d’un projet, le fichier est créé dans le même dossier que le fichier projet. Vous pouvez le vérifier en ouvrant le dossier dans l’Explorateur Windows.
5. Dans le fichier, ajoutez un élément **Project** et un élément **ItemGroup** :

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Si vous souhaitez exclure des dossiers du package Web, ajoutez un élément **ExcludeFromPackageFolders** à l’élément **ItemGroup** :

   1. Dans l’attribut **include** , fournissez une liste séparée par des points-virgules des dossiers que vous souhaitez exclure.
   2. Dans l’élément de métadonnées **FromTarget** , fournissez une valeur significative pour indiquer la raison pour laquelle les dossiers sont exclus, comme le nom du fichier *. WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Si vous souhaitez exclure des fichiers du package Web, ajoutez un élément **ExcludeFromPackageFiles** à l’élément **ItemGroup** :

   1. Dans l’attribut **include** , fournissez une liste séparée par des points-virgules des fichiers que vous souhaitez exclure.
   2. Dans l’élément de métadonnées **FromTarget** , fournissez une valeur significative pour indiquer la raison pour laquelle les fichiers sont exclus, comme le nom du fichier *. WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. Le fichier *[nom du projet]. WPP. targets* doit maintenant ressembler à ceci :

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Enregistrez et fermez le fichier *[nom du projet]. WPP. targets* .

La prochaine fois que vous générez et empaquetez votre projet d’application Web, le WPP détectera automatiquement le fichier *. WPP. targets* . Tous les fichiers et dossiers que vous avez spécifiés ne seront pas inclus dans le package Web.

## <a name="conclusion"></a>Conclusion

Cette rubrique explique comment exclure des fichiers et des dossiers spécifiques quand vous générez un package Web, en créant un fichier *. WPP. targets* personnalisé dans le même dossier que votre fichier projet d’application Web.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur l’utilisation de fichiers de projet de Microsoft Build Engine personnalisé (MSBuild) pour contrôler le processus de déploiement, consultez [comprendre le fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Pour plus d’informations sur le processus d’empaquetage et de déploiement, consultez [génération et empaquetage de projets d’application Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configuration des paramètres pour le déploiement de packages Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)et [déploiement de packages Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Précédent](deploying-membership-databases-to-enterprise-environments.md)
> [Suivant](taking-web-applications-offline-with-web-deploy.md)
