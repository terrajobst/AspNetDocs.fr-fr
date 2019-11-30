---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Stratégies de partitionnement des données (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 2f79b1f459aff3e81dab7ea7eb4ebf3f71084463
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585814"
---
# <a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Stratégies de partitionnement des données (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur la série, consultez [le premier chapitre](introduction.md).

Plus haut, nous avons vu combien il est facile de mettre à l’échelle le niveau Web d’une application Cloud en ajoutant et en supprimant des serveurs Web. Mais s’ils atteignent tous le même magasin de données, le goulet d’étranglement de votre application se déplace du serveur frontal vers le serveur principal, et la couche données est la plus difficile à mettre à l’échelle. Dans ce chapitre, nous examinons comment vous pouvez adapter votre niveau de données en partitionnant les données en plusieurs bases de données relationnelles, ou en combinant le stockage de base de données relationnelle avec d’autres options de stockage de données.

La configuration d’un schéma de partitionnement est effectuée au mieux avant pour la même raison mentionnée précédemment : il est très difficile de modifier votre stratégie de stockage des données une fois qu’une application est en production. Si vous envisagez de faire face à des approches différentes, vous pouvez éviter d’avoir un « moment Twitter » lorsque votre application se bloque ou tombe en panne pendant un certain temps lorsque vous réorganisez les données et le code d’accès aux données de votre application.

## <a name="the-three-vs-of-data-storage"></a>Les trois différences entre le stockage de données

Pour déterminer si vous avez besoin d’une stratégie de partitionnement et de ce qu’elle doit être, posez-vous trois questions sur vos données :

- Volume : quelle quantité de données finira-elle de stocker ? Deux gigaoctets ? Deux centaines de gigaoctets ? Téraoctets? Voire?
- Vélocité : quelle est la vitesse à laquelle vos données vont croître ? S’agit-il d’une application interne qui ne génère pas beaucoup de données ? Une application externe dans laquelle les clients vont télécharger des images et des vidéos ?
- Variété : quel type de données allez-vous stocker ? Les graphiques relationnels, les images, les paires clé-valeur et les graphiques sociaux ?

Si vous pensez que vous aurez beaucoup de volume, de vélocité ou de variété, vous devez réfléchir soigneusement au type de schéma de partitionnement qui permettra à votre application de se mettre à l’échelle de manière efficace et efficace à mesure qu’elle augmente, et de vous assurer que vous ne rencontrez aucun goulot d’étranglement.

Il existe essentiellement trois approches de partitionnement :

- Partitionnement vertical
- Partitionnement horizontal
- Partitionnement hybride

## <a name="vertical-partitioning"></a>Partitionnement vertical

La séparation verticale revient à fractionner une table par colonne : un ensemble de colonnes est placé dans un magasin de données, et un autre ensemble de colonnes est placé dans un magasin de données différent.

Par exemple, supposons que mon application stocke des données sur des personnes, y compris des images :

![Table de données](data-partitioning-strategies/_static/image1.png)

Lorsque vous représentez ces données sous la forme d’une table et que vous examinez les différentes variétés de données, vous pouvez voir que les trois colonnes de gauche possèdent des données de chaîne qui peuvent être stockées efficacement par une base de données relationnelle, tandis que les deux colonnes à droite sont essentiellement des tableaux d’octets qui c OME à partir des fichiers image. Il est possible de stocker des données de fichier image dans une base de données relationnelle, et de nombreuses personnes font cela, car elles ne veulent pas enregistrer les données dans le système de fichiers. Il se peut qu’ils ne disposent pas d’un système de fichiers permettant de stocker les volumes de données requis ou qu’ils ne souhaitent pas gérer un système de sauvegarde et de restauration distinct. Cette approche fonctionne bien pour les bases de données locales et pour les petites quantités de données dans les bases de données Cloud. Dans l’environnement local, il peut être plus facile de laisser le DBA s’occuper de tout.

Toutefois, dans une base de données Cloud, le stockage est relativement onéreux et un volume élevé d’images peut augmenter la taille de la base de données au-delà des limites auxquelles elle peut fonctionner efficacement. Vous pouvez résoudre ces problèmes en partitionnant les données verticalement, ce qui signifie que vous choisissez le magasin de données le plus approprié pour chaque colonne de votre table de données. Ce qui peut être le mieux adapté à cet exemple consiste à placer les données de chaîne dans une base de données relationnelle et les images dans le stockage d’objets BLOB.

![Table de données partitionnée verticalement](data-partitioning-strategies/_static/image2.png)

Le stockage d’images dans le stockage d’objets BLOB au lieu d’une base de données est plus pratique dans le Cloud que dans un environnement local, car vous n’avez pas à vous soucier de la configuration des serveurs de fichiers ou de la gestion de la sauvegarde et de la restauration des données stockées en dehors de la base de données relationnelle : tout qui est géré automatiquement par le service de stockage d’objets BLOB.

Il s’agit de l’approche de partitionnement que nous avons implémentée dans l’application Fix it. nous examinerons le code correspondant dans le [chapitre stockage d’objets BLOB](unstructured-blob-storage.md). Sans ce schéma de partitionnement, et en supposant une taille d’image moyenne de 3 mégaoctets, l’application Fix it ne peut stocker que environ 40 000 tâches avant d’atteindre la taille de base de données maximale de 150 gigaoctets. Après avoir supprimé les images, la base de données peut stocker 10 fois plus de tâches ; vous pouvez aller plus longtemps avant de devoir envisager d’implémenter un schéma de partitionnement horizontal. Au fur et à mesure que l’application évolue, vos dépenses augmentent plus lentement, car la majeure partie de vos besoins de stockage est dans un stockage d’objets BLOB très peu coûteux.

## <a name="horizontal-partitioning-sharding"></a>Partitionnement horizontal (partitionnement)

La segmentation horizontale revient à fractionner une table par lignes : un ensemble de lignes est placé dans un magasin de données, et un autre ensemble de lignes est placé dans un magasin de données différent.

Étant donné le même jeu de données, une autre option consiste à stocker différentes plages de noms de clients dans différentes bases de données.

![Table de données partitionnée horizontalement](data-partitioning-strategies/_static/image3.png)

Vous souhaitez être très attentif à votre schéma partitionnement pour vous assurer que les données sont distribuées uniformément afin d’éviter les zones réactives. Cet exemple simple utilisant la première lettre du nom de famille ne répond pas à cette exigence, car de nombreux utilisateurs ont des noms qui commencent par certaines lettres courantes. Vous auriez pu atteindre des limitations de taille de table plus tôt que prévu, car certaines bases de données seraient très volumineuses, alors que la plupart resterait très petit.

L’un des côtés du partitionnement horizontal est qu’il peut être difficile d’effectuer des requêtes sur l’ensemble des données. Dans cet exemple, une requête doit créer jusqu’à 26 bases de données différentes pour obtenir toutes les données stockées par l’application.

## <a name="hybrid-partitioning"></a>Partitionnement hybride

Vous pouvez combiner le partitionnement vertical et horizontal. Par exemple, dans les exemples de données, vous pouvez stocker les images dans le stockage d’objets BLOB et partitionner horizontalement les données de chaîne.

![Table de données hybride partitionnée](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partitionnement d’une application de production

Conceptuellement, il est facile de voir comment un schéma de partitionnement fonctionne, mais n’importe quel schéma de partitionnement augmente la complexité du code et introduit de nombreuses complications que vous devez gérer. Si vous déplacez des images vers le stockage d’objets BLOB, que faites-vous lorsque le service de stockage est inactif ? Comment gérer la sécurité des objets BLOB ? Que se passe-t-il si la base de données et le stockage BLOB ne sont pas synchronisés ? Si vous êtes partitionnement, comment allez-vous gérer l’interrogation de toutes les bases de données ?

Les complications sont gérables tant que vous les planifiez avant de passer en production. De nombreuses personnes qui n’ont pas fait cela veulent qu’elles aient été plus tard. En moyenne, notre équipe CAT (Customer Advisory Team) obtient des appels téléphoniques paniqués sur une fois par mois à partir de clients dont les applications sont en cours d’exécution de façon très importante et qui n’ont pas effectué cette planification. Et, par exemple : «aide ! Je place tout dans un magasin de données unique et, dans 45 jours, je n’ai plus d’espace disponible sur le disque !» Et si vous avez beaucoup de logique métier intégrée à la façon dont vous accédez à votre magasin de données et que vous avez des clients qui utilisent votre application, il n’y a pas de temps pour faire un jour pendant la migration. Nous commençons par les efforts Hercule pour aider le client à partitionner ses données à la volée sans temps d’arrêt. C’est très passionnant et très effrayant, et ne vous intéresse pas si vous pouvez éviter ! Si vous envisagez de le faire, il est plus facile de l’intégrer à votre application si l’application croît plus tard.

## <a name="summary"></a>Récapitulatif

Un schéma de partitionnement efficace peut permettre à votre application Cloud de s’adapter à plusieurs pétaoctets de données dans le Cloud sans goulots d’étranglement. Et vous n’avez pas à payer au préalable pour des machines volumineuses ou une infrastructure étendue, comme vous le pourriez si vous exécutiez l’application dans un centre de données local. Dans le Cloud, vous pouvez augmenter la capacité de façon incrémentielle en fonction de vos besoins, et vous payez uniquement pour autant que vous l’utilisez.

Dans le [chapitre suivant](unstructured-blob-storage.md) , nous allons voir comment l’application Fix it implémente le partitionnement vertical en stockant des images dans le stockage d’objets BLOB.

## <a name="resources"></a>Ressources

Pour plus d’informations sur les stratégies de partitionnement, consultez les ressources suivantes.

Documentation :

- [Meilleures pratiques pour la conception de services à grande échelle sur les services Cloud Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Livre blanc en Mark SIMM et Michael thomassy.
- [Modèles et pratiques Microsoft-modèles de conception de Cloud](https://msdn.microsoft.com/library/dn568099.aspx). Consultez l’aide sur le partitionnement des données, modèle partitionnement.

Vidéos :

- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série en neuf parties de Ulrich Homann, Marc Mercuri et Mark SIMM. Présente des concepts de haut niveau et des principes architecturaux de manière très accessible et intéressante, avec des histoires tirées de l’expérience de l’équipe de conseil clientèle de Microsoft avec les clients réels. Consultez la discussion sur le partitionnement dans épisode 7.
- [Création de Big : leçons tirées des clients Windows Azure-1ère partie](https://channel9.msdn.com/Events/Build/2012/3-029). Mark SIMM traite des schémas de partitionnement, des stratégies partitionnement, de la mise en œuvre de partitionnement et de SQL Database fédérations, à partir de 19:49. Semblable à la série Failsafe, mais présente des détails supplémentaires.

Exemple de code :

- [Notions de base du service Cloud dans Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Exemple d’application qui comprend une base de données partitionnée. Pour obtenir une description du schéma partitionnement implémenté, consultez [dal – partitionnement de SGBDR](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) sur le blog Windows Azure.

> [!div class="step-by-step"]
> [Précédent](data-storage-options.md)
> [Suivant](unstructured-blob-storage.md)
