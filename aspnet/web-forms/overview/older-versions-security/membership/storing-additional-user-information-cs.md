---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Stockage d’informations utilisateur supplémentairesC#() | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons répondre à cette question en créant une application de livre d’or très rudimentaire. Dans ce cas, nous examinerons différentes options pour modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 24b96e86bc93e03d2639b73e35ed1fd1271bac5a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74641450"
---
# <a name="storing-additional-user-information-c"></a>Stockage d’informations supplémentaires sur l’utilisateur (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> Dans ce didacticiel, nous allons répondre à cette question en créant une application de livre d’or très rudimentaire. Dans ce cas, nous examinerons différentes options de modélisation des informations utilisateur dans une base de données, puis voyons comment associer ces données aux comptes d’utilisateur créés par l’infrastructure d’appartenance.

## <a name="introduction"></a>Introduction

ASP. L’infrastructure d’appartenance de NET offre une interface flexible pour la gestion des utilisateurs. L’API d’appartenance comprend des méthodes pour valider les informations d’identification, récupérer des informations sur l’utilisateur actuellement connecté, créer un nouveau compte d’utilisateur et supprimer un compte d’utilisateur, entre autres. Chaque compte d’utilisateur dans l’infrastructure d’appartenance contient uniquement les propriétés nécessaires à la validation des informations d’identification et à l’exécution des tâches essentielles relatives au compte d’utilisateur. Cela est prouvé par les méthodes et les propriétés de la [classe`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), qui modélise un compte d’utilisateur dans l’infrastructure d’appartenance. Cette classe possède des propriétés telles que [`UserName`](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [`Email`](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)et [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), ainsi que des méthodes telles que [`GetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) et [`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Souvent, les applications doivent stocker des informations utilisateur supplémentaires non incluses dans l’infrastructure d’appartenance. Par exemple, un détaillant en ligne devra peut-être permettre à chaque utilisateur de stocker ses adresses de livraison et de facturation, les informations de paiement, les préférences de remise et le numéro de téléphone du contact. En outre, chaque commande dans le système est associée à un compte d’utilisateur particulier.

La classe `MembershipUser` n’inclut pas de propriétés telles que `PhoneNumber` ou `DeliveryPreferences` ou `PastOrders`. Comment suivre les informations utilisateur requises par l’application et s’intégrer à l’infrastructure d’appartenance ? Dans ce didacticiel, nous allons répondre à cette question en créant une application de livre d’or très rudimentaire. Dans ce cas, nous examinerons différentes options de modélisation des informations utilisateur dans une base de données, puis voyons comment associer ces données aux comptes d’utilisateur créés par l’infrastructure d’appartenance. Commençons !

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Étape 1 : création du modèle de données de l’application de livre d’or

Il existe diverses techniques qui peuvent être utilisées pour capturer les informations utilisateur dans une base de données et les associer aux comptes d’utilisateur créés par l’infrastructure d’appartenance. Pour illustrer ces techniques, nous devons augmenter l’application Web du didacticiel pour qu’elle capture un certain nombre de données liées à l’utilisateur. (Actuellement, le modèle de données de l’application contient uniquement les tables des services d’application nécessaires à la `SqlMembershipProvider`.)

Créons une application de livre d’or très simple dans laquelle un utilisateur authentifié peut laisser un commentaire. En plus de stocker les commentaires du livre d’or, permettez à chaque utilisateur de stocker sa ville d’accueil, sa page d’accueil et sa signature. Si elle est fournie, la ville de l’utilisateur, la page d’accueil et la signature apparaissent sur chaque message laissé dans le livre d’or.

### <a name="adding-theguestbookcommentstable"></a>Ajout de la table`GuestbookComments`

Pour capturer les commentaires du livre d’or, nous devons créer une table de base de données nommée `GuestbookComments` qui contient des colonnes comme `CommentId`, `Subject`, `Body`et `CommentDate`. Nous devons également faire en sorte que chaque enregistrement de la table `GuestbookComments` référence l’utilisateur qui a laissé le commentaire.

Pour ajouter cette table à notre base de données, accédez à la explorateur de base de données dans Visual Studio et explorez la base de données `SecurityTutorials`. Cliquez avec le bouton droit sur le dossier tables, puis choisissez Ajouter une nouvelle table. Cela ouvre une interface qui nous permet de définir les colonnes de la nouvelle table.

[![ajouter une nouvelle table à la base de données SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Figure 1**: ajouter une nouvelle table à la base de données `SecurityTutorials` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image3.png))

Ensuite, définissez les colonnes de `GuestbookComments`. Commencez par ajouter une colonne nommée `CommentId` de type `uniqueidentifier`. Cette colonne identifie de façon unique chaque commentaire dans le livre d’or, donc interdisez `NULL` s et marquez-la comme clé primaire de la table. Au lieu de fournir une valeur pour le champ `CommentId` sur chaque `INSERT`, nous pouvons indiquer qu’une nouvelle valeur de `uniqueidentifier` doit être générée automatiquement pour ce champ sur `INSERT` en affectant à la valeur par défaut de la colonne la valeur `NEWID()`. Après avoir ajouté ce premier champ, en le marquant comme clé primaire et en définissant sa valeur par défaut, votre écran doit ressembler à la capture d’écran illustrée à la figure 2.

[![ajouter une colonne principale nommée CommentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Figure 2**: ajouter une colonne principale nommée `CommentId` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image6.png))

Ensuite, ajoutez une colonne nommée `Subject` de type `nvarchar(50)` et une colonne nommée `Body` de type `nvarchar(MAX)`, en désautorisant `NULL` s dans les deux colonnes. Après cela, ajoutez une colonne nommée `CommentDate` de type `datetime`. Interdisez `NULL` s et définissez la valeur par défaut de la colonne `CommentDate` sur `getdate()`.

Il ne reste plus qu’à ajouter une colonne qui associe un compte d’utilisateur à chaque commentaire de livre d’or. L’une des options consiste à ajouter une colonne nommée `UserName` de type `nvarchar(256)`. Il s’agit d’un choix approprié lors de l’utilisation d’un fournisseur d’appartenances autre que le `SqlMembershipProvider`. Toutefois, lors de l’utilisation du `SqlMembershipProvider`, comme nous l’avons fait dans cette série de didacticiels, il n’est pas garanti que la colonne `UserName` de la table `aspnet_Users` soit unique. La clé primaire de la table `aspnet_Users` est `UserId` et est de type `uniqueidentifier`. Par conséquent, la table `GuestbookComments` a besoin d’une colonne nommée `UserId` de type `uniqueidentifier` (en désaccordant les valeurs `NULL`). Ajoutez cette colonne.

> [!NOTE]
> Comme nous l’avons vu dans le didacticiel [*création du schéma d’appartenance dans SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) , l’infrastructure d’appartenance est conçue pour permettre à plusieurs applications Web avec différents comptes d’utilisateur de partager le même magasin d’utilisateurs. Pour ce faire, il partitionne les comptes d’utilisateur en différentes applications. Et si chaque nom d’utilisateur est garanti comme étant unique au sein d’une application, le même nom d’utilisateur peut être utilisé dans différentes applications utilisant le même magasin d’utilisateurs. Il existe une contrainte de `UNIQUE` composite dans la table `aspnet_Users` sur les champs `UserName` et `ApplicationId`, mais pas sur le champ `UserName`. Par conséquent, il est possible pour la table utilisateurs ASPNET\_d’avoir deux enregistrements (ou plus) avec la même valeur de `UserName`. Toutefois, il existe une contrainte de `UNIQUE` sur le champ `UserId` de la table `aspnet_Users` (puisqu’il s’agit de la clé primaire). Une contrainte de `UNIQUE` est importante, car elle ne peut pas établir de contrainte de clé étrangère entre les tables `GuestbookComments` et `aspnet_Users`.

Après avoir ajouté la colonne `UserId`, enregistrez la table en cliquant sur l’icône Enregistrer dans la barre d’outils. Nommez la nouvelle table `GuestbookComments`.

Nous avons un dernier problème à atteindre avec la table `GuestbookComments` : nous devons créer une contrainte de [clé étrangère](https://msdn.microsoft.com/library/ms175464.aspx) entre la colonne `GuestbookComments.UserId` et la colonne `aspnet_Users.UserId`. Pour ce faire, cliquez sur l’icône relation dans la barre d’outils pour ouvrir la boîte de dialogue relations de clé étrangère. (Vous pouvez également lancer cette boîte de dialogue en accédant au menu Concepteur de tables et en choisissant relations.)

Cliquez sur le bouton Ajouter dans le coin inférieur gauche de la boîte de dialogue relations de clé étrangère. Cette opération ajoute une nouvelle contrainte de clé étrangère, même si nous devons toujours définir les tables qui participent à la relation.

[![utiliser la boîte de dialogue relations de clé étrangère pour gérer les contraintes de clé étrangère d’une table](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Figure 3**: utiliser la boîte de dialogue relations de clé étrangère pour gérer les contraintes de clé étrangère d’une table ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image9.png))

Ensuite, cliquez sur l’icône de sélection dans la ligne « spécifications de table et de colonnes » à droite. La boîte de dialogue tables et colonnes s’ouvre et vous permet de spécifier la table et la colonne de clé primaire et la colonne de clé étrangère de la table `GuestbookComments`. En particulier, sélectionnez `aspnet_Users` et `UserId` en tant que table et colonne de clé primaire, puis `UserId` à partir de la table `GuestbookComments` comme colonne de clé étrangère (voir figure 4). Après avoir défini les tables et les colonnes de clé primaire et étrangère, cliquez sur OK pour revenir à la boîte de dialogue relations de clé étrangère.

[![établir une contrainte de clé étrangère entre les tables aspnet_Users et GuesbookComments](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Figure 4**: établir une contrainte de clé étrangère entre les tables `aspnet_Users` et `GuesbookComments` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image12.png))

À ce stade, la contrainte de clé étrangère a été établie. La présence de cette contrainte garantit l' [intégrité relationnelle](http://en.wikipedia.org/wiki/Referential_integrity) entre les deux tables en garantissant qu’aucune entrée de livre d’or ne fait référence à un compte d’utilisateur inexistant. Par défaut, une contrainte de clé étrangère interdit la suppression d’un enregistrement parent s’il existe des enregistrements enfants correspondants. Autrement dit, si un utilisateur crée un ou plusieurs commentaires de livre d’or, puis tente de supprimer ce compte d’utilisateur, la suppression échoue, sauf si ses commentaires de livre d’or sont supprimés en premier.

Les contraintes de clé étrangère peuvent être configurées pour supprimer automatiquement les enregistrements enfants associés lorsqu’un enregistrement parent est supprimé. En d’autres termes, nous pouvons configurer cette contrainte de clé étrangère afin que les entrées de livre d’or d’un utilisateur soient automatiquement supprimées quand son compte d’utilisateur est supprimé. Pour ce faire, développez la section « Spécification d’insertion et de mise à jour » et affectez à la propriété « supprimer la règle » la valeur cascade.

[![configurer la contrainte de clé étrangère sur les suppressions en cascade](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Figure 5**: configurer la contrainte de clé étrangère sur les suppressions en cascade ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image15.png))

Pour enregistrer la contrainte de clé étrangère, cliquez sur le bouton Fermer pour quitter les relations de clé étrangère. Cliquez ensuite sur l’icône Enregistrer dans la barre d’outils pour enregistrer la table et la relation This.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Stockage de la localité, de la page d’accueil et de la signature de l’utilisateur

Le tableau `GuestbookComments` montre comment stocker des informations qui partagent une relation un-à-plusieurs avec des comptes d’utilisateur. Étant donné que chaque compte d’utilisateur peut avoir un nombre arbitraire de commentaires associés, cette relation est modélisée en créant une table pour contenir l’ensemble de commentaires qui comprend une colonne qui lie chaque commentaire à un utilisateur particulier. Lors de l’utilisation de la `SqlMembershipProvider`, ce lien est mieux établi en créant une colonne nommée `UserId` de type `uniqueidentifier` et une contrainte de clé étrangère entre cette colonne et `aspnet_Users.UserId`.

Nous devons maintenant associer trois colonnes à chaque compte d’utilisateur pour stocker la ville d’accueil, la page d’accueil et la signature de l’utilisateur, qui apparaîtront dans ses commentaires de livre d’or. Il existe plusieurs façons de procéder :

- <strong>Ajoutez de nouvelles colonnes aux</strong> tables<strong>`aspnet_Users`</strong> <strong>ou</strong> <strong>`aspnet_Membership`</strong> <strong>.</strong> Je ne recommande pas cette approche, car elle modifie le schéma utilisé par le `SqlMembershipProvider`. Cette décision peut être retournée pour vous Haunt. Par exemple, si une future version de ASP.NET utilise un schéma de `SqlMembershipProvider` différent. Microsoft peut inclure un outil pour migrer les données ASP.NET 2,0 `SqlMembershipProvider` vers le nouveau schéma, mais si vous avez modifié le schéma ASP.NET 2,0 `SqlMembershipProvider`, une telle conversion peut ne pas être possible.

- **Utilisez ASP. Infrastructure de profil de NET, définissant une propriété de profil pour la ville d’accueil, la page d’accueil et la signature.** ASP.NET comprend un Framework de profil conçu pour stocker des données supplémentaires spécifiques à l’utilisateur. À l’instar de l’infrastructure d’appartenance, l’infrastructure de profil est générée au-dessus du modèle de fournisseur. Le .NET Framework est fourni avec un `SqlProfileProvider` sthat stocke les données de profil dans une base de données SQL Server. En fait, notre base de données a déjà la table utilisée par le `SqlProfileProvider` (`aspnet_Profile`), car elle a été ajoutée lorsque nous avons ajouté les services <a id="_msoanchor_2"> </a>d’application dans le didacticiel [*création du schéma d’appartenance dans SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) .   
  Le principal avantage de l’infrastructure de profil est qu’elle permet aux développeurs de définir les propriétés de profil dans `Web.config` – aucun code n’a besoin d’être écrit pour sérialiser les données de profil vers et depuis le magasin de données sous-jacent. En bref, il est extrêmement facile de définir un ensemble de propriétés de profil et de les utiliser dans le code. Toutefois, le système de profil laisse beaucoup à désirer s’il s’agit d’un contrôle de version. par conséquent, si vous avez une application dans laquelle vous pensez que de nouvelles propriétés spécifiques à l’utilisateur doivent être ajoutées ultérieurement, ou si elles doivent être supprimées ou modifiées, l’infrastructure de profil peut ne pas être la  meilleure option. En outre, l' `SqlProfileProvider` stocke les propriétés de profil de manière très dénormalisée, ce qui rend impossible l’exécution des requêtes directement sur les données de profil (par exemple, le nombre d’utilisateurs ayant une ville d’origine de New York).   
  Pour plus d’informations sur l’infrastructure de profil, reportez-vous à la section « autres lectures » à la fin de ce didacticiel.

- <strong>Ajoutez ces trois colonnes à une nouvelle table dans la base de données et établissez une relation un-à-un entre cette table et</strong> <strong>`aspnet_Users`</strong> <strong>.</strong> Cette approche implique un peu plus de travail que l’infrastructure de profil, mais offre une flexibilité maximale dans la manière dont les propriétés utilisateur supplémentaires sont modélisées dans la base de données. Il s’agit de l’option que nous allons utiliser dans ce didacticiel.

Nous allons créer une nouvelle table nommée `UserProfiles` pour enregistrer la ville d’accueil, la page d’accueil et la signature pour chaque utilisateur. Cliquez avec le bouton droit sur le dossier tables dans la fenêtre explorateur de base de données et choisissez de créer une nouvelle table. Nommez la première colonne `UserId` et définissez son type sur `uniqueidentifier`. Interdisez les valeurs de `NULL` et marquez la colonne comme clé primaire. Ensuite, ajoutez les colonnes nommées : `HomeTown` de type `nvarchar(50)`; `HomepageUrl` de type `nvarchar(100)`; et la signature de type `nvarchar(500)`. Chacune de ces trois colonnes peut accepter une valeur de `NULL`.

[![créer la table UserProfiles](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Figure 6**: créer la table `UserProfiles` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image18.png))

Enregistrez la table et nommez-la `UserProfiles`. Enfin, établissez une contrainte de clé étrangère entre le champ `UserId` de la table `UserProfiles` et le champ `aspnet_Users.UserId`. Comme nous l’avons fait avec la contrainte de clé étrangère entre les tables `GuestbookComments` et `aspnet_Users`, cette contrainte est supprimée en cascade. Étant donné que le champ `UserId` dans `UserProfiles` est la clé primaire, cela permet de s’assurer qu’il n’y aura pas plus d’un enregistrement dans la table `UserProfiles` pour chaque compte d’utilisateur. Ce type de relation est désigné sous le terme « un-à-un ».

Maintenant que nous avons créé le modèle de données, nous sommes prêts à l’utiliser. Dans les étapes 2 et 3, nous verrons comment l’utilisateur actuellement connecté peut afficher et modifier sa ville d’accueil, sa page d’accueil et ses informations de signature. À l’étape 4, nous allons créer l’interface pour les utilisateurs authentifiés afin qu’ils envoient de nouveaux commentaires au livre d’or et affichent ceux qui existent déjà.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Étape 2 : affichage de la ville, de la page d’accueil et de la signature de l’utilisateur

Il existe plusieurs façons d’autoriser l’utilisateur actuellement connecté à afficher et à modifier sa ville d’accueil, sa page d’accueil et ses informations de signature. Nous pourrions créer manuellement l’interface utilisateur avec des contrôles TextBox et label, ou nous pourrions utiliser l’un des contrôles Web de données, tels que le contrôle DetailsView. Pour exécuter les instructions `SELECT` et `UPDATE` de la base de données, nous pourrions écrire du code ADO.NET dans la classe code-behind de la page ou bien utiliser une approche déclarative avec le SqlDataSource. Dans l’idéal, notre application contient une architecture à plusieurs niveaux, que nous pourrions appeler par programmation à partir de la classe code-behind de la page ou de manière déclarative via le contrôle ObjectDataSource.

Dans la mesure où cette série de didacticiels porte sur l’authentification par formulaire, l’autorisation, les comptes d’utilisateurs et les rôles, il n’y aura pas de description détaillée de ces différentes options d’accès aux données ou de la raison pour laquelle une architecture à plusieurs niveaux est préférable à l’exécution directe des instructions SQL. à partir de la page ASP.NET. Je vais passer en revue l’utilisation d’un DetailsView et d’un SqlDataSource, l’option la plus rapide et la plus simple, mais les concepts abordés peuvent certainement être appliqués à d’autres contrôles Web et à la logique d’accès aux données. Pour plus d’informations sur l’utilisation des données dans ASP.NET, consultez la série de didacticiels sur *[l’utilisation des données dans ASP.NET 2,0](../../data-access/index.md)* .

Ouvrez la page `AdditionalUserInfo.aspx` dans le dossier `Membership` et ajoutez un contrôle DetailsView à la page, en affectant à sa propriété `ID` la valeur `UserProfile` et en effaçant ses propriétés `Width` et `Height`. Développez la balise active du contrôle DetailsView et choisissez de le lier à un nouveau contrôle de source de données. Cette opération lance l’Assistant Configuration de source de source (voir la figure 7). La première étape vous demande de spécifier le type de source de données. Étant donné que nous allons nous connecter directement à la base de données `SecurityTutorials`, choisissez l’icône de base de données, en spécifiant le `ID` comme `UserProfileDataSource`.

[![ajouter un nouveau contrôle SqlDataSource nommé UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Figure 7**: ajouter un nouveau contrôle SqlDataSource nommé `UserProfileDataSource` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image21.png))

L’écran suivant vous invite à indiquer la base de données à utiliser. Nous avons déjà défini une chaîne de connexion dans `Web.config` pour la base de données `SecurityTutorials`. Ce nom de chaîne de connexion (`SecurityTutorialsConnectionString`) doit figurer dans la liste déroulante. Sélectionnez cette option et cliquez sur suivant.

[![sélectionnez SecurityTutorialsConnectionString dans la liste déroulante.](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Figure 8**: choisir `SecurityTutorialsConnectionString` dans la liste déroulante ([Cliquer pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image24.png))

L’écran suivant nous demande de spécifier la table et les colonnes à interroger. Sélectionnez la table `UserProfiles` dans la liste déroulante et cochez toutes les colonnes.

[![ramener toutes les colonnes de la table UserProfiles](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Figure 9**: rétablir toutes les colonnes de la table `UserProfiles` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image27.png))

La requête en cours dans la figure 9 retourne *tous* les enregistrements dans `UserProfiles`, mais nous sommes uniquement intéressés par l’enregistrement de l’utilisateur actuellement connecté. Pour ajouter une clause `WHERE`, cliquez sur le bouton `WHERE` pour afficher la boîte de dialogue Ajouter une clause `WHERE` (voir figure 10). Ici, vous pouvez sélectionner la colonne à filtrer, l’opérateur et la source du paramètre de filtre. Sélectionnez `UserId` en tant que colonne et « = » comme opérateur.

Malheureusement, il n’existe aucune source de paramètre intégrée pour retourner la valeur `UserId` de l’utilisateur actuellement connecté. Nous devons récupérer cette valeur par programmation. Par conséquent, définissez la liste déroulante source sur « None », cliquez sur le bouton Ajouter pour ajouter le paramètre, puis cliquez sur OK.

[![ajouter un paramètre de filtre sur la colonne UserId](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Figure 10**: ajouter un paramètre de filtre sur la colonne `UserId` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image30.png))

Après avoir cliqué sur OK, vous revenez à l’écran illustré à la figure 9. Cette fois, toutefois, la requête SQL en bas de l’écran doit inclure une clause `WHERE`. Cliquez sur suivant pour passer à l’écran « tester la requête ». Ici, vous pouvez exécuter la requête et consulter les résultats. Cliquez sur Terminer pour terminer l'Assistant.

À la fin de l’Assistant Configuration de source de donnée, Visual Studio crée le contrôle SqlDataSource en fonction des paramètres spécifiés dans l’Assistant. En outre, il ajoute manuellement BoundFields au DetailsView pour chaque colonne retournée par l' `SelectCommand`du SqlDataSource. Il n’est pas nécessaire d’afficher le champ `UserId` dans le DetailsView, puisque l’utilisateur n’a pas besoin de connaître cette valeur. Vous pouvez supprimer ce champ directement du balisage déclaratif du contrôle DetailsView ou en cliquant sur le lien « modifier les champs » à partir de sa balise active.

À ce stade, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Nous devons définir par programmation le paramètre `UserId` du contrôle SqlDataSource sur le `UserId` de l’utilisateur actuellement connecté pour que les données soient sélectionnées. Pour ce faire, vous pouvez créer un gestionnaire d’événements pour l’événement `Selecting` du SqlDataSource et y ajouter le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Le code ci-dessus commence par obtenir une référence à l’utilisateur actuellement connecté en appelant la méthode `GetUser` de la classe `Membership`. Cela retourne un objet `MembershipUser` dont la propriété `ProviderUserKey` contient le `UserId`. La valeur `UserId` est ensuite assignée au paramètre `@UserId` du SqlDataSource.

> [!NOTE]
> La méthode `Membership.GetUser()` retourne des informations sur l’utilisateur actuellement connecté. Si un utilisateur anonyme visite la page, il retourne la valeur `null`. Dans ce cas, cela aboutit à une `NullReferenceException` sur la ligne de code suivante lors de la tentative de lecture de la propriété `ProviderUserKey`. Bien entendu, nous n’avons pas à vous soucier `Membership.GetUser()` de retourner une valeur `null` dans la page `AdditionalUserInfo.aspx`, car nous avons configuré l’autorisation d’URL dans un didacticiel précédent afin que seuls les utilisateurs authentifiés puissent accéder aux ressources ASP.NET dans ce dossier. Si vous avez besoin d’accéder à des informations sur l’utilisateur actuellement connecté dans une page où l’accès anonyme est autorisé, veillez à vérifier qu’un objet non`null MembershipUser` est retourné à partir de la méthode `GetUser()` avant de référencer ses propriétés.

Si vous accédez à la page de `AdditionalUserInfo.aspx` via un navigateur, vous verrez une page vide, car nous n’avons pas encore ajouté de lignes au tableau `UserProfiles`. À l’étape 6, nous examinerons comment personnaliser le contrôle CreateUserWizard pour ajouter automatiquement une nouvelle ligne à la table `UserProfiles` lors de la création d’un nouveau compte d’utilisateur. Pour l’instant, toutefois, nous devons créer manuellement un enregistrement dans la table.

Accédez au explorateur de base de données dans Visual Studio et développez le dossier tables. Cliquez avec le bouton droit sur la table `aspnet_Users` et choisissez « afficher les données de la table » pour afficher les enregistrements dans la table. faites la même chose pour la table `UserProfiles`. La figure 11 illustre ces résultats en mosaïque verticale. Dans ma base de données, il existe actuellement des enregistrements `aspnet_Users` pour Bruce, Fred et Tito, mais aucun enregistrement dans la table `UserProfiles`.

[![le contenu des tables aspnet_Users et UserProfiles sont affichés](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Figure 11**: affichage du contenu des tableaux `aspnet_Users` et `UserProfiles` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image33.png))

Ajoutez un nouvel enregistrement à la table `UserProfiles` en tapant manuellement les valeurs des champs `HomeTown`, `HomepageUrl`et `Signature`. Le moyen le plus simple d’obtenir une valeur de `UserId` valide dans le nouvel enregistrement de `UserProfiles` consiste à sélectionner le champ `UserId` à partir d’un compte d’utilisateur particulier dans la table `aspnet_Users` et à le copier et à le coller dans le champ `UserId` de `UserProfiles`. La figure 12 illustre la table `UserProfiles` après l’ajout d’un nouvel enregistrement pour Bruce.

[![un enregistrement a été ajouté à UserProfiles pour Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Figure 12**: un enregistrement a été ajouté à `UserProfiles` pour Bruce ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image36.png))

Revenez à la page `AdditionalUserInfo.aspx`, en vous connectant en tant que Bruce. Comme le montre la figure 13, les paramètres de Bruce s’affichent.

[![l’utilisateur actuellement visité affiche ses paramètres](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Figure 13**: l’utilisateur actuellement visité affiche ses paramètres ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image39.png))

> [!NOTE]
> Continuez et ajoutez manuellement les enregistrements dans la table `UserProfiles` pour chaque utilisateur d’appartenance. À l’étape 6, nous examinerons comment personnaliser le contrôle CreateUserWizard pour ajouter automatiquement une nouvelle ligne à la table `UserProfiles` lors de la création d’un nouveau compte d’utilisateur.

## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Étape 3 : permettre à l’utilisateur de modifier sa ville d’accueil, sa page d’accueil et sa signature

À ce stade, l’utilisateur actuellement connecté peut afficher sa ville d’accueil, sa page d’accueil et son paramètre de signature, mais il ne peut pas encore le modifier. Nous allons mettre à jour le contrôle DetailsView afin que les données puissent être modifiées.

La première chose à faire est d’ajouter un `UpdateCommand` pour le SqlDataSource, en spécifiant l’instruction `UPDATE` à exécuter et ses paramètres correspondants. Sélectionnez le SqlDataSource et, dans la Fenêtre Propriétés, cliquez sur les ellipses en regard de la propriété UpdateQuery pour afficher la boîte de dialogue Éditeur de commandes et de paramètres. Entrez l’instruction `UPDATE` suivante dans la zone de texte :

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Ensuite, cliquez sur le bouton « actualiser les paramètres », ce qui crée un paramètre dans la collection `UpdateParameters` du contrôle SqlDataSource pour chacun des paramètres de l’instruction `UPDATE`. Laissez la source de tous les paramètres défini sur aucun, puis cliquez sur le bouton OK pour fermer la boîte de dialogue.

[![spécifier les UpdateCommand et UpdateParameters du SqlDataSource](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Figure 14**: spécifier le `UpdateCommand` et les `UpdateParameters` du SqlDataSource ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image42.png))

En raison des ajouts que nous avons apportés au contrôle SqlDataSource, le contrôle DetailsView peut désormais prendre en charge la modification. À partir de la balise active du contrôle DetailsView, cochez la case « Activer la modification ». Cela ajoute une CommandField à la collection `Fields` du contrôle avec sa propriété `ShowEditButton` définie sur true. Cela génère le rendu d’un bouton modifier lorsque le contrôle DetailsView est affiché en mode lecture seule et les boutons mettre à jour et annuler lorsqu’il est affiché en mode édition. Au lieu de demander à l’utilisateur de cliquer sur modifier, toutefois, le contrôle DetailsView peut s’afficher dans un État « toujours modifiable » en affectant à la [propriété`DefaultMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) du contrôle DetailsView la valeur `Edit`.

