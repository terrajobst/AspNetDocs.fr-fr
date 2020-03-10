---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Personnalisation des déploiements de bases de données pour plusieurs environnements | Microsoft Docs
author: jrjlee
description: 'Cette rubrique explique comment adapter les propriétés d’une base de données à des environnements cibles spécifiques dans le cadre du processus de déploiement. Remarque : la rubrique part du principe que th...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 8ae8cb1a322afb95c5d2e8d5e73c7825c7b2fe5a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78604026"
---
# <a name="customizing-database-deployments-for-multiple-environments"></a>Personnalisation de déploiements de base de données pour plusieurs environnements

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment adapter les propriétés d’une base de données à des environnements cibles spécifiques dans le cadre du processus de déploiement.
> 
> > [!NOTE]
> > La rubrique part du principe que vous déployez un projet de base de données Visual Studio 2010 à l’aide de MSBuild. exe et VSDBCMD. exe. Pour plus d’informations sur la raison pour laquelle vous pouvez choisir cette approche, consultez [déploiement Web dans l’entreprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) et [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Lorsque vous déployez un projet de base de données vers plusieurs destinations, vous souhaiterez souvent personnaliser les propriétés de déploiement de la base de données pour chaque environnement cible. Par exemple, dans les environnements de test, vous devez généralement recréer la base de données sur chaque déploiement, alors que dans les environnements intermédiaires ou de production, vous seriez beaucoup plus susceptible de créer des mises à jour incrémentielles pour conserver vos données.
> 
> Dans un projet de base de données Visual Studio 2010, les paramètres de déploiement sont contenus dans un fichier de configuration de déploiement (. sqldeployment). Cette rubrique vous montre comment créer des fichiers de configuration de déploiement spécifiques à l’environnement et comment spécifier celle que vous souhaitez utiliser en tant que paramètre VSDBCMD.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Cette rubrique suppose que :

- Vous utilisez l’approche fractionner le fichier projet pour le déploiement de solutions, comme décrit dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Vous appelez VSDBCMD à partir du fichier projet pour déployer votre projet de base de données, comme décrit dans [fonctionnement du processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Pour créer un système de déploiement qui prend en charge la variation des propriétés de déploiement de base de données entre les environnements cibles, vous devez :

- Créez un fichier de configuration de déploiement (. sqldeployment) pour chaque environnement cible.
- Créez une commande VSDBCMD qui spécifie le fichier de configuration de déploiement en tant que commutateur de ligne de commande.
- Paramétrez la commande VSDBCMD dans un fichier projet Microsoft Build Engine (MSBuild), afin que les options VSDBCMD soient appropriées à l’environnement cible.

Cette rubrique vous indique comment effectuer chacune de ces procédures.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Création de fichiers de configuration de déploiement spécifiques à l’environnement

Par défaut, un projet de base de données contient un seul fichier *de configuration de déploiement nommé Database. sqldeployment*. Si vous ouvrez ce fichier dans Visual Studio 2010, vous pouvez voir les différentes options de déploiement disponibles :

- **Classement**de la comparaison du déploiement. Cela vous permet de choisir s’il faut utiliser le classement de base de données de votre projet (classement *source* ) ou le classement de base de données de votre serveur de destination (classement *cible* ). Dans la plupart des cas, vous souhaiterez utiliser le classement source lorsque vous déployez dans un environnement de développement ou de test. Lorsque vous déployez dans un environnement intermédiaire ou de production, vous souhaitez généralement ne pas modifier le classement cible afin d’éviter tout problème d’interopérabilité.
- **Déployer les propriétés de la base de données**. Cela vous permet de choisir s’il faut appliquer les propriétés de la base de données, comme défini dans le fichier *Database. sqlsettings* . Lorsque vous déployez une base de données pour la première fois, vous devez déployer les propriétés de la base de données. Si vous mettez à jour une base de données existante, les propriétés doivent déjà être en place et vous ne devriez pas avoir à les déployer à nouveau.
- **Toujours recréer la base de données**. Cela vous permet de choisir si vous souhaitez recréer la base de données cible chaque fois que vous déployez ou apportez des modifications incrémentielles pour mettre à jour la base de données cible avec votre schéma. Si vous recréez la base de données, vous perdrez toutes les données de la base de données existante. Par conséquent, vous devez généralement définir cette valeur sur **false** pour les déploiements dans des environnements intermédiaires ou de production.
- **Bloquer le déploiement incrémentiel si une perte de données peut se produire**. Cela vous permet de choisir si le déploiement doit s’arrêter si une modification du schéma de base de données entraîne une perte de données. Vous pouvez généralement définir cette **propriété sur true** pour un déploiement dans un environnement de production, afin de vous permettre d’intervenir et de protéger les données importantes. Si vous avez défini **toujours recréer la base de données** sur **false**, ce paramètre n’aura aucun effet.
- **Exécuter le déploiement en mode mono-utilisateur**. Ce n’est généralement pas un problème dans les environnements de développement ou de test. Toutefois, vous devez généralement définir cette **propriété sur true** pour les déploiements dans des environnements intermédiaires ou de production. Cela empêche les utilisateurs d’apporter des modifications à la base de données pendant que le déploiement est en cours.
- **Sauvegarder la base de données avant le déploiement**. Vous définissez généralement cette valeur sur **true** lorsque vous déployez dans un environnement de production, par souci de précaution contre la perte de données. Vous pouvez également définir cette **propriété sur true** lorsque vous déployez dans un environnement intermédiaire, si votre base de données de mise en lots contient beaucoup de données.
- **Générez des instructions DROP pour les objets qui se trouvent dans la base de données cible, mais qui ne sont pas dans le projet de base de données**. Dans la plupart des cas, il s’agit d’une partie intégrante et essentielle de l’apport de modifications incrémentielles à une base de données. Si vous avez défini **toujours recréer la base de données** sur **false**, ce paramètre n’aura aucun effet.
- **N’utilisez pas d’instructions ALTER assembly pour mettre à jour des types CLR**. Ce paramètre détermine comment SQL Server doit mettre à jour les types de common language runtime (CLR) vers des versions d’assembly plus récentes. Cette valeur doit être définie sur **false** dans la plupart des scénarios.

Ce tableau présente les paramètres de déploiement typiques pour différents environnements de destination. Toutefois, vos paramètres peuvent différer en fonction de vos besoins exacts.

|  | Développeur/test | Intermédiaire/intégration | Production |
| --- | --- | --- | --- |
| **Classement de comparaison du déploiement** | `Source` | Target | Target |
| **Déployer les propriétés de la base de données** | True | Première fois uniquement | Première fois uniquement |
| **Toujours recréer la base de données** | True | False | False |
| **Bloquer le déploiement incrémentiel si une perte de données peut se produire** | False | Peut-être | True |
| **Exécuter le script de déploiement en mode mono-utilisateur** | False | True | True |
| **Sauvegarder la base de données avant le déploiement** | False | Peut-être | True |
| **Générer des instructions DROP pour les objets qui se trouvent dans la base de données cible, mais qui ne sont pas dans le projet de base de données** | False | True | True |
| **Ne pas utiliser d’instructions ALTER ASSEMBLy pour mettre à jour des types CLR** | False | False | False |

> [!NOTE]
> Pour plus d’informations sur les propriétés de déploiement de base de données et les considérations relatives à l’environnement, consultez [vue d’ensemble des paramètres de projet de base de données](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [procédure : configurer des propriétés pour les détails du déploiement](https://msdn.microsoft.com/library/dd172125.aspx), [générer et déployer une base de données dans un environnement de développement isolé](https://msdn.microsoft.com/library/dd193409.aspx)et [créer et déployer des bases de données dans un environnement intermédiaire ou de production](https://msdn.microsoft.com/library/dd193413.aspx).

Pour prendre en charge le déploiement d’un projet de base de données vers plusieurs destinations, vous devez créer un fichier de configuration de déploiement pour chaque environnement cible.

**Pour créer un fichier de configuration spécifique à l’environnement**

1. Dans Visual Studio 2010, dans la fenêtre **Explorateur de solutions** , cliquez avec le bouton droit sur votre projet de base de données, puis cliquez sur **Propriétés**.
2. Sur la page Propriétés du projet de base de données, sous l’onglet **déployer** , dans la ligne **fichier de configuration de déploiement** , cliquez sur **nouveau**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. Dans la boîte de dialogue **nouveau fichier de configuration de déploiement** , attribuez un nom explicite au fichier (par exemple, **TestEnvironment. sqldeployment**), puis cliquez sur **Enregistrer**.
4. Sur la page *[filename] * * *. sqldeployment**, définissez les propriétés de déploiement en fonction des spécifications de votre environnement de destination, puis enregistrez le fichier.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Notez que le nouveau fichier est ajouté au dossier Propriétés dans votre projet de base de données.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Spécification du fichier de configuration de déploiement dans VSDBCMD

Lorsque vous utilisez des configurations de solution (comme Debug et Release) dans Visual Studio 2010, vous pouvez associer un fichier de configuration de déploiement à chaque configuration. Lorsque vous générez une configuration particulière, le processus de génération génère un fichier manifeste de déploiement spécifique à la configuration qui pointe vers le fichier de configuration de déploiement spécifique à la configuration. Toutefois, l’un des principaux objectifs de l’approche du déploiement décrit dans ces didacticiels est de permettre aux utilisateurs de contrôler le processus de déploiement sans utiliser Visual Studio 2010 et les configurations de solutions. Dans cette approche, la configuration de la solution est la même, quel que soit l’environnement de déploiement cible. Pour adapter le déploiement de votre base de données à un environnement de destination spécifique, vous pouvez utiliser les options de ligne de commande VSDBCMD pour spécifier votre fichier de configuration de déploiement.

Pour spécifier un fichier de configuration de déploiement dans VSDBCMD, utilisez le commutateur **p:/DeploymentConfigurationFile** et indiquez le chemin d’accès complet à votre fichier. Cela remplacera le fichier de configuration de déploiement identifié par le manifeste de déploiement. Par exemple, vous pouvez utiliser cette commande VSDBCMD pour déployer la base de données **ContactManager** dans un environnement de test :

[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]

> [!NOTE]
> Notez que le processus de génération peut renommer votre fichier. sqldeployment lorsqu’il copie le fichier dans le répertoire de sortie.

Si vous utilisez des variables de commande SQL dans vos scripts SQL de prédéploiement ou de pré-déploiement, vous pouvez utiliser une approche similaire pour associer un fichier. sqlcmdvars spécifique à l’environnement à votre déploiement. Dans ce cas, vous utilisez le commutateur **p:/SqlCommandVariablesFile** pour identifier votre fichier. sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Exécution de la commande VSDBCMD à partir d’un fichier projet MSBuild

Vous pouvez appeler une commande VSDBCMD à partir d’un fichier projet MSBuild à l’aide d’une tâche **Exec** dans une cible MSBuild. Dans sa forme la plus simple, elle ressemble à ceci :

[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]

- Dans la pratique, pour faciliter la lecture et la réutilisation de vos fichiers projet, vous souhaiterez créer des propriétés pour stocker les différents paramètres de ligne de commande. Cela permet aux utilisateurs de fournir plus facilement des valeurs de propriété dans un fichier projet spécifique à l’environnement ou de substituer des valeurs par défaut à partir de la ligne de commande MSBuild. Si vous utilisez l’approche fractionner le fichier projet décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), vous devez diviser les instructions de génération et les propriétés entre les deux fichiers en conséquence :
- Les paramètres spécifiques à l’environnement, comme le nom de fichier de configuration de déploiement, la chaîne de connexion de base de données et le nom de la base de données cible, doivent se trouver dans le fichier projet spécifique à l’environnement.
- La cible MSBuild qui exécute la commande VSDBCMD, ainsi que toutes les propriétés universelles telles que l’emplacement de l’exécutable VSDBCMD, doit se trouver dans le fichier de projet universel.

Vous devez également vous assurer que vous générez le projet de base de données avant d’appeler VSDBCMD afin que le fichier. DeployManifest soit créé et prêt à être utilisé. Vous pouvez voir un exemple complet de cette approche dans la rubrique [Présentation du processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md), qui vous guide dans les fichiers projet de l' [exemple de solution du gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique a décrit comment adapter les propriétés de la base de données à différents environnements de destination lorsque vous déployez des projets de base de données à l’aide de MSBuild et VSDBCMD. Cette approche est utile lorsque vous devez déployer des projets de base de données dans le cadre de solutions à l’échelle de l’entreprise plus grandes. Ces solutions sont souvent déployées dans plusieurs destinations, comme les environnements de développement ou de test sandbox, les plateformes intermédiaires ou d’intégration, ainsi que les environnements de production ou en direct. Chacun de ces environnements cibles requiert généralement un ensemble unique de propriétés de déploiement de base de données.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur le déploiement de projets de base de données à l’aide de VSDBCMD. exe, consultez [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pour plus d’informations sur l’utilisation de fichiers de projet MSBuild personnalisés pour contrôler le processus de déploiement, consultez [comprendre le fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Ces articles sur MSDN fournissent des instructions plus générales sur le déploiement de bases de données :

- [Vue d’ensemble des paramètres de projet de base de données](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Procédure : configurer des propriétés pour les détails du déploiement](https://msdn.microsoft.com/library/dd172125.aspx)
- [Créer et déployer des bases de données dans un environnement de développement isolé](https://msdn.microsoft.com/library/dd193409.aspx)
- [Créez et déployez des bases de données dans un environnement intermédiaire ou de production](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Précédent](performing-a-what-if-deployment.md)
> [Suivant](deploying-database-role-memberships-to-test-environments.md)
