---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Choix de la bonne approche pour le déploiement Web | Microsoft Docs
author: jrjlee
description: Lorsque vous utilisez l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS) 2,0 ou une version ultérieure, vous pouvez utiliser trois approches principales...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13f784dd8e6404806104d56b026b3c41ca178892
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548481"
---
# <a name="choosing-the-right-approach-to-web-deployment"></a>Choix de la bonne approche pour le déploiement web

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Lorsque vous utilisez l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS) 2,0 ou une version ultérieure, vous pouvez utiliser trois approches principales pour obtenir vos applications Web empaquetées sur un serveur Web. Vous pouvez :
> 
> - Déployez l’application à partir d’un emplacement distant en ciblant le *service de Deployment agent Web* (également appelé « agent distant ») sur le serveur de destination.
> - Déployez l’application à partir d’un emplacement distant à l’aide de Web Deploy à la demande (également appelé « agent Temp »).
> - Déployez l’application à partir d’un emplacement distant en ciblant le *Gestionnaire de Web Deploy IIS* sur le serveur de destination.
> - Déployez l’application en copiant manuellement le package Web sur le serveur de destination et en l’important via le gestionnaire des services Internet.
> 
> La façon dont vous configurez vos serveurs Web de destination dépend de l’approche du déploiement que vous souhaitez utiliser. Cette rubrique vous aidera à choisir l’approche à utiliser pour le déploiement.

Ce tableau présente les principaux avantages et inconvénients de chaque approche de déploiement, ainsi que les scénarios qui correspondent le plus généralement à chaque approche.

| Approche | Avantages | Inconvénients | Scénarios typiques |
| --- | --- | --- | --- |
| Agent distant | Il est facile à configurer. Il convient pour les mises à jour régulières du contenu et des applications Web. | L’utilisateur doit être un administrateur sur le serveur cible. l’utilisateur ne peut pas fournir d’autres informations d’identification. | Environnements de développement. Environnements de test. |
| Agent temporaire | Il n’est pas nécessaire d’installer Web Deploy sur l’ordinateur cible. La dernière version de Web Deploy est utilisée automatiquement. | L’utilisateur doit être un administrateur sur le serveur cible. l’utilisateur ne peut pas fournir d’autres informations d’identification. | Environnements de développement. Environnements de test. |
| Gestionnaire de Web Deploy | Les utilisateurs non-administrateurs peuvent déployer du contenu. Il convient pour les mises à jour régulières du contenu et des applications Web. | Il est beaucoup plus complexe à configurer. | Environnements intermédiaires. Environnements de production intranet. Environnements hébergés. |
| Déploiement hors connexion | Il est très facile à configurer. Il convient pour les environnements isolés. | L’administrateur du serveur doit copier et importer manuellement le package Web à chaque fois. | Environnements de production accessibles sur Internet. Environnements réseau isolés. |

## <a name="using-the-remote-agent"></a>Utilisation de l’agent distant

Lorsque vous installez Web Deploy à l’aide des paramètres par défaut sur un serveur de destination, le service Web Deployment Agent (l’agent distant) est automatiquement installé et démarré. Par défaut, l’agent distant expose un point de terminaison HTTP à cette adresse :

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]

> [!NOTE]
> Vous pouvez remplacer [*Server*] par le nom d’ordinateur de votre serveur Web, une adresse IP pour votre serveur Web ou un nom d’hôte qui correspond à votre serveur Web.

Les administrateurs de serveur peuvent déployer des packages Web à partir d’un emplacement distant, comme un ordinateur de développement ou un serveur de builds, en spécifiant cette adresse de point de terminaison. Par exemple, supposons que Matt Hink chez Fabrikam, Inc. a créé le projet d’application Web ContactManager. Mvc sur son ordinateur de développement. Le processus de génération génère un package Web, ainsi qu’un fichier *. deploy. cmd* qui contient les commandes Web Deploy requises pour installer le package. Si Matt est un administrateur de serveur sur le serveur TESTWEB1, il peut déployer l’application Web sur le serveur Web de test en exécutant cette commande sur son ordinateur de développement :

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]

