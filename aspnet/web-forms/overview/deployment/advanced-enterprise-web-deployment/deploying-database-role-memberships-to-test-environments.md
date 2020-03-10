---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Déploiement des appartenances de rôle de base de données dans des environnements de test | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment ajouter des comptes d’utilisateur à des rôles de base de données dans le cadre d’un déploiement de solution dans un environnement de test. Lorsque vous déployez une solution contenant...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a15f5bf5f659d151e91ef9e53c5ad55bcd8e2b01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587590"
---
# <a name="deploying-database-role-memberships-to-test-environments"></a>Déploiement d’appartenances aux rôles de base de données dans les environnements de test

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment ajouter des comptes d’utilisateur à des rôles de base de données dans le cadre d’un déploiement de solution dans un environnement de test.
> 
> Lorsque vous déployez une solution contenant un projet de base de données dans un environnement intermédiaire ou de production, vous ne souhaitez généralement pas que le développeur automatise l’ajout de comptes d’utilisateurs aux rôles de base de données. Dans la plupart des cas, le développeur ne connaît pas les comptes d’utilisateurs qui doivent être ajoutés aux rôles de base de données, et ces exigences peuvent changer à tout moment. Toutefois, lorsque vous déployez une solution contenant un projet de base de données dans un environnement de développement ou de test, la situation est généralement plutôt différente :
> 
> - En général, le développeur redéploie régulièrement la solution, souvent plusieurs fois par jour.
> - La base de données est généralement recréée à chaque déploiement, ce qui signifie que les utilisateurs de base de données doivent être créés et ajoutés aux rôles après chaque déploiement.
> - Le développeur a généralement un contrôle total sur l’environnement de développement ou de test cible.
> 
> Dans ce scénario, il est souvent préférable de créer automatiquement des utilisateurs de base de données et d’attribuer des appartenances de rôle de base de données dans le cadre du processus de déploiement.
> 
> Le facteur clé est que cette opération doit être conditionnelle en fonction de l’environnement cible. Si vous effectuez un déploiement dans un environnement intermédiaire ou de production, vous souhaitez ignorer l’opération. Si vous effectuez un déploiement dans un environnement de développement ou de test, vous souhaitez déployer des appartenances à des rôles sans intervention supplémentaire. Cette rubrique décrit une approche que vous pouvez utiliser pour résoudre ce problème.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Cette rubrique suppose que :

- Vous utilisez l’approche fractionner le fichier projet pour le déploiement de solutions, comme décrit dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Vous appelez VSDBCMD à partir du fichier projet pour déployer votre projet de base de données, comme décrit dans [fonctionnement du processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Pour créer des utilisateurs de base de données et assigner des appartenances aux rôles lorsque vous déployez un projet de base de données dans un environnement de test, vous devez :

- Créez un script Transact langage SQL (Transact-SQL) qui apporte les modifications nécessaires à la base de données.
- Créez une cible Microsoft Build Engine (MSBuild) qui utilise l’utilitaire sqlcmd. exe pour exécuter le script SQL.
- Configurez vos fichiers projet pour appeler la cible lorsque vous déployez votre solution dans un environnement de test.

Cette rubrique vous indique comment effectuer chacune de ces procédures.

## <a name="scripting-the-database-role-memberships"></a>Script des appartenances au rôle de base de données

Vous pouvez créer un script Transact-SQL de différentes façons et à n’importe quel emplacement de votre choix. L’approche la plus simple consiste à créer le script dans votre solution dans Visual Studio 2010.

**Pour créer un script SQL**

1. Dans la fenêtre **Explorateur de solutions** , développez le nœud de votre projet de base de données.
2. Cliquez avec le bouton droit sur le dossier **scripts** , pointez sur **Ajouter**, puis cliquez sur **nouveau dossier**.
3. Tapez **test** comme nom de dossier, puis appuyez sur entrée.
4. Cliquez avec le bouton droit sur le dossier **test** , pointez sur **Ajouter**, puis cliquez sur **script**.
5. Dans la boîte de dialogue **Ajouter un nouvel élément** , donnez un nom explicite à votre script (par exemple, **AddRoleMemberships. SQL**), puis cliquez sur **Ajouter**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. Dans le fichier *AddRoleMemberships. SQL* , ajoutez les instructions Transact-SQL suivantes :

    1. Créez un utilisateur de base de données pour la connexion SQL Server qui accédera à votre base de données.
    2. Ajoutez l’utilisateur de base de données à tous les rôles de base de données requis.
7. Le fichier doit ressembler à ceci :

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Enregistrez le fichier.

## <a name="executing-the-script-on-the-target-database"></a>Exécution du script sur la base de données cible

Dans l’idéal, vous devez exécuter tous les scripts Transact-SQL nécessaires dans le cadre d’un script de publication après le déploiement lorsque vous déployez votre projet de base de données. Toutefois, les scripts de publication ne vous permettent pas d’exécuter une logique de manière conditionnelle en fonction des configurations de solution ou des propriétés de génération. L’alternative consiste à exécuter vos scripts SQL directement à partir du fichier projet MSBuild, en créant un élément **target** qui exécute une commande sqlcmd. exe. Vous pouvez utiliser cette commande pour exécuter votre script sur la base de données cible :

[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]

> [!NOTE]
> Pour plus d’informations sur les options de ligne de commande sqlcmd, consultez l' [utilitaire sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

Avant d’incorporer cette commande dans une cible MSBuild, vous devez prendre en compte les conditions dans lesquelles vous souhaitez que le script s’exécute :

- La base de données cible doit exister avant que vous ne modifiiez ses appartenances aux rôles. Par conséquent, vous devez exécuter ce script *après* le déploiement de la base de données.
- Vous devez inclure une condition afin que le script soit exécuté uniquement pour les environnements de test.
- Si vous exécutez un déploiement&#x2014;« What If » en d’autres termes, si vous générez des scripts de déploiement sans réellement les&#x2014;exécuter, vous ne devez pas exécuter le script SQL.

Si vous utilisez l’approche fractionner le fichier projet décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), comme le montre l’exemple de solution du gestionnaire de contacts, vous pouvez fractionner les instructions de génération pour votre script SQL comme suit :

