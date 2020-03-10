---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity : utilisation du stockage MySQL avec un fournisseur d’EntityFrameworkC#MySQL ()-ASP.net 4. x'
author: maumar
description: Ce didacticiel vous montre comment remplacer le mécanisme de stockage de données par défaut pour ASP.NET Identity avec EntityFramework (fournisseur SQL client) avec un provid MySQL...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: e89ed139657c5ce9ddcc56879946c62038919483
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584006"
---
# <a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity : utilisation du stockage MySQL avec un fournisseur MySQL d'C#EntityFramework ()

par [Maurycy Markowski](https://github.com/maumar), [Raquel Soares de Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Ce didacticiel vous montre comment remplacer le mécanisme de stockage de données par défaut pour [**ASP.net Identity**](introduction-to-aspnet-identity.md) avec EntityFramework (fournisseur SQL client) avec un fournisseur MySQL.

Les rubriques suivantes sont traitées dans ce didacticiel :

- Création d’une base de données MySQL sur Azure
- Création d’une application MVC à l’aide d’Visual Studio 2013 modèle MVC
- Configuration d’EntityFramework pour fonctionner avec un fournisseur de base de données MySQL
- Exécution de l’application pour vérifier les résultats

À la fin de ce didacticiel, vous disposerez d’une application MVC avec le magasin de ASP.NET Identity qui utilise une base de données MySQL hébergée dans Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Création d’une instance de base de données MySQL sur Azure

1. Connectez-vous au [portail Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Cliquez sur **nouveau** en bas de la page, puis sélectionnez **stocker**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. Dans l’Assistant **choisir et ajouter** , sélectionnez **base de données ClearDB MySQL**, puis cliquez sur la flèche **suivant** en bas du cadre :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Conservez le plan **gratuit** par défaut, changez le **nom** en **IdentityMySQLDatabase**, sélectionnez la région la plus proche de vous, puis cliquez sur la flèche **suivant** en bas du cadre :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Cliquez sur la coche d' **achat** pour terminer la création de la base de données.

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Une fois la base de données créée, vous pouvez la gérer à partir de l’onglet **ADD-ONS** du portail de gestion. Pour récupérer les informations de connexion de votre base de données, cliquez sur **informations de connexion** en bas de la page :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copiez la chaîne de connexion en cliquant sur le bouton Copier dans le champ **CONNECTIONSTRING** et enregistrez-la. vous utiliserez ces informations plus loin dans ce didacticiel pour votre application MVC :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Création d’un projet d’application MVC

Pour effectuer les étapes de cette section du didacticiel, vous devez d’abord installer [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou le [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Une fois Visual Studio installé, procédez comme suit pour créer un projet d’application MVC :

1. Ouvrez Visual Studio 2103.
2. Cliquez sur **nouveau projet** dans la page de **démarrage** , ou cliquez sur le menu **fichier** , puis sur **nouveau projet**:

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Quand la boîte de dialogue **nouveau projet** s’affiche, développez **visuel C#**  dans la liste des modèles, cliquez sur **Web**, puis sélectionnez **application Web ASP.net**. Nommez votre projet **IdentityMySQLDemo** , puis cliquez sur **OK**:

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez **MVC** templatewith les options par défaut. Cela permet de configurer **des comptes d’utilisateur individuels** comme méthode d’authentification. Cliquez sur **OK** :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configurer EntityFramework pour utiliser une base de données MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Mettre à jour l’assembly Entity Framework pour votre projet

L’application MVC créée à partir du modèle Visual Studio 2013 contient une référence au package [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) , mais des mises à jour ont été apportées à cet assembly depuis sa publication, ce qui inclut des améliorations significatives en matière de performances. Pour pouvoir utiliser ces dernières mises à jour dans votre application, procédez comme suit.

1. Ouvrez votre projet MVC dans Visual Studio.
2. Cliquez sur **Outils**, puis sur **Gestionnaire de package NuGet**, puis sur **console du gestionnaire de package**:

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. La **console du gestionnaire de package** s’affiche dans la partie inférieure de Visual Studio. Tapez &quot;**Update-Package EntityFramework**&quot; et appuyez sur entrée :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Installer le fournisseur MySQL pour EntityFramework

Pour permettre à EntityFramework de se connecter à la base de données MySQL, vous devez installer un fournisseur MySQL. Pour ce faire, ouvrez la **console du gestionnaire de package** et tapez &quot;**Install-Package MySQL. Data. Entity-pre**&quot;, puis appuyez sur entrée.

> [!NOTE]
> Il s’agit d’une version préliminaire de l’assembly et, par conséquent, peut contenir des bogues. Vous ne devez pas utiliser une version préliminaire du fournisseur en production.

[Cliquez sur l’image suivante pour la développer.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Modification de la configuration du projet dans le fichier Web. config de votre application

Dans cette section, vous allez configurer le Entity Framework pour utiliser le fournisseur MySQL que vous venez d’installer, inscrire la fabrique de fournisseurs MySQL et ajouter votre chaîne de connexion à partir d’Azure.

> [!NOTE]
> Les exemples suivants contiennent une version d’assembly spécifique pour MySql. Data. dll. Si la version de l’assembly change, vous devrez modifier les paramètres de configuration appropriés avec la version correcte.

1. Ouvrez le fichier Web. config de votre projet dans Visual Studio 2013.
2. Recherchez les paramètres de configuration suivants, qui définissent le fournisseur de base de données par défaut et la fabrique pour l’Entity Framework :

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Remplacez ces paramètres de configuration par le code suivant, qui configurera les Entity Framework pour utiliser le fournisseur MySQL :

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Recherchez la section &lt;connectionStrings&gt; et remplacez-la par le code suivant, qui définit la chaîne de connexion pour votre base de données MySQL hébergée sur Azure (Notez que la valeur providerName a également été modifiée par rapport à l’original) :

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Ajout d’un contexte MigrationHistory personnalisé

Entity Framework Code First utilise une table **MigrationHistory** pour effectuer le suivi des modifications de modèle et garantir la cohérence entre le schéma de base de données et le schéma conceptuel. Toutefois, cette table ne fonctionne pas pour MySQL par défaut, car la clé primaire est trop volumineuse. Pour remédier à cette situation, vous devrez réduire la taille de la clé pour cette table. Pour ce faire, procédez comme suit :

1. Les informations de schéma de cette table sont capturées dans un **HistoryContext**, qui peut être modifié comme n’importe quel autre **DbContext**. Pour ce faire, ajoutez un nouveau fichier de classe nommé **MySqlHistoryContext.cs** au projet, puis remplacez son contenu par le code suivant :

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Ensuite, vous devez configurer Entity Framework pour utiliser le **HistoryContext**modifié, plutôt que celui par défaut. Pour ce faire, vous pouvez tirer parti des fonctionnalités de configuration basées sur le code. Pour ce faire, ajoutez un nouveau fichier de classe nommé **MySqlConfiguration.cs** à votre projet et remplacez son contenu par :

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Création d’un initialiseur EntityFramework personnalisé pour ApplicationDbContext

Le fournisseur MySQL qui est proposé dans ce didacticiel ne prend pas actuellement en charge les migrations de Entity Framework. vous devrez donc utiliser des initialiseurs de modèle pour vous connecter à la base de données. Étant donné que ce didacticiel utilise une instance MySQL sur Azure, vous devrez créer un initialiseur de Entity Framework personnalisé.

> [!NOTE]
> Cette étape n’est pas requise si vous vous connectez à une instance de SQL Server sur Azure ou si vous utilisez une base de données hébergée localement.

Pour créer un initialiseur de Entity Framework personnalisé pour MySQL, procédez comme suit :

1. Ajoutez un nouveau fichier de classe nommé **MySqlInitializer.cs** au projet, puis remplacez son contenu par le code suivant :

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Ouvrez le fichier **IdentityModels.cs** de votre projet, qui se trouve dans le répertoire **Models** et remplacez son contenu par ce qui suit :

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Exécution de l’application et vérification de la base de données

Une fois que vous avez effectué les étapes décrites dans les sections précédentes, vous devez tester votre base de données. Pour ce faire, procédez comme suit :

1. Appuyez sur **CTRL + F5** pour générer et exécuter l’application Web.
2. Cliquez sur l’onglet **inscrire** en haut de la page :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Entrez un nouveau nom d’utilisateur et un nouveau mot de passe, puis cliquez sur **s’inscrire**:

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. À ce stade, les tables de ASP.NET Identity sont créées sur la base de données MySQL, et l’utilisateur est inscrit et connecté à l’application :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Installation de MySQL Workbench Tool pour vérifier les données

1. Installer l’outil **MySQL Workbench** à partir de la [page téléchargements MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Dans l’Assistant Installation : onglet **sélection des fonctionnalités** , sélectionnez **MySQL Workbench** sous la section **applications** .
3. Lancez l’application et ajoutez une nouvelle connexion à l’aide des données de chaîne de connexion de la base de données Azure MySQL que vous avez créée à l’bâton de ce didacticiel.
4. Après avoir établi la connexion, examinez les tables de **ASP.net Identity** créées sur le **IdentityMySQLDatabase.**
5. Vous verrez que toutes les ASP.NET Identity tables requises sont créées comme indiqué dans l’image ci-dessous :

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Inspectez la table **aspnetusers** pour rechercher des entrées lorsque vous inscrivez de nouveaux utilisateurs.

   [Cliquez sur l’image suivante pour la développer. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