Avec ces modifications, le balisage déclaratif de votre contrôle DetailsView doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Notez l’ajout de la propriété CommandField et de la propriété `DefaultMode`.

Poursuivez et testez cette page par le biais d’un navigateur. Lorsque vous vous rendez avec un utilisateur qui a un enregistrement correspondant dans `UserProfiles`, les paramètres de l’utilisateur s’affichent dans une interface modifiable.

[![le contrôle DetailsView rend une interface modifiable](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Figure 15**: le contrôle DetailsView rend une interface modifiable ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image45.png))

Essayez de modifier les valeurs et de cliquer sur le bouton mettre à jour. Elle apparaît comme si rien ne se passe. Il existe une publication (postback) et les valeurs sont enregistrées dans la base de données, mais il n’existe aucun commentaire visuel indiquant que l’enregistrement s’est produit.

Pour y remédier, revenez à Visual Studio et ajoutez un contrôle Label au-dessus du contrôle DetailsView. Définissez ses `ID` sur `SettingsUpdatedMessage`, sa propriété `Text` sur « vos paramètres ont été mis à jour », et ses propriétés `Visible` et `EnableViewState` sur `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Nous devons afficher le `SettingsUpdatedMessage` étiquette à chaque mise à jour du contrôle DetailsView. Pour ce faire, créez un gestionnaire d’événements pour l’événement `ItemUpdated` du contrôle DetailsView et ajoutez le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Revenez à la page de `AdditionalUserInfo.aspx` via un navigateur et mettez à jour les données. Cette fois-ci, un message d’État utile s’affiche.

[![un message bref s’affiche lorsque les paramètres sont mis à jour](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Figure 16**: un message bref s’affiche lorsque les paramètres sont mis à jour ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image48.png))

> [!NOTE]
> L’interface de modification du contrôle DetailsView laisse beaucoup à désirer. Il utilise des zones de texte de taille standard, mais le champ de signature doit probablement être une zone de texte multiligne. Un RegularExpressionValidator doit être utilisé pour s’assurer que l’URL de la page d’accueil, si elle est entrée, commence par « http:// » ou « https:// ». En outre, étant donné que le contrôle DetailsView a sa propriété `DefaultMode` définie sur `Edit`, le bouton Annuler ne fait rien. Elle doit être supprimée ou, lorsque vous cliquez dessus, rediriger l’utilisateur vers une autre page (par exemple `~/Default.aspx`). Je laisse ces améliorations en tant qu’exercice pour le lecteur.

### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Ajout d’un lien vers la page de`AdditionalUserInfo.aspx`dans la page maître

Actuellement, le site Web ne fournit aucun lien vers la page de `AdditionalUserInfo.aspx`. La seule façon d’y accéder consiste à entrer l’URL de la page directement dans la barre d’adresse du navigateur. Nous allons ajouter un lien vers cette page dans la page maître de `Site.master`.

Rappelez-vous que la page maître contient un contrôle Web LoginView dans son `LoginContent` ContentPlaceHolder qui affiche des balises différentes pour les visiteurs authentifiés et anonymes. Mettez à jour le `LoggedInTemplate` du contrôle LoginView pour inclure un lien vers la page de `AdditionalUserInfo.aspx`. Après avoir apporté ces modifications, le balisage déclaratif du contrôle LoginView doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Notez l’ajout du contrôle de lien hypertexte `lnkUpdateSettings` au `LoggedInTemplate`. Une fois ce lien en place, les utilisateurs authentifiés peuvent accéder rapidement à la page pour afficher et modifier leur ville d’accueil, leur page d’accueil et leurs paramètres de signature.

## <a name="step-4-adding-new-guestbook-comments"></a>Étape 4 : ajout de nouveaux commentaires de livre d’or

La page `Guestbook.aspx` permet aux utilisateurs authentifiés d’afficher le livre d’or et de conserver un commentaire. Commençons par créer l’interface pour ajouter de nouveaux commentaires de livre d’or.

Ouvrez la page `Guestbook.aspx` dans Visual Studio et créez une interface utilisateur composée de deux contrôles TextBox, l’un pour le sujet du nouveau commentaire et l’autre pour son corps. Affectez à la propriété `ID` du premier contrôle TextBox la valeur `Subject` et à sa propriété `Columns` la valeur 40 ; Définissez le `ID` du second sur `Body`, son `TextMode` sur `MultiLine`, et ses propriétés `Width` et `Rows` sur « 95% » et 8, respectivement. Pour terminer l’interface utilisateur, ajoutez un contrôle Web Button nommé `PostCommentButton` et affectez à sa propriété `Text` la valeur « poster votre commentaire ».

Étant donné que chaque commentaire de livre d’or requiert un objet et un corps, ajoutez un RequiredFieldValidator pour chaque zone de texte. Affectez à la propriété `ValidationGroup` de ces contrôles la valeur « EnterComment ». de même, affectez à la propriété `ValidationGroup` du contrôle `PostCommentButton` la valeur "EnterComment". Pour plus d’informations sur ASP. Contrôles de validation du réseau, vérifiez la [validation de formulaire dans ASP.net](http://www.4guysfromrolla.com/webtech/090200-1.shtml), en [discroisant les contrôles de validation dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)et le didacticiel sur les [contrôles serveur de validation](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) sur [W3Schools](http://www.w3schools.com/).

Après avoir créé l’interface utilisateur, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Une fois l’interface utilisateur terminée, la tâche suivante consiste à insérer un nouvel enregistrement dans la table `GuestbookComments` lorsque l’utilisateur clique sur le `PostCommentButton`. Cela peut être accompli de plusieurs manières : nous pouvons écrire le code ADO.NET dans le gestionnaire d’événements `Click` du bouton ; Nous pouvons ajouter un contrôle SqlDataSource à la page, configurer son `InsertCommand`, puis appeler sa méthode `Insert` à partir du gestionnaire d’événements `Click` ; ou nous pourrions créer un niveau intermédiaire chargé d’insérer de nouveaux commentaires de livre d’or, et d’appeler cette fonctionnalité à partir du gestionnaire d’événements `Click`. Étant donné que nous avons examiné l’utilisation d’un SqlDataSource à l’étape 3, nous utilisons ici le code ADO.NET.

> [!NOTE]
> Les classes ADO.NET utilisées pour accéder par programmation aux données d’une base de données Microsoft SQL Server se trouvent dans l’espace de noms `System.Data.SqlClient`. Vous devrez peut-être importer cet espace de noms dans la classe code-behind de votre page (c’est-à-dire, `using System.Data.SqlClient;`).

Créez un gestionnaire d’événements pour l’événement `Click` de l' `PostCommentButton`et ajoutez le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

Le gestionnaire d’événements `Click` commence par vérifier que les données fournies par l’utilisateur sont valides. Si ce n’est pas le cas, le gestionnaire d’événements se termine avant l’insertion d’un enregistrement. En supposant que les données fournies sont valides, la valeur `UserId` de l’utilisateur actuellement connecté est récupérée et stockée dans la variable locale `currentUserId`. Cette valeur est nécessaire car nous devons fournir une valeur de `UserId` lors de l’insertion d’un enregistrement dans `GuestbookComments`.

Après cela, la chaîne de connexion pour la base de données `SecurityTutorials` est récupérée à partir de `Web.config` et la `INSERT` instruction SQL est spécifiée. Un objet `SqlConnection` est ensuite créé et ouvert. Ensuite, un objet `SqlCommand` est construit et les valeurs des paramètres utilisés dans la requête `INSERT` sont assignées. L’instruction `INSERT` est ensuite exécutée et la connexion est fermée. À la fin du gestionnaire d’événements, les propriétés `Subject` et `Body` TextBox' `Text` sont effacées afin que les valeurs de l’utilisateur ne soient pas conservées dans la publication (postback).

Poursuivez et testez cette page dans un navigateur. Étant donné que cette page se trouve dans le dossier `Membership` elle n’est pas accessible aux visiteurs anonymes. Par conséquent, vous devez d’abord vous connecter (si vous ne l’avez pas déjà fait). Entrez une valeur dans les zones de texte `Subject` et `Body`, puis cliquez sur le bouton `PostCommentButton`. Cela entraîne l’ajout d’un nouvel enregistrement à `GuestbookComments`. Lors de la publication (postback), l’objet et le corps que vous avez fournis sont effacés des zones de texte.

Après avoir cliqué sur le bouton `PostCommentButton`, aucun commentaire visuel n’a été ajouté au livre d’or. Nous devons toujours mettre à jour cette page pour afficher les commentaires de livre d’or existants, que nous allons faire à l’étape 5. Une fois que nous avons accompli cela, le commentaire qui vient d’être ajouté apparaît dans la liste de commentaires, ce qui fournit des commentaires visuels adéquats. Pour le moment, vérifiez que votre commentaire de livre d’or a été enregistré en examinant le contenu de la table `GuestbookComments`.

La figure 17 montre le contenu de la table `GuestbookComments` une fois que deux commentaires ont été laissés.

[![vous pouvez voir les commentaires du livre d’or dans la table GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Figure 17**: vous pouvez voir les commentaires du livre d’or dans la table `GuestbookComments` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image51.png))

> [!NOTE]
> Si un utilisateur tente d’insérer un commentaire de livre d’or qui contient un balisage potentiellement dangereux, tel que HTML – ASP.NET lèvera une `HttpRequestValidationException`. Pour en savoir plus sur cette exception, pourquoi elle est levée et comment permettre aux utilisateurs d’envoyer des valeurs potentiellement dangereuses, consultez le [livre blanc](../../../../whitepapers/request-validation.md)sur la validation des demandes.

## <a name="step-5-listing-the-existing-guestbook-comments"></a>Étape 5 : liste des commentaires du livre d’or existants

En plus de laisser des commentaires, un utilisateur visitant la page de `Guestbook.aspx` doit également être en mesure d’afficher les commentaires existants du livre d’or. Pour ce faire, ajoutez un contrôle ListView nommé `CommentList` au bas de la page.

> [!NOTE]
> Le contrôle ListView est une nouveauté de ASP.NET version 3,5. Il est conçu pour afficher une liste d’éléments dans une disposition très personnalisable et flexible, tout en offrant des fonctionnalités intégrées de modification, d’insertion, de suppression, de pagination et de tri, comme le contrôle GridView. Si vous utilisez ASP.NET 2,0, vous devrez utiliser le contrôle DataList ou Repeater à la place. Pour plus d’informations sur l’utilisation de ListView, consultez l’entrée de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [le contrôle ASP : ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)et mon article, [affichage des données avec le contrôle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Ouvrez la balise active de ListView et, dans la liste déroulante choisir la source de données, liez le contrôle à une nouvelle source de données. Comme nous l’avons vu à l’étape 2, cette opération lance l’Assistant Configuration de source de données. Sélectionnez l’icône de base de données, nommez le `CommentsDataSource`SqlDataSource résultant, puis cliquez sur OK. Ensuite, sélectionnez la chaîne de connexion `SecurityTutorialsConnectionString` dans la liste déroulante, puis cliquez sur suivant.

À ce stade de l’étape 2, nous avons spécifié les données à interroger en choisissant la table `UserProfiles` dans la liste déroulante et en sélectionnant les colonnes à retourner (reportez-vous à la figure 9). Cette fois, cependant, nous souhaitons créer une instruction SQL qui extrait non seulement les enregistrements de `GuestbookComments`, mais également la ville d’accueil, la page d’accueil, la signature et le nom d’utilisateur du commentaire. Par conséquent, sélectionnez la case d’option « spécifier une instruction SQL personnalisée ou une procédure stockée », puis cliquez sur suivant.

L’écran « définir des instructions personnalisées ou des procédures stockées » s’affiche. Cliquez sur le bouton Générateur de requêtes pour créer une requête graphiquement. Le Générateur de requêtes démarre en nous invitant à spécifier les tables à partir desquelles nous souhaitons interroger. Sélectionnez les tables `GuestbookComments`, `UserProfiles`et `aspnet_Users`, puis cliquez sur OK. Cette opération ajoute les trois tables à l’aire de conception. Étant donné qu’il existe des contraintes de clé étrangère entre les tables `GuestbookComments`, `UserProfiles`et `aspnet_Users`, le Générateur de requêtes `JOIN` automatiquement ces tables.

Il ne reste plus qu’à spécifier les colonnes à retourner. Dans la table `GuestbookComments`, sélectionnez les colonnes `Subject`, `Body`et `CommentDate` ; retourne les colonnes `HomeTown`, `HomepageUrl`et `Signature` de la table `UserProfiles` ; et retournent `UserName` à partir de `aspnet_Users`. En outre, ajoutez «`ORDER BY CommentDate DESC`» à la fin de la requête `SELECT` afin que les publications les plus récentes soient retournées en premier. Après avoir effectué ces sélections, votre interface Générateur de requêtes doit ressembler à la capture d’écran de la figure 18.

[![la requête construite joint les tables GuestbookComments, UserProfiles et aspnet_Users](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Figure 18**: la requête construite `JOIN` s les tables `GuestbookComments`, `UserProfiles`et `aspnet_Users` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image54.png))

Cliquez sur OK pour fermer la fenêtre de Générateur de requêtes et revenir à l’écran « définir des instructions personnalisées ou des procédures stockées ». Cliquez sur suivant pour passer à l’écran « tester la requête », dans lequel vous pouvez afficher les résultats de la requête en cliquant sur le bouton tester la requête. Lorsque vous êtes prêt, cliquez sur Terminer pour terminer l’Assistant Configuration de la source de données.

Quand nous avons terminé l’Assistant Configuration de la source de données à l’étape 2, la collection de `Fields` du contrôle DetailsView associé a été mise à jour pour inclure un BoundField pour chaque colonne retournée par l' `SelectCommand`. Toutefois, le ListView reste inchangé. Nous devons toujours définir sa disposition. La disposition de ListView peut être construite manuellement par le biais de son balisage déclaratif ou de l’option « configurer ListView » dans sa balise active. Je préfère généralement définir le balisage manuellement, mais utiliser la méthode la plus naturelle pour vous.

J’ai fini par utiliser les `LayoutTemplate`, `ItemTemplate`et `ItemSeparatorTemplate` suivants pour mon contrôle ListView :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

Le `LayoutTemplate` définit le balisage émis par le contrôle, tandis que le `ItemTemplate` restitue chaque élément retourné par le SqlDataSource. Le balisage résultant de `ItemTemplate`est placé dans le contrôle `itemPlaceholder` du `LayoutTemplate`. En plus de la `itemPlaceholder`, le `LayoutTemplate` comprend un contrôle DataPager, qui limite le ListView à afficher uniquement 10 commentaires de livre d’or par page (valeur par défaut) et restitue une interface de pagination.

Mon `ItemTemplate` affiche le sujet de chaque commentaire de livre d’or dans un élément de `<h4>` avec le corps situé sous le sujet. Notez que la syntaxe utilisée pour afficher le corps prend les données retournées par l' `Eval("Body")` instruction DataBinding, le convertit en une chaîne et remplace les sauts de ligne par l’élément `<br />`. Cette conversion est nécessaire pour afficher les sauts de ligne entrés lors de l’envoi du commentaire, car l’espace blanc est ignoré par le HTML. La signature de l’utilisateur est affichée sous le corps en italique, suivi de la ville d’accueil de l’utilisateur, d’un lien vers sa page d’accueil, de la date et de l’heure du commentaire et du nom d’utilisateur de la personne qui a laissé le commentaire.

Prenez un moment pour afficher la page via un navigateur. Les commentaires que vous avez ajoutés au livre d’or à l’étape 5 doivent apparaître ici.

[![livre d’or. aspx affiche désormais les commentaires du livre d’or](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Figure 19**: `Guestbook.aspx` affiche maintenant les commentaires du livre d’or ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image57.png))

Essayez d’ajouter un nouveau commentaire au livre d’or. Lorsque vous cliquez sur le bouton `PostCommentButton`, la page est publiée et le commentaire est ajouté à la base de données, mais le contrôle ListView n’est pas mis à jour pour afficher le nouveau commentaire. Cela peut être résolu par :

- Mise à jour du gestionnaire d’événements `Click` du bouton `PostCommentButton` afin qu’il appelle la méthode `DataBind()` du contrôle ListView après avoir inséré le nouveau commentaire dans la base de données ou
- Définition de la propriété `EnableViewState` du contrôle ListView sur `false`. Cette approche fonctionne, car en désactivant l’état d’affichage du contrôle, elle doit être reliée aux données sous-jacentes à chaque publication (postback).

Le site Web du didacticiel téléchargeable à partir de ce didacticiel illustre ces deux techniques. La propriété `EnableViewState` du contrôle ListView à `false` et le code nécessaire pour relier par programmation les données à ListView est présent dans le gestionnaire d’événements `Click`, mais il est mis en commentaire.

> [!NOTE]
> Actuellement, la page `AdditionalUserInfo.aspx` permet à l’utilisateur d’afficher et de modifier sa ville d’accueil, sa page d’accueil et ses paramètres de signature. Il peut être utile de mettre à jour `AdditionalUserInfo.aspx` pour afficher les commentaires du livre d’or de l’utilisateur connecté. Autrement dit, en plus d’examiner et de modifier ses informations, un utilisateur peut visiter la page de `AdditionalUserInfo.aspx` pour voir les commentaires de livre d’or qu’il a créés par le passé. Je laisse cela en tant qu’exercice pour le lecteur intéressé.

## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Étape 6 : personnalisation du contrôle CreateUserWizard pour inclure une interface pour la ville d’accueil, la page d’accueil et la signature

La requête `SELECT` utilisée par la page `Guestbook.aspx` utilise un `INNER JOIN` pour combiner les enregistrements associés entre les tables `GuestbookComments`, `UserProfiles`et `aspnet_Users`. Si un utilisateur qui n’a pas d’enregistrement dans `UserProfiles` crée un commentaire de livre d’or, le commentaire ne s’affiche pas dans le ListView, car le `INNER JOIN` retourne uniquement les enregistrements `GuestbookComments` lorsqu’il existe des enregistrements correspondants dans `UserProfiles` et `aspnet_Users`. Comme nous l’avons vu à l’étape 3, si un utilisateur n’a pas d’enregistrement dans `UserProfiles` il ne peut pas afficher ou modifier ses paramètres dans la page de `AdditionalUserInfo.aspx`.

Inutile de préciser, en raison de nos décisions de conception, il est important que chaque compte d’utilisateur dans le système d’appartenance ait un enregistrement correspondant dans le tableau `UserProfiles`. Ce que nous voulons, c’est pour qu’un enregistrement correspondant soit ajouté à `UserProfiles` chaque fois qu’un nouveau compte d’utilisateur d’appartenance est créé via CreateUserWizard.

Comme indiqué dans le didacticiel [*création de comptes d’utilisateur*](creating-user-accounts-cs.md) , une fois le nouveau compte d’utilisateur d’appartenance créé, le contrôle CreateUserWizard déclenche son [événement`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Nous pouvons créer un gestionnaire d’événements pour cet événement, obtenir l’ID utilisateur de l’utilisateur qui vient d’être créé, puis insérer un enregistrement dans la table `UserProfiles` avec les valeurs par défaut pour les colonnes `HomeTown`, `HomepageUrl`et `Signature`. De plus, il est possible d’inviter l’utilisateur à entrer ces valeurs en personnalisant l’interface du contrôle CreateUserWizard pour inclure des zones de texte supplémentaires.

