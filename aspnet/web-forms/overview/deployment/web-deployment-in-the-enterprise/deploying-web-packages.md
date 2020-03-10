---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Déploiement de packages Web | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment vous pouvez publier des packages de déploiement Web sur un serveur distant à l’aide de l’outil de déploiement Web Internet Information Services (IIS) (Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 91b99e6e250342851aea6860164b6f6af54818d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575949"
---
# <a name="deploying-web-packages"></a>Déploiement de packages web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment vous pouvez publier des packages de déploiement Web sur un serveur distant à l’aide de l’Web Deploy outil de déploiement Web Internet Information Services (IIS) 2,0.
> 
> Vous pouvez déployer un package Web sur un serveur distant de deux manières :
> 
> - Vous pouvez utiliser l’utilitaire de ligne de commande MSDeploy. exe directement.
> - Vous pouvez exécuter le fichier *[nom du projet]. deploy. cmd* généré par le processus de génération.
> 
> Le résultat final est le même quelle que soit l’approche que vous utilisez. Fondamentalement, tout le fichier *. deploy. cmd* consiste à exécuter MSDeploy. exe avec des valeurs prédéterminées, afin que vous n’ayez pas à fournir autant d’informations que nécessaire pour déployer le package. Cela simplifie le processus de déploiement. En revanche, l’utilisation de MSDeploy. exe vous offre beaucoup plus de souplesse sur le déploiement exact de votre package.
> 
> L’approche que vous utilisez dépend de plusieurs facteurs, notamment la quantité de contrôle dont vous avez besoin sur le processus de déploiement et si vous ciblez le service d’agent distant Web Deploy ou le gestionnaire de Web Deploy. Cette rubrique explique comment utiliser chaque approche et identifie le moment approprié pour chaque approche.
> 
> Les tâches et procédures pas à pas de cette rubrique supposent que :
> 
> - Vous avez généré et empaqueté votre application Web, comme décrit dans [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md).
> - Vous avez modifié le fichier *SetParameters. xml* pour fournir les valeurs de paramètre appropriées pour votre environnement cible, comme décrit dans [Configuration des paramètres pour le déploiement de package Web](configuring-parameters-for-web-package-deployment.md).

L’exécution du fichier [*nom du projet*] *. deploy. cmd* est le moyen le plus simple de déployer un package Web. En particulier, l’utilisation du fichier *. deploy. cmd* offre ces avantages par rapport à l’utilisation directe de MSDeploy. exe :

- Vous n’avez pas besoin de spécifier l’emplacement du package&#x2014;de déploiement Web. le fichier *. deploy. cmd* sait déjà où il se trouve.
- Vous n’avez pas besoin de spécifier l’emplacement du fichier&#x2014; *SetParameters. xml* que le fichier *. deploy. cmd* connaît déjà.
- Vous n’avez pas besoin de spécifier les fournisseurs&#x2014;MSDeploy source et de destination. le fichier *. deploy. cmd* connaît déjà les valeurs à utiliser.
- Vous n’avez pas besoin de spécifier les&#x2014;paramètres de l’opération MSDeploy le fichier *. deploy. cmd* ajoute automatiquement les valeurs généralement requises à la commande MSDeploy. exe.

Avant d’utiliser le fichier *. deploy. cmd* pour déployer un package Web, vous devez vous assurer que :

- Le fichier *. deploy. cmd* , [*nom du projet*]. Fichier *SetParameters. xml* et le package Web ([*nom du projet*]. *zip*) se trouvent dans le même dossier.
- Web Deploy (MSDeploy. exe) est installé sur l’ordinateur qui exécute le fichier *. deploy. cmd* .

Le fichier *. deploy. cmd* prend en charge différentes options de ligne de commande. Lorsque vous exécutez le fichier à partir d’une invite de commandes, il s’agit de la syntaxe de base :

[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]

Vous devez spécifier un indicateur **/t** ou **/y** pour indiquer si vous souhaitez effectuer une exécution d’essai ou un déploiement en direct (n’utilisez pas les deux indicateurs dans la même commande). Ce tableau explique l’objectif de chacun de ces indicateurs.

| Indicateur | Description |
| --- | --- |
| **Commutateur** | Appelle MSDeploy. exe avec l’indicateur **– WhatIf** , qui indique une exécution d’essai. Au lieu de déployer le package, il crée un rapport sur ce qui se passerait si vous aviez déployé le package. |
| **/Y** | Appelle MSDeploy. exe sans l’indicateur **– WhatIf** . Cela déploie le package sur l’ordinateur local ou sur le serveur de destination spécifié. |
| **/M** | Spécifie le nom du serveur de destination ou l’URL du service. Pour plus d’informations sur les valeurs que vous pouvez fournir ici, consultez la section **considérations relatives aux points de terminaison** dans cette rubrique. Si vous omettez l’indicateur **/m** , le package est déployé sur l’ordinateur local. |
| **/A** | Spécifie le type d’authentification que MSDeploy. exe doit utiliser pour effectuer le déploiement. Les valeurs possibles sont **NTLM** et de **base**. Si vous omettez l’indicateur **/a** , le type d’authentification est défini par défaut sur **NTLM** pour le déploiement sur le Web Deploy service de l’agent distant et sur **Basic** pour le déploiement dans le gestionnaire de Web Deploy. |
| **/U** | Spécifie le nom d'utilisateur. Cela s’applique uniquement si vous utilisez l’authentification de base. |
| **/P** | Spécifie le mot de passe. Cela s’applique uniquement si vous utilisez l’authentification de base. |
| **/L** | Indique que le package doit être déployé sur l’instance de IIS Express locale. |
| **/G** | Spécifie que le package est déployé à l’aide du [paramètre du fournisseur tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Si vous omettez l’indicateur **/g** , la valeur par défaut est **false**. |

> [!NOTE]
> Chaque fois que le processus de génération crée un package Web, il crée également un fichier nommé *[Project Name]. deploy-Readme. txt* qui explique ces options de déploiement.

En plus de ces indicateurs, vous pouvez spécifier Web Deploy paramètres d’opération en tant que paramètres supplémentaires *. deploy. cmd* . Tous les paramètres supplémentaires que vous spécifiez sont simplement transmis à la commande MSDeploy. exe sous-jacente. Pour plus d’informations sur ces paramètres, consultez Paramètres de l' [opération Web Deploy](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Supposons que vous souhaitiez déployer le projet d’application Web ContactManager. Mvc dans un environnement de test en exécutant le fichier *. deploy. cmd* . Votre environnement de test est configuré pour utiliser le service de l’agent distant Web Deploy, comme décrit dans [configurer un serveur Web pour la publication Web Deploy (agent distant)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Pour déployer l’application Web, vous devez effectuer les étapes suivantes.

**Pour déployer une application Web à l’aide du fichier. deploy. cmd**

1. Générez et Empaquetez le projet d’application Web, comme décrit dans [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md).
2. Modifiez le fichier *ContactManager. Mvc. SetParameters. xml* pour qu’il contienne les valeurs de paramètres appropriées pour votre environnement de test, comme décrit dans [Configuration des paramètres pour le déploiement de package Web](configuring-parameters-for-web-package-deployment.md).
3. Ouvrez une fenêtre d’invite de commandes et accédez à l’emplacement du fichier *ContactManager. Mvc. deploy. cmd* .
4. Tapez cette commande et appuyez sur entrée :

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

Dans cet exemple :

- L’indicateur **/y** indique que vous souhaitez réellement déployer le package, plutôt que d’effectuer une exécution d’essai.
- L’indicateur **/m** indique que vous souhaitez déployer le package sur le serveur nommé TESTWEB1. À partir de cette valeur, MSDeploy. exe va tenter de déployer le package sur le service de l’agent distant Web Deploy sur http://TESTWEB1/MSDeployAgentService.
- L’indicateur **/a** indique que vous souhaitez utiliser l’authentification NTLM. Par conséquent, vous n’avez pas besoin de spécifier un nom d’utilisateur et un mot de passe.

Pour illustrer comment l’utilisation du fichier *. deploy. cmd* simplifie le processus de déploiement, examinez la commande MSDeploy. exe qui est générée et exécutée lorsque vous exécutez *ContactManager. Mvc. deploy. cmd* à l’aide des options indiquées ci-dessus.

[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]

Pour plus d’informations sur l’utilisation du fichier *. deploy. cmd* pour déployer un package Web, consultez [procédure : installer un package de déploiement à l’aide du fichier Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Utilisation de MSDeploy. exe

Bien que l’utilisation du fichier *. deploy. cmd* simplifie généralement le processus de déploiement, il existe des situations où il est préférable d’utiliser MSDeploy. exe directement. Exemple :

- Si vous souhaitez déployer le gestionnaire de Web Deploy en tant qu’utilisateur non-administrateur, vous ne pouvez pas utiliser le fichier *. deploy. cmd* . Cela est dû à un bogue dans Web Deploy 2,0, comme décrit dans **considérations relatives aux points de terminaison**.
- Si vous souhaitez basculer manuellement entre différents fichiers *SetParameters. xml* dans différents emplacements, vous préférerez peut-être utiliser MSDeploy. exe directement.
- Si vous souhaitez remplacer plusieurs arguments de ligne de commande MSDeploy. exe, vous préférerez peut-être utiliser directement MSDeploy. exe.

Lorsque vous utilisez MSDeploy. exe, vous devez fournir trois éléments d’information clés :

- Paramètre **-source** qui indique où proviennent vos données.
- Paramètre **– dest** qui indique où vos données vont.
- Un paramètre **– verb** qui indique l' [opération](https://technet.microsoft.com/library/dd568989(WS.10).aspx) que vous souhaitez effectuer.

MSDeploy. exe s’appuie sur les [fournisseurs de Web Deploy](https://technet.microsoft.com/library/dd569040(WS.10).aspx) pour traiter les données sources et de destination. Web Deploy inclut un grand nombre de fournisseurs qui représentent la gamme d’applications et de sources de données qu'&#x2014;il peut utiliser, par exemple, il existe des fournisseurs pour les bases de données SQL Server, les serveurs Web IIS, les certificats, les assemblys global assembly cache (GAC), divers fichiers de configuration différents et un grand nombre d’autres types de données. Le paramètre **– source** et le paramètre **– dest** doivent spécifier un fournisseur, sous la forme **– source**: [*providerName*] = [*location*]. Lorsque vous déployez un package Web sur un site Web IIS, vous devez utiliser les valeurs suivantes :

- Le fournisseur de **source-** est toujours un [package](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Exemple :

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Le fournisseur **– dest** est toujours [auto](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Par exemple :

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- Le **verbe –** est toujours **synchronisé**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

En outre, vous devrez spécifier d’autres [paramètres spécifiques au fournisseur](https://technet.microsoft.com/library/dd569001(WS.10).aspx) et des paramètres d' [opération](https://technet.microsoft.com/library/dd569089(WS.10).aspx)généraux. Supposons, par exemple, que vous souhaitiez déployer l’application Web ContactManager. Mvc dans un environnement intermédiaire. Le déploiement ciblera le gestionnaire de Web Deploy et doit utiliser l’authentification de base. Pour déployer l’application Web, vous devez effectuer les étapes suivantes.

**Pour déployer une application Web à l’aide de MSDeploy. exe**

1. Générez et Empaquetez le projet d’application Web, comme décrit dans [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md).
2. Modifiez le fichier *ContactManager. Mvc. SetParameters. xml* pour qu’il contienne les valeurs de paramètres correctes pour votre environnement intermédiaire, comme décrit dans [Configuring parameters for Web package Deployment](configuring-parameters-for-web-package-deployment.md).
3. Ouvrez une fenêtre d’invite de commandes et accédez à l’emplacement de MSDeploy. exe. C’est généralement le%PROGRAMFILES%\IIS\Microsoft Web Deploy V2\msdeploy.exe.
4. Tapez cette commande et appuyez sur entrée (ignorez les sauts de ligne) :

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

Dans cet exemple :

- Le paramètre **– source** spécifie le fournisseur de **package** et indique l’emplacement du package Web.
- Le paramètre **– dest** spécifie le fournisseur **automatique** . Le paramètre **ComputerName** fournit l’URL de service du gestionnaire de Web Deploy sur le serveur de destination. Le paramètre **AuthType** indique que vous souhaitez utiliser l’authentification de base et, par conséquent, vous devez fournir un **nom d’utilisateur** et un **mot de passe**. Enfin, le paramètre **includeAcls = "false"** indique que vous ne voulez pas copier les listes de contrôle d’accès (ACL) des fichiers de votre application Web source sur le serveur de destination.
- L’argument **– verb : Sync** indique que vous souhaitez répliquer le contenu source sur le serveur de destination.
- Les arguments **– disableLink** indiquent que vous ne souhaitez pas répliquer les pools d’applications, la configuration de répertoire virtuel ou les certificats protocole SSL (SSL) sur le serveur de destination. Pour plus d’informations, consultez [Web Deploy des extensions de liens](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Le paramètre **– setParamFile** fournit l’emplacement du fichier *SetParameters. xml* .
- Le commutateur **– allowUntrusted** indique que Web Deploy doit accepter les certificats SSL qui n’ont pas été émis par une autorité de certification approuvée. Si vous effectuez un déploiement sur le gestionnaire de Web Deploy et que vous avez utilisé un certificat auto-signé pour sécuriser l’URL du service, vous devez inclure ce commutateur.

## <a name="automating-web-package-deployment"></a>Automatisation du déploiement de packages Web

Dans de nombreux scénarios d’entreprise, vous souhaiterez déployer vos packages Web dans le cadre d’un déploiement à étape unique ou automatisée. Que vous choisissiez ou non de déployer vos packages Web en exécutant le fichier *. deploy. cmd* ou en utilisant directement MSDeploy. exe, vous pouvez paramétrer vos commandes et les appeler à partir d’une cible dans un fichier projet Microsoft Build Engine (MSBuild).

Dans l’exemple de solution du gestionnaire de contacts, jetez un coup d’œil à la cible **PublishWebPackages** dans le fichier *Publish. proj* . Cette cible est exécutée une fois pour chaque fichier *. deploy. cmd* identifié par une liste d’éléments nommée **PublishPackages**. La cible utilise des propriétés et des métadonnées d’élément pour générer un jeu complet de valeurs d’arguments pour chaque fichier *. deploy. cmd* , puis utilise la tâche **Exec** pour exécuter la commande.

[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]

> [!NOTE]
> Pour obtenir une vue d’ensemble plus générale du modèle de fichier projet dans l’exemple de solution et une introduction aux fichiers projet personnalisés en général, consultez [Présentation du fichier projet](understanding-the-project-file.md) et [Présentation du processus de génération](understanding-the-build-process.md).

## <a name="endpoint-considerations"></a>Considérations relatives aux points de terminaison

Que vous déployiez votre package Web en exécutant le fichier *. deploy. cmd* ou à l’aide de MSDeploy. exe directement, vous devez spécifier un nom d’ordinateur ou un point de terminaison de service pour votre déploiement.

Si le serveur Web de destination est configuré pour le déploiement à l’aide du service d’agent distant Web Deploy, vous spécifiez l’URL du service cible comme destination.

[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]

Vous pouvez également spécifier le nom du serveur seul comme destination, et Web Deploy déduire l’URL du service de l’agent distant.

[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]

Si le serveur Web de destination est configuré pour le déploiement à l’aide du gestionnaire de Web Deploy, vous devez spécifier l’adresse de point de terminaison du service de gestion Web IIS (WMSvc) comme destination. Par défaut, le format est le suivant :

[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]

Vous pouvez cibler l’un de ces points de terminaison à l’aide du fichier *. deploy. cmd* ou de MSDeploy. exe directement. Toutefois, si vous souhaitez déployer le gestionnaire de Web Deploy en tant qu’utilisateur non-administrateur, comme décrit dans [configurer un serveur Web pour la publication Web Deploy (gestionnaire de Web Deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), vous devez ajouter une chaîne de requête à l’adresse du point de terminaison de service.

[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]

Cela est dû au fait que l’utilisateur non-administrateur ne dispose pas d’un accès au niveau du serveur pour IIS ; Il a uniquement accès à un site Web IIS spécifique. Au moment de l’écriture, en raison d’un bogue dans le pipeline de publication Web, vous ne pouvez pas exécuter le fichier *. deploy. cmd* à l’aide d’une adresse de point de terminaison qui comprend une chaîne de requête. Dans ce scénario, vous devez déployer votre package Web à l’aide de MSDeploy. exe directement.

> [!NOTE]
> Pour plus d’informations sur les Web Deploy service de l’agent distant et du gestionnaire de Web Deploy, consultez [choix de l’approche appropriée pour le déploiement Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Pour obtenir des conseils sur la façon de configurer vos fichiers projet spécifiques à l’environnement à déployer sur ces points de terminaison, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="authentication-considerations"></a>Considérations relatives à l'authentification

Que vous déployiez votre package Web en exécutant le fichier *. deploy. cmd* ou à l’aide de MSDeploy. exe directement, vous devez spécifier un type d’authentification. Web Deploy accepte deux valeurs possibles : **NTLM** ou de **base**. Si vous spécifiez l’authentification de base, vous devez également fournir un nom d’utilisateur et un mot de passe. Vous devez tenir compte de différents facteurs lorsque vous sélectionnez un type d’authentification :

- Si vous effectuez un déploiement sur le service de l’agent distant Web Deploy, vous devez utiliser l’authentification NTLM. Le service de l’agent distant n’accepte pas les informations d’authentification de base.
- Si vous effectuez un déploiement sur le gestionnaire de Web Deploy, vous pouvez utiliser l’authentification NTLM ou de base. Le paramètre par défaut est l’authentification de base. Bien que l’authentification de base repose sur le fait que les noms d’utilisateur et les mots de passe sont transmis en texte brut, vos informations d’identification sont protégées car le gestionnaire de Web Deploy utilise toujours le chiffrement SSL.
- Si votre package Web comprend une base de données et que le serveur Web et le serveur de base de données sont des machines distinctes, vous ne pourrez pas déployer la base de données à l’aide de l’authentification NTLM en raison de la [limitation « double saut » NTLM](https://go.microsoft.com/?linkid=9805120). Vous devez utiliser SQL Server informations d’identification dans votre chaîne de connexion de déploiement ou fournir des informations d’authentification de base pour Web Deploy. Ce problème est décrit plus en détail dans [déploiement de bases de données d’appartenance dans des environnements d’entreprise](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit comment vous pouvez déployer un package Web en exécutant le fichier *. deploy. cmd* ou en utilisant directement MSDeploy. exe. Il explique quand chaque approche peut être appropriée et décrit comment vous pouvez paramétrer et exécuter une commande de déploiement dans le cadre d’un processus de génération automatisé ou à une seule étape plus large.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la création et le paramétrage d’un package de déploiement Web, consultez [génération et empaquetage de projets d’application Web](building-and-packaging-web-application-projects.md) et [configuration de paramètres pour le déploiement de packages Web](configuring-parameters-for-web-package-deployment.md). Pour obtenir des conseils sur la façon de créer et déployer des packages Web à partir d’une instance de Team Foundation Server (TFS), consultez [Configuration des Team Foundation Server pour le déploiement Web automatisé](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Pour plus d’informations sur la personnalisation et le dépannage du processus de déploiement, consultez [exclusion de fichiers et de dossiers du déploiement](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Précédent](configuring-parameters-for-web-package-deployment.md)
> [Suivant](deploying-database-projects.md)
