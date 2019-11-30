---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Options de stockage des données (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: f97d973d87db895441f813376d757a8a2e94b255
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585931"
---
# <a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Options de stockage des données (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

La plupart des personnes sont utilisées pour les bases de données relationnelles, et elles ont tendance à ignorer d’autres options de stockage de données lors de la conception d’une application Cloud. Le résultat peut être des performances non optimales, des dépenses élevées ou pire, car les bases de données [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (non relationnelles) peuvent gérer plus efficacement certaines tâches que les bases de données relationnelles. Lorsque les clients nous demandent de l’aide pour résoudre un problème de stockage de données critique, cela est souvent dû au fait qu’ils disposent d’une base de données relationnelle dans laquelle l’une des options NoSQL fonctionnait mieux. Dans ces situations, le client aurait été mieux adapté s’il avait implémenté la solution NoSQL avant de déployer l’application en production.

D’un autre côté, il serait également une erreur de supposer qu’une base de données NoSQL peut faire tout bien ou correctement. Il n’existe pas de meilleur choix de gestion des données pour toutes les tâches de stockage de données. différentes solutions de gestion des données sont optimisées pour différentes tâches. La plupart des applications Cloud réelles ont une variété de besoins en stockage de données et sont souvent mieux servies par une combinaison de plusieurs solutions de stockage de données.

L’objectif de ce chapitre est de vous offrir un sens plus large des options de stockage de données disponibles pour une application Cloud, ainsi que des conseils de base sur la façon de choisir ceux qui correspondent à votre scénario. Il est préférable de connaître les options à votre disposition et de réfléchir à leurs forces et faiblesses avant de développer une application. La modification des options de stockage des données dans une application de production peut être extrêmement difficile, par exemple la modification d’un moteur Jet alors que le plan est en vol.

## <a name="data-storage-options-on-azure"></a>Options de stockage des données sur Azure

Grâce au Cloud, il est relativement facile d’utiliser un grand nombre de magasins de données relationnels et NoSQL. Voici quelques-unes des plates-formes de stockage de données que vous pouvez utiliser dans Azure.

![](data-storage-options/_static/image1.png)

Le tableau ci-dessous présente quatre types de bases de données NoSQL :

- Les [bases de données clé/valeur](https://msdn.microsoft.com/library/dn313285.aspx#sec7) stockent un objet sérialisé unique pour chaque valeur de clé. Ils sont utiles pour stocker de gros volumes de données où vous souhaitez obtenir un élément pour une valeur de clé donnée et vous n’avez pas besoin d’interroger en fonction d’autres propriétés de l’élément.

    Le [stockage d’objets BLOB Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) est une base de données clé/valeur qui fonctionne comme le stockage de fichiers dans le Cloud, avec des valeurs de clés qui correspondent aux noms de dossiers et de fichiers. Vous récupérez un fichier par son nom de dossier et de fichier, et non par la recherche de valeurs dans le contenu du fichier.

    Le [stockage table Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) est également une base de données clé/valeur. Chaque valeur est appelée une *entité* (semblable à une ligne, identifiée par une clé de partition et une clé de ligne) et contient plusieurs *Propriétés* (similaires aux colonnes, mais toutes les entités d’une table ne doivent pas partager les mêmes colonnes). L’interrogation sur des colonnes autres que la clé est extrêmement inefficace et doit être évitée. Par exemple, vous pouvez stocker les données de profil utilisateur, avec une partition qui stocke les informations relatives à un seul utilisateur. Vous pouvez stocker des données telles que le nom d’utilisateur, le hachage de mot de passe, la date de naissance, etc. dans des propriétés distinctes d’une entité ou dans des entités distinctes de la même partition. Mais vous ne souhaitez pas interroger tous les utilisateurs ayant une plage de dates de naissance donnée, et vous ne pouvez pas exécuter une requête JOIN entre votre table de profil et une autre table. Le stockage table est plus évolutif et moins onéreux qu’une base de données relationnelle, mais il n’active pas les requêtes ou les jointures complexes.
- Les [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) sont des bases de données clé/valeur dans lesquelles les valeurs sont des *documents*. « Document » ici n’est pas utilisé dans le sens d’un document Word ou Excel, mais il s’agit d’une collection de valeurs et de champs nommés, qui peuvent être un document enfant. Par exemple, dans une table d’historique des commandes, un document de commande peut contenir les champs numéro de commande, date de commande et client ; et le champ Customer peut contenir des champs Name et Address. La base de données encode les données de champ dans un format tel que XML, YAML, JSON ou BSON ; ou elle peut utiliser du texte brut. Une fonctionnalité qui définit les bases de données de documents en dehors des bases de données clé/valeur est la capacité à interroger des champs non-clés et à définir des index secondaires pour rendre l’interrogation plus efficace. Cette fonctionnalité rend une base de données de documents plus adaptée aux applications qui doivent récupérer des données en fonction de critères plus complexes que la valeur de la clé de document. Par exemple, dans une base de données de documents historique des commandes client, vous pouvez interroger différents champs, tels que ID du produit, ID client, nom du client, etc. [MongoDB](http://www.mongodb.org/) est une base de données de documents populaires.
- Les [bases de données de familles de colonnes](https://msdn.microsoft.com/library/dn313285.aspx#sec9) sont des magasins de données clé/valeur qui vous permettent de structurer le stockage des données dans des collections de colonnes associées appelées familles de colonnes. Par exemple, une base de données de recensement peut avoir un groupe de colonnes pour le nom d’une personne (prénom, deuxième prénom, dernier), un groupe pour l’adresse de la personne et un groupe pour les informations de profil de la personne (DOB, sexe, etc.). La base de données peut ensuite stocker chaque famille de colonnes dans une partition distincte tout en conservant toutes les données d’une personne liée à la même clé. Vous pouvez ensuite lire toutes les informations de profil sans avoir à lire toutes les informations de nom et d’adresse. [Cassandra](http://cassandra.apache.org/) est une base de données de familles de colonnes courante.
- Les [bases de données de graphique](https://msdn.microsoft.com/library/dn313285.aspx#sec10) stockent les informations sous la forme d’une collection d’objets et de relations. L’objectif d’une base de données de graphiques est de permettre à une application d’effectuer efficacement des requêtes qui parcourent le réseau d’objets et les relations entre eux. Par exemple, les objets peuvent être des employés d’une base de données de ressources humaines, et vous pouvez souhaiter faciliter les requêtes telles que « rechercher tous les employés qui travaillent directement ou indirectement à Scott ». [Neo4j](http://www.neo4j.org/) est une base de données de graphiques populaire.

Par rapport aux bases de données relationnelles, les options NoSQL offrent une évolutivité et une rentabilité supérieures pour le stockage et l’analyse des données non structurées. Le compromis est qu’ils ne fournissent pas les fonctionnalités d’interrogation et d’intégrité des données relationnelles les plus performantes des bases de données relationnelles. NoSQL fonctionne bien pour les données de journal IIS, ce qui implique un volume élevé sans nécessiter de requêtes de jointure. NoSQL ne fonctionnera pas si bien pour les transactions bancaires, ce qui nécessite une intégrité absolue des données et implique de nombreuses relations avec d’autres données relatives au compte.

Il existe également une nouvelle catégorie de plateforme de base de données appelée [NewSQL](http://en.wikipedia.org/wiki/NewSQL) qui associe l’extensibilité d’une base de données NoSQL avec l’intégrité transactionnelle et la requête d’une base de données relationnelle. Les bases de données NewSQL sont conçues pour le stockage distribué et le traitement des requêtes, ce qui est souvent difficile à implémenter dans les bases de données « OldSQL ». [NuoDB](http://www.nuodb.com/) est un exemple de base de données NewSQL qui peut être utilisée sur Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop et MapReduce

Les volumes élevés de données que vous pouvez stocker dans des bases de données NoSQL peuvent être difficiles à analyser efficacement en temps voulu. Pour ce faire, vous pouvez utiliser un Framework comme [Hadoop](http://hadoop.apache.org/) qui implémente la fonctionnalité [MapReduce](http://en.wikipedia.org/wiki/MapReduce) . L’un des processus MapReduce est essentiellement le suivant :

- Limitez la taille des données qui doivent être traitées en ne sélectionnant dans le magasin de données que les données que vous devez réellement analyser. Par exemple, si vous souhaitez connaître la composition de votre base d’utilisateurs par année de naissance, vous ne devez sélectionner que des années de naissance en dehors de votre magasin de données de profil utilisateur.
- Scindez les données en parties et envoyez-les à différents ordinateurs pour traitement. L’ordinateur A calcule le nombre de personnes avec 1950-1959 dates, l’ordinateur B 1960-1969, etc. Ce groupe d’ordinateurs s’appelle un *cluster Hadoop*.
- Regroupez les résultats de chaque partie une fois le traitement des parties terminé. Vous disposez maintenant d’une liste relativement succincte du nombre de personnes pour chaque année de naissance et la tâche de calcul des pourcentages dans cette liste globale est gérable.

Sur Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) vous permet de traiter, d’analyser et d’obtenir de nouvelles informations à partir de Big Data à l’aide de la puissance de Hadoop. Par exemple, vous pouvez l’utiliser pour analyser les journaux du serveur Web :

- Activez la journalisation du serveur Web dans votre compte de stockage. Cela configure Azure pour écrire des journaux dans le service BLOB pour chaque requête HTTP adressée à votre application. Le service BLOB est fondamentalement un stockage de fichiers Cloud et s’intègre parfaitement avec HDInsight.

    ![Journaux dans le stockage d’objets BLOB](data-storage-options/_static/image2.png)
- À mesure que l’application obtient le trafic, les journaux IIS du serveur Web sont écrits dans le stockage d’objets BLOB.

    ![Journaux de serveur Web](data-storage-options/_static/image3.png)
- Dans le portail, cliquez sur **nouveau** - **Data Services** - **HDInsight** - **création rapide**, puis spécifiez un nom de cluster HDInsight, la taille du cluster (nombre de nœuds de données du cluster HDInsight) et un nom d’utilisateur et un mot de passe pour le cluster HDInsight.

    ![HDInsight](data-storage-options/_static/image4.png)

Vous pouvez maintenant configurer des tâches MapReduce pour analyser vos journaux et obtenir des réponses à des questions telles que :

- Quelles heures de la journée mon application obtient-elle le trafic le plus ou le moins élevé ?
- Dans quels pays provient mon trafic ?
- Quel est le revenu moyen des quartiers des zones à partir desquels mon trafic provient. (Il existe un jeu de données public qui vous donne un revenu de quartier par adresse IP, et vous pouvez le faire correspondre à l’adresse IP dans les journaux du serveur Web.)
- Comment les revenus des quartiers sont-ils corrélés à des pages ou des produits spécifiques du site ?

Vous pouvez ensuite utiliser les réponses à des questions telles que celles-ci pour cibler les publicités en fonction de la probabilité qu’un client s’intéresse ou souhaite acheter un produit particulier.

Comme expliqué dans le [chapitre automatiser tout](automate-everything.md), la plupart des fonctions que vous pouvez effectuer dans le portail peuvent être automatisées, et cela inclut la configuration et l’exécution de tâches d’analyse HDInsight. Un script HDInsight typique peut contenir les étapes suivantes :

- Approvisionnez un cluster HDInsight et liez-le à votre compte de stockage pour l’entrée de stockage d’objets BLOB.
- Chargez les exécutables de tâche MapReduce (fichiers. jar ou. exe) dans le cluster HDInsight.
- Soumettez un MapReduce qui stocke les données de sortie dans le stockage d’objets BLOB.
- Attendez la fin de la tâche.
- Supprimez le cluster HDInsight.
- Accédez à la sortie à partir du stockage d’objets BLOB.

En exécutant un script qui effectue tout cela, vous réduisez la durée pendant laquelle le cluster HDInsight est approvisionné, ce qui réduit vos coûts.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Plateforme en tant que service (PaaS) et infrastructure en tant que service (IaaS)

Les options de stockage de données répertoriées précédemment incluent les solutions PaaS (Platform-as-a-service) et IaaS (infrastructure-as-a-service). PaaS signifie que nous allons gérer l’infrastructure matérielle et logicielle et que vous utilisez simplement le service. SQL Database est une fonctionnalité PaaS d’Azure. Vous demandez des bases de données et, en arrière-plan, Azure installe et configure les machines virtuelles et configure les bases de données sur ces dernières. Vous n’avez pas d’accès direct aux machines virtuelles et vous n’avez pas à les gérer. IaaS vous permet de configurer, de configurer et de gérer les machines virtuelles qui s’exécutent dans notre infrastructure de centre de données, et de placer tout ce que vous souhaitez. Nous fournissons une galerie d’images de machine virtuelle préconfigurées pour les configurations de machine virtuelle courantes. Par exemple, vous pouvez installer des images de machine virtuelle préconfigurées pour Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle Database, etc.

Les solutions de données PaaS offertes par Azure sont les suivantes :

- Azure SQL Database (anciennement SQL Azure). Base de données relationnelle Cloud basée sur SQL Server.
- Stockage table Azure. Base de données NoSQL clé/valeur.
- Stockage d’objets BLOB Azure. Stockage de fichiers dans le Cloud.

Pour IaaS, vous pouvez exécuter tout ce que vous pouvez charger sur une machine virtuelle, par exemple :

- Bases de données relationnelles telles que SQL Server, Oracle, MySQL, SQL Compact, SQLite ou postgres.
- Magasins de données clé/valeur tels que memcached, Redims, Cassandra et Riak.
- Magasins de données de colonne tels que HBase.
- Documentez les bases de données telles que MongoDB, RavenDB et CouchDB.
- Bases de données de graphiques telles que neo4j.

![Options de stockage des données sur Azure](data-storage-options/_static/image5.png)

L’option IaaS vous offre des options de stockage de données presque illimitées, et beaucoup d’entre elles sont particulièrement faciles à utiliser, car vous pouvez créer des machines virtuelles à l’aide d’images préconfigurées. Par exemple, dans le portail de gestion, accédez à **machines virtuelles**, cliquez sur l’onglet **images** , puis sur Parcourir l’entrepôt de l' **ordinateur virtuel**.

![Parcourir l’entrepôt de machines virtuelles](data-storage-options/_static/image6.png)

Vous voyez alors une liste de [centaines d’images de machine virtuelle préconfigurées](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)et vous pouvez créer une machine virtuelle à partir d’une image disposant d’un système de gestion de base de données préinstallé, tel que MongoDB, Neo4J, ReDim, Cassandra ou CouchDB :

![MongoDB dans l’entrepôt de machines virtuelles](data-storage-options/_static/image7.png)

Azure rend les options de stockage des données IaaS aussi faciles à utiliser que possible, mais les offres PaaS présentent de nombreux avantages qui les rendent plus rentables et pratiques pour de nombreux scénarios :

- Vous n’êtes pas obligé de créer des machines virtuelles, vous utilisez simplement le portail ou un script pour configurer un magasin de données. Si vous souhaitez un magasin de données de 200 téraoctets, vous pouvez simplement cliquer sur un bouton ou exécuter une commande, et en secondes, il est prêt à être utilisé.
- Vous n’avez pas besoin de gérer ou de corriger les machines virtuelles utilisées par le service. Microsoft fait cela automatiquement pour vous. vous n’avez pas à vous soucier de la configuration de l’infrastructure pour la mise à l’échelle ou la haute disponibilité. Microsoft gère tout cela pour vous.
- Vous n’avez pas besoin d’acheter des licences. les frais de licence sont inclus dans les frais de service.
- Vous payez uniquement ce que vous utilisez.

Les options de stockage de données PaaS dans Azure incluent des offres par des fournisseurs tiers. Par exemple, vous pouvez choisir le [module complémentaire MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) dans le magasin Azure pour approvisionner une base de données MongoDB en tant que service.

## <a name="choosing-a-data-storage-option"></a>Choix d’une option de stockage de données

Aucune approche n’est adaptée à tous les scénarios. Si quelqu’un dit que cette technologie est la réponse, la première chose à demander est « quelle est la question ? », car les différentes solutions sont optimisées pour différents points. Le modèle relationnel présente des avantages définis ; C’est pourquoi il s’agit d’une solution trop longue. Mais il existe également des côtés de SQL qui peuvent être traités avec une solution NoSQL.

Souvent, nous voyons qu’il s’agit d’une approche de composition, dans laquelle vous utilisez SQL et NoSQL dans une solution unique. Même lorsque les gens disent qu’ils adoptent NoSQL, si vous explorez ce qu’ils font, vous trouvez souvent qu’ils utilisent plusieurs frameworks NoSQL différents : ils utilisent [CouchDB](http://wiki.apache.org/couchdb/Introduction)et les [ReDim](http://redis.io/), et [Riak](http://basho.com/riak/) pour des choses différentes. Même Facebook, qui utilise considérablement NoSQL, utilise différentes infrastructures NoSQL pour différentes parties du service. La flexibilité pour mélanger et faire correspondre les approches de stockage des données est l’un des aspects intéressants du Cloud, car il est facile d’utiliser plusieurs solutions de données et de les intégrer dans une même application.

Voici quelques questions à prendre en compte lorsque vous choisissez une approche :

| Sémantique des données | -Qu’est-ce que le stockage de données principal et la sémantique d’accès aux données (vous stockez des données relationnelles ou non structurées) ? Les données non structurées, telles que les fichiers multimédias, conviennent au stockage d’objets BLOB. une collection de données associées, telles que produits, inventaires, fournisseurs, commandes client, etc., s’adapte mieux à une base de données relationnelle. |
| --- | --- |
| Prise en charge des requêtes | -Est-il facile d’interroger les données ? -Quels types de questions peuvent être posés efficacement ? Les magasins de données de clé/valeur sont très efficaces pour obtenir une seule ligne en fonction d’une valeur de clé, mais pas si bonne pour les requêtes complexes. Pour un magasin de données de profil utilisateur dans lequel vous obtenez toujours les données pour un utilisateur particulier, un magasin de données de clés/valeurs peut fonctionner correctement. dans le cas d’un catalogue de produits dans lequel vous souhaitez obtenir différents regroupements basés sur différents attributs de produit, une base de données relationnelle peut fonctionner mieux. Les bases de données NoSQL peuvent stocker des volumes importants de données de manière efficace, mais vous devez structurer la base de données autour de la manière dont l’application interroge les données, ce qui rend les requêtes ad hoc plus difficiles à faire. Avec une base de données relationnelle, vous pouvez générer presque n’importe quel type de requête. |
| Projection fonctionnelle | -Les questions, les agrégations, etc., doivent-elles être exécutées côté serveur ? Si j’exécute SELECT COUNT (\*) à partir d’une table dans SQL, il effectue très efficacement tout le travail sur le serveur et retourne le nombre que je recherche. Si je souhaite effectuer le même calcul à partir d’un magasin de données NoSQL qui ne prend pas en charge l’agrégation, il s’agit d’une « requête non liée » inefficace qui expirera probablement. Même si la requête est réussie, je dois extraire toutes les données du serveur vers le client et compter les lignes sur le client. -Quels langages ou types d’expressions peuvent être utilisés ? Avec une base de données relationnelle, je peux utiliser SQL. Avec certaines bases de données NoSQL telles que le stockage table Azure, j’utiliserai [OData](http://www.odata.org/), et tout ce que je peux faire est filtrer sur la clé primaire et obtenir des projections (sélectionnez un sous-ensemble des champs disponibles). |
| Facilité d’évolutivité | -À quelle fréquence et de quelle manière les données doivent-elles être mises à l’échelle ? -La plateforme implémente-t-elle en mode natif la montée en puissance parallèle ? -À quel point il est facile d’ajouter/de supprimer de la capacité (taille et débit) ? Les bases de données relationnelles et les tables ne sont pas automatiquement partitionnées pour les rendre évolutives. elles sont donc difficiles à mettre à l’échelle au-delà de certaines limitations. Les magasins de données NoSQL comme le stockage table Azure partitionnent tous les éléments, et il n’y a quasiment aucune limite à l’ajout de partitions. Vous pouvez facilement mettre à l’échelle le stockage de table jusqu’à 200 téraoctets, mais la taille de base de données maximale pour Azure SQL Database est de 500 gigaoctets. Vous pouvez mettre à l’échelle des données relationnelles en les partitionnant en plusieurs bases de données, mais la configuration d’une application pour prendre en charge ce modèle implique de nombreuses tâches de programmation. |
| Instrumentation et facilité de gestion | -Quelle est la facilité d’instrumentation, de surveillance et de gestion de la plateforme ? Vous devrez tenir informé de l’intégrité et des performances de votre magasin de données. vous devez donc connaître les mesures qu’une plateforme vous offre gratuitement et ce que vous devez développer vous-même. |
| Opérations | -Quelle est la facilité de déploiement et d’exécution de la plateforme sur Azure ? PaaS? IaaS? Linux? Le stockage de tables et les SQL Database sont faciles à configurer sur Azure. Les plateformes qui ne sont pas des solutions PaaS Azure intégrées nécessitent davantage d’efforts. |
| Prise en charge des API | -Est une API disponible qui facilite l’utilisation de la plateforme. Pour le service de table Azure, il existe un kit de développement logiciel (SDK) avec une API .NET qui prend en charge le modèle de programmation asynchrone .NET 4,5. Si vous écrivez une application .NET, il est beaucoup plus facile d’écrire et de tester le code pour le service de table Azure par rapport à une autre plateforme de magasin de données de colonne clé/valeur qui n’a pas d’API ou moins complète. |
| Intégrité transactionnelle et cohérence des données | -Est-il essentiel que la plateforme prenne en charge les transactions afin de garantir la cohérence des données ? Pour assurer le suivi des courriers électroniques envoyés, les performances et le faible coût de stockage des données peuvent être plus importants que la prise en charge automatique des transactions ou de l’intégrité référentielle dans la plate-forme de données, ce qui rend le service de table Azure un bon choix. Pour le suivi des soldes de compte bancaire ou des bons de commande, une plateforme de base de données relationnelle qui fournit des garanties transactionnelles fortes est un meilleur choix. |
| Continuité des activités | -Quelle est la simplicité de la sauvegarde, de la restauration et de la récupération d’urgence ? Les données de production tôt ou tard seront endommagées et vous aurez besoin d’une fonction d’annulation. Les bases de données relationnelles ont souvent des capacités de restauration affinées, telles que la possibilité de restaurer à un point dans le temps. La compréhension des fonctionnalités de restauration disponibles dans chaque plateforme que vous envisagez est un facteur important à prendre en compte. |
| Cost | -Si plusieurs plateformes peuvent prendre en charge la charge de travail de vos données, comment les comparer en coût ? Par exemple, si vous utilisez ASP.NET Identity, vous pouvez stocker les données de profil utilisateur dans le service de table Azure ou Azure SQL Database. Si vous n’avez pas besoin des fonctions d’interrogation enrichie de SQL Database, vous pouvez choisir des tables Azure en partie, car cela coûte beaucoup moins cher pour une quantité de stockage donnée. |

Nous recommandons généralement de connaître la réponse aux questions de chacune de ces catégories avant de choisir vos solutions de stockage de données.

En outre, votre charge de travail peut avoir des exigences spécifiques que certaines plateformes peuvent prendre en charge mieux que d’autres. Par exemple :

- Votre application nécessite-t-elle des fonctionnalités d’audit ?
- Quelles sont vos exigences en matière de longévité des données : avez-vous besoin de fonctionnalités d’archivage ou de purge automatiques ?
- Avez-vous des besoins spécifiques en matière de sécurité ? Par exemple, les données incluent des informations d’identification personnelle (personnellement identifiables), mais vous devez être en mesure de vous assurer que les informations d’identification personnelle sont exclues des résultats de la requête.
- Si certaines de vos données ne peuvent pas être stockées dans le Cloud pour des raisons réglementaires ou technologiques, vous aurez peut-être besoin d’une plateforme de stockage de données Cloud qui facilite l’intégration avec votre stockage local.

## <a name="demo--using-sql-database-in-azure"></a>Démonstration – utilisation de SQL Database dans Azure

L’application Fix It utilise une base de données relationnelle pour stocker des tâches. Le script Windows PowerShell de création de l’environnement présenté dans le [chapitre automatiser tout](automate-everything.md) crée deux instances de SQL Database. Vous pouvez les voir dans le portail en cliquant sur l’onglet **bases de données SQL** .

![Bases de données SQL dans le portail](data-storage-options/_static/image8.png)

Il est également facile de créer des bases de données à l’aide du portail.

Cliquez sur **nouveau--Data Services** -- **SQL Database** -- **création rapide**, entrez un nom de base de données, choisissez un serveur déjà présent dans votre compte ou créez-en un nouveau, puis cliquez sur **créer une SQL Database**.

![Nouvelle base de données SQL](data-storage-options/_static/image9.png)

Patientez quelques secondes, et une base de données dans Azure vous est prête à être utilisée.

![Nouveau SQL Database créé](data-storage-options/_static/image10.png)

Par conséquent, Azure effectue en quelques secondes, ce qui peut vous permettre de réaliser une journée ou une semaine ou plus dans l’environnement local. Et dans la mesure où vous pouvez facilement créer des bases de données automatiquement dans un script ou à l’aide d’une API de gestion, vous pouvez augmenter dynamiquement la puissance en répartissant vos données sur plusieurs bases de données, tant que votre application a été programmée pour cela.

Il s’agit d’un exemple de notre modèle Platform-as-a-service. Vous n’avez pas à gérer les serveurs, nous le faisons. Vous n’avez pas à vous soucier des sauvegardes, c’est ce que nous faisons. Il s’exécute en haute disponibilité : les données de la base de données sont répliquées automatiquement sur trois serveurs. Si un ordinateur meurt, nous basculons automatiquement et vous perdez aucune donnée. Le serveur est corrigé régulièrement, vous n’avez pas à vous soucier de cela.

Cliquez sur un bouton pour obtenir la chaîne de connexion exacte dont vous avez besoin et vous pouvez commencer immédiatement à utiliser la nouvelle base de données.

![Chaînes de connexion](data-storage-options/_static/image11.png)

Le tableau de bord affiche l’historique des connexions et la quantité de stockage utilisée.

![Tableau de bord SQL Database](data-storage-options/_static/image12.png)

Vous pouvez gérer des bases de données dans le portail ou à l’aide de SQL Server outils que vous connaissez déjà, notamment SQL Server Management Studio (SSMS) et Visual Studio Tools Explorateur d’objets SQL Server (SSOX) et Explorateur de serveurs.

![SSOX](data-storage-options/_static/image13.png)

Un autre aspect intéressant est le modèle de tarification. Vous pouvez commencer le développement avec une base de données gratuite de 20 Mo, et une base de données de production commence à environ $5 par mois. Vous payez uniquement pour la quantité de données que vous stockez dans la base de données, et non la capacité maximale. Vous n’avez pas besoin d’acheter une licence.

SQL Database est facile à mettre à l’échelle. Pour l’application Fix it, la base de données que nous créons dans notre script d’automatisation est limitée à 1 Go. Si vous souhaitez la mettre à l’échelle jusqu’à 150 Go, vous pouvez simplement accéder au portail et modifier ce paramètre, ou exécuter une commande API REST, et en quelques secondes, vous disposez d’une base de données 150 GIG dans laquelle vous pouvez déployer des données.

![Éditions et tailles de SQL Database](data-storage-options/_static/image14.png)

C’est la puissance du Cloud qui permet de mettre en place une infrastructure rapidement et facilement et de commencer à l’utiliser immédiatement.

L’application Fix It utilise deux bases de données SQL, l’une pour l’appartenance (authentification et autorisation) et l’autre pour les données, et c’est tout ce que vous devez faire pour le configurer et le mettre à l’échelle. Vous avez vu précédemment comment approvisionner les bases de données par le biais de scripts Windows PowerShell, et vous avez maintenant vu combien il est facile de le faire dans le portail.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework et accès direct à la base de données à l’aide de ADO.NET

L’application Fix it accède à ces bases de données à l’aide de la Entity Framework, ORM (Object-Relational Mapper) recommandée par Microsoft pour les applications .NET. Un ORM est un excellent outil qui facilite la productivité des développeurs, mais la productivité s’accompagne de la dégradation des performances dans certains scénarios. Dans une application Cloud réelle, vous ne faites pas le choix entre l’utilisation d’EF ou l’utilisation directe de ADO.NET. vous utiliserez les deux. La plupart du temps, lorsque vous écrivez du code qui fonctionne avec la base de données, obtenir des performances maximales n’est pas critique et vous pouvez tirer parti du codage et des tests simplifiés que vous obtenez avec la Entity Framework. Dans les situations où la surcharge EF entraînerait des performances inacceptables, vous pouvez écrire et exécuter vos propres requêtes à l’aide de ADO.NET, idéalement en appelant des procédures stockées.

Quelle que soit la méthode utilisée pour accéder à la base de données, vous souhaitez réduire autant que possible le « échanges excessifs ». En d’autres termes, si vous pouvez obtenir toutes les données dont vous avez besoin dans un jeu de résultats de requête plus large plutôt que des douzaines ou des centaines de plus petits, cela est généralement préférable. Par exemple, si vous avez besoin de répertorier les étudiants et les cours dans lesquels ils sont inscrits, il est généralement préférable d’obtenir toutes les données dans une seule requête de jointure, plutôt que d’obtenir les étudiants dans une requête et d’exécuter des requêtes distinctes pour chaque cours de l’étudiant.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Bases de données SQL et Entity Framework dans l’application Fix it

Dans l’application Fix it, la classe `FixItContext`, qui dérive de la classe `DbContext` Entity Framework, identifie la base de données et spécifie les tables dans la base de données. Le contexte spécifie un jeu d’entités (table) pour les tâches, et le code transmet au contexte le nom de la chaîne de connexion. Ce nom fait référence à une chaîne de connexion qui est définie dans le fichier Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

La chaîne de connexion dans le fichier *Web. config* est nommée appdb (ici pointant vers la base de données de développement local) :

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

L’Entity Framework crée une table *FixItTasks* basée sur les propriétés incluses dans la classe d’entité `FixItTask`. Il s’agit d’une simple classe POCO (Plain Old CLR Object), ce qui signifie qu’elle n’hérite pas de ou n’a aucune dépendance sur le Entity Framework. Mais Entity Framework sait comment créer une table basée sur elle et exécuter des opérations CRUD (Create-Read-Update-Delete).

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Table FixItTasks](data-storage-options/_static/image15.png)

L’application Fix it comprend une interface de référentiel qu’elle utilise pour les opérations CRUD qui fonctionnent avec le magasin de données.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Notez que les méthodes de référentiel sont toutes asynchrones, donc tous les accès aux données peuvent être effectués de manière totalement asynchrone.

L’implémentation du référentiel appelle Entity Framework méthodes Async pour travailler avec les données, y compris les requêtes LINQ, ainsi que pour les opérations d’insertion, de mise à jour et de suppression. Voici un exemple de code pour la recherche d’une tâche Fix it.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Vous remarquerez qu’il y a également du code de journalisation des erreurs et de la synchronisation dans le [chapitre de surveillance et de télémétrie](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Choix d’SQL Database (PaaS) par rapport à SQL Server dans une machine virtuelle (IaaS) dans Azure

Une chose intéressante concernant les SQL Server et les Azure SQL Database est que le modèle de programmation de base pour ces deux types d’éléments est identique. Vous pouvez utiliser la plupart des compétences des deux environnements. Vous pouvez même utiliser une base de données SQL Server en développement et une instance SQL Database dans le Cloud, qui est la configuration de l’application Fix it.

Vous pouvez également exécuter le même SQL Server dans le Cloud que vous exécutez localement en l’installant sur des machines virtuelles IaaS. Pour certaines applications héritées, l’exécution de SQL Server dans une machine virtuelle peut être une meilleure solution. Étant donné qu’une base de données SQL Server s’exécute sur une machine virtuelle dédiée, elle a plus de ressources disponibles qu’une base de données SQL Database qui s’exécute sur un serveur partagé. Cela signifie qu’une base de données SQL Server peut être plus grande et néanmoins fonctionner correctement. En général, plus la taille de la base de données et la taille de la table sont réduites, plus le cas d’utilisation fonctionne pour SQL Database (PaaS).

Voici quelques instructions sur la façon de choisir entre les deux modèles.

| Azure SQL Database (PaaS) | SQL Server sur une machine virtuelle (IaaS) |
| --- | --- |
| **Avantages** : vous n’êtes pas obligé de créer ou de gérer des machines virtuelles, de mettre à jour ou de corriger le système d’exploitation ou SQL ; Azure le fait pour vous. -Haute disponibilité intégrée, avec un contrat SLA au niveau de la base de données. -Coût total de possession (TCO) faible, car vous payez uniquement pour ce que vous utilisez (aucune licence requise). -Idéal pour gérer un grand nombre de bases de données plus petites (&lt;= 500 Go chacune). -Faciles à créer dynamiquement de nouvelles bases de données pour permettre la montée en puissance parallèle. | ***Avantages*** -fonctionnalités compatibles avec les SQL Server locaux. -Peut implémenter SQL Server [haute disponibilité via AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) dans plus de 2 machines virtuelles, avec un contrat SLA au niveau de la machine virtuelle. -Vous avez un contrôle total sur la façon dont SQL est géré. -Peut réutiliser des licences SQL que vous possédez déjà ou payer à l’heure pour une. -Idéal pour gérer des bases de données moins volumineuses (1 to +). |
| **Inconvénients** -certains écarts de fonctionnalités par rapport aux SQL Server locaux (absence d' [intégration du CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [prise en charge](https://technet.microsoft.com/library/cc280449.aspx)de la compression, [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), etc.)-limite de 500 Go pour la taille de la base de données. | Les mises à jour/correctifs ***cons*** -Updates (système d’exploitation et SQL) sont votre responsabilité : la création et la gestion des bases de données sont votre responsabilité-IOPS (opérations d’entrée/sortie par seconde) limitées à environ 8000 (via 16 lecteurs de données). |

Si vous souhaitez utiliser SQL Server sur une machine virtuelle, vous pouvez utiliser votre propre licence SQL Server, ou vous pouvez payer l’un après l’heure. Par exemple, dans le portail ou via l’API REST, vous pouvez créer une machine virtuelle à l’aide d’une image de SQL Server.

![Créer une machine virtuelle avec SQL Server](data-storage-options/_static/image16.png)

![Liste des images de machine virtuelle SQL Server](data-storage-options/_static/image17.png)

Lorsque vous créez une machine virtuelle avec une image SQL Server, nous évaluons le coût de la licence SQL Server par heure en fonction de votre utilisation de la machine virtuelle. Si vous avez un projet qui ne sera exécuté que pendant quelques mois, il est moins cher de payer à l’heure. Si vous pensez que votre projet va durer des années, il est moins onéreux d’acheter la licence comme vous le feriez normalement.

## <a name="summary"></a>Récapitulatif

Le Cloud Computing permet de combiner et de faire correspondre les approches de stockage des données pour répondre au mieux aux besoins de votre application. Si vous créez une nouvelle application, réfléchissez bien aux questions répertoriées ici afin de choisir des approches qui continueront à fonctionner correctement lorsque votre application se développe. Le [chapitre suivant](data-partitioning-strategies.md) explique certaines stratégies de partitionnement que vous pouvez utiliser pour combiner plusieurs approches de stockage de données.

## <a name="resources"></a>Ressources

Pour plus d'informations, voir les ressources ci-dessous.

Choix d’une plateforme de base de données :

- [Accès aux données pour des solutions hautement évolutives : utilisation de la persistance SQL, NoSQL et polyglotte](https://aka.ms/dag-doc). Livre électronique de modèles et de pratiques Microsoft qui va en profondeur dans les différents types de magasins de données disponibles pour les applications Cloud.
- [Modèles et pratiques Microsoft-conseils Azure](https://msdn.microsoft.com/library/ff898430.aspx). Consultez initiation à la cohérence des données, aide à la réplication et à la synchronisation des données, modèle de table d’index, modèle de vue matérialisée.
- [Base : solution de remplacement acid](http://queue.acm.org/detail.cfm?id=1394128). Article sur les compromis entre la cohérence des données et l’évolutivité.
- [Sept bases de données en sept semaines : Guide des bases de données modernes et mouvement NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Livrez par Eric Redmond et Jim R. Wilson. Vivement recommandée pour vous familiariser avec la gamme de plates-formes de stockage de données disponibles aujourd’hui.

Choix entre SQL Server et SQL Database :

- [Aperçu Premium pour obtenir de l’aide SQL Database](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Une introduction à SQL Database Premium et des conseils sur la façon de le choisir sur les éditions Web et Business de SQL Database.
- [Instructions et limitations (Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Page du portail qui renvoie à la documentation sur les limitations de SQL Database, y compris sur les fonctionnalités SQL Server que SQL Database ne prend pas en charge.
- [SQL Server dans les machines virtuelles Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Page du portail qui renvoie à la documentation relative à l’exécution de SQL Server dans Azure.
- [Scott Guthrie explique les bases de données SQL dans Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). présentation vidéo de 6 minutes à SQL Database par Scott Guthrie.
- [Modèles d’application et stratégies de développement pour les SQL Server dans les machines virtuelles Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Utilisation de Entity Framework et SQL Database dans une application Web ASP.NET

- [Prise en main avec EF 6 à l’aide de MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série de didacticiels en neuf parties qui vous guident dans la création d’une application MVC qui utilise EF et déploie la base de données sur Azure et SQL Database.
- [Déploiement Web ASP.net à l’aide de Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Série de didacticiels en 12 parties qui explique plus en détail comment déployer une base de données à l’aide d’EF Code First.
- [Déployez une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et SQL Database à un site Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Didacticiel pas à pas qui vous guide dans la création d’une application Web qui utilise l’authentification, stocke des tables d’application dans la base de données d’appartenance, modifie le schéma de la base de données et déploie l’application dans Azure.
- [Mappage de contenu d’accès aux données ASP.net](https://go.microsoft.com/fwlink/p/?LinkId=282414). Liens vers les ressources relatives à l’utilisation d’EF et de SQL Database.

Utilisation de MongoDB sur Azure :

- [MongoLab-MongoDB sur Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Page du portail pour obtenir de la documentation sur l’exécution de MongoDB sur Azure.
- [Créez un site Web Azure qui se connecte à MongoDB exécuté sur une machine virtuelle dans Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Didacticiel pas à pas qui montre comment utiliser une base de données MongoDB dans une application Web ASP.NET.

HDInsight (Hadoop sur Azure) :

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Portail vers HDInsight documentation sur le site Web [Azure](https://azure.microsoft.com/) .
- [Hadoop et HDInsight : Big Data dans Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Article de MSDN Magazine de Bruno Terkaly et Ricardo Villalobos, présentation de Hadoop sur Azure.
- [Modèles et pratiques Microsoft-conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez le modèle MapReduce.

> [!div class="step-by-step"]
> [Précédent](single-sign-on.md)
> [Suivant](data-partitioning-strategies.md)
