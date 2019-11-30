---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: Options d’hébergement ASP.NETC#() | Microsoft Docs
author: rick-anderson
description: Les applications Web ASP.NET sont généralement conçues, créées et testées dans un environnement de développement local et doivent être déployées dans un environnement de production...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 2eafa750167d89fa996a442633e79dce3d5b85bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620773"
---
# <a name="aspnet-hosting-options-c"></a>Options d’hébergement ASP.NET (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Les applications Web ASP.NET sont généralement conçues, créées et testées dans un environnement de développement local et doivent être déployées dans un environnement de production une fois qu’elles sont prêtes pour la publication. Ce didacticiel fournit une vue d’ensemble du processus de déploiement et sert de présentation de cette série de didacticiels.

## <a name="introduction"></a>Introduction

Les applications Web sont généralement conçues, créées et testées dans un environnement de développement accessible uniquement aux programmeurs qui travaillent sur le site. Une fois que l’application est prête à être libérée, elle est déplacée dans un environnement de production où le site est accessible à quiconque sur Internet. Ce processus de déploiement présente un certain nombre de défis :

- Un environnement de production doit exister et être correctement configuré avant qu’une application ASP.NET puisse être déployée ; en outre, l’environnement de production doit être mis à jour avec les derniers correctifs de sécurité.
- L’ensemble correct des fichiers de balisage, des fichiers de code et des fichiers de prise en charge doivent être copiés de l’environnement de développement vers l’environnement de production. Pour les applications pilotées par les données, il peut également être nécessaire de copier le schéma et/ou les données de la base de données.
- Il peut y avoir des différences de configuration entre les deux environnements. La chaîne de connexion de base de données ou le serveur de messagerie utilisé dans l’environnement de développement sera probablement différent de l’environnement de production. De plus, le comportement de l’application peut dépendre de l’environnement. Par exemple, lorsqu’une erreur se produit dans le développement, les détails de l’erreur peuvent s’afficher à l’écran, mais lorsqu’une erreur se produit en production, une page d’erreur conviviale doit s’afficher à la place et les détails de l’erreur sont envoyés aux développeurs.

Pour éviter la première difficulté à configurer et à gérer un environnement de production, plusieurs personnes et entreprises externalisent leurs environnements de production aux *fournisseurs d’hébergement Web*. Un fournisseur d’hébergement Web est une entreprise qui gère l’environnement de production à votre place. Il existe des fournisseurs d’hôtes Web sans nombre, chacun avec des tarifs et des niveaux de service différents ; consultez la section « recherche d’un fournisseur d’hôte Web » pour obtenir des conseils sur la localisation d’un tel fournisseur de services.

Il s’agit de la première d’une série de didacticiels qui examinent les étapes impliquées dans le déploiement d’une application Web ASP.NET dans un environnement de production géré par un fournisseur d’hébergement Web. Au cours de ces didacticiels, nous examinerons les éléments suivants :

- Les fichiers qui doivent être déployés sur le fournisseur de l’hôte Web.
- Outils permettant de simplifier le processus de déploiement.
- Comment déployer une base de données.
- Conseils pour le déploiement d’une base de données qui utilise [le fournisseur d’appartenances et de rôles SQL](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), ainsi que des méthodes pour imiter l’outil d’administration de site Web dans un environnement de production.
- Stratégies de mise à jour fluide de la base de données en production avec les modifications effectuées pendant le développement.
- Techniques pour la journalisation des erreurs qui se produisent lors de la production et méthodes pour notifier les développeurs lorsqu’une erreur se produit.

Ces didacticiels sont destinés à être concis et à fournir des instructions pas à pas avec de nombreuses captures d’écran pour vous guider tout au long du processus. Ce didacticiel plus inaugural fournit une vue d’ensemble du processus de déploiement de ASP.NET et des conseils sur la recherche d’un fournisseur d’hébergement Web. Commençons !

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Vue d’ensemble du processus de déploiement ASP.NET

Pour résumer, le déploiement d’une application ASP.NET implique les trois étapes suivantes :

1. Configurez l’application Web, le serveur Web et la base de données dans l’environnement de production.
2. Synchronisez les pages ASP.NET, les fichiers de code, les assemblys dans le dossier `Bin` et les fichiers de prise en charge HTML, tels que les fichiers CSS et JavaScript.
3. Synchronisez le schéma et/ou les données de la base de données.

