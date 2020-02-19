---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migration des données du fournisseur universel pour les profils d’appartenance etC#utilisateur vers ASP.net Identity ()-ASP.net 4. x
author: rustd
description: Ce didacticiel décrit les étapes nécessaires à la migration des données d’utilisateur et de rôle, ainsi que des données de profil utilisateur créées à l’aide de Fournisseurs universels d’une application existante...
ms.author: riande
ms.date: 12/13/2013
ms.custom: seoapril2019
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 31f02a0cec3c531c45c37b7aad8456e01e80b5ea
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456112"
---
# <a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migration des données du fournisseur universel pour l’appartenance et les profils utilisateur vers ASP.NET Identity (C#)

par [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> Ce didacticiel décrit les étapes nécessaires à la migration des données d’utilisateur et de rôle, ainsi que des données de profil utilisateur créées à l’aide de Fournisseurs universels d’une application existante vers le modèle de ASP.NET Identity. L’approche mentionnée ici pour migrer les données de profil utilisateur peut également être utilisée dans une application avec l’appartenance SQL.

Avec la publication de Visual Studio 2013, l’équipe ASP.NET a introduit un nouveau système ASP.NET Identity, et vous pouvez en savoir plus sur cette version [ici](../../index.md). À la suite de l’article pour migrer des applications Web de l' [appartenance SQL vers le nouveau système d’identité](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), cet article décrit les étapes de migration des applications existantes qui suivent le modèle de fournisseurs pour la gestion des utilisateurs et des rôles vers le nouveau modèle d’identité. Ce didacticiel se concentre principalement sur la migration des données de profil utilisateur pour les raccorder en toute transparence au nouveau système. La migration des informations d’utilisateur et de rôle est similaire pour l’appartenance SQL. L’approche suivie pour migrer des données de profil peut également être utilisée dans une application avec l’appartenance SQL.

Par exemple, nous allons commencer par une application Web créée à l’aide de Visual Studio 2012 qui utilise le modèle de fournisseurs. Nous allons ensuite ajouter du code pour la gestion des profils, inscrire un utilisateur, ajouter des données de profil pour les utilisateurs, migrer le schéma de base de données, puis modifier l’application pour utiliser le système d’identité pour la gestion des utilisateurs et des rôles. En guise de test de migration, les utilisateurs créés à l’aide de Fournisseurs universels doivent pouvoir se connecter et de nouveaux utilisateurs doivent être en mesure de s’inscrire.

> [!NOTE]
> Vous trouverez l’exemple complet sur [https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations).

## <a name="profile-data-migration-summary"></a>Résumé de la migration des données de profil

Avant de commencer avec les migrations, penchons-nous sur l’expérience de stockage des données de profil dans le modèle fournisseurs. Les données de profil pour les utilisateurs d’application peuvent être stockées de plusieurs façons, les plus fréquentes parmi celles qui utilisent les fournisseurs de profils intégrés fournis avec l’Fournisseurs universels. Les étapes sont les suivantes :

1. Ajoutez une classe qui possède des propriétés utilisées pour stocker des données de profil.
2. Ajoutez une classe qui étend « ProfileBase » et implémente des méthodes pour obtenir les données de profil ci-dessus pour l’utilisateur.
3. Activez l’utilisation des fournisseurs de profils par défaut dans le fichier *Web. config* et définissez la classe déclarée dans l’étape #2 à utiliser pour accéder aux informations de profil.

Les informations de profil sont stockées sous forme de données XML et binaires sérialisées dans la table « profiles » de la base de données.

Après la migration de l’application pour utiliser le nouveau système de ASP.NET Identity, les informations de profil sont désérialisées et stockées sous forme de propriétés sur la classe utilisateur. Chaque propriété peut ensuite être mappée sur les colonnes de la table utilisateur. L’avantage ici est que les propriétés peuvent être utilisées directement à l’aide de la classe utilisateur, en plus de ne pas avoir à sérialiser/désérialiser les informations de données à chaque fois qu’elle y accède.

## <a name="getting-started"></a>Mise en route

1. Créez une nouvelle application ASP.NET 4,5 Web Forms dans Visual Studio 2012. L’exemple actuel utilise le modèle Web Forms, mais vous pouvez également utiliser l’application MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Créer un nouveau dossier « Models » pour stocker les informations de profil  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. À titre d’exemple, laissez-nous stocker la date de naissance, la ville, la hauteur et le poids de l’utilisateur dans le profil. La hauteur et la pondération sont stockées sous la forme d’une classe personnalisée appelée « PersonalStats ». Pour stocker et récupérer le profil, nous avons besoin d’une classe qui étend’ProfileBase'. Créons une nouvelle classe « AppProfile » pour obtenir et stocker les informations de profil.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Activez le profil dans le fichier *Web. config* . Entrez le nom de la classe à utiliser pour stocker/récupérer les informations utilisateur créées à l’étape #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Ajoutez une page Web Forms dans le dossier « Account » pour obtenir les données de profil de l’utilisateur et les stocker. Cliquez avec le bouton droit sur le projet, puis sélectionnez Ajouter un nouvel élément. Ajoutez une nouvelle page Web Forms avec la page maître « AddProfileData. aspx ». Copiez ce qui suit dans la section « MainContent » :

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Ajoutez le code suivant dans le code-behind :

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Ajoutez l’espace de noms sous lequel la classe AppProfile est définie pour supprimer les erreurs de compilation.
6. Exécutez l’application et créez un nouvel utilisateur avec le nom d’utilisateur «**Olduser ».** Accédez à la page « AddProfileData » et ajoutez des informations de profil pour l’utilisateur.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Vous pouvez vérifier que les données sont stockées en tant que code XML sérialisé dans la table « profils » à l’aide de la fenêtre Explorateur de serveurs. Dans Visual Studio, dans le menu Affichage, choisissez « Explorateur de serveurs ». Il doit y avoir une connexion de données pour la base de données définie dans le fichier *Web. config* . En cliquant sur la connexion de données, vous affichez différentes sous-catégories. Développez « tables » pour afficher les différentes tables de votre base de données, cliquez avec le bouton droit sur « profils », puis choisissez « afficher les données de la table » pour afficher les données de profil stockées dans la table profils.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migration du schéma de base de données

Pour que la base de données existante fonctionne avec le système d’identité, nous devons mettre à jour le schéma dans la base de données d’identité afin de prendre en charge les champs que vous avez ajoutés à la base de données d’origine. Pour ce faire, vous pouvez utiliser des scripts SQL pour créer des tables et copier les informations existantes. Dans la fenêtre’Explorateur de serveurs', développez « DefaultConnection » pour afficher les tables. Cliquez avec le bouton droit sur tables, puis sélectionnez nouvelle requête.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Collez le script SQL de [https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) et exécutez-le. Si « DefaultConnection » est actualisé, nous pouvons voir que les nouvelles tables sont ajoutées. Vous pouvez vérifier les données dans les tables pour voir que les informations ont été migrées.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migration de l’application pour qu’elle utilise ASP.NET Identity

1. Installez les packages NuGet nécessaires pour ASP.NET Identity :

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Vous trouverez plus d’informations sur la gestion des packages NuGet [ici](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog) .
2. Pour travailler avec des données existantes dans la table, nous devons créer des classes de modèle qui mappent aux tables et les raccordent dans le système d’identité. Dans le cadre du contrat d’identité, les classes de modèle doivent implémenter les interfaces définies dans la dll Identity. Core ou étendre l’implémentation existante de ces interfaces disponibles dans Microsoft. AspNet. Identity. EntityFramework. Nous utiliserons les classes existantes pour le rôle, les connexions utilisateur et les revendications d’utilisateur. Nous devons utiliser un utilisateur personnalisé pour notre exemple. Cliquez avec le bouton droit sur le projet et créez un dossier « IdentityModels ». Ajoutez une nouvelle classe « User » comme indiqué ci-dessous :

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Notez que « ProfileInfo » est désormais une propriété de la classe User. Par conséquent, nous pouvons utiliser la classe d’utilisateur pour travailler directement avec les données de profil.

Copiez les fichiers dans les dossiers **IdentityModels** et **IdentityAccount** à partir de la source de téléchargement ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Il s’agit des classes de modèle restantes et des nouvelles pages nécessaires à la gestion des utilisateurs et des rôles à l’aide des API ASP.NET Identity. L’approche utilisée est similaire à l’appartenance SQL et l’explication détaillée est disponible [ici](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Copie des données de profil dans les nouvelles tables

Comme mentionné précédemment, nous devons désérialiser les données XML dans les tables de profils et les stocker dans les colonnes de la table AspNetUsers. Les nouvelles colonnes ont été créées dans le tableau utilisateurs à l’étape précédente, de sorte que tout ce qui reste est de remplir ces colonnes avec les données nécessaires. Pour ce faire, nous allons utiliser une application console qui est exécutée une fois pour remplir les colonnes nouvellement créées dans la table users.

1. Créez une nouvelle application console dans la solution de sortie.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Installez la dernière version du package Entity Framework.
3. Ajoutez l’application Web créée ci-dessus en tant que référence à l’application console. Pour ce faire, cliquez avec le bouton droit sur projet, ajoutez « ajouter des références », puis sur solution, cliquez sur le projet et cliquez sur OK.
4. Copiez le code ci-dessous dans la classe Program.cs. Cette logique lit les données de profil pour chaque utilisateur, les sérialise en tant qu’objet « ProfileInfo » et les stocke dans la base de données.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Certains des modèles utilisés sont définis dans le dossier’IdentityModels’du projet d’application Web. vous devez donc inclure les espaces de noms correspondants.
5. Le code ci-dessus fonctionne sur le fichier de base de données dans le dossier application\_Data du projet d’application Web créé au cours des étapes précédentes. Pour référencer cela, mettez à jour la chaîne de connexion dans le fichier app. config de l’application console avec la chaîne de connexion dans le fichier Web. config de l’application Web. Fournissez également le chemin d’accès physique complet dans la propriété « AttachDbFilename ».
6. Ouvrez une invite de commandes et accédez au dossier bin de l’application console ci-dessus. Exécutez le fichier exécutable et passez en revue la sortie du journal, comme indiqué dans l’image suivante.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Ouvrez la table « AspNetUsers » dans la Explorateur de serveurs et vérifiez les données dans les nouvelles colonnes qui contiennent les propriétés. Ils doivent être mis à jour avec les valeurs de propriété correspondantes.

## <a name="verify-functionality"></a>Vérifier la fonctionnalité

Utilisez les pages d’appartenance nouvellement ajoutées qui sont implémentées à l’aide de ASP.NET Identity pour se connecter à un utilisateur à partir de l’ancienne base de données. L’utilisateur doit pouvoir se connecter à l’aide des mêmes informations d’identification. Essayez les autres fonctionnalités telles que l’ajout d’OAuth, la création d’un utilisateur, la modification d’un mot de passe, l’ajout de rôles, l’ajout d’utilisateurs à des rôles, etc.

Les données de profil de l’ancien utilisateur et des nouveaux utilisateurs doivent être extraites et stockées dans la table utilisateurs. L’ancienne table ne doit plus être référencée.

## <a name="conclusion"></a>Conclusion

L’article a décrit le processus de migration d’applications Web qui utilisaient le modèle de fournisseur pour l’appartenance à ASP.NET Identity. L’article a également décrit la migration des données de profil pour que les utilisateurs soient connectés au système d’identité. Laissez les commentaires ci-dessous pour les questions et les problèmes rencontrés lors de la migration de votre application.

*Merci à Rick Anderson et Robert McMurray de passer en revue l’article.*
