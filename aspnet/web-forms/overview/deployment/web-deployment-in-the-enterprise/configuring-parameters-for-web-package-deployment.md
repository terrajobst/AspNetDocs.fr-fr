---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configuration des paramètres pour le déploiement de packages Web | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment définir des valeurs de paramètre, comme les noms d’application Web Internet Information Services (IIS), les chaînes de connexion et les points de terminaison de service,...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: f04ace98d81a33053b10cab7e40dbd75a6c0992c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544953"
---
# <a name="configuring-parameters-for-web-package-deployment"></a>Configuration des paramètres pour le déploiement de package web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment définir des valeurs de paramètre, comme les noms d’application Web Internet Information Services (IIS), les chaînes de connexion et les points de terminaison de service, lorsque vous déployez un package Web sur un serveur Web IIS distant.

Quand vous générez un projet d’application Web, le processus de génération et d’empaquetage génère trois fichiers de clé :

- Un fichier *[nom du projet]. zip* . Il s’agit du package de déploiement Web pour votre projet d’application Web. Ce package contient tous les assemblys, fichiers, scripts de base de données et ressources nécessaires à la recréation de votre application Web sur un serveur Web IIS distant.
- Un fichier *[nom du projet]. deploy. cmd* . Contient un ensemble de commandes Web Deploy paramétrées (MSDeploy. exe) qui publient votre package de déploiement Web sur un serveur Web IIS distant.
- Un *[nom du projet]. Fichier SetParameters. xml* . Cela fournit un ensemble de valeurs de paramètre à la commande MSDeploy. exe. Vous pouvez mettre à jour les valeurs de ce fichier et les transmettre à Web Deploy en tant que paramètre de ligne de commande lorsque vous déployez votre package Web.

> [!NOTE]
> Pour plus d’informations sur le processus de génération et d’empaquetage, consultez [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md).

Le fichier *SetParameters. xml* est généré dynamiquement à partir de votre fichier projet d’application Web et de tous les fichiers de configuration de votre projet. Lorsque vous générez et empaquetez votre projet, le pipeline de publication Web (WPP) détecte automatiquement un grand nombre de variables susceptibles d’être modifiées entre les environnements de déploiement, comme l’application Web IIS de destination et les chaînes de connexion à la base de données. Ces valeurs sont automatiquement paramétrées dans le package de déploiement Web et ajoutées au fichier *SetParameters. xml* . Par exemple, si vous ajoutez une chaîne de connexion au fichier *Web. config* dans votre projet d’application Web, le processus de génération détecte cette modification et ajoute une entrée au fichier *SetParameters. xml* en conséquence.

Dans de nombreux cas, ce paramétrage automatique sera suffisant. Toutefois, si vos utilisateurs doivent modifier d’autres paramètres entre des environnements de déploiement, tels que les paramètres d’application ou les URL de point de terminaison de service, vous devez indiquer au WPP de paramétrer ces valeurs dans le package de déploiement et d’ajouter les entrées correspondantes au fichier *SetParameters. xml* . Les sections qui suivent expliquent comment procéder.

### <a name="automatic-parameterization"></a>Paramétrage automatique

Lorsque vous générez et Empaquetez une application Web, le WPP va automatiquement paramétrer les éléments suivants :

- Le chemin d’accès et le nom de l’application Web IIS de destination.
- Toutes les chaînes de connexion dans votre fichier *Web. config* .
- Chaînes de connexion pour toutes les bases de données que vous ajoutez à l’onglet **Package/Publication SQL** dans les pages de propriétés du projet.

Par exemple, si vous deviez créer et empaqueter l’exemple de solution du [Gestionnaire de contacts](the-contact-manager-solution.md) sans toucher au processus de paramétrage, le WPP générera ce fichier *ContactManager. Mvc. SetParameters. xml* :

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]

Dans ce cas :

- Le paramètre nom de l' **application Web IIS** est le chemin d’accès IIS dans lequel vous souhaitez déployer l’application Web. La valeur par défaut est extraite de la page **Web Package/Publication** dans les pages de propriétés du projet.
- Le paramètre de **chaîne de connexion ApplicationServices-Web. config** a été généré à partir d’un élément **connectionStrings/Add** dans le fichier *Web. config* . Il représente la chaîne de connexion que l’application doit utiliser pour contacter la base de données d’appartenance. La valeur que vous fournissez ici sera remplacée par le fichier *Web. config* déployé. La valeur par défaut est extraite du fichier *Web. config* de prédéploiement.