Les informations de configuration d’une application Web se trouvent généralement dans le fichier `Web.config` et incluent les chaînes de connexion à la base de données, les critères de gestion des erreurs, les règles de réécriture d’URL et les informations du serveur de messagerie. Ces informations sont souvent différentes pour une application en cours de développement et pour la même application en production. Par exemple, lors du développement d’une application, il est préférable d’utiliser une base de données de développement afin de ne pas effectuer de test sur la base de données de production. Par conséquent, les chaînes de connexion de la base de données diffèrent généralement entre les applications de développement et de production. En raison de ces différences, une partie du déploiement implique de modifier les informations de configuration de l’application Web.

En plus des modifications apportées à la configuration de l’application Web, l’étape 1 peut également entraîner la configuration du serveur Web et de la base de données. Par exemple, si une page ASP.NET crée ou supprime des fichiers d’un répertoire sur le serveur Web, le serveur Web doit être configuré pour autoriser ces modifications du système de fichiers. De même, il peut y avoir des paramètres d’autorisation ou d’authentification qui doivent être définis dans la base de données.

L’étape 2 implique la synchronisation de l’ensemble des pages de ASP.NET essentielles et des fichiers de prise en charge entre les environnements de développement et de production. L’ensemble particulier des fichiers liés à ASP.NET qui doivent être synchronisés entre les deux environnements dépend du type de projet que vous avez créé dans Visual Studio. il s’agit de la discussion dans le didacticiel suivant, [*qui détermine quels fichiers doivent être déployés*](determining-what-files-need-to-be-deployed-cs.md). Troisième et quatrième didacticiels : [*déploiement de votre site à l’aide de FTP*](deploying-your-site-using-an-ftp-client-cs.md) et [*déploiement de votre site à l’aide de Visual Studio*](deploying-your-site-using-visual-studio-cs.md) -examinez différents outils et techniques pour la synchronisation de ces fichiers.

Lorsque vous créez des applications pilotées par les données, deux bases de données sont généralement utilisées : une pour le développement et une sur la production. Pendant le développement, le schéma de la base de données de développement peut être modifié pour inclure de nouvelles tables, colonnes, procédures stockées et déclencheurs, ou peut être modifié pour supprimer ou renommer des objets de base de données existants. Entre le moment où ces modifications sont effectuées et le moment où l’application est déployée en production, les bases de données de production et de développement sont désynchronisées. Cet asynchronie doit être corrigé pendant le processus de déploiement. Ces défis seront examinés dans les prochains didacticiels.

## <a name="finding-a-web-host-provider"></a>Recherche d’un fournisseur d’hébergement Web

Les applications ASP.NET peuvent être déployées sur n’importe quel serveur Web sur lequel le .NET Framework et le Internet Information Services (IIS) sont installés. Vous pouvez héberger un site à partir de votre ordinateur personnel, en supposant que vous disposiez d’une connexion haut débit à Internet et que vous sachiez comment configurer votre routeur pour autoriser les requêtes Web entrantes. Vous pouvez également héberger un site à partir d’un ordinateur d’un intranet, comme le font le nombre d’entreprises. Toutefois, l’objectif de ces didacticiels est d’héberger votre site Web avec un fournisseur d’hébergement Web.