Commençons par examiner comment ajouter une nouvelle ligne à la table `UserProfiles` dans le gestionnaire d’événements `CreatedUser` avec les valeurs par défaut. Après cela, nous verrons comment personnaliser l’interface utilisateur du contrôle CreateUserWizard pour inclure des champs de formulaire supplémentaires afin de collecter la ville d’accueil, la page d’accueil et la signature du nouvel utilisateur.

### <a name="adding-a-default-row-touserprofiles"></a>Ajout d’une ligne par défaut à`UserProfiles`

Dans le didacticiel [*création de comptes d’utilisateur*](creating-user-accounts-cs.md) , nous avons ajouté un contrôle CreateUserWizard à la page `CreatingUserAccounts.aspx` dans le dossier `Membership`. Pour que le contrôle CreateUserWizard ajoute un enregistrement à `UserProfiles` table lors de la création du compte d’utilisateur, nous devons mettre à jour les fonctionnalités du contrôle CreateUserWizard. Au lieu d’apporter ces modifications à la page de `CreatingUserAccounts.aspx`, nous allons ajouter un nouveau contrôle CreateUserWizard à `EnhancedCreateUserWizard.aspx` page et y apporter les modifications pour ce didacticiel.

Ouvrez la page `EnhancedCreateUserWizard.aspx` dans Visual Studio et faites glisser un contrôle CreateUserWizard de la boîte à outils vers la page. Affectez à la propriété `ID` du contrôle CreateUserWizard la valeur `NewUserWizard`. Comme nous l’avons vu <a id="_msoanchor_5"> </a>dans le didacticiel [*création de comptes d’utilisateur*](creating-user-accounts-cs.md) , l’interface utilisateur par défaut de CreateUserWizard invite le visiteur à saisir les informations nécessaires. Une fois ces informations fournies, le contrôle crée en interne un nouveau compte d’utilisateur dans l’infrastructure d’appartenance, sans avoir à écrire une seule ligne de code.

