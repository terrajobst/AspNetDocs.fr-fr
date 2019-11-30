---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Stockage d’objets BLOB non structuré (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 2afd4b5cf640eb97080de7e5280409f5e5347731
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583622"
---
# <a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Stockage d’objets BLOB non structuré (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Dans le chapitre précédent, nous avons examiné les schémas de partitionnement et expliqué comment l’application Fix it stocke les images dans le service Azure Storage Blob, ainsi que d’autres données de tâche dans Azure SQL Database. Dans ce chapitre, nous allons approfondir le service BLOB et montrer comment il est implémenté dans le code de projet Fix it.

## <a name="what-is-blob-storage"></a>Qu’est-ce que le stockage BLOB ?

Le service Azure Storage Blob fournit un moyen de stocker des fichiers dans le Cloud. Le service BLOB présente un certain nombre d’avantages par rapport au stockage de fichiers dans un système de fichiers de réseau local :

- Elle est hautement évolutive. Un compte de stockage unique peut stocker [des centaines de téraoctets](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), et vous pouvez avoir plusieurs comptes de stockage. Certains des plus grands clients Azure stockent des centaines de pétaoctets. Microsoft SkyDrive utilise le stockage d’objets BLOB.
- Elle est durable. Chaque fichier que vous stockez dans le service BLOB est automatiquement sauvegardé.
- Il offre une haute disponibilité. Le [contrat SLA pour le stockage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promet 99,9% ou 99,99% de temps d’activité, en fonction de l’option de géo-redondance choisie.
- Il s’agit d’une fonctionnalité PaaS (Platform-as-a-service) d’Azure, ce qui signifie que vous pouvez simplement stocker et récupérer des fichiers, payer uniquement pour la quantité réelle de stockage que vous utilisez et Azure se charge automatiquement de la configuration et de la gestion de toutes les machines virtuelles et lecteurs de disque requis pour le services.
- Vous pouvez accéder au service BLOB à l’aide d’une API REST ou à l’aide d’une API de langage de programmation. Les kits de développement logiciel (SDK) sont disponibles pour .NET, Java, Ruby et d’autres.
- Lorsque vous stockez un fichier dans le service BLOB, vous pouvez facilement le rendre accessible via Internet.
- Vous pouvez sécuriser des fichiers dans le service BLOB pour qu’ils puissent être accessibles uniquement par les utilisateurs autorisés, ou vous pouvez fournir des jetons d’accès temporaires qui les rendent disponibles pour une personne uniquement pendant une période limitée.

Chaque fois que vous créez une application pour Azure et que vous souhaitez stocker un grand nombre de données dans un environnement local, dans des fichiers, tels que des images, des vidéos, des fichiers PDF, des feuilles de calcul, etc., pensez au service BLOB.

## <a name="creating-a-storage-account"></a>Création d’un compte de stockage

Pour commencer à utiliser le service BLOB, vous devez créer un compte de stockage dans Azure. Dans le portail, cliquez sur **nouveau** -- **Data Services** -- **stockage** -- **création rapide**, puis entrez une URL et un emplacement de centre de données. L’emplacement du centre de données doit être le même que votre application Web.

![Créer un compte de stockage](unstructured-blob-storage/_static/image1.png)

Sélectionnez la région primaire dans laquelle vous souhaitez stocker le contenu, et si vous choisissez l’option [géo-réplication](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) , Azure crée des réplicas de toutes vos données dans un centre de données différent dans une autre région du pays. Par exemple, si vous choisissez le centre de données ouest des États-Unis, lorsque vous stockez un fichier dans le centre de données des États-Unis de l’Ouest, mais dans l’arrière-plan, Azure le copie également dans l’un des centres de données des États-Unis. Si un incident se produit dans une région du pays, vos données sont toujours sûres.

Azure ne réplique pas les données dans les limites géo-politiques : Si votre emplacement principal est situé aux États-Unis, vos fichiers sont répliqués uniquement vers une autre région située aux États-Unis. Si votre emplacement principal est l’Australie, vos fichiers sont répliqués uniquement vers un autre centre de données en Australie.

Bien entendu, vous pouvez également créer un compte de stockage en exécutant des commandes à partir d’un script, comme nous l’avons vu précédemment. Voici une commande Windows PowerShell pour créer un compte de stockage :

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Une fois que vous avez un compte de stockage, vous pouvez immédiatement commencer à stocker des fichiers dans le service BLOB.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Utilisation du stockage d’objets BLOB dans l’application Fix it

L’application Fix it vous permet de télécharger des photos.

![Créer une tâche Fix it](unstructured-blob-storage/_static/image2.png)

Lorsque vous cliquez sur **créer le correctif**, l’application télécharge le fichier image spécifié et le stocke dans le service BLOB.

### <a name="set-up-the-blob-container"></a>Configurer le conteneur d’objets BLOB

Pour stocker un fichier dans le service BLOB, vous devez disposer d’un *conteneur* dans lequel le stocker. Un conteneur de service BLOB correspond à un dossier de système de fichiers. Les scripts de création de l’environnement que nous avons examinés dans le [chapitre automatiser tout](automate-everything.md) créent le compte de stockage, mais ils ne créent pas de conteneur. L’objectif de la méthode `CreateAndConfigure` de la classe `PhotoService` consiste donc à créer un conteneur s’il n’existe pas déjà. Cette méthode est appelée à partir de la méthode `Application_Start` dans *global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Le nom du compte de stockage et la clé d’accès sont stockés dans la collection `appSettings` du fichier *Web. config* , et le code dans la méthode `StorageUtils.StorageAccount` utilise ces valeurs pour créer une chaîne de connexion et établir une connexion :

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

La méthode `CreateAndConfigureAsync` crée ensuite un objet qui représente le service BLOB et un objet qui représente un conteneur (dossier) nommé « images » dans le service BLOB :

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Si un conteneur nommé « images » n’existe pas encore, ce qui est vrai la première fois que vous exécutez l’application sur un nouveau compte de stockage : le code crée le conteneur et définit des autorisations pour le rendre public. (Par défaut, les nouveaux conteneurs d’objets BLOB sont privés et sont accessibles uniquement aux utilisateurs qui ont l’autorisation d’accéder à votre compte de stockage.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Stocker la photo chargée dans le stockage d’objets BLOB

Pour télécharger et enregistrer le fichier image, l’application utilise une interface `IPhotoService` et une implémentation de l’interface dans la classe `PhotoService`. Le fichier *Photoservice.cs* contient tout le code de l’application Fix it qui communique avec le service BLOB.

La méthode de contrôleur MVC suivante est appelée quand l’utilisateur clique sur **créer la FixIt**. Dans ce code, `photoService` fait référence à une instance de la classe `PhotoService`, et `fixittask` fait référence à une instance de la classe d’entité `FixItTask` qui stocke les données d’une nouvelle tâche.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

La méthode `UploadPhotoAsync` de la classe `PhotoService` stocke le fichier téléchargé dans le service BLOB et retourne une URL qui pointe vers le nouvel objet BLOB.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Comme dans la méthode `CreateAndConfigure`, le code se connecte au compte de stockage et crée un objet qui représente le conteneur d’objets BLOB « images », à la différence près qu’il suppose que le conteneur existe déjà.

Ensuite, il crée un identificateur unique pour l’image à télécharger, en concaténant une nouvelle valeur GUID avec l’extension de fichier :

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Le code utilise ensuite l’objet de conteneur d’objets BLOB et le nouvel identificateur unique pour créer un objet BLOB, définit un attribut sur cet objet qui indique le type de fichier, puis utilise l’objet BLOB pour stocker le fichier dans le stockage d’objets BLOB.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Enfin, elle obtient une URL qui fait référence à l’objet BLOB. Cette URL est stockée dans la base de données et peut être utilisée dans les pages Web Fix it pour afficher l’image chargée.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Cette URL est stockée dans la base de données sous la forme de l’une des colonnes de la table FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Avec uniquement l’URL dans la base de données, et les images dans le stockage d’objets BLOB, l’application Fix it conserve la base de données petite, évolutive et peu coûteuse, tandis que les images sont stockées là où le stockage est bon marché et capable de gérer des téraoctets ou des pétaoctets. Un compte de stockage peut stocker des centaines de téraoctets de photos de réparation et vous payez uniquement ce que vous utilisez. Par conséquent, vous pouvez commencer à payer 9 cents pour le premier gigaoctet et ajouter d’autres images pour les cents par gigaoctet supplémentaire.

### <a name="display-the-uploaded-file"></a>Afficher le fichier chargé

L’application Fix It affiche le fichier image téléchargé lorsqu’il affiche les détails d’une tâche.

![Corriger les détails des tâches informatiques avec photo](unstructured-blob-storage/_static/image3.png)

Pour afficher l’image, l’ensemble de la vue MVC doit inclure la valeur `PhotoUrl` dans le code HTML envoyé au navigateur. Le serveur Web et la base de données n’utilisent pas de cycles pour afficher l’image, mais ils servent uniquement quelques octets à l’URL de l’image. Dans le code Razor suivant, `Model` fait référence à une instance de la classe d’entité `FixItTask`.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Si vous examinez le code HTML de la page qui s’affiche, vous voyez l’URL qui pointe directement vers l’image dans le stockage d’objets BLOB, comme suit :

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Récapitulatif

Vous avez vu comment l’application Fix it stocke des images dans le service BLOB et uniquement des URL d’image dans la base de données SQL. L’utilisation du service BLOB permet de conserver la base de données SQL beaucoup plus petite que ce qui serait le cas, ce qui permet une mise à l’échelle jusqu’à un nombre quasiment illimité de tâches et peut être effectuée sans écrire beaucoup de code.

Vous pouvez avoir des centaines de téraoctets dans un compte de stockage, et le coût de stockage est beaucoup moins onéreux que SQL Database stockage, à partir d’environ 3 cents par gigaoctet par mois, plus un coût de transaction réduit. Et gardez à l’esprit que vous ne payez pas pour la capacité maximale, mais uniquement pour le montant que vous stockez, donc votre application est prête à être mise à l’échelle, mais vous n’êtes pas facturé pour cette capacité supplémentaire.

Dans le [chapitre suivant](design-to-survive-failures.md) , nous parlerons de l’importance de rendre une application Cloud capable de gérer correctement les défaillances.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez les ressources suivantes :

- [Présentation du stockage d’objets BLOB Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog de Mike Wood.
- [Utilisation du service de stockage d’objets BLOB Azure dans .net](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentation officielle sur le site MicrosoftAzure.com. Brève introduction au stockage d’objets BLOB, suivie d’exemples de code illustrant la connexion au stockage d’objets BLOB, la création de conteneurs, le chargement et le téléchargement d’objets BLOB, etc.
- [Failsafe : création de services Cloud évolutifs et résilients](https://channel9.msdn.com/Series/FailSafe). Série de vidéos en neuf parties par Ulrich Homann, Marc Mercuri et Mark SIMM. Présente des concepts de haut niveau et des principes architecturaux de manière très accessible et intéressante, avec des histoires tirées de l’expérience de l’équipe de conseil clientèle de Microsoft avec les clients réels. Pour plus d’informations sur le service de stockage Azure et les objets BLOB, consultez épisode 5 à partir de 35:13.
- [Modèles et pratiques Microsoft-conseils Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consultez modèle de clé valet.

> [!div class="step-by-step"]
> [Précédent](data-partitioning-strategies.md)
> [Suivant](design-to-survive-failures.md)