> [!NOTE]
> [IIS](https://www.iis.net/) est le serveur Web de Microsoft de niveau entreprise. Il est fourni avec les éditions non familiales de Windows, telles que Windows Server 2008 et certaines éditions de Windows Vista. Vous n’avez pas besoin d’installer IIS pour servir des applications ASP.NET dans un environnement de développement, car Visual Studio comprend le serveur Web de développement ASP.NET. Toutefois, le serveur Web de développement ASP.NET n’accepte que les connexions locales et ne peut donc pas être utilisé dans un environnement de production.

Avant de pouvoir déployer votre site sur un fournisseur d’hébergement Web, vous devez d’abord décider de l’entreprise avec laquelle il doit s’y faire. Il existe des sociétés d’hébergement Web innombrables dans la place de marché ; une recherche de « société d’hébergement Web » renvoie plus de 5 millions résultats. Comment trouver celle qui vous convient le plus ? Votre moteur de recherche préféré est un bon point de départ, comme c’est le cas de sites Web tels que les [hôtes](http://www.tophosts.com/) et les [HostCritique](http://www.hostcritique.net/), qui comparent et contrastent différents services d’hébergement. Je conseille également de demander à vos collègues et collègues de faire des recommandations. vous pouvez également demander des recommandations sur le [Forum « hébergement ouvert](https://forums.asp.net/158.aspx) » ici sur les [Forums ASP.net](https://forums.asp.net/).

Les sociétés d’hébergement Web proposent généralement des plans d’hébergement partagés et des plans d’hébergement dédiés. Avec l’hébergement partagé, un serveur Web unique héberge des douzaines de sites Web différents. Avec un hébergement dédié, vous louez un ordinateur de l’entreprise qui dessert votre site et votre site seul. Un plan d’hébergement partagé peut inclure la prise en charge des pages ASP.NET, la possibilité d’utiliser des bases de données Microsoft Access, 5 Go d’espace disque et 100 Go de trafic de bande passante mensuel pour $9,95 par mois. Un autre plan d’hébergement partagé peut inclure la prise en charge des pages ASP.NET, l’accès au serveur de base de données Microsoft SQL Server 2008, 10 Go d’espace disque et 250 Go de trafic de bande passante mensuel pour $19,95 par mois. Les plans d’hébergement dédiés sont généralement beaucoup plus onéreux, ce qui coûte plusieurs centaines de dollars par mois, mais offre de meilleures performances et davantage de contrôle que les options d’hébergement partagé. Le plan que vous choisissez dépend de votre budget, du volume de trafic que votre site Web reçoit et des fonctionnalités dont vous avez besoin.

Deux points importants sont à prendre en compte lors du choix d’un fournisseur d’hôte Web : service client et qualité de service. Si vous avez une question ou un problème de configuration, combien de temps faut-il pour envoyer votre problème au support technique de l’hôte Web jusqu’à ce que vous obteniez une réponse ? Quel est le degré de fiabilité des services de l’entreprise ? Les interruptions de la base de données sont-elles fréquentes ? À quelle fréquence le serveur de messagerie passe-t-il hors connexion ? Vous pouvez toujours demander à une entreprise de fournir des détails sur son temps d’activité et de se renseigner sur sa stratégie de service client, mais une méthode plus plaisanter consiste à solliciter les commentaires des clients actuels et passés, ce que vous pouvez faire via les forums en ligne, les groupes de discussion et les e-mails listservs .

> [!NOTE]
> Certaines entreprises d’hébergement Web concentrent leur activité sur une pile technologique particulière, telle que .NET ou [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **a** Pache, **M** ySQL et **P** HP). Assurez-vous que la société que vous sélectionnez héberge des applications ASP.net. Vérifiez également qu’ils prennent en charge la version de ASP.NET que vous utilisez pour générer votre application. Et si vous générez une application pilotée par les données, assurez-vous que l’hôte Web offre le même serveur de base de données et la même version que ceux que vous utilisez.

## <a name="summary"></a>Récapitulatif

Les applications Web ASP.NET sont généralement conçues, créées et testées dans un environnement de développement local. Une fois qu’une version est prête pour la mise en production, elle est déplacée vers un environnement de production. Bien qu’il soit possible d’héberger des sites Web ASP.NET sur votre ordinateur personnel ou sur des serveurs de votre entreprise, de nombreuses entreprises et personnes choisissent d’externaliser leur hébergement sur un fournisseur d’hébergement Web.

Cette série de didacticiels examine les étapes de déploiement d’une application ASP.NET sur un fournisseur d’hébergement Web, en explorant les défis courants. Ce didacticiel vous a présenté une vue d’ensemble du processus de déploiement de ASP.NET et a donné des conseils pour trouver un fournisseur d’hébergement Web approprié. Le didacticiel suivant examine les fichiers liés à ASP.NET qui doivent être copiés dans l’environnement de production lors du déploiement de votre site Web.

Bonne programmation !

### <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Suivant](determining-what-files-need-to-be-deployed-cs.md)
