---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Exécution d’un déploiement de What If | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment effectuer des déploiements (ou simulés) à l’aide de l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS) et de...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 73a0e038cc0d4ebae0ffc8ed3fd2de4c9dad673c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628890"
---
# <a name="performing-a-what-if-deployment"></a>Exécution d’un déploiement « Scénario »

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment effectuer des déploiements « quels si » (ou simulés) à l’aide de l’outil de déploiement Web de Internet Information Services (IIS) (Web Deploy) et VSDBCMD. Cela vous permet de déterminer les effets de votre logique de déploiement sur un environnement cible particulier avant de déployer réellement votre application.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération et de déploiement est&#x2014;contrôlé par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Exécution d’un déploiement « What If » pour les packages Web

Web Deploy comprend des fonctionnalités qui vous permettent d’effectuer des déploiements en mode « quoi si » (ou d’essai). Lorsque vous déployez des artefacts en mode « quoi si », Web Deploy génère un fichier journal comme si vous aviez effectué le déploiement, mais il ne modifie en fait rien sur le serveur de destination. L’examen du fichier journal peut vous aider à comprendre l’impact que votre déploiement aura sur le serveur de destination, en particulier :

- Ce qui va être ajouté.
- Ce qui va être mis à jour.
- Ce qui va être supprimé.

Étant donné qu’un déploiement « What If » n’est pas réellement modifié sur le serveur de destination, ce qu’il ne peut pas toujours faire est de prédire si un déploiement s’effectue correctement.

Comme décrit dans [déploiement de packages Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), vous pouvez déployer des packages Web à l’aide de&#x2014;Web Deploy de deux manières en utilisant l’utilitaire de ligne de commande MSDeploy. exe directement ou en exécutant le fichier *. deploy. cmd* généré par le processus de génération.

Si vous utilisez MSDeploy. exe directement, vous pouvez exécuter un déploiement « What If » en ajoutant l’indicateur **– WhatIf** à votre commande. Par exemple, pour évaluer ce qui se passerait si vous avez déployé le package ContactManager. Mvc. zip dans un environnement intermédiaire, la commande MSDeploy devrait ressembler à ceci :

[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]

Lorsque vous êtes satisfait des résultats de votre déploiement « quoi si », vous pouvez supprimer l’indicateur **– WhatIf** pour exécuter un déploiement en direct.

> [!NOTE]
> Pour plus d’informations sur les options de ligne de commande pour MSDeploy. exe, consultez Paramètres de l' [opération Web Deploy](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Si vous utilisez le fichier *. deploy. cmd* , vous pouvez exécuter un déploiement « What If » en incluant l’indicateur **/t** (mode d’évaluation) au lieu de l’indicateur **/y** (« Yes » ou le mode de mise à jour) dans votre commande. Par exemple, pour évaluer ce qui se passe si vous avez déployé le package ContactManager. Mvc. zip en exécutant le fichier *. deploy. cmd* , votre commande doit ressembler à ceci :

[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]

Lorsque vous êtes satisfait des résultats de votre déploiement « mode d’évaluation », vous pouvez remplacer l’indicateur **/t** par un indicateur **/y** pour exécuter un déploiement en direct :

[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]

