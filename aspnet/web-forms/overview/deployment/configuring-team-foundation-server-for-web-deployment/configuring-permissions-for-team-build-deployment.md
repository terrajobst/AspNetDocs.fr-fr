---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configuration des autorisations pour le déploiement Team Build | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment configurer des autorisations pour permettre à votre serveur de builds de déployer du contenu sur des serveurs Web et des serveurs de base de données dans le cadre d’une configuration b automatisée...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5699f72af6b8d7f18d1a2c631dfdedd63c66e1e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638424"
---
# <a name="configuring-permissions-for-team-build-deployment"></a>Configuration des autorisations pour le déploiement de Team Build

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment configurer des autorisations pour permettre à votre serveur de builds de déployer du contenu sur des serveurs Web et des serveurs de base de données dans le cadre d’un processus de génération automatisé.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Quand vous installez le service de build Team Foundation Server (TFS) 2010, vous spécifiez l’identité avec laquelle vous souhaitez que le service s’exécute. Par défaut, il s’agit du compte service réseau. Vous pouvez également configurer le service de build pour qu’il s’exécute à l’aide d’un compte de domaine.

Toutes les tâches de déploiement qui requièrent l’authentification Windows et que vous envisagez d’automatiser à l’aide de Team Build, s’exécutent à l’aide de l’identité du service de Build. Par conséquent, vous devez accorder à l’identité du service de build toutes les autorisations requises sur vos serveurs Web et vos serveurs de base de données.

> [!NOTE]
> Le compte de service réseau utilise le compte d’ordinateur pour s’authentifier auprès d’autres ordinateurs. Les comptes d’ordinateur prennent la forme * [nom de domaine]\[nom de l’ordinateur] * **$** &#x2014;par exemple, **FABRIKAM\TFSBUILD $** . Par conséquent, si votre service de build s’exécute à l’aide de l’identité du service réseau, vous devez accorder toutes les autorisations requises à l’identité du compte d’ordinateur de votre serveur de builds.

## <a name="configuring-web-server-permissions"></a>Configuration des autorisations de serveur Web

Comme décrit dans [la rubrique choix de l’approche appropriée pour le déploiement Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), il existe deux approches principales que vous pouvez utiliser si vous souhaitez déployer des packages Web sur un serveur Web distant :

- Déployez l’application à partir d’un emplacement distant en ciblant le *service de Deployment agent Web* (également appelé agent distant) sur le serveur de destination.
- Déployez l’application à partir d’un emplacement distant en ciblant le gestionnaire de Web Deploy *Internet Information Services* (*IIS)* sur le serveur de destination.

L’agent distant a deux limitations principales dans ce cas :

- L’agent distant ne prend en charge que l’authentification NTLM. En d’autres termes, le déploiement doit utiliser l’identité&#x2014;du service de Build. vous ne pouvez pas emprunter l’identité d’un autre compte.
- Pour utiliser l’agent distant, le compte qui effectue le déploiement doit être un administrateur sur le serveur cible.

Ensemble, ces deux limitations rendent l’approche de l’agent distant indésirable pour le déploiement d’une build d’équipe automatisée. Pour utiliser cette approche, vous devez faire du compte de service de build un administrateur sur tous les serveurs Web cibles.

En revanche, l’approche du gestionnaire de Web Deploy offre différents avantages :

- Le gestionnaire de Web Deploy prend en charge l’authentification de base sur HTTPs, ce qui vous permet de transmettre les informations d’identification d’un autre compte à l’outil de déploiement Web IIS (Web Deploy).
- Vous pouvez configurer des serveurs Web cibles pour permettre aux utilisateurs non-administrateurs de déployer du contenu vers des sites Web IIS spécifiques à l’aide du gestionnaire de Web Deploy.

Par conséquent, il est clairement préférable de cibler le gestionnaire de Web Deploy lorsque vous automatisez le déploiement de packages Web à partir de Team Build. Il s’agit du processus recommandé :

1. Créez un compte de domaine à faibles privilèges à utiliser pour le déploiement.
2. Configurez le gestionnaire de Web Deploy et accordez au compte les autorisations nécessaires pour déployer du contenu sur un site Web IIS spécifique, comme décrit dans [configuration d’un serveur Web pour la publication Web Deploy (gestionnaire de Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Appelez Web Deploy et ciblez le gestionnaire de Web Deploy, à l’aide de l’authentification de base et en fournissant les informations d’identification du compte de domaine que vous avez créé, pour effectuer le déploiement.

Dans l’exemple de solution du [Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , vous spécifiez le type d’authentification (de base ou NTLM), les informations d’identification Web Deploy et l’adresse du point de terminaison (agent distant ou gestionnaire de Web Deploy) dans le fichier projet spécifique à l’environnement. Ces valeurs sont utilisées pour formuler et exécuter une commande Web Deploy lors de l’exécution du fichier projet. Pour plus d’informations, consultez [déploiement de packages Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Pour plus d’informations sur la configuration du gestionnaire de Web Deploy, notamment sur la configuration des autorisations, consultez [configuration d’un serveur Web pour la publication Web Deploy (gestionnaire de Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Pour plus d’informations sur la configuration de l’agent distant, consultez [configuration d’un serveur Web pour la publication de Web Deploy (agent distant)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configuration des autorisations de serveur de base de données

Pour déployer une base de données sur SQL Server, vous devez :

- Créez une connexion pour le compte de déploiement sur l’instance SQL Server.
- Accordez les autorisations de connexion **dbcreator** sur l’instance de SQL Server.
- Après le déploiement initial, ajoutez la connexion au rôle **propriétaire** de la base de données\_sur la base de données cible. Cela est nécessaire, car lors des déploiements suivants, vous modifiez une base de données existante au lieu de créer une nouvelle base de données.

Vous pouvez vous authentifier auprès d’une instance de SQL Server à l’aide de l’authentification NTLM ou de l’authentification SQL Server :

- Si vous utilisez l’authentification NTLM, vous devez accorder les autorisations décrites ci-dessus au compte de service de Build.
- Si vous utilisez l’authentification SQL Server, vous devez accorder les autorisations décrites ci-dessus au compte SQL Server. Vous devez également inclure le nom d’utilisateur et le mot de passe SQL Server dans la chaîne de connexion que vous utilisez pour déployer la base de données.

Pour obtenir des informations détaillées sur la configuration des autorisations pour le déploiement de base de données, consultez [configuration d’un serveur de base de données pour la publication de Web Deploy](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusion

À ce stade, vous devez comprendre les autorisations requises, ainsi que les options d’authentification qui vous sont ouvertes, lorsque vous automatisez les déploiements d’applications et de bases de données Web à partir de Team Build. Vous devez également pouvoir implémenter les autorisations nécessaires sur les serveurs Web IIS et les serveurs de base de données SQL Server.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur la configuration des environnements Windows Server pour prendre en charge le déploiement à distance, consultez [Configuration des environnements de serveur pour le déploiement Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Précédent](deploying-a-specific-build.md)
