---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Déploiement de bases de données d’appartenance dans des environnements d’entreprise | Microsoft Docs
author: jrjlee
description: Cette rubrique explique les principaux points à prendre en compte lors de la configuration des bases de données des services d’application ASP.NET (plus courantes...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 50f49af502b75aa5ad52756a76a5e7340aca53f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526004"
---
# <a name="deploying-membership-databases-to-enterprise-environments"></a>Déploiement de bases de données d’appartenance pour les environnements d’entreprise

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique les principaux points à prendre en compte lorsque vous approvisionnez des bases de données de services d’application ASP.NET (plus communément appelées bases de données d’appartenance) dans des environnements de test, intermédiaires ou de production. Il décrit également les approches que vous pouvez utiliser pour répondre à ces défis.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Quels sont les problèmes lorsque vous déployez une base de données d’appartenance ?

Dans la plupart des cas, lorsque vous concevez une stratégie de déploiement pour une base de données, la première chose que vous devez prendre en compte est celle que vous souhaitez déployer. Dans un environnement de développement ou de test, vous souhaiterez peut-être déployer des données de compte d’utilisateur pour accélérer et faciliter les tests. Dans un environnement intermédiaire ou de production, il est très peu probable que vous souhaitiez déployer des données de compte d’utilisateur.

Malheureusement, les bases de données d’appartenance ASP.NET présentent des défis spécifiques qui rendent cette décision beaucoup plus complexe :

- Un déploiement de schéma uniquement laisse la base de données d’appartenance dans un État non opérationnel. Cela est dû au fait que la base de données d’appartenance comprend des données de configuration (dans la table **aspnet\_SchemaVersions** ) dont la base de données a besoin pour fonctionner. Par conséquent, si vous effectuez un déploiement de schéma uniquement de votre base de données d’appartenance afin d’exclure les données de compte d’utilisateur, vous devez exécuter un script de publication pour ajouter les données de configuration essentielles.
- Selon la configuration de votre base de données d’appartenance, le fournisseur d’appartenances peut utiliser la clé de l’ordinateur pour chiffrer les mots de passe et les stocker dans la base de données. Dans ce cas, les données de compte d’utilisateur que vous déployez avec la base de données deviennent inutilisables sur le serveur de destination. Pour cette raison, le déploiement des données de compte d’utilisateur n’est pas un scénario pris en charge.

## <a name="choosing-a-membership-database-strategy"></a>Choix d’une stratégie de base de données d’appartenance

Suivez ces instructions lorsque vous choisissez le mode d’approvisionnement d’une base de données d’appartenance dans un environnement de serveur d’entreprise :

- Dans la mesure du possible, ne déployez pas les bases de données d’appartenance. Au lieu de cela, créez la base de données d’appartenance manuellement sur le serveur de base de données cible. Si vous n’avez pas personnalisé le schéma de votre base de données d’appartenance, vous pouvez simplement en créer un nouveau dans l’emplacement de destination à l’aide de l' [outil ASP.NET SQL Server Registration (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Si vous n’avez pas d’option mais que vous souhaitez&#x2014;déployer une base de données d’appartenance, par exemple, si vous&#x2014;avez apporté des modifications importantes au schéma de base de données, vous devez effectuer un déploiement de la base de données d’appartenance dans un schéma uniquement, pour exclure les données de compte d’utilisateur, puis exécuter un script de publication pour ajouter les données de configuration requises. Vous trouverez des instructions générales sur ces approches dans [Comment : déployer la base de données d’appartenance ASP.net sans inclure de comptes d’utilisateur](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Il est important de se souvenir que *le schéma de votre base de données d’appartenance est susceptible d’être relativement statique*. Même si vous avez personnalisé la base de données d’appartenance, il est peu probable que vous deviez mettre à jour le&#x2014;schéma régulièrement. il ne va pas changer avec la même fréquence que le code d’une application Web ou d’un projet de base de données. Par conséquent, il n’est pas nécessaire d’inclure la base de données d’appartenance dans des processus de déploiement automatisés ou en une seule étape.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Utilisation de VSDBCMD pour mettre à jour un schéma de base de données d’appartenance

Si vous modifiez la structure de votre base de données d’appartenance après votre premier déploiement, vous ne souhaiterez peut-être pas utiliser l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS) pour redéployer la base de données. La fonctionnalité de déploiement de base de données dans Web Deploy n’inclut pas la possibilité d’effectuer des mises à&#x2014;jour différentielles à une base de données de destination à la place, Web Deploy devez supprimer et recréer la base de données. Cela signifie que vous perdez les données de compte d’utilisateur existantes, ce qui n’est généralement pas souhaitable dans les environnements intermédiaires ou de production.

L’alternative consiste à utiliser l’utilitaire VSDBCMD pour mettre à jour le schéma de votre base de données de destination. VSDBCMD comprend deux fonctionnalités importantes. Tout d’abord, il peut importer le schéma d’une base de données existante dans un fichier. dbschema. Deuxièmement, il peut déployer un fichier. dbschema dans une base de données existante en tant que mise à jour différentielle, ce qui signifie qu’il effectue uniquement les modifications nécessaires pour mettre à jour la base de données cible et que vous ne perdez aucune donnée.

Vous pouvez utiliser ces étapes de haut niveau pour mettre à jour un schéma de base de données d’appartenance :

1. Utilisez l’action d' **importation** VSDBCMD pour générer un fichier. dbschema pour votre base de données d’appartenance source. Cette procédure est décrite dans [procédure : importer un schéma à partir d’une invite de commandes](https://msdn.microsoft.com/library/dd172135.aspx).
2. Utilisez l’action VSDBCMD **Deploy** pour déployer le fichier. dbschema dans votre base de données d’appartenance de destination. Cette procédure est décrite dans [référence de la ligne de commande pour VSDBCMD. EXE (déploiement et importation de schéma)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit certains des problèmes que vous pouvez rencontrer lorsque vous avez besoin d’approvisionner des bases de données d’appartenance ASP.NET dans différents environnements cibles. En particulier, il expliquait pourquoi les déploiements de schéma uniquement laissent la base de données d’appartenance dans un État non opérationnel et pourquoi le déploiement des données de compte d’utilisateur n’est pas pris en charge. Elle explique également comment approvisionner, déployer et mettre à jour des bases de données d’appartenance dans différents scénarios.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des instructions et des exemples d’utilisation de VSDBCMD, consultez [référence de ligne de commande pour VSDBCMD. EXE (déploiement et importation de schéma)](https://msdn.microsoft.com/library/dd193283.aspx) et [procédure : importer un schéma à partir d’une invite de commandes](https://msdn.microsoft.com/library/dd172135.aspx). Pour plus d’informations sur l’utilisation de ASPNET\_RegSql. exe pour créer des bases de données d’appartenance, consultez [ASP.NET SQL Server Registration Tool (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Pour obtenir des instructions plus générales sur le déploiement de bases de données d’appartenance, consultez [procédure : déployer la base de données d’appartenance ASP.net sans inclure de comptes d’utilisateur](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Précédent](deploying-database-role-memberships-to-test-environments.md)
> [Suivant](excluding-files-and-folders-from-deployment.md)