- Toutes les propriétés spécifiques à l’environnement requises, ainsi que la propriété qui détermine s’il faut déployer des autorisations, doivent être placées dans le fichier projet spécifique à l’environnement (par exemple, *env-dev. proj*).
- La cible MSBuild elle-même, ainsi que toutes les propriétés qui ne changent pas entre les environnements de destination, doivent être placées dans le fichier de projet universel (par exemple, *Publish. proj*).

Dans le fichier projet spécifique à l’environnement, vous devez définir le nom du serveur de base de données, le nom de la base de données cible et une propriété booléenne qui permet à l’utilisateur de spécifier s’il faut déployer les appartenances aux rôles.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]

Dans le fichier projet universel, vous devez fournir l’emplacement de l’exécutable sqlcmd et l’emplacement du script SQL que vous souhaitez exécuter. Ces propriétés restent les mêmes, quel que soit l’environnement de destination. Vous devez également créer une cible MSBuild pour exécuter la commande sqlcmd.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]

Notez que vous ajoutez l’emplacement de l’exécutable sqlcmd en tant que propriété statique, car cela peut s’avérer utile pour d’autres cibles. En revanche, vous définissez l’emplacement de votre script SQL et la syntaxe de la commande sqlcmd en tant que propriétés dynamiques dans la cible, car elles ne sont pas nécessaires avant l’exécution de la cible. Dans ce cas, la cible **DeployTestDBPermissions** est exécutée uniquement si les conditions suivantes sont remplies :

- La propriété **DeployTestDBRoleMemberships** a la valeur **true**.
- L’utilisateur n’a pas spécifié un indicateur **WhatIf = true** .

Enfin, n’oubliez pas d’appeler la cible. Dans le fichier *Publish. proj* , vous pouvez le faire en ajoutant la cible à la liste de dépendances pour la cible **FullPublish** par défaut. Vous devez vous assurer que la cible **DeployTestDBPermissions** n’est pas exécutée tant que la cible **PublishDbPackages** n’a pas été exécutée.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit une méthode permettant d’ajouter des utilisateurs de base de données et des appartenances aux rôles en tant qu’action de suite au déploiement lorsque vous déployez un projet de base de données. Cela est généralement utile lorsque vous recréez régulièrement une base de données dans un environnement de test, mais cela est généralement évité quand vous déployez des bases de données dans des environnements intermédiaires ou de production. Par conséquent, vous devez vous assurer que vous utilisez la logique conditionnelle nécessaire pour que les utilisateurs de base de données et les appartenances aux rôles soient créés uniquement quand cela est approprié.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur l’utilisation de VSDBCMD pour déployer des projets de base de données, consultez [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md). Pour obtenir des conseils sur la personnalisation des déploiements de bases de données pour différents environnements cibles, consultez [Personnalisation des déploiements de bases de données pour plusieurs environnements](customizing-database-deployments-for-multiple-environments.md). Pour plus d’informations sur l’utilisation de fichiers de projet MSBuild personnalisés pour contrôler le processus de déploiement, consultez [comprendre le fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Pour plus d’informations sur les options de ligne de commande sqlcmd, consultez l' [utilitaire sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Précédent](customizing-database-deployments-for-multiple-environments.md)
> [Suivant](deploying-membership-databases-to-enterprise-environments.md)
