---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Déploiement de projets de base de données | Microsoft Docs
author: jrjlee
description: 'Remarque : dans de nombreux scénarios de déploiement d’entreprise, vous avez besoin de publier des mises à jour incrémentielles sur une base de données déployée. L’alternative consiste à recréer...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 221808758492aedb8e8329364e511df28fd11105
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634266"
---
# <a name="deploying-database-projects"></a>Déploiement de projets de base de données

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> Dans de nombreux scénarios de déploiement d’entreprise, vous avez besoin de publier des mises à jour incrémentielles sur une base de données déployée. L’alternative consiste à recréer la base de données sur chaque déploiement, ce qui signifie que vous perdez toutes les données de la base de données existante. Lorsque vous travaillez avec Visual Studio 2010, l’utilisation de VSDBCMD est l’approche recommandée pour la publication de base de données incrémentielle. Toutefois, la prochaine version de Visual Studio et le pipeline de publication Web incluent des outils qui prennent en charge directement la publication incrémentielle.

Si vous ouvrez l’exemple de solution du gestionnaire de contacts dans Visual Studio 2010, vous verrez que le projet de base de données inclut un dossier Propriétés qui contient quatre fichiers.

![](deploying-database-projects/_static/image1.png)

Avec le fichier projet (*ContactManager. Database. dbproj* dans ce cas), ces fichiers contrôlent différents aspects du processus de génération et de déploiement :

- Le fichier *Database. sqlcmdvars* fournit des valeurs pour toutes les variables SQLCMD que vous utilisez lorsque vous déployez le projet. Chaque configuration de solution (par exemple, Debug et Release) peut spécifier un fichier. sqlcmdvars différent.
- Le fichier *Database. sqldeployment* fournit des paramètres spécifiques au déploiement, par exemple s’il faut utiliser le classement défini dans votre projet ou le classement du serveur de destination, si vous souhaitez recréer la base de données de destination à chaque fois ou modifier simplement la base de données existante pour la mettre à jour, et ainsi de suite. Chaque configuration de solution peut spécifier un autre fichier. sqldeployment.
- Le fichier *Database. sqlpermissions* est un document XML que vous pouvez utiliser pour définir les autorisations que vous souhaitez ajouter à la base de données cible. Toutes les configurations de solution partagent le même fichier. sqlpermissions.
- Le fichier *Database. sqlsettings* spécifie les propriétés au niveau de la base de données à utiliser lors de la création de la base de données, comme le classement à utiliser, le comportement des opérateurs de comparaison, etc. Toutes les configurations de solution partagent le même fichier. sqlsettings.

Il est judicieux de prendre un moment pour ouvrir ces fichiers dans Visual Studio et vous familiariser avec le contenu.

Quand vous générez un projet de base de données, le processus de génération crée deux fichiers :

- *Schéma de base de données* (fichier. dbschema). Cela décrit le schéma de la base de données que vous souhaitez créer au format XML.
- Un *manifeste de déploiement* (fichier. DeployManifest). Contient toutes les informations nécessaires à la création et au déploiement de votre base de données. Elle fait référence au fichier. dbschema avec d’autres ressources, telles que les instructions de déploiement (fichier. sqldeployment) et les scripts SQL de prédéploiement ou de redémarrage.

Cela montre la relation entre ces ressources :

![](deploying-database-projects/_static/image2.png)

Comme vous pouvez le voir, le fichier. sqlsettings et le fichier. sqlpermissions sont des entrées du processus de génération. Avec le fichier de projet de base de données, ces fichiers sont utilisés pour créer le fichier de schéma de base de données. Le fichier. sqldeployment et le fichier. sqlcmdvars passent par le processus de génération inchangé. Le manifeste de déploiement indique l’emplacement du schéma de base de données, le fichier. sqldeployment, le fichier. sqlcmdvars et tout script SQL de prédéploiement ou de redémarrage.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Pourquoi utiliser VSDBCMD pour déployer un projet de base de données ?

Il existe différentes approches pour déployer des projets de base de données. Toutefois, ils ne sont pas tous adaptés au déploiement d’un projet de base de données sur des serveurs distants dans un environnement d’entreprise. Réfléchissez à ce que vous souhaitez pour un déploiement de projet de base de données. Dans les scénarios de déploiement d’entreprise, vous avez probablement besoin des éléments suivants :

