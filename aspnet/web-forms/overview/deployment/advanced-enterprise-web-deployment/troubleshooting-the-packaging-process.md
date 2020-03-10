---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Résolution des problèmes liés au processus d’empaquetage | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment collecter des informations détaillées sur le processus d’empaquetage à l’aide de la propriété EnablePackageProcessLoggingAndAssert dans la section M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 8ad649dfff085a8774cc13c11d8a3e3d48277d66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628211"
---
# <a name="troubleshooting-the-packaging-process"></a>Résolution des problèmes du processus d’empaquetage

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment vous pouvez collecter des informations détaillées sur le processus d’empaquetage à l’aide de la propriété **EnablePackageProcessLoggingAndAssert** dans le Microsoft Build Engine (MSBuild).
> 
> Lorsque vous affectez la **valeur true**à la propriété **EnablePackageProcessLoggingAndAssert** , MSBuild :
> 
> - Ajoutez des informations supplémentaires sur le processus de création de package aux journaux de génération.
> - Consignez les erreurs dans certaines conditions, par exemple, si des fichiers dupliqués sont trouvés dans la liste des packages.
> - Créez un répertoire de journaux dans le dossier *ProjectName*\_package et utilisez-le pour enregistrer des informations sur les fichiers que vous empaquetez.
> 
> Si le processus d’empaquetage échoue, ou si vos packages de déploiement Web ne contiennent pas les fichiers attendus, vous pouvez utiliser ces informations pour résoudre les problèmes liés au processus et identifier les problèmes qui se posent.
> 
> > [!NOTE]
> > La propriété **EnablePackageProcessLoggingAndAssert** ne fonctionne que si vous générez votre projet à l’aide de la configuration **Debug** . La propriété est ignorée dans les autres configurations.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Fonctionnement de la propriété EnablePackageProcessLoggingAndAssert

[Génération et empaquetage des projets d’application Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) décrit comment le pipeline de publication Web (WPP) fournit un ensemble de cibles MSBuild qui étendent les fonctionnalités de MSBuild et lui permettent de s’intégrer à l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS). Lorsque vous empaquetez un projet d’application Web, vous appelez des cibles WPP.

Un grand nombre de ces objectifs de WPP incluent la logique conditionnelle qui journalise des informations supplémentaires lorsque la propriété **EnablePackageProcessLoggingAndAssert** est définie sur **true**. Par exemple, si vous examinez la cible du **package** , vous pouvez voir qu’elle crée un répertoire de journal supplémentaire et écrit une liste de fichiers dans un fichier texte si **EnablePackageProcessLoggingAndAssert** est égal à **true**.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]

> [!NOTE]
> Les cibles WPP sont définies dans le fichier *Microsoft. Web. Publishing. targets* dans le dossier% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web Vous pouvez ouvrir ce fichier et passer en revue les cibles dans Visual Studio 2010 ou dans tout éditeur XML. Veillez à ne pas modifier le contenu du fichier.

## <a name="enabling-the-additional-logging"></a>Activation de la journalisation supplémentaire

Vous pouvez fournir une valeur pour la propriété **EnablePackageProcessLoggingAndAssert** de différentes manières, en fonction de la façon dont vous générez votre projet.

Si vous générez votre projet à partir de la ligne de commande, vous pouvez fournir une valeur pour la propriété **EnablePackageProcessLoggingAndAssert** sous la forme d’un argument de ligne de commande :

[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]

Si vous utilisez un fichier projet personnalisé pour générer vos projets, vous pouvez inclure la valeur **EnablePackageProcessLoggingAndAssert** dans l’attribut **Properties** de la tâche **MSBuild** :

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]