En fait, le Web Deploy exécutable peut déduire l’adresse du point de terminaison de l’agent distant si vous fournissez le nom de l’ordinateur. Matt n’a donc qu’à taper ce qui suit :

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]

> [!NOTE]
> Pour plus d’informations sur Web Deploy syntaxe de ligne de commande et les fichiers *. deploy. cmd* , consultez [procédure : installer un package de déploiement à l’aide du fichier Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

L’agent distant offre un moyen simple de déployer du contenu à partir d’un emplacement distant, et cette approche peut fonctionner correctement avec un déploiement en un seul clic ou automatisé. Toutefois, l’utilisateur qui exécute la commande de déploiement doit également être un administrateur de domaine ou un membre du groupe Administrateurs local sur le serveur de destination. En outre, l’agent distant ne prend pas en charge l’authentification de base, vous ne pouvez donc pas transmettre d’autres informations d’identification sur la ligne de commande.

L’agent distant offre une approche utile pour le déploiement dans des scénarios de développement ou de test, où il n’est pas rare pour les développeurs d’avoir un contrôle complet de l’administrateur sur un environnement de serveur de test, et les applications sont généralement reconstruites et redéployées très rapidement. fréquemment. Toutefois, cette approche est généralement moins acceptable pour les environnements intermédiaires ou de production.

Pour obtenir un exemple de bout en bout d’un scénario qui utilise l’approche de l’agent distant, consultez [scénario : configuration d’un environnement de test pour le déploiement Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Utilisation de l’agent Temp

L’approche de l’agent temporaire pour le déploiement est similaire à l’approche de l’agent distant. Toutefois, contrairement à l’approche de l’agent distant, vous n’avez pas besoin d’installer Web Deploy sur le serveur Web de destination. Au lieu de cela, lorsque vous effectuez le déploiement, Web Deploy installe une version temporaire du service de l’agent de déploiement Web sur le serveur de destination et l’utilise pour déployer votre contenu sur IIS. Une fois le déploiement terminé, tous les fichiers temporaires sont supprimés.

Si vous souhaitez utiliser le paramètre du fournisseur de l’agent temporaire, ajoutez l’indicateur **/g** à votre commande de déploiement :

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]

> [!NOTE]
> Vous ne pouvez pas utiliser l’agent temporaire si le service de l’agent de déploiement Web est installé sur l’ordinateur de destination, même si le service n’est pas en cours d’exécution.

L’avantage de cette approche est que vous n’avez pas besoin de gérer les installations de Web Deploy sur vos serveurs de destination. En outre, vous n’avez pas besoin de vous assurer que les ordinateurs source et de destination exécutent la même version de Web Deploy. Toutefois, cette approche subit les mêmes limitations que l’approche de l’agent distant, à savoir que vous devez être un administrateur local sur le serveur de destination pour déployer le contenu, et que seule l’authentification NTLM est prise en charge. L’approche de l’agent temporaire nécessite également beaucoup plus de configuration initiale de l’environnement de destination.

Pour plus d’informations sur l’utilisation de l’agent temporaire, consultez [procédure : installer un package de déploiement à l’aide du fichier Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx) et [Web Deploy à la demande](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Utilisation du gestionnaire de Web Deploy

Pour IIS 7, Web Deploy offre une autre approche de déploiement via le gestionnaire de Web Deploy IIS. Le gestionnaire de Web Deploy est étroitement intégré au service de gestion Web IIS (WMSvc), qui est conçu pour permettre aux utilisateurs de gérer les sites Web IIS à partir d’emplacements distants.

Par défaut, l’agent distant expose un point de terminaison HTTP à cette adresse :

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]

> [!NOTE]
> Vous pouvez remplacer [*Server*] par le nom d’ordinateur de votre serveur Web, une adresse IP pour votre serveur Web ou un nom d’hôte qui correspond à votre serveur Web.

Le principal avantage du gestionnaire de Web Deploy sur l’agent distant, et l’agent temporaire, est que vous pouvez configurer IIS pour permettre aux utilisateurs non-administrateurs de déployer des applications et du contenu sur des sites Web IIS spécifiques. Le gestionnaire de Web Deploy prend également en charge l’authentification de base, de sorte que vous pouvez fournir d’autres informations d’identification en tant que paramètres dans vos commandes de Web Deploy. Le principal inconvénient est que le gestionnaire de Web Deploy est initialement beaucoup plus compliqué à configurer et à configurer.

Dans le cas d’utilisateurs qui ne sont pas administrateurs, le service de gestion Web (WMSvc) autorise uniquement l’utilisateur à se connecter à IIS à l’aide d’une connexion au niveau du site, plutôt qu’une connexion au niveau du serveur. Pour accéder à un site particulier, vous pouvez inclure une chaîne de requête spécifique au site dans l’adresse du point de terminaison :

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]