Le contrôle CreateUserWizard déclenche un certain nombre d’événements pendant son flux de travail. Lorsqu’un visiteur fournit les informations de demande et envoie le formulaire, le contrôle CreateUserWizard déclenche initialement son [événement`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Si un problème survient au cours du processus de création, l' [événement`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) est déclenché ; Toutefois, si l’utilisateur est correctement créé, l' [événement`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) est déclenché. Dans le <a id="_msoanchor_6"> </a>didacticiel [*création de comptes d’utilisateur*](creating-user-accounts-cs.md) , nous avons créé un gestionnaire d’événements pour l’événement `CreatingUser` pour vous assurer que le nom d’utilisateur fourni ne contenait aucun espace de début ou de fin et que le nom d’utilisateur n’apparaissait nulle part dans le mot de passe.

Pour ajouter une ligne dans la table `UserProfiles` pour l’utilisateur que vous venez de créer, nous devons créer un gestionnaire d’événements pour l’événement `CreatedUser`. Au moment du déclenchement de l’événement `CreatedUser`, le compte d’utilisateur a déjà été créé dans l’infrastructure d’appartenance, ce qui nous permet de récupérer la valeur UserId du compte.

Créez un gestionnaire d’événements pour l’événement `CreatedUser` de l' `NewUserWizard`et ajoutez le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Le code ci-dessus en récupérant l’ID d’utilisateur du compte d’utilisateur qui vient d’être ajouté. Pour ce faire, utilisez la méthode `Membership.GetUser(username)` pour renvoyer des informations sur un utilisateur particulier, puis utilisez la propriété `ProviderUserKey` pour récupérer son UserId. Le nom d’utilisateur entré par l’utilisateur dans le contrôle CreateUserWizard est disponible par le biais de sa [propriété`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Ensuite, la chaîne de connexion est récupérée à partir de `Web.config` et l’instruction `INSERT` est spécifiée. Les objets ADO.NET nécessaires sont instanciés et la commande est exécutée. Le code assigne une instance de [`DBNull`](https://msdn.microsoft.com/library/system.dbnull.aspx) aux paramètres `@HomeTown`, `@HomepageUrl`et `@Signature`, ce qui a pour effet d’insérer des valeurs de `NULL` de base de données pour les champs `HomeTown`, `HomepageUrl`et `Signature`.