Le WPP paramètre également ces propriétés dans le package de déploiement qu’il génère. Vous pouvez fournir des valeurs pour ces propriétés lors de l’installation du package de déploiement. Si vous installez le package manuellement via le gestionnaire des services Internet (IIS), comme décrit dans [installation manuelle de packages Web](manually-installing-web-packages.md), l’Assistant installation vous invite à fournir des valeurs pour tous les paramètres. Si vous installez le package à distance à l’aide du fichier *. deploy. cmd* , comme décrit dans [déploiement de Packages Web](deploying-web-packages.md), Web Deploy examine ce fichier *SetParameters. xml* pour fournir les valeurs de paramètre. Vous pouvez modifier les valeurs dans le fichier *SetParameters. xml* manuellement, ou vous pouvez personnaliser le fichier dans le cadre d’un processus de génération et de déploiement automatisés. Ce processus est décrit plus en détail plus loin dans cette rubrique.

### <a name="custom-parameterization"></a>Paramétrage personnalisé

Dans les scénarios de déploiement plus complexes, vous souhaiterez souvent paramétrer des propriétés supplémentaires avant de déployer votre projet. En règle générale, vous devez paramétrer les propriétés et les paramètres qui varient d’un environnement de destination à l’autre. Il peut s’agir des éléments suivants :

- Points de terminaison de service dans le fichier *Web. config* .
- Paramètres d’application dans le fichier *Web. config* .
- Toutes les autres propriétés déclaratives que vous souhaitez inviter les utilisateurs à spécifier.

Le moyen le plus simple de paramétrer ces propriétés consiste à ajouter un fichier *Parameters. xml* au dossier racine de votre projet d’application Web. Par exemple, dans la solution du gestionnaire de contacts, le projet ContactManager. Mvc comprend un fichier *Parameters. xml* dans le dossier racine.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Si vous ouvrez ce fichier, vous verrez qu’il contient une seule entrée de **paramètre** . L’entrée utilise une requête XPath (XML Path Language) pour rechercher et paramétrer l’URL de point de terminaison du service ContactService Windows Communication Foundation (WCF) dans le fichier *Web. config* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]

Outre le paramétrage de l’URL de point de terminaison dans le package de déploiement, le WPP ajoute également une entrée correspondante au fichier *SetParameters. xml* qui est généré en même temps que le package de déploiement.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]

Si vous installez manuellement le package de déploiement, le gestionnaire des services Internet vous invite à indiquer l’adresse du point de terminaison de service en même temps que les propriétés qui ont été paramétrées automatiquement. Si vous installez le package de déploiement en exécutant le fichier *. deploy. cmd* , vous pouvez modifier le fichier *SetParameters. xml* pour fournir une valeur pour l’adresse de point de terminaison de service ainsi que des valeurs pour les propriétés qui ont été paramétrées automatiquement.

Pour plus d’informations sur la création d’un fichier *Parameters. xml* , consultez [Comment : utiliser des paramètres pour configurer les paramètres de déploiement lors de l’installation d’un package](https://msdn.microsoft.com/library/ff398068.aspx). La procédure nommée **pour utiliser les paramètres de déploiement pour le fichier Web. config** fournit des instructions pas à pas.

## <a name="modifying-the-setparametersxml-file"></a>Modification du fichier SetParameters. Xml

Si vous envisagez de déployer le package d'&#x2014;application Web manuellement en exécutant le fichier *. deploy. cmd* ou en exécutant MSDeploy. exe à partir&#x2014;de la ligne de commande, rien ne vous empêche de modifier manuellement le fichier *SetParameters. xml* avant le déploiement. Toutefois, si vous travaillez sur une solution à l’échelle de l’entreprise, vous devrez peut-être déployer un package d’application Web dans le cadre d’un processus de génération et de déploiement plus étendu et automatisé. Dans ce scénario, vous avez besoin du Microsoft Build Engine (MSBuild) pour modifier le fichier *SetParameters. xml* pour vous. Pour ce faire, vous pouvez utiliser la tâche MSBuild **XmlPoke** .

L' [exemple de solution du gestionnaire de contacts](the-contact-manager-solution.md) illustre ce processus. Les exemples de code suivants ont été modifiés pour afficher uniquement les détails pertinents pour cet exemple.

> [!NOTE]
> Pour obtenir une vue d’ensemble plus générale du modèle de fichier projet dans l’exemple de solution et une introduction aux fichiers projet personnalisés en général, consultez [Présentation du fichier projet](understanding-the-project-file.md) et [Présentation du processus de génération](understanding-the-build-process.md).

Tout d’abord, les valeurs de paramètre intéressantes sont définies en tant que propriétés dans le fichier projet spécifique à l’environnement (par exemple, *env-dev. proj*).

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]

