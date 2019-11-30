---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Mise en cache distribuée (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 07/20/2015
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: c66187b990a828c53bd2f8115e3c9660fc6022ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582808"
---
# <a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Mise en cache distribuée (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Le chapitre précédent a examiné la gestion des erreurs temporaires et la mise en cache mentionnée comme une stratégie de disjoncteur. Ce chapitre fournit plus d’informations sur la mise en cache, y compris le moment où l’utiliser, les modèles courants d’utilisation et la façon de l’implémenter dans Azure.

## <a name="what-is-distributed-caching"></a>Présentation de la mise en cache distribuée

Un cache fournit un accès à débit élevé et à faible latence aux données d’application fréquemment utilisées, en stockant les données en mémoire. Pour une application Cloud, le type de cache le plus utile est le cache distribué, ce qui signifie que les données ne sont pas stockées sur la mémoire de chaque serveur Web, mais sur d’autres ressources Cloud, et que les données mises en cache sont mises à la disposition de tous les serveurs Web d’une application (ou d’autres machines virtuelles Cloud e utilisé par l’application).

![Diagramme montrant plusieurs serveurs Web accédant aux mêmes serveurs de cache](distributed-caching/_static/image1.png)

Lorsque l’application est mise à l’échelle en ajoutant ou en supprimant des serveurs, ou lorsque des serveurs sont remplacés en raison de mises à niveau ou d’erreurs, les données mises en cache restent accessibles à chaque serveur qui exécute l’application.

En évitant l’accès aux données à latence élevée d’un magasin de données persistant, la mise en cache peut améliorer considérablement la réactivité de l’application. Par exemple, la récupération de données à partir du cache est beaucoup plus rapide que la récupération à partir d’une base de données relationnelle.

Un avantage secondaire de la mise en cache est la réduction du trafic vers la Banque de données persistante, ce qui peut entraîner des coûts inférieurs lorsqu’il existe des frais de sortie de données pour la Banque de données persistante.

## <a name="when-to-use-distributed-caching"></a>Quand utiliser la mise en cache distribuée

La mise en cache fonctionne mieux pour les charges de travail d’application qui effectuent plus de lectures que l’écriture de données, et lorsque le modèle de données prend en charge l’organisation clé/valeur que vous utilisez pour stocker et récupérer des données dans le cache. Elle est également plus utile lorsque les utilisateurs d’applications partagent un grand nombre de données communes. par exemple, le cache ne fournit pas autant d’avantages que si chaque utilisateur récupère généralement des données propres à cet utilisateur. Un catalogue de produits est un exemple où la mise en cache peut être très avantageuse, car les données ne changent pas fréquemment et que tous les clients examinent les mêmes données.

L’avantage de la mise en cache devient de plus en plus mesurable, tandis que les limites de débit et de latence du magasin de données persistant deviennent plus limitées en termes de performances globales de l’application. Toutefois, vous pouvez implémenter la mise en cache pour d’autres raisons que les performances. Pour les données qui ne doivent pas être parfaitement à jour lorsqu’elles sont affichées à l’utilisateur, l’accès au cache peut servir de disjoncteur lorsque la Banque de données persistante ne répond pas ou n’est pas disponible.

## <a name="popular-cache-population-strategies"></a>Stratégies de remplissage de cache populaires

Pour être en mesure de récupérer des données à partir du cache, vous devez d’abord les stocker. Il existe plusieurs stratégies pour obtenir les données dont vous avez besoin dans un cache :

- À la demande/mise en cache

    L’application tente de récupérer les données du cache et, lorsque le cache n’a pas de données (« manquant »), l’application stocke les données dans le cache afin qu’elles soient disponibles la prochaine fois. La prochaine fois que l’application tentera d’obtenir les mêmes données, elle trouvera ce qu’elle recherche dans le cache (un « accès »). Pour empêcher l’extraction des données mises en cache qui ont été modifiées sur la base de données, vous invalidez le cache lors de l’apport de modifications au magasin de données.
- Push de données en arrière-plan

    Les services d’arrière-plan transmettent les données dans le cache à intervalles réguliers et l’application extrait toujours du cache. Cette approche fonctionne bien avec les sources de données à latence élevée qui ne nécessitent pas que vous reveniez toujours aux données les plus récentes.
- Disjoncteur

    Normalement, l’application communique directement avec le magasin de données persistant, mais lorsque le magasin de données persistant présente des problèmes de disponibilité, l’application récupère les données du cache. Les données ont peut-être été placées dans le cache à l’aide de la stratégie de transmission de données de cache ou d’arrière-plan. Il s’agit d’une stratégie de gestion des erreurs plutôt qu’une stratégie d’amélioration des performances.

Afin de conserver les données en cache, vous pouvez supprimer les entrées de cache associées lorsque votre application crée, met à jour ou supprime des données. Si, pour votre application, vous pouvez parfois obtenir des données légèrement obsolètes, vous pouvez vous appuyer sur un délai d’expiration configurable pour définir une limite sur la manière dont les anciennes données de cache peuvent être définies.

Vous pouvez configurer l’expiration absolue (durée depuis la création de l’élément de cache) ou l’expiration décalée (durée depuis la dernière tentative d’accès à un élément de cache). L’expiration absolue est utilisée lorsque vous dépendez du mécanisme d’expiration du cache pour empêcher les données de devenir trop obsolètes. Dans l’application Fix it, nous supprimerons manuellement les éléments de cache obsolètes et nous utiliserons l’expiration décalée pour conserver les données les plus récentes dans le cache. Quelle que soit la stratégie d’expiration choisie, le cache supprime automatiquement les éléments les plus anciens (les moins récemment utilisés ou LRU) lorsque la limite de mémoire du cache est atteinte.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Exemple de code de mise en cache pour l’application Fix it

Dans l’exemple de code suivant, nous vérifions d’abord le cache lors de la récupération d’une tâche Fix it. Si la tâche est trouvée dans le cache, nous la retournons ; s’il est introuvable, nous l’obtenons de la base de données et la stockons dans le cache. Les modifications apportées pour ajouter la mise en cache à la méthode `FindTaskByIdAsync` sont mises en surbrillance.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Lorsque vous mettez à jour ou supprimez une tâche Fix it, vous devez invalider (supprimer) la tâche mise en cache. Dans le cas contraire, les futures tentatives de lecture de cette tâche continueront d’obtenir les anciennes données du cache.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Voici des exemples pour illustrer le code de mise en cache simple ; la mise en cache n’a pas été implémentée dans le projet de correctif téléchargeable.

## <a name="azure-caching-services"></a>Services de mise en cache Azure

Azure propose les services de mise en cache suivants : le [cache redims Azure](https://msdn.microsoft.com/library/dn690523.aspx) et le [cache géré Azure](https://msdn.microsoft.com/library/dn386094.aspx). Le cache Redims Azure est basé sur le célèbre [cache ReDim Open source](http://redis.io/) et constitue le premier choix pour la plupart des scénarios de mise en cache.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>État de session ASP.NET à l’aide d’un fournisseur de cache

Comme mentionné dans le [chapitre sur les meilleures pratiques pour le développement Web](web-development-best-practices.md), il est recommandé d’éviter d’utiliser l’état de session. Si votre application requiert un état de session, la meilleure pratique consiste à éviter le fournisseur en mémoire par défaut, car il n’active pas la montée en charge (plusieurs instances du serveur Web). Le fournisseur d’état de session ASP.NET SQL Server permet à un site qui s’exécute sur plusieurs serveurs Web d’utiliser l’état de session, mais il entraîne un coût de latence élevé par rapport à un fournisseur en mémoire. La meilleure solution si vous devez utiliser l’état de session consiste à utiliser un fournisseur de cache, tel que le [fournisseur d’état de session pour le cache Azure](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Récapitulatif

Vous avez vu comment l’application Fix it pouvait implémenter la mise en cache afin d’améliorer le temps de réponse et l’évolutivité, et de permettre à l’application de continuer à être réactive pour les opérations de lecture lorsque la base de données n’est pas disponible. Dans le [chapitre suivant](queue-centric-work-pattern.md) , nous allons vous montrer comment améliorer l’évolutivité et rendre l’application toujours réactive pour les opérations d’écriture.

## <a name="resources"></a>Ressources

Pour plus d’informations sur la mise en cache, consultez les ressources suivantes.

Documentation

- [Cache Azure](https://msdn.microsoft.com/library/gg278356.aspx). Documentation officielle MSDN sur la mise en cache dans Azure.
- [Modèles et pratiques Microsoft-conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Voir Guide de mise en cache et modèle de mise en cache.
- [Failsafe : aide sur les architectures résilientes du Cloud](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Livre blanc de Marc Mercuri, Ulrich Homann et Andrew Townhill. Consultez la section relative à la mise en cache.
- [Meilleures pratiques pour la conception de services à grande échelle sur les services Cloud Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). S. Livre blanc en Mark SIMM et Michael thomassy. Consultez la section relative à la mise en cache distribuée.
- [Mise en cache distribuée sur le chemin d’accès à l’évolutivité](https://msdn.microsoft.com/magazine/dd942840.aspx). Un ancien magazine MSDN (2009), mais une présentation clairement écrite de la mise en cache distribuée en général ; va plus loin que les sections de la mise en cache des livres blancs sur les méthodes de secours et les meilleures pratiques.

Vidéos

- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série en neuf parties de Ulrich Homann, Marc Mercuri et Mark SIMM. Présente une vue de niveau 400 de l’architecture des applications Cloud. Cette série porte sur la théorie et les raisons ; Pour plus d’informations, consultez la page création d’une série de mémoires virtuelles de marque. Consultez la discussion sur la mise en cache dans épisode 3 à partir de 1:24:14.
- [Développement Big : leçons apprises par les clients Azure, partie I](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies discute de la mise en cache distribuée à partir de 46:00. Semblable à la série Failsafe, mais présente des détails supplémentaires. La présentation a été fournie le 31 octobre 2012. elle ne couvre donc pas le service de mise en cache de Web Apps dans Azure App Service qui a été introduite dans 2013.

Exemple de code

- [Notions de base du service Cloud dans Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application qui implémente la mise en cache distribuée. Consultez le billet de blog qui l’accompagne [notions de base du service Cloud-principes de base de la mise en cache](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Précédent](transient-fault-handling.md)
> [Suivant](queue-centric-work-pattern.md)