Accédez à la page `EnhancedCreateUserWizard.aspx` via un navigateur et créez un nouveau compte d’utilisateur. Après cela, revenez à Visual Studio et examinez le contenu des tables `aspnet_Users` et `UserProfiles` (comme nous l’avons fait dans la figure 12). Vous devez voir le nouveau compte d’utilisateur dans `aspnet_Users` et une ligne de `UserProfiles` correspondante (avec des valeurs `NULL` pour `HomeTown`, `HomepageUrl`et `Signature`).

[![un nouveau compte d’utilisateur et un nouvel enregistrement UserProfiles ont été ajoutés](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Figure 20**: un nouveau compte d’utilisateur et un nouvel enregistrement de `UserProfiles` ont été ajoutés ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image60.png))

Une fois que le visiteur a fourni ses nouvelles informations de compte et cliqué sur le bouton « créer un utilisateur », le compte d’utilisateur est créé et une ligne est ajoutée à la table `UserProfiles`. Le CreateUserWizard affiche ensuite son `CompleteWizardStep`, qui affiche un message de réussite et un bouton continuer. Le fait de cliquer sur le bouton continuer entraîne une publication, mais aucune action n’est effectuée, ce qui laisse l’utilisateur coincé sur la page de `EnhancedCreateUserWizard.aspx`.