Par exemple, supposons qu’un processus de génération est configuré pour déployer automatiquement une application Web dans un environnement intermédiaire après chaque génération réussie. Si vous avez utilisé l’approche de l’agent distant, vous devez faire de l’identité du processus de génération un administrateur sur vos serveurs de destination. En revanche, à l’aide de l’approche du gestionnaire de Web Deploy, vous pouvez&#x2014;accorder à un utilisateur&#x2014;non-administrateur**FABRIKAM\stagingdeployer** , dans ce cas, l’autorisation à un site Web IIS spécifique, et le processus de génération peut fournir ces informations d’identification pour déployer le package Web.

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]

> [!NOTE]
> Pour plus d’informations sur Web Deploy opérations et la syntaxe de ligne de commande, consultez [Web Deploy référence de ligne de commande](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Pour plus d’informations sur l’utilisation du fichier *. deploy. cmd* , consultez [procédure : installer un package de déploiement à l’aide du fichier Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Le gestionnaire de Web Deploy fournit une approche utile pour le déploiement dans des environnements intermédiaires, des environnements hébergés et des environnements de production intranet, où l’accès à distance au serveur est disponible, mais pas les informations d’identification d’administrateur.

Pour obtenir un exemple de bout en bout d’un scénario qui utilise l’approche du gestionnaire de Web Deploy, consultez [scénario : configuration d’un environnement intermédiaire pour le déploiement Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Utilisation du déploiement hors connexion

Dans certains cas, il n’est pas possible ou pratique de déployer des applications et du contenu sur un site Web IIS à partir d’un emplacement distant. Par exemple, les ordinateurs source et de destination peuvent se trouver dans des réseaux isolés ou des segments réseau, ou la stratégie de pare-feu peut ne pas autoriser l’accès à distance.

Dans des scénarios tels que ceux-ci, vous pouvez toujours utiliser les fonctionnalités d’empaquetage et de publication de Web Deploy ; vous ne pouvez pas les utiliser à partir d’un emplacement distant. Au lieu de cela, un administrateur sur le serveur de destination doit copier le package Web sur le serveur et l’importer par le biais du gestionnaire des services Internet.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

L’approche de déploiement hors connexion est généralement utile dans les environnements de production accessibles sur Internet, où les serveurs d’un réseau de périmètre peuvent avoir une connectivité limitée avec les ordinateurs du réseau interne.

Pour obtenir un exemple de bout en bout d’un scénario qui utilise l’approche de déploiement hors connexion, consultez [scénario : configuration d’un environnement de production pour le déploiement Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur Web Deploy opérations et la syntaxe de ligne de commande, consultez [Web Deploy référence de ligne de commande](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Pour plus d’informations sur l’utilisation du fichier *. deploy. cmd* , consultez [procédure : installer un package de déploiement à l’aide du fichier Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Pour obtenir des conseils plus généraux sur les différentes façons de déployer des packages Web à partir d’un ordinateur distant, consultez [utilisation de Web Deploy à distance](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Pour plus d’informations sur l’utilisation de Web Deploy à la demande, consultez [Web Deploy à la demande](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Précédent](configuring-server-environments-for-web-deployment.md)
> [Suivant](scenario-configuring-a-test-environment-for-web-deployment.md)