- Possibilité de déployer le projet de base de données à partir d’un emplacement distant.
- Possibilité d’effectuer des mises à jour incrémentielles sur une base de données existante.
- La possibilité d’inclure des scripts de prédéploiement ou des scripts de pré-déploiement.
- Capacité d’adapter le déploiement à plusieurs environnements de destination.
- Possibilité de déployer le projet de base de données dans le cadre d’un déploiement de solution en une seule étape, en général scripté.

Il existe trois approches principales que vous pouvez utiliser pour déployer un projet de base de données :

- Vous pouvez utiliser la fonctionnalité de déploiement avec le type de projet de base de données dans Visual Studio 2010. Quand vous générez et déployez un projet de base de données dans Visual Studio 2010, le processus de déploiement utilise le manifeste de déploiement pour générer un fichier de déploiement basé sur SQL spécifique à la configuration de Build. Cette opération crée la base de données si elle n’existe pas déjà ou apporte des modifications nécessaires à la base de données si elle existe déjà. Vous pouvez utiliser SQLCMD. exe pour exécuter ce fichier sur votre serveur de destination, ou vous pouvez configurer Visual Studio pour créer et exécuter le fichier. L’inconvénient de cette approche est que vous n’avez qu’un contrôle limité sur les paramètres de déploiement. Vous devrez peut-être également modifier le fichier de déploiement SQL pour fournir des valeurs de variables spécifiques à l’environnement. Vous pouvez utiliser cette approche uniquement à partir d’un ordinateur sur lequel Visual Studio 2010 est installé, et le développeur doit connaître et fournir des chaînes de connexion et des informations d’identification pour tous les environnements de destination.
- Vous pouvez utiliser l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS) pour [déployer une base de données dans le cadre d’un projet d’application Web](https://msdn.microsoft.com/library/dd465343.aspx). Toutefois, cette approche est beaucoup plus complexe si vous souhaitez déployer un projet de base de données au lieu de simplement répliquer une base de données locale existante sur un serveur de destination. Vous pouvez configurer Web Deploy pour exécuter le script de déploiement SQL généré par le projet de base de données, mais pour ce faire, vous devez créer un fichier personnalisé WPP targets pour votre projet d’application Web. Cela ajoute une quantité importante de complexité au processus de déploiement. En outre, Web Deploy ne prend pas directement en charge les mises à jour incrémentielles apportées aux bases de données existantes. Pour plus d’informations sur cette approche, consultez [extension du pipeline de publication Web au fichier de base de données de package projet de base de données SQL déployé](https://go.microsoft.com/?linkid=9805121).
- Vous pouvez utiliser l’utilitaire VSDBCMD pour déployer la base de données à l’aide du schéma de base de données ou du manifeste de déploiement. Vous pouvez appeler VSDBCMD. exe à partir d’une cible MSBuild, ce qui vous permet de publier des bases de données dans le cadre d’un processus de déploiement plus volumineux et à l’écriture de scripts. Vous pouvez remplacer les variables dans votre fichier. sqlcmdvars et de nombreuses autres propriétés de base de données à partir d’une commande VSDBCMD, ce qui vous permet de personnaliser votre déploiement pour différents environnements sans créer plusieurs configurations de Build. VSDBCMD fournit des fonctionnalités de différenciation, ce qui signifie qu’il effectue uniquement les modifications nécessaires pour aligner une base de données de destination avec votre schéma de base de données. VSDBCMD offre également un large éventail d’options de ligne de commande, qui vous donnent un contrôle affiné sur le processus de déploiement.

À partir de cette vue d’ensemble, vous pouvez voir que l’utilisation de VSDBCMD avec MSBuild est l’approche la plus adaptée à un scénario de déploiement d’entreprise classique :

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Prend en charge le déploiement à distance ? | Oui | Oui | Oui |
| Prend en charge les mises à jour incrémentielles ? | Oui | Non | Oui |
| Prend en charge les scripts antérieurs ou postérieurs au déploiement ? | Oui | Oui | Oui |
| Prend en charge le déploiement de plusieurs environnements ? | Limité | Limité | Oui |
| Prend en charge le déploiement par script ? | Limité | Oui | Oui |

Le reste de cette rubrique décrit l’utilisation de VSDBCMD avec MSBuild pour déployer des projets de base de données.

## <a name="understanding-the-deployment-process"></a>Fonctionnement du processus de déploiement

L’utilitaire VSDBCMD vous permet de déployer une base de données à l’aide du schéma de base de données (fichier. dbschema) ou du manifeste de déploiement (fichier. DeployManifest). Dans la pratique, vous utiliserez presque toujours le manifeste de déploiement, car le manifeste de déploiement vous permet de fournir des valeurs par défaut pour diverses propriétés de déploiement et d’identifier les scripts SQL de prédéploiement ou de redémarrage que vous souhaitez exécuter. Par exemple, cette commande VSDBCMD est utilisée pour déployer la base de données **ContactManager** sur un serveur de base de données dans un environnement de test :

[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]

Dans ce cas :

- Le commutateur **/a** (ou **/action**) spécifie ce que vous voulez que VSDBCMD fasse. Vous pouvez définir cette valeur pour l' **importation** ou le **déploiement**. L’option d' **importation** permet de générer un fichier. dbschema à partir d’une base de données existante, et l’option de **déploiement** est utilisée pour déployer un fichier. dbschema dans une base de données cible.
- Le commutateur **/Manifest** (ou **/MANIFESTFILE**) identifie le fichier. DeployManifest que vous souhaitez déployer. Si vous souhaitez utiliser le fichier. dbschema à la place, vous devez utiliser le commutateur **/Model** (ou **/ModelFile**).
- Le commutateur **/CS** (ou **/ConnectionString**) fournit la chaîne de connexion pour le serveur de base de données cible. Notez que cela n’inclut pas le nom de la&#x2014;base de données que l’VSDBCMD doit se connecter au serveur pour créer la base de données. Il n’a pas besoin de se connecter à une base de données individuelle. Si votre fichier. DeployManifest contient une chaîne de connexion, vous pouvez omettre ce commutateur. Si vous utilisez le commutateur quand même, la valeur du commutateur remplacera la valeur. DeployManifest.
- La propriété <strong>/p : TargetDatabase</strong> fournit le nom que vous souhaitez affecter à la base de données cible lors de la création. Cela remplace la valeur de la propriété <strong>TargetDatabase</strong> dans le fichier. DeployManifest. Vous pouvez utiliser la syntaxe <strong>/p :</strong> <em>[nom de la propriété]</em>pour définir un large éventail de propriétés de déploiement et pour remplacer toutes les variables SQLCMD déclarées dans votre fichier. sqlcmdvars.
- Le commutateur **/DD +** (ou **/DeployToDatabase +** ) indique que vous souhaitez créer un déploiement et le déployer dans l’environnement cible. Si vous spécifiez **/DD-** ou omettez le commutateur, VSDBCMD génère un script de déploiement, mais ne le déploie pas dans l’environnement cible. Ce commutateur est souvent la source de confusion et est expliqué plus en détail dans la section suivante.
- Le commutateur **/script** (ou **/DeploymentScriptFile**) spécifie l’emplacement où vous souhaitez générer le script de déploiement. Cette valeur n’affecte pas le processus de déploiement.

Pour plus d’informations sur VSDBCMD, consultez [référence de ligne de commande pour VSDBCMD. EXE (déploiement et importation de schéma)](https://msdn.microsoft.com/library/dd193283.aspx) et [procédure : préparer une base de données pour le déploiement à partir d’une invite de commandes à l’aide de VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Pour obtenir un exemple de la façon dont vous pouvez utiliser VSDBCMD à partir d’un fichier projet MSBuild, consultez [fonctionnement du processus de génération](understanding-the-build-process.md). Pour obtenir des exemples de configuration des paramètres de déploiement de base de données pour plusieurs environnements, consultez [Personnalisation des déploiements de bases de données pour plusieurs environnements](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Fonctionnement du commutateur DeployToDatabase

Le comportement du commutateur **/DD** ou **/DeployToDatabase** varie selon que vous utilisez VSDBCMD avec un fichier. dbschema ou. DeployManifest. Si vous utilisez un fichier. dbschema, le comportement est assez simple :

- Si vous spécifiez **/DD +** ou **/DD**, VSDBCMD génère un script de déploiement et déploie la base de données.
- Si vous spécifiez **/DD-** ou omettez le commutateur, VSDBCMD ne génère qu’un script de déploiement.

Si vous utilisez un fichier. DeployManifest, le comportement est beaucoup plus compliqué. Cela est dû au fait que le fichier. DeployManifest contient un nom de propriété **DeployToDatabase** qui détermine également si la base de données est déployée.

[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]

La valeur de cette propriété est définie en fonction des propriétés du projet de base de données. Si vous définissez l' **action déployer** pour **créer un script de déploiement (. Sql)** , la valeur est **false**. Si vous définissez l' **action déployer** pour **créer un script de déploiement (. Sql) et le déployer sur la base de données**, la valeur est **true**.

> [!NOTE]
> Ces paramètres sont associés à une plateforme et une configuration de build spécifiques. Par exemple, si vous configurez les paramètres de la configuration **Debug** , puis que vous publiez à l’aide de la configuration **Release** , vos paramètres ne seront pas utilisés.

![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> Dans ce scénario, l' **action de déploiement** doit toujours être définie pour **créer un script de déploiement (. Sql)** , car vous ne souhaitez pas que Visual Studio 2010 déploie votre base de données. En d’autres termes, la propriété **DeployToDatabase** doit toujours avoir la **valeur false**.

Quand une propriété **DeployToDatabase** est spécifiée, le commutateur **/DD** remplace uniquement la propriété si la valeur de la propriété est **false**:

- Si la **propriété DeployToDatabase** a la **valeur false**et que vous spécifiez **/DD +** ou **/DD**, VSDBCMD remplace la propriété **DeployToDatabase** et déploie la base de données.
- Si la propriété **DeployToDatabase** a la **valeur false**et que vous spécifiez **/DD-** ou omettez le commutateur, VSDBCMD ne déploie pas la base de données.
- Si la propriété **DeployToDatabase** a la **valeur true**, VSDBCMD ignore le commutateur et déploie la base de données.
- Un script de déploiement est généré dans chaque cas, que vous déployiez également la base de données ou non.

## <a name="conclusion"></a>Conclusion

Cette rubrique a fourni une vue d’ensemble du processus de génération et de déploiement pour les projets de base de données dans Visual Studio 2010. Elle a également décrit comment vous pouvez utiliser VSDBCMD. exe avec MSBuild pour prendre en charge le déploiement de base de données à l’échelle de l’entreprise.

Pour plus d’informations sur la façon dont cela fonctionne dans la pratique, consultez [Personnalisation des déploiements de bases de données pour plusieurs environnements](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur la personnalisation des déploiements de bases de données en créant un fichier de configuration de déploiement distinct pour chaque environnement, consultez [Personnalisation des déploiements de bases de données pour plusieurs environnements](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Pour obtenir des conseils sur la configuration des appartenances aux rôles de base de données en exécutant un script de publication, consultez [déploiement d’appartenances à des rôles de base de données dans des environnements de test](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Pour obtenir des conseils sur la gestion de certains des défis uniques imposés par les bases de données d’appartenance, consultez [déploiement de bases de données d’appartenance dans des environnements d’entreprise](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Ces rubriques sur MSDN fournissent des instructions et des informations générales générales sur les projets de base de données Visual Studio et le processus de déploiement de base de données :

- [Projets de base de données SQL Server Visual Studio 2010](https://msdn.microsoft.com/library/ff678491.aspx)
- [Gestion de la modification de la base de données](https://msdn.microsoft.com/library/aa833404.aspx)
- [Comment : préparer une base de données pour le déploiement à partir d’une invite de commandes à l’aide de VSDBCMD. EXÉCUTABLE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Vue d’ensemble de la Build and Deployment de base de données](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Précédent](deploying-web-packages.md)
> [Suivant](creating-and-running-a-deployment-command-file.md)