> [!NOTE]
> Pour obtenir des conseils sur la personnalisation des fichiers projet spécifiques à l’environnement pour vos propres environnements de serveur, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Ensuite, le fichier *Publish. proj* importe ces propriétés. Étant donné que chaque fichier *SetParameters. xml* est associé à un fichier *. deploy. cmd* et que nous souhaitons finalement que le fichier projet appelle chaque fichier *. deploy. cmd* , le fichier projet crée *un élément* MSBuild pour chaque fichier *. deploy. cmd* et définit les propriétés intéressantes comme *métadonnées d’élément*.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]

Dans ce cas :

- La valeur de métadonnées **ParametersXml** indique l’emplacement du fichier *SetParameters. xml* .
- La valeur **IisWebAppName** est le chemin d’accès IIS vers lequel vous souhaitez déployer l’application Web.
- La valeur **MembershipDBConnectionString** est la chaîne de connexion pour la base de données d’appartenance, et la valeur **MembershipDBConnectionName** est l’attribut **Name** du paramètre correspondant dans le fichier *SetParameters. xml* .
- La valeur **ServiceEndpointValue** est l’adresse du point de terminaison du service WCF sur le serveur de destination, et la valeur **ServiceEndpointParamName** est l’attribut Name du paramètre correspondant dans le fichier *SetParameters. xml* .

Enfin, dans le fichier *Publish. proj* , la cible **PublishWebPackages** utilise la tâche **XmlPoke** pour modifier ces valeurs dans le fichier *SetParameters. xml* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]

Vous remarquerez que chaque tâche **XmlPoke** spécifie quatre valeurs d’attribut :

- L’attribut **XmlInputPath** indique à la tâche où trouver le fichier que vous souhaitez modifier.
- L’attribut de **requête** est une requête XPath qui identifie le nœud XML que vous souhaitez modifier.
- L’attribut **value** est la nouvelle valeur que vous souhaitez insérer dans le nœud XML sélectionné.
- L’attribut **condition** est le critère sur lequel la tâche doit être exécutée ou non. Dans ce cas, la condition garantit que vous n’essayez pas d’insérer une valeur null ou vide dans le fichier *SetParameters. xml* .

## <a name="conclusion"></a>Conclusion

Cette rubrique a décrit le rôle du fichier *SetParameters. xml* et a expliqué comment elle est générée lorsque vous générez un projet d’application Web. Il a expliqué comment vous pouvez paramétrer des paramètres supplémentaires en ajoutant un fichier *Parameters. xml* à votre projet. Elle a également décrit comment vous pouvez modifier le fichier *SetParameters. xml* dans le cadre d’un processus de génération automatisé plus grand, à l’aide de la tâche **XmlPoke** dans vos fichiers projet.

La rubrique suivante, [déploiement de packages Web](deploying-web-packages.md), décrit comment vous pouvez déployer un package Web en exécutant le fichier *. deploy. cmd* ou en utilisant les commandes msdeploy. exe directement. Dans les deux cas, vous pouvez spécifier votre fichier *SetParameters. xml* en tant que paramètre de déploiement.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur la création de packages Web, consultez [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md). Pour obtenir des conseils sur le déploiement réel d’un package Web, consultez [déploiement de packages Web](deploying-web-packages.md). Pour obtenir une procédure pas à pas sur la création d’un fichier *Parameters. xml* , consultez [Comment : utiliser des paramètres pour configurer les paramètres de déploiement lors de l’installation d’un package](https://msdn.microsoft.com/library/ff398068.aspx).

Pour plus d’informations générales sur le paramétrage dans Web Deploy, consultez [Web Deploy le paramétrage en action](https://go.microsoft.com/?linkid=9805119) (billet de blog).

> [!div class="step-by-step"]
> [Précédent](building-and-packaging-web-application-projects.md)
> [Suivant](deploying-web-packages.md)