Nous pouvons spécifier une URL à laquelle envoyer l’utilisateur lorsque l’utilisateur clique sur le bouton continuer via la [propriété`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)du contrôle CreateUserWizard. Affectez à la propriété `ContinueDestinationPageUrl` la valeur « ~/Membership/AdditionalUserInfo.aspx ». Cela amène le nouvel utilisateur à `AdditionalUserInfo.aspx`, où il peut afficher et mettre à jour ses paramètres.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personnalisation de l’interface de CreateUserWizard pour demander à la ville d’accueil, à la page d’accueil et à la signature d’un nouvel utilisateur

L’interface par défaut du contrôle CreateUserWizard est suffisante pour les scénarios de création de compte simples où seules les informations de compte d’utilisateur de base telles que le nom d’utilisateur, le mot de passe et la messagerie électronique doivent être collectées. Mais que se passe-t-il si nous voulions inviter le visiteur à entrer sa ville d’accueil, sa page d’accueil et sa signature lors de la création de son compte ? Il est possible de personnaliser l’interface du contrôle CreateUserWizard pour collecter des informations supplémentaires à l’inscription, et ces informations peuvent être utilisées dans le gestionnaire d’événements `CreatedUser` pour insérer des enregistrements supplémentaires dans la base de données sous-jacente.

Le contrôle CreateUserWizard étend le contrôle de l’Assistant ASP.NET, qui est un contrôle qui permet à un développeur de pages de définir une série de `WizardSteps`ordonnées. Le contrôle de l’Assistant restitue l’étape active et fournit une interface de navigation qui permet au visiteur de parcourir ces étapes. Le contrôle Wizard est idéal pour décomposer une longue tâche en plusieurs étapes courtes. Pour plus d’informations sur le contrôle de l’Assistant, consultez [création d’une interface utilisateur pas à pas avec le contrôle de l’assistant ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Le balisage par défaut du contrôle CreateUserWizard définit deux `WizardSteps`: `CreateUserWizardStep` et `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

La première `WizardStep`, `CreateUserWizardStep`, restitue l’interface qui invite à entrer le nom d’utilisateur, le mot de passe, l’adresse de messagerie, etc. Une fois que le visiteur a fourni ces informations et cliqué sur « Create User » (créer un utilisateur), il affiche le `CompleteWizardStep`, qui affiche le message de réussite et un bouton continue.

Pour personnaliser l’interface du contrôle CreateUserWizard de manière à inclure des champs de formulaire supplémentaires, nous pouvons :

- <strong>Créez un ou plusieurs nouveaux</strong> <strong>`WizardStep`</strong> <strong>s pour contenir les éléments d’interface utilisateur supplémentaires</strong>. Pour ajouter un nouveau `WizardStep` à CreateUserWizard, cliquez sur le lien « ajouter/supprimer des `WizardSteps`» de sa balise active pour lancer l’éditeur de collections `WizardStep`. À partir de là, vous pouvez ajouter, supprimer ou réorganiser les étapes de l’Assistant. Il s’agit de l’approche que nous allons utiliser pour ce didacticiel.

- <strong>Convertit le</strong> <strong>`CreateUserWizardStep`</strong> <strong>en`WizardStep`modifiable</strong> <strong>.</strong> Cela remplace le `CreateUserWizardStep` par un `WizardStep` équivalent dont le balisage définit une interface utilisateur qui correspond à la `CreateUserWizardStep`. En convertissant le `CreateUserWizardStep` en `WizardStep` nous pouvons repositionner les contrôles ou ajouter des éléments d’interface utilisateur supplémentaires à cette étape. Pour convertir le `CreateUserWizardStep` ou le `CompleteWizardStep` en `WizardStep`modifiable, cliquez sur le lien « Personnaliser l’étape de création d’utilisateur » ou « personnaliser l’étape » à partir de la balise active du contrôle.

- **Utilisez une combinaison des deux options ci-dessus.**

Une chose importante à garder à l’esprit est que le contrôle CreateUserWizard exécute son processus de création de compte d’utilisateur lorsque l’utilisateur clique sur le bouton « créer un utilisateur » dans son `CreateUserWizardStep`. Peu importe s’il existe des `WizardStep` supplémentaires après l' `CreateUserWizardStep`.

Quand vous ajoutez une `WizardStep` personnalisée au contrôle CreateUserWizard pour collecter des entrées d’utilisateur supplémentaires, le `WizardStep` personnalisé peut être placé avant ou après le `CreateUserWizardStep`. S’il précède la `CreateUserWizardStep`, l’entrée utilisateur supplémentaire collectée à partir de la `WizardStep` personnalisée est disponible pour le gestionnaire d’événements `CreatedUser`. Toutefois, si la `WizardStep` personnalisée vient après `CreateUserWizardStep` après l’affichage de la `WizardStep` personnalisée, le nouveau compte d’utilisateur a déjà été créé et l’événement `CreatedUser` a déjà été déclenché.

La figure 21 illustre le flux de travail lorsque l' `WizardStep` ajoutée précède le `CreateUserWizardStep`. Étant donné que les informations utilisateur supplémentaires ont été collectées au moment du déclenchement de l’événement `CreatedUser`, il vous suffit de mettre à jour le gestionnaire d’événements `CreatedUser` pour récupérer ces entrées et de les utiliser pour les valeurs de paramètres de l’instruction `INSERT` (plutôt que `DBNull.Value`).

[![le flux de travail CreateUserWizard lorsqu’un WizardStep supplémentaire précède le CreateUserWizardStep](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Figure 21**: workflow CreateUserWizard lorsqu’un `WizardStep` supplémentaire précède le `CreateUserWizardStep` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image63.png))

Toutefois, si la `WizardStep` personnalisée est placée *après* l' `CreateUserWizardStep`, le processus de création d’un compte d’utilisateur se produit avant que l’utilisateur ait eu l’occasion d’entrer sa ville d’accueil, sa page d’accueil ou sa signature. Dans ce cas, ces informations supplémentaires doivent être insérées dans la base de données après la création du compte d’utilisateur, comme illustré dans la figure 22.

[![le workflow CreateUserWizard lorsqu’un WizardStep supplémentaire vient après CreateUserWizardStep](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Figure 22**: workflow CreateUserWizard lorsqu’un `WizardStep` supplémentaire vient après l' `CreateUserWizardStep` ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image66.png))

Le workflow illustré à la figure 22 attend l’insertion d’un enregistrement dans la table `UserProfiles` jusqu’à la fin de l’étape 2. Si le visiteur ferme son navigateur après l’étape 1, toutefois, nous aurons atteint un État dans lequel un compte d’utilisateur a été créé, mais aucun enregistrement n’a été ajouté à `UserProfiles`. Une solution de contournement consiste à avoir un enregistrement avec `NULL` ou des valeurs par défaut insérées dans `UserProfiles` dans le gestionnaire d’événements `CreatedUser` (qui se déclenche après l’étape 1), puis à mettre à jour cet enregistrement après l’exécution de l’étape 2. Cela permet de s’assurer qu’un enregistrement de `UserProfiles` est ajouté pour le compte d’utilisateur, même si l’utilisateur quitte le processus d’inscription à mi-chemin.

Pour ce didacticiel, nous allons créer un `WizardStep` qui se produit après la `CreateUserWizardStep` mais avant le `CompleteWizardStep`. Commençons par faire en sorte que le WizardStep soit en place et configuré, puis nous examinerons le code.

À partir de la balise active du contrôle CreateUserWizard, sélectionnez l’option « Ajouter/supprimer `WizardStep` s », qui affiche la boîte de dialogue de l’éditeur de collections `WizardStep`. Ajoutez un nouveau `WizardStep`, en définissant son `ID` sur `UserSettings`, son `Title` sur « vos paramètres » et son `StepType` sur `Step`. Ensuite, positionnez-le de sorte qu’il se trouve après le `CreateUserWizardStep` (« inscrivez-vous pour votre nouveau compte ») et avant le `CompleteWizardStep` (« terminé »), comme illustré à la figure 23.

[![ajouter un nouvel WizardStep au contrôle CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Figure 23**: ajouter un nouveau `WizardStep` au contrôle CreateUserWizard ([cliquez pour afficher l’image en taille réelle](storing-additional-user-information-cs/_static/image69.png))

Cliquez sur OK pour fermer la boîte de dialogue Éditeur de collection de `WizardStep`. Le nouveau `WizardStep` est prouvé par le balisage déclaratif mis à jour du contrôle CreateUserWizard :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Notez le nouvel élément `<asp:WizardStep>`. Nous devons ajouter l’interface utilisateur pour collecter ici la ville d’accueil, la page d’accueil et la signature du nouvel utilisateur. Vous pouvez entrer ce contenu dans la syntaxe déclarative ou par le biais du concepteur. Pour utiliser le concepteur, sélectionnez l’étape « vos paramètres » dans la liste déroulante de la balise active pour afficher l’étape dans le concepteur.

> [!NOTE]
> La sélection d’un pas à pas détaillé dans la liste déroulante des balises actives met à jour la [propriété`ActiveStepIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)du contrôle CreateUserWizard, qui spécifie l’index de l’étape de départ. Par conséquent, si vous utilisez cette liste déroulante pour modifier l’étape « vos paramètres » dans le concepteur, veillez à rétablir la valeur « s’inscrire pour votre nouveau compte » afin que cette étape s’affiche lorsque les utilisateurs accèdent pour la première fois à la page de `EnhancedCreateUserWizard.aspx`.

Créez une interface utilisateur au sein de l’étape « vos paramètres » qui contient trois contrôles TextBox nommés `HomeTown`, `HomepageUrl`et `Signature`. Une fois cette interface créée, le balisage déclaratif de CreateUserWizard doit ressembler à ce qui suit :

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Accédez à cette page via un navigateur et créez un nouveau compte d’utilisateur, en spécifiant des valeurs pour la ville d’accueil, la page d’accueil et la signature. À la fin de la `CreateUserWizardStep` le compte d’utilisateur est créé dans l’infrastructure d’appartenance et le gestionnaire d’événements `CreatedUser` s’exécute, ce qui ajoute une nouvelle ligne à `UserProfiles`, mais avec une valeur de `NULL` de base de données pour `HomeTown`, `HomepageUrl`et `Signature`. Les valeurs entrées pour la ville d’accueil, la page d’accueil et la signature ne sont jamais utilisées. Le résultat net est un nouveau compte d’utilisateur avec un enregistrement de `UserProfiles` dont les champs `HomeTown`, `HomepageUrl`et `Signature` n’ont pas encore été spécifiés.

Nous devons exécuter le code après l’étape « vos paramètres » qui prend la ville d’habitation, honepage et les valeurs de signature entrées par l’utilisateur et met à jour l’enregistrement de `UserProfiles` approprié. Chaque fois que l’utilisateur passe d’une étape à l’autre dans un contrôle de l’Assistant, l' [événement`ActiveStepChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) de l’Assistant se déclenche. Nous pouvons créer un gestionnaire d’événements pour cet événement et mettre à jour la table `UserProfiles` lorsque l’étape « vos paramètres » est terminée.

Ajoutez un gestionnaire d’événements pour l’événement `ActiveStepChanged` de CreateUserWizard et ajoutez le code suivant :

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Le code ci-dessus commence par déterminer si nous avons simplement atteint l’étape « complète ». Étant donné que l’étape « complète » se produit immédiatement après l’étape « vos paramètres », quand le visiteur atteint l’étape « terminé », cela signifie qu’il vient de terminer l’étape « vos paramètres ».

Dans ce cas, nous devons référencer par programmation les contrôles TextBox dans le `UserSettings WizardStep`. Pour ce faire, vous devez d’abord utiliser la méthode `FindControl` pour référencer le `UserSettings WizardStep`par programme, puis pour référencer les zones de texte à partir de l' `WizardStep`. Une fois les zones de texte référencées, nous sommes prêts à exécuter l’instruction `UPDATE`. L’instruction `UPDATE` a le même nombre de paramètres que l’instruction `INSERT` dans le gestionnaire d’événements `CreatedUser`, mais nous utilisons ici la ville d’accueil, la page d’accueil et les valeurs de signature fournies par l’utilisateur.

Une fois ce gestionnaire d’événements en place, accédez à la page de `EnhancedCreateUserWizard.aspx` via un navigateur et créez un nouveau compte d’utilisateur en spécifiant des valeurs pour la ville d’accueil, la page d’accueil et la signature. Une fois le nouveau compte créé, vous devez être redirigé vers la page `AdditionalUserInfo.aspx`, où la ville d’accueil, la page d’accueil et les informations de signature que vous venez d’entrer sont affichées.

> [!NOTE]
> Notre site Web contient actuellement deux pages à partir desquelles un visiteur peut créer un nouveau compte : `CreatingUserAccounts.aspx` et `EnhancedCreateUserWizard.aspx`. Les pages sitemap et connexion du site Web pointent vers la page de `CreatingUserAccounts.aspx`, mais la page `CreatingUserAccounts.aspx` ne demande pas à l’utilisateur de fournir sa ville d’accueil, sa page d’accueil et ses informations de signature et n’ajoute pas de ligne correspondante à `UserProfiles`. Par conséquent, vous devez mettre à jour la page de `CreatingUserAccounts.aspx` pour qu’elle offre cette fonctionnalité ou mettre à jour la page Sitemap et connexion afin de référencer `EnhancedCreateUserWizard.aspx` au lieu de `CreatingUserAccounts.aspx`. Si vous choisissez cette dernière option, veillez à mettre à jour le fichier `Web.config` du dossier `Membership` pour permettre aux utilisateurs anonymes d’accéder à la page `EnhancedCreateUserWizard.aspx`.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons examiné les techniques de modélisation des données liées aux comptes d’utilisateur au sein de l’infrastructure d’appartenance. En particulier, nous avons examiné les entités de modélisation qui partagent une relation un-à-plusieurs avec les comptes d’utilisateur, ainsi que les données qui partagent une relation un-à-un. En outre, nous avons vu comment ces informations associées pouvaient être affichées, insérées et mises à jour, avec quelques exemples utilisant le contrôle SqlDataSource et d’autres à l’aide du code ADO.NET.

Ce didacticiel termine notre examen des comptes d’utilisateur. À partir du prochain didacticiel, nous nous pencherons sur les rôles. Dans les prochains didacticiels, nous examinerons l’infrastructure des rôles, voyons comment créer des rôles, comment attribuer des rôles aux utilisateurs, comment déterminer les rôles auxquels un utilisateur appartient et comment appliquer l’autorisation basée sur les rôles.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Accès et mise à jour des données dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Contrôle de l’Assistant ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Création d’une interface utilisateur pas à pas avec le contrôle de l’Assistant ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Création de paramètres de contrôle DataSource personnalisés](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personnalisation du contrôle CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Démarrages rapides du contrôle DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Affichage des données avec le contrôle ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Discroisement des contrôles de validation dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Modification de l’insertion et de la suppression de données](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Validation de formulaire dans ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Collecte d’informations d’inscription d’utilisateur personnalisées](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profils dans ASP.NET 2,0](http://www.odetocode.com/Articles/440.aspx)
- [Le contrôle ASP : ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Démarrage rapide des profils utilisateur](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](user-based-authorization-cs.md)
> [Suivant](creating-the-membership-schema-in-sql-server-vb.md)