> [!NOTE]
> Pour plus d’informations sur les options de ligne de commande pour les fichiers *. deploy. cmd* , consultez [procédure : installer un package de déploiement à l’aide du fichier Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Si vous exécutez le fichier *. deploy. cmd* sans spécifier d’indicateurs, l’invite de commandes affiche la liste des indicateurs disponibles.

## <a name="performing-a-what-if-deployment-for-databases"></a>Exécution d’un déploiement « What If » pour les bases de données

Cette section part du principe que vous utilisez l’utilitaire VSDBCMD pour effectuer un déploiement de base de données incrémentiel basé sur un schéma. Cette approche est décrite plus en détail dans [déploiement des projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md). Nous vous recommandons de vous familiariser avec cette rubrique avant d’appliquer les concepts décrits ici.

Quand vous utilisez VSDBCMD en mode de **déploiement** , vous pouvez utiliser l’indicateur **/DD** (ou **/DEPLOYTODATABASE**) pour contrôler si VSDBCMD déploie réellement la base de données ou s’il vient de générer un script de déploiement. Si vous déployez un fichier. dbschema, le comportement est le suivant :

- Si vous spécifiez **/DD +** ou **/DD**, VSDBCMD génère un script de déploiement et déploie la base de données.
- Si vous spécifiez **/DD-** ou omettez le commutateur, VSDBCMD ne génère qu’un script de déploiement.

> [!NOTE]
> Si vous déployez un fichier. DeployManifest plutôt qu’un fichier. dbschema, le comportement du commutateur **/DD** est beaucoup plus compliqué. En fait, VSDBCMD ignore la valeur du commutateur **/DD** si le fichier. DeployManifest contient un élément **DeployToDatabase** avec la valeur **true**. Le [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md) décrit ce comportement dans son intégralité.

Par exemple, pour générer un script de déploiement pour la base de données **ContactManager** sans déployer réellement la base de données, votre commande VSDBCMD doit ressembler à ceci :

[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]

VSDBCMD est un outil de déploiement de base de données différentiel et, en tant que tel, le script de déploiement est généré dynamiquement pour contenir toutes les commandes SQL nécessaires à la mise à jour de la base de données actuelle, le cas échéant, vers le schéma spécifié. L’examen du script de déploiement est un moyen utile pour déterminer l’impact que votre déploiement aura sur la base de données actuelle et les données qu’elle contient. Par exemple, vous souhaiterez peut-être déterminer les éléments suivants :

- Si des tables existantes seront supprimées, et si cela entraînera une perte de données.
- Si l’ordre des opérations présente un risque de perte de données, par exemple si vous fractionnez ou fusionnez des tables.

Si vous êtes satisfait du script de déploiement, vous pouvez répéter l’VSDBCMD avec un indicateur **/DD +** pour effectuer les modifications. Vous pouvez également modifier le script de déploiement pour répondre à vos besoins, puis l’exécuter manuellement sur le serveur de base de données.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Intégration de la fonctionnalité « What If » dans les fichiers projet personnalisés

Dans les scénarios de déploiement plus complexes, vous pouvez utiliser un fichier projet Microsoft Build Engine (MSBuild) personnalisé pour encapsuler votre logique de génération et de déploiement, comme décrit dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Par exemple, dans l’exemple de solution du [Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , le fichier *Publish. proj* :

- Génère la solution.
- Utilise Web Deploy pour empaqueter et déployer l’application ContactManager. Mvc.
- Utilise Web Deploy pour empaqueter et déployer l’application ContactManager. service.
- Déploie la base de données **ContactManager** .

Lorsque vous intégrez le déploiement de plusieurs packages Web et/ou bases de données dans un processus en une seule étape, vous pouvez également choisir d’effectuer le déploiement dans son intégralité en mode « quoi si ».

Le fichier *Publish. proj* montre comment procéder. Tout d’abord, vous devez créer une propriété pour stocker la valeur « quoi si » :

[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]

Dans ce cas, vous avez créé une propriété nommée **WhatIf** avec la valeur par défaut **false**. Les utilisateurs peuvent remplacer cette valeur en affectant à la propriété la valeur **true** dans un paramètre de ligne de commande, comme vous le verrez bientôt.

L’étape suivante consiste à paramétrer les commandes Web Deploy et VSDBCMD afin que les indicateurs reflètent la valeur de la propriété **WhatIf** . Par exemple, la cible suivante (extraite du fichier *Publish. proj* et simplifiée) exécute le fichier *. deploy. cmd* pour déployer un package Web. Par défaut, la commande comprend un commutateur **/y** (« oui » ou « mode de mise à jour »). Si **WhatIf** a la valeur **true**, il est remplacé par un commutateur **/t** (version d’évaluation ou mode « What If »).

[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]

De même, la cible suivante utilise l’utilitaire VSDBCMD pour déployer une base de données. Par défaut, un commutateur **/DD** n’est pas inclus. Cela signifie que VSDBCMD génère un script de déploiement, mais ne déploie pas&#x2014;la base de données en d’autres termes, un scénario de simulation. Si la propriété **WhatIf** n’a pas la valeur **true**, un commutateur **/DD** est ajouté et VSDBCMD déploie la base de données.

[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]

Vous pouvez utiliser la même approche pour paramétrer toutes les commandes appropriées dans votre fichier projet. Lorsque vous souhaitez exécuter un déploiement « What If », vous pouvez simplement fournir une valeur de propriété **WhatIf** à partir de la ligne de commande :

[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]

De cette façon, vous pouvez exécuter un déploiement « What If » pour tous vos composants de projet en une seule étape.

## <a name="conclusion"></a>Conclusion

Cette rubrique explique comment exécuter des déploiements « What If » à l’aide de Web Deploy, VSDBCMD et MSBuild. Un déploiement « What If » vous permet d’évaluer l’impact d’un déploiement proposé avant d’apporter des modifications à l’environnement de destination.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur Web Deploy syntaxe de ligne de commande, consultez Paramètres de l' [opération Web Deploy](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Pour obtenir de l’aide sur les options de ligne de commande lorsque vous utilisez le fichier *. deploy. cmd* , consultez [procédure : installer un package de déploiement à l’aide du fichier Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Pour obtenir des conseils sur la syntaxe de la ligne de commande VSDBCMD, consultez [référence de la ligne de commande pour VSDBCMD. EXE (déploiement et importation de schéma)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Précédent](advanced-enterprise-web-deployment.md)
> [Suivant](customizing-database-deployments-for-multiple-environments.md)