Si vous utilisez une définition de build Team Foundation Server (TFS) pour générer vos projets, vous pouvez fournir une valeur pour la propriété **EnablePackageProcessLoggingAndAssert** dans la ligne d' **Arguments MSBuild** :![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Pour plus d’informations sur la création et la configuration des définitions de build, consultez [création d’une définition de build qui prend en charge le déploiement](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Si vous souhaitez inclure le package dans chaque Build, vous pouvez également modifier le fichier projet de votre projet d’application Web pour définir la propriété **EnablePackageProcessLoggingAndAssert** sur **true**. Vous devez ajouter la propriété au premier élément **PropertyGroup** dans votre fichier. csproj ou. vbproj.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]

## <a name="reviewing-the-log-files"></a>Examen des fichiers journaux

Quand vous générez et Empaquetez un projet d’application Web avec **EnablePackageProcessLoggingAndAssert** défini sur **true**, MSBuild crée un dossier supplémentaire nommé log dans le dossier *ProjectName*\_package. Le dossier log contient différents fichiers :

![](troubleshooting-the-packaging-process/_static/image2.png)

La liste des fichiers que vous voyez varie en fonction des éléments de votre projet et de votre processus de génération. Toutefois, ces fichiers sont généralement utilisés pour enregistrer la liste des fichiers que le WPP recueille pour l’empaquetage, à différentes étapes du processus :

- Le fichier *PreExcludePipelineCollectFilesPhaseFileList. txt* répertorie les fichiers que MSBuild collecte pour l’empaquetage avant la suppression de tous les fichiers spécifiés pour l’exclusion.
- Le fichier *AfterExcludeFilesFilesList. txt* contient la liste des fichiers modifiés après la suppression de tous les fichiers spécifiés pour l’exclusion.

    > [!NOTE]
    > Pour plus d’informations sur l’exclusion de fichiers et de dossiers du processus d’empaquetage, consultez [exclusion de fichiers et de dossiers du déploiement](excluding-files-and-folders-from-deployment.md).
- Le fichier *AfterTransformWebConfig. txt* répertorie les fichiers collectés pour l’empaquetage après l’exécution de toutes les transformations *Web. config* . Dans cette liste, tous les fichiers de transformation *Web. config* spécifiques à la configuration, tels que *Web. Debug. config* et *Web. Release. config*, sont exclus de la liste des fichiers à empaqueter. Un *fichier Web. config* transformé unique est inclus à leur place.
- Le fichier *PostAutoParameterizationWebConfigConnectionStrings. txt* contient la liste des fichiers une fois que les chaînes de connexion dans le fichier *Web. config* ont été paramétrables. C’est le processus qui vous permet de remplacer vos chaînes de connexion par les paramètres appropriés pour votre environnement cible lorsque vous déployez le package.
- Le fichier de *préemballage. txt* contient la liste de prébuilds finalisée des fichiers à inclure dans le package.

> [!NOTE]
> Les noms des fichiers journaux supplémentaires correspondent généralement aux cibles WPP. Vous pouvez examiner ces cibles en examinant le fichier *Microsoft. Web. Publishing. targets* dans le dossier% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web

Si le contenu de votre package Web n’est pas ce que vous attendiez, l’examen de ces fichiers peut être un moyen utile d’identifier à quel moment du processus les problèmes se sont produits.

## <a name="conclusion"></a>Conclusion

Cette rubrique a décrit comment vous pouvez utiliser la propriété **EnablePackageProcessLoggingAndAssert** dans MSBuild pour résoudre les problèmes liés au processus d’empaquetage. Il a expliqué les différentes façons dont vous pouvez fournir la valeur de la propriété au processus de génération, et il a décrit les informations supplémentaires qui sont enregistrées lorsque vous affectez à la propriété la valeur **true**.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur l’utilisation de fichiers de projet MSBuild personnalisés pour contrôler le processus de déploiement, consultez [comprendre le fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Pour plus d’informations sur le WPP et la façon dont il gère le processus d’empaquetage, consultez [génération et empaquetage de projets d’application Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Pour obtenir des conseils sur la façon d’exclure des fichiers et des dossiers spécifiques des packages de déploiement Web, consultez [exclusion de fichiers et de dossiers du déploiement](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Précédent](running-windows-powershell-scripts-from-msbuild-project-files.md)
