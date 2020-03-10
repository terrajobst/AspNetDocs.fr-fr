---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Création et gestion de rôlesC#() | Microsoft Docs
author: rick-anderson
description: Ce didacticiel examine les étapes nécessaires à la configuration de l’infrastructure des rôles. Nous allons ensuite créer des pages Web pour créer et supprimer des rôles.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: a7883d0b05f2fa5a3fdac887f8c8b39d70418fb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640244"
---
# <a name="creating-and-managing-roles-c"></a>Création et gestion de rôles (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Ce didacticiel examine les étapes nécessaires à la configuration de l’infrastructure des rôles. Nous allons ensuite créer des pages Web pour créer et supprimer des rôles.

## <a name="introduction"></a>Introduction

Dans le <a id="_msoanchor_1"> </a>didacticiel [*sur l’autorisation basée sur l’utilisateur*](../membership/user-based-authorization-cs.md) , nous avons examiné l’utilisation de l’autorisation d’URL pour restreindre certains utilisateurs d’un ensemble de pages et j’ai exploré les techniques déclaratives et de programmation pour ajuster les fonctionnalités d’une page ASP.net en fonction de l’utilisateur visité. Toutefois, l’octroi d’autorisations pour l’accès aux pages ou les fonctionnalités utilisateur par utilisateur peut devenir un cauchemar en matière de maintenance dans les scénarios où il existe de nombreux comptes d’utilisateur ou lorsque les privilèges des utilisateurs changent souvent. À chaque fois qu’un utilisateur obtient ou perd l’autorisation d’effectuer une tâche particulière, l’administrateur doit mettre à jour les règles d’autorisation d’URL appropriées, le balisage déclaratif et le code.

Il permet généralement de classer les utilisateurs en groupes ou *rôles* , puis d’appliquer des autorisations pour chaque rôle. Par exemple, la plupart des applications Web ont un certain ensemble de pages ou de tâches réservées aux utilisateurs administratifs. À l’aide des techniques apprises dans le didacticiel *sur les autorisations basées sur l’utilisateur* , nous allons ajouter les règles d’autorisation d’URL appropriées, le balisage déclaratif et le code pour permettre aux comptes d’utilisateur spécifiés d’effectuer des tâches d’administration. Toutefois, si un nouvel administrateur a été ajouté ou si un administrateur existant devait avoir ses droits d’administration révoqués, nous devrions retourner et mettre à jour les fichiers de configuration et les pages Web. Toutefois, avec les rôles, nous pourrions créer un rôle nommé administrateurs et affecter ces utilisateurs approuvés au rôle Administrateurs. Ensuite, nous ajoutons les règles d’autorisation d’URL appropriées, le balisage déclaratif et le code pour permettre au rôle Administrateurs d’effectuer les différentes tâches d’administration. Une fois cette infrastructure en place, l’ajout de nouveaux administrateurs sur le site ou la suppression des administrateurs existants est aussi simple que l’ajout ou la suppression de l’utilisateur du rôle Administrateurs. Aucune configuration, aucune balise déclarative ou modification du code n’est nécessaire.

ASP.NET offre une infrastructure de rôles permettant de définir des rôles et de les associer à des comptes d’utilisateur. Avec l’infrastructure des rôles, nous pouvons créer et supprimer des rôles, ajouter des utilisateurs à un rôle ou en supprimer, déterminer l’ensemble des utilisateurs qui appartiennent à un rôle particulier et savoir si un utilisateur appartient à un rôle particulier. Une fois l’infrastructure de rôles configurée, nous pouvons limiter l’accès aux pages rôle par rôle par le biais de règles d’autorisation d’URL et afficher ou masquer des informations ou des fonctionnalités supplémentaires sur une page en fonction des rôles de l’utilisateur actuellement connecté.

Ce didacticiel examine les étapes nécessaires à la configuration de l’infrastructure des rôles. Nous allons ensuite créer des pages Web pour créer et supprimer des rôles. Dans le <a id="_msoanchor_2"> </a>didacticiel [*attribution de rôles aux utilisateurs*](assigning-roles-to-users-cs.md) , nous allons examiner comment ajouter et supprimer des utilisateurs dans des rôles. Et dans le <a id="_msoanchor_3"> </a>didacticiel sur les [*autorisations basées*](role-based-authorization-cs.md) sur les rôles, nous verrons comment limiter l’accès aux pages rôle par rôle, ainsi que comment ajuster les fonctionnalités de page en fonction du rôle de l’utilisateur visiteur. C’est parti !

## <a name="step-1-adding-new-aspnet-pages"></a>Étape 1 : ajout de nouvelles pages ASP.NET

Dans ce didacticiel et les deux prochaines, nous examinerons les fonctions et fonctionnalités liées aux rôles. Nous aurons besoin d’une série de pages ASP.NET pour implémenter les rubriques examinées dans ces didacticiels. Nous allons créer ces pages et mettre à jour le plan du site.

Commencez par créer un nouveau dossier dans le projet nommé `Roles`. Ensuite, ajoutez quatre nouvelles pages ASP.NET au dossier `Roles`, en liant chaque page à la page maître `Site.master`. Nommer les pages :

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

À ce stade, le Explorateur de solutions de votre projet doit ressembler à la capture d’écran illustrée à la figure 1.

[![quatre nouvelles pages ont été ajoutées au dossier Roles](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figure 1**: quatre nouvelles pages ont été ajoutées au dossier `Roles` ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image3.png))

À ce stade, chaque page doit avoir les deux contrôles de contenu, un pour chaque ContentPlaceHolders : `MainContent` et `LoginContent`de la page maître.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

N’oubliez pas que le balisage par défaut de `LoginContent` ContentPlaceHolder affiche un lien permettant d’ouvrir une session ou de se déconnecter du site, selon que l’utilisateur est authentifié ou non. La présence du contrôle de contenu `Content2` dans la page ASP.NET, cependant, remplace le balisage par défaut de la page maître. Comme nous l’avons <a id="_msoanchor_4"> </a>vu dans [*une vue d’ensemble du didacticiel sur l’authentification par formulaire*](../introduction/an-overview-of-forms-authentication-cs.md) , le remplacement du balisage par défaut est utile dans les pages où vous ne souhaitez pas afficher les options relatives à la connexion dans la colonne de gauche.

Toutefois, pour ces quatre pages, nous souhaitons afficher le balisage par défaut de la page maître pour le `LoginContent` ContentPlaceHolder. Par conséquent, supprimez le balisage déclaratif pour le contrôle de contenu `Content2`. Après cela, chaque balisage de la page en quatre pages ne doit contenir qu’un seul contrôle de contenu.

Enfin, nous allons mettre à jour le plan de site (`Web.sitemap`) pour inclure ces nouvelles pages Web. Ajoutez le code XML suivant après le `<siteMapNode>` que nous avons ajouté pour les didacticiels d’adhésion.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Une fois le plan de site mis à jour, accédez au site via un navigateur. Comme le montre la figure 2, la navigation à gauche comprend désormais des éléments pour les didacticiels de rôles.

[![quatre nouvelles pages ont été ajoutées au dossier Roles](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figure 2**: quatre nouvelles pages ont été ajoutées au dossier `Roles` ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image6.png))

## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Étape 2 : spécification et configuration du fournisseur de l’infrastructure des rôles

À l’instar de l’infrastructure d’appartenance, l’infrastructure de rôles est générée au-dessus du modèle de fournisseur. Comme indiqué dans le <a id="_msoanchor_5"> </a>didacticiel sur la [*sécurité et la prise en charge ASP.net*](../introduction/security-basics-and-asp-net-support-cs.md) , le .NET Framework est fourni avec trois fournisseurs de rôles intégrés : [`AuthorizationStoreRoleProvider`](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx), [`WindowsTokenRoleProvider`](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)et [`SqlRoleProvider`](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Cette série de didacticiels se concentre sur le `SqlRoleProvider`, qui utilise une base de données Microsoft SQL Server comme magasin de rôles.

Sous le couvre l’infrastructure des rôles et `SqlRoleProvider` fonctionnent comme l’infrastructure d’appartenance et les `SqlMembershipProvider`. Le .NET Framework contient une classe `Roles` qui sert d’API à l’infrastructure des rôles. La classe `Roles` a des méthodes statiques comme `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`, et ainsi de suite. Quand l’une de ces méthodes est appelée, la classe `Roles` délègue l’appel au fournisseur configuré. Le `SqlRoleProvider` fonctionne avec les tables spécifiques aux rôles (`aspnet_Roles` et `aspnet_UsersInRoles`) en réponse.

Pour pouvoir utiliser le fournisseur de `SqlRoleProvider` dans notre application, nous devons spécifier la base de données à utiliser comme magasin. Le `SqlRoleProvider` s’attend à ce que le magasin de rôles spécifié dispose de certaines tables de base de données, vues et procédures stockées. Ces objets de base de données nécessaires peuvent être ajoutés à l’aide de l' [outil`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). À ce stade, nous avons déjà une base de données avec le schéma nécessaire à la `SqlRoleProvider`. De retour dans <a id="_msoanchor_6"> </a>le didacticiel [*création du schéma d’appartenance dans SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) , nous avons créé une base de données nommée `SecurityTutorials.mdf` et utilisé `aspnet_regsql.exe` pour ajouter les services d’application, qui comprenaient les objets de base de données requis par le `SqlRoleProvider`. Par conséquent, nous devons simplement indiquer à l’infrastructure des rôles d’activer la prise en charge des rôles et d’utiliser le `SqlRoleProvider` avec la base de données `SecurityTutorials.mdf` comme magasin de rôles.

L’infrastructure de rôles est configurée via le &lt;`roleManager`&gt; élément dans le fichier de `Web.config` de l’application. Par défaut, la prise en charge des rôles est désactivée. Pour l’activer, vous devez définir le [&lt;`roleManager`&gt;](https://msdn.microsoft.com/library/ms164660.aspx) attribut `enabled` de l’élément pour qu’il `true` comme suit :

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Par défaut, toutes les applications Web ont un fournisseur de rôles nommé `AspNetSqlRoleProvider` de type `SqlRoleProvider`. Ce fournisseur par défaut est inscrit dans `machine.config` (situé dans `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`) :

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

L’attribut `connectionStringName` du fournisseur spécifie le magasin de rôles utilisé. Le fournisseur `AspNetSqlRoleProvider` définit cet attribut sur `LocalSqlServer`, qui est également défini dans `machine.config` et points, par défaut, dans une base de données SQL Server 2005 Express Edition dans le dossier `App_Data` nommé `aspnet.mdf`.

Par conséquent, si nous activons simplement l’infrastructure Roles sans spécifier d’informations de fournisseur dans le fichier de `Web.config` de l’application, l’application utilise le fournisseur de rôles inscrits par défaut, `AspNetSqlRoleProvider`. Si la base de données `~/App_Data/aspnet.mdf` n’existe pas, le runtime ASP.NET le crée automatiquement et ajoute le schéma des services d’application. Toutefois, nous ne souhaitons pas utiliser la base de données `aspnet.mdf` ; au lieu de cela, nous souhaitons utiliser la base de données `SecurityTutorials.mdf` que nous avons déjà créée et auquel vous avez ajouté le schéma des services d’application. Cette modification peut être effectuée de deux manières :

- <strong>Spécifiez une valeur pour le</strong> nom de la chaîne de connexion<strong>`LocalSqlServer`</strong> <strong>dans</strong> <strong>`Web.config`</strong> <strong>.</strong> En remplaçant la valeur de nom de la chaîne de connexion `LocalSqlServer` dans `Web.config`, nous pouvons utiliser le fournisseur de rôles inscrits par défaut (`AspNetSqlRoleProvider`) et le faire fonctionner correctement avec la base de données `SecurityTutorials.mdf`. Pour plus d’informations sur cette technique, consultez le billet de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [configuring ASP.NET 2,0 Services d’application to use SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Ajoutez un nouveau fournisseur inscrit de type</strong> <strong>`SqlRoleProvider`</strong> <strong>et configurez son</strong> paramètre<strong>`connectionStringName`</strong> <strong>pour qu’il pointe vers la</strong> <strong>base de données</strong> <strong>`SecurityTutorials.mdf`</strong>. Il s’agit de l’approche recommandée et utilisée dans <a id="_msoanchor_7"> </a>le didacticiel [*création du schéma d’appartenance dans SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) . il s’agit également de l’approche que je vais utiliser dans ce didacticiel.

Ajoutez la balise de configuration des rôles suivante au fichier `Web.config`. Ce balisage inscrit un nouveau fournisseur nommé `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

Le balisage ci-dessus définit le `SecurityTutorialsSqlRoleProvider` comme fournisseur par défaut (via l’attribut `defaultProvider` dans l’élément `<roleManager>`). Il définit également le paramètre de `applicationName` de l' `SecurityTutorialsSqlRoleProvider`sur `SecurityTutorials`, qui est le même paramètre de `applicationName` utilisé par le fournisseur d’appartenances (`SecurityTutorialsSqlMembershipProvider`). Bien que cela ne soit pas illustré ici, l' [élément`<add>`](https://msdn.microsoft.com/library/ms164662.aspx) de la `SqlRoleProvider` peut également contenir un attribut `commandTimeout` pour spécifier la durée du délai d’expiration de la base de données, en secondes. La valeur par défaut est 30.

Avec ce balisage de configuration en place, nous sommes prêts à commencer à utiliser les fonctionnalités de rôle dans notre application.

> [!NOTE]
> Le balisage de configuration ci-dessus illustre l’utilisation de l' &lt;`roleManager`&gt; `enabled` et les attributs de `defaultProvider` de l’élément. Il existe un certain nombre d’autres attributs qui affectent la façon dont l’infrastructure de rôles associe les informations de rôle à chaque utilisateur. Nous examinerons ces paramètres dans le <a id="_msoanchor_8"> </a>didacticiel [*sur les autorisations basées sur les rôles*](role-based-authorization-cs.md) .

## <a name="step-3-examining-the-roles-api"></a>Étape 3 : examen de l’API des rôles

La fonctionnalité de l’infrastructure Roles est exposée via la [classe`Roles`](https://msdn.microsoft.com/library/system.web.security.roles.aspx), qui contient treize méthodes statiques pour effectuer des opérations basées sur les rôles. Lorsque nous examinons la création et la suppression de rôles dans les étapes 4 et 6, nous allons utiliser les méthodes [`CreateRole`](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) et [`DeleteRole`](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) , qui ajoutent ou suppriment un rôle du système.

Pour obtenir la liste de tous les rôles dans le système, utilisez la [méthode`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (Voir l’étape 5). La [méthode`RoleExists`](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) retourne une valeur booléenne indiquant si un rôle spécifié existe.

Dans le didacticiel suivant, nous allons examiner comment associer des utilisateurs à des rôles. Les méthodes [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [`AddUserToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [`AddUsersToRole`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)et [`AddUsersToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) de la classe `Roles` ajouter un ou plusieurs utilisateurs à un ou plusieurs rôles. Pour supprimer des utilisateurs des rôles, utilisez les méthodes [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [`RemoveUserFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [`RemoveUsersFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)ou [`RemoveUsersFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) .

Dans le <a id="_msoanchor_9"> </a>didacticiel [*sur l’autorisation basée*](role-based-authorization-cs.md) sur les rôles, nous allons examiner les différentes façons d’afficher ou de masquer des fonctionnalités en fonction du rôle de l’utilisateur actuellement connecté. Pour ce faire, nous pouvons utiliser les méthodes [`FindUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [`GetRolesForUser`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [`GetUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)ou [`IsUserInRole`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) de la classe `Role`.

> [!NOTE]
> N’oubliez pas que chaque fois que l’une de ces méthodes est appelée, la classe `Roles` délègue l’appel au fournisseur configuré. Dans notre cas, cela signifie que l’appel est envoyé au `SqlRoleProvider`. L' `SqlRoleProvider` effectue ensuite l’opération de base de données appropriée en fonction de la méthode appelée. Par exemple, le code `Roles.CreateRole("Administrators")` produit l' `SqlRoleProvider` exécutant la procédure stockée `aspnet_Roles_CreateRole`, qui insère un nouvel enregistrement dans la table `aspnet_Roles` nommée administrateurs.

Le reste de ce didacticiel se penche sur l’utilisation des méthodes de `CreateRole`, `GetAllRoles`et `DeleteRole` de la classe `Roles` pour gérer les rôles dans le système.

## <a name="step-4-creating-new-roles"></a>Étape 4 : création de nouveaux rôles

Les rôles offrent un moyen de regrouper arbitrairement les utilisateurs et, le plus souvent, ce regroupement est utilisé pour un moyen plus pratique d’appliquer des règles d’autorisation. Toutefois, pour pouvoir utiliser des rôles en tant que mécanisme d’autorisation, nous devons tout d’abord définir les rôles qui existent dans l’application. Malheureusement, ASP.NET n’inclut pas de contrôle CreateRoleWizard. Afin d’ajouter de nouveaux rôles, nous devons créer une interface utilisateur appropriée et appeler l’API Roles. La bonne nouvelle, c’est que cela est très facile à accomplir.

> [!NOTE]
> Bien qu’il n’existe aucun contrôle Web CreateRoleWizard, il existe l' [outil d’administration de site web ASP.net](https://msdn.microsoft.com/library/ms228053.aspx), qui est une application ASP.net locale conçue pour faciliter l’affichage et la gestion de la configuration de votre application Web. Toutefois, je ne suis pas un grand fan de l’outil d’administration de site Web ASP.NET pour deux raisons. Tout d’abord, il s’agit d’un bogue de bits et l’expérience utilisateur laisse beaucoup à désirer. Deuxièmement, l’outil Administration de site Web ASP.NET est conçu pour fonctionner uniquement localement, ce qui signifie que vous devrez créer vos propres pages Web de gestion des rôles si vous avez besoin de gérer des rôles sur un site actif à distance. Pour ces deux raisons, ce didacticiel et la suivante s’intéressent à la création des outils de gestion des rôles nécessaires dans une page Web au lieu de s’appuyer sur l’outil d’administration de site Web ASP.NET.

Ouvrez la page `ManageRoles.aspx` dans le dossier `Roles` et ajoutez une zone de texte et un contrôle Web Button à la page. Définissez la propriété `ID` du contrôle TextBox sur `RoleName` et les propriétés `ID` et `Text` du bouton sur `CreateRoleButton` et créer un rôle, respectivement. À ce stade, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Ensuite, double-cliquez sur le contrôle bouton `CreateRoleButton` dans le concepteur pour créer un gestionnaire d’événements `Click`, puis ajoutez le code suivant :

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

Le code ci-dessus commence par attribuer le nom de rôle conformé entré dans la zone de texte `RoleName` à la variable `newRoleName`. Ensuite, la méthode `RoleExists` de la classe `Roles` est appelée pour déterminer si le rôle `newRoleName` existe déjà dans le système. Si le rôle n’existe pas, il est créé via un appel à la méthode `CreateRole`. Si un nom de rôle qui existe déjà dans le système est passé à la méthode `CreateRole`, une exception `ProviderException` est levée. C’est la raison pour laquelle le code vérifie d’abord si le rôle n’existe pas déjà dans le système avant d’appeler `CreateRole`. Le gestionnaire d’événements `Click` se termine en effaçant la propriété `Text` de la zone de texte `RoleName`.

> [!NOTE]
> Vous vous demandez peut-être ce qui se produit si l’utilisateur n’entre aucune valeur dans la zone de texte `RoleName`. Si la valeur passée dans la méthode `CreateRole` est `null` ou une chaîne vide, une exception est levée. De même, si le nom de rôle contient une virgule, une exception est levée. Par conséquent, la page doit contenir des contrôles de validation pour s’assurer que l’utilisateur entre un rôle et qu’il ne contient pas de virgule. Je laisse un exercice pour le lecteur.

Créons un rôle nommé administrateurs. Accédez à la page de `ManageRoles.aspx` via un navigateur, tapez administrateurs dans la zone de texte (voir figure 3), puis cliquez sur le bouton créer un rôle.

[![créer un rôle Administrateurs](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figure 3**: créer un rôle Administrateurs ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image9.png))

Ce qui se produit Une publication (postback) se produit, mais il n’existe aucun signal visuel indiquant que le rôle a réellement été ajouté au système. Nous mettrons à jour cette page à l’étape 5 pour inclure des commentaires visuels. Pour l’instant, toutefois, vous pouvez vérifier que le rôle a été créé en accédant à la base de données `SecurityTutorials.mdf` et en affichant les données de la table `aspnet_Roles`. Comme le montre la figure 4, la table `aspnet_Roles` contient un enregistrement pour les rôles administrateurs juste ajoutés.

[![la table aspnet_Roles contient une ligne pour les administrateurs](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figure 4**: la table `aspnet_Roles` contient une ligne pour les administrateurs ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image12.png))

## <a name="step-5-displaying-the-roles-in-the-system"></a>Étape 5 : affichage des rôles dans le système

Ajoutons la page `ManageRoles.aspx` pour inclure une liste des rôles actuels dans le système. Pour ce faire, ajoutez un contrôle GridView à la page et affectez à sa propriété `ID` la valeur `RoleList`. Ensuite, ajoutez une méthode à la classe code-behind de la page nommée `DisplayRolesInGrid` à l’aide du code suivant :

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

La méthode `GetAllRoles` de la classe `Roles` retourne tous les rôles du système sous la forme d’un tableau de chaînes. Ce tableau de chaînes est ensuite lié au GridView. Pour lier la liste de rôles au contrôle GridView lorsque la page est chargée pour la première fois, vous devez appeler la méthode `DisplayRolesInGrid` à partir du gestionnaire d’événements `Page_Load` de la page. Le code suivant appelle cette méthode lorsque la page est visitée pour la première fois, mais pas lors des publications (postbacks) suivantes.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Une fois ce code en place, accédez à la page via un navigateur. Comme le montre la figure 5, vous devriez voir une grille avec une seule colonne étiquetée élément. La grille comprend une ligne pour le rôle administrateurs que nous avons ajouté à l’étape 4.

[![le contrôle GridView affiche les rôles dans une seule colonne](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figure 5**: le contrôle GridView affiche les rôles dans une seule colonne ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image15.png))

Le contrôle GridView affiche une colonne solitaire étiquetée Item, car la propriété `AutoGenerateColumns` de GridView a la valeur true (valeur par défaut), ce qui amène le GridView à créer automatiquement une colonne pour chaque propriété de son `DataSource`. Un tableau a une propriété unique qui représente les éléments dans le tableau, donc la colonne unique dans le GridView.

Quand vous affichez des données avec un GridView, je préfère définir des colonnes de manière explicite au lieu de les générer implicitement par le GridView. En définissant explicitement les colonnes, il est beaucoup plus facile de mettre en forme les données, de réorganiser les colonnes et d’effectuer d’autres tâches courantes. Par conséquent, nous allons mettre à jour le balisage déclaratif de GridView afin que ses colonnes soient définies explicitement.

Commencez par affecter la valeur false à la propriété `AutoGenerateColumns` de GridView. Ensuite, ajoutez un TemplateField à la grille, affectez à sa propriété `HeaderText` la valeur Roles, puis configurez son `ItemTemplate` afin qu’il affiche le contenu du tableau. Pour ce faire, ajoutez un contrôle Web Label nommé `RoleNameLabel` au `ItemTemplate` et liez sa propriété `Text` à `Container.DataItem`.

Ces propriétés et le contenu de `ItemTemplate`peuvent être définis de façon déclarative ou par le biais de la boîte de dialogue champs de GridView et de l’interface de modification des modèles. Pour accéder à la boîte de dialogue champs, cliquez sur le lien modifier les colonnes dans la balise active de GridView. Ensuite, désactivez la case à cocher générer automatiquement les champs pour affecter à la propriété `AutoGenerateColumns` la valeur false et ajoutez un TemplateField au GridView, en affectant à sa propriété `HeaderText` la valeur role. Pour définir le contenu de `ItemTemplate`, choisissez l’option modifier les modèles à partir de la balise active de GridView. Faites glisser un contrôle Web Label sur le `ItemTemplate`, définissez sa propriété `ID` sur `RoleNameLabel`et configurez ses paramètres DataBinding afin que sa propriété `Text` soit liée à `Container.DataItem`.

Quelle que soit l’approche que vous utilisez, le balisage déclaratif résultant de GridView doit ressembler à ce qui suit lorsque vous avez terminé.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> Le contenu du tableau est affiché à l’aide de la syntaxe DataBinding `<%# Container.DataItem %>`. Une description détaillée de la raison pour laquelle cette syntaxe est utilisée lors de l’affichage du contenu d’un tableau lié au GridView dépasse le cadre de ce didacticiel. Pour plus d’informations sur ce sujet, reportez-vous à la rubrique [liaison d’un tableau scalaire à un contrôle Web de données](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Actuellement, le `RoleList` GridView est lié uniquement à la liste des rôles lorsque la page est visitée pour la première fois. Nous devons actualiser la grille chaque fois qu’un nouveau rôle est ajouté. Pour ce faire, mettez à jour le gestionnaire d’événements `Click` du bouton `CreateRoleButton` afin qu’il appelle la méthode `DisplayRolesInGrid` si un nouveau rôle est créé.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Désormais, lorsque l’utilisateur ajoute un nouveau rôle, le `RoleList` GridView affiche le rôle qui vient d’être ajouté lors de la publication, ce qui fournit des commentaires visuels indiquant que le rôle a été créé avec succès. Pour illustrer cela, accédez à la page `ManageRoles.aspx` via un navigateur et ajoutez un rôle nommé superviseurs. Quand vous cliquez sur le bouton créer un rôle, une publication s’affiche et la grille est mise à jour pour inclure les administrateurs ainsi que le nouveau rôle, les superviseurs.

[![le rôle superviseurs a été ajouté](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figure 6**: le rôle superviseurs a été ajouté ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image18.png))

## <a name="step-6-deleting-roles"></a>Étape 6 : suppression des rôles

À ce stade, un utilisateur peut créer un nouveau rôle et afficher tous les rôles existants à partir de la page `ManageRoles.aspx`. Permettez aux utilisateurs de supprimer également des rôles. La méthode `Roles.DeleteRole` a deux surcharges :

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) : supprime *le rôle de*rôle. Une exception est levée si le rôle contient un ou plusieurs membres.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) : supprime *le rôle de*rôle. Si *throwOnPopulateRole* est `true`, une exception est levée si le rôle contient un ou plusieurs membres. Si *throwOnPopulateRole* est `false`, le rôle est supprimé, qu’il contienne ou non des membres. En interne, la méthode `DeleteRole(roleName)` appelle `DeleteRole(roleName, true)`.

La méthode `DeleteRole` lèvera également une exception si *roleName* est `null` ou une chaîne vide, ou si *roleName* contient une virgule. Si *roleName* n’existe pas dans le système, `DeleteRole` échoue en mode silencieux, sans lever d’exception.

Ajoutons le contrôle GridView dans `ManageRoles.aspx` pour inclure un bouton supprimer qui, lorsque vous cliquez dessus, supprime le rôle sélectionné. Commencez par ajouter un bouton Supprimer au contrôle GridView en accédant à la boîte de dialogue champs et en ajoutant un bouton supprimer, situé sous l’option CommandField. Faites du bouton supprimer la colonne la plus à gauche et définissez sa propriété `DeleteText` sur supprimer le rôle.

[![ajouter un bouton Supprimer au GridView RoleList](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figure 7**: ajouter un bouton supprimer au `RoleList` GridView ([cliquez pour afficher l’image en taille réelle](creating-and-managing-roles-cs/_static/image21.png))

Après avoir ajouté le bouton supprimer, le balisage déclaratif de votre GridView doit ressembler à ce qui suit :

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Ensuite, créez un gestionnaire d’événements pour l’événement `RowDeleting` de GridView. Il s’agit de l’événement qui est déclenché lors de la publication (postback) lorsque l’utilisateur clique sur le bouton supprimer un rôle. Ajoutez le code suivant au gestionnaire d’événements.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

Le code commence par programmer en référençant le `RoleNameLabel` contrôle Web dans la ligne où l’utilisateur a cliqué sur le bouton supprimer le rôle. La méthode `Roles.DeleteRole` est ensuite appelée, passant le `Text` du `RoleNameLabel` et `false`, supprimant ainsi le rôle, qu’il y ait ou non des utilisateurs associés au rôle. Enfin, le `RoleList` GridView est actualisé de sorte que le rôle juste à supprimer n’apparaisse plus dans la grille.

> [!NOTE]
> Le bouton supprimer un rôle ne nécessite pas de confirmation de la part de l’utilisateur avant de supprimer le rôle. L’une des méthodes les plus simples pour confirmer une action consiste à utiliser une boîte de dialogue de confirmation côté client. Pour plus d’informations sur cette technique, consultez [Ajout d’une confirmation côté client lors de la suppression](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

## <a name="summary"></a>Récapitulatif

De nombreuses applications Web possèdent certaines règles d’autorisation ou des fonctionnalités au niveau de la page qui sont uniquement disponibles pour certaines classes d’utilisateurs. Par exemple, il peut y avoir un ensemble de pages Web auxquelles seuls les administrateurs peuvent accéder. Plutôt que de définir ces règles d’autorisation pour chaque utilisateur, il est souvent plus utile de définir les règles en fonction d’un rôle. Autrement dit, plutôt que d’autoriser explicitement les utilisateurs Scott et Jisun à accéder aux pages Web d’administration, une approche plus facile à gérer consiste à autoriser les membres du rôle administrateurs à accéder à ces pages, puis à indiquer Scott et Jisun en tant qu’utilisateurs appartenant au Rôle Administrateurs.

L’infrastructure Roles facilite la création et la gestion des rôles. Dans ce didacticiel, nous avons examiné comment configurer l’infrastructure des rôles pour utiliser le `SqlRoleProvider`, qui utilise une base de données Microsoft SQL Server comme magasin de rôles. Nous avons également créé une page Web qui répertorie les rôles existants dans le système et permet de créer de nouveaux rôles et de les supprimer. Dans les didacticiels suivants, nous verrons comment affecter des utilisateurs aux rôles et comment appliquer l’autorisation basée sur les rôles.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Examen de l’appartenance, des rôles et du profil de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Procédure : utiliser le gestionnaire de rôles dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Fournisseurs de rôles](https://msdn.microsoft.com/library/aa478950.aspx)
- [Déploiement de votre propre outil d’administration de site Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentation technique pour l’élément `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)
- [Utilisation des API d’appartenance et de gestionnaire de rôles](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel incluent Alicja maziarz, par exemple, Banerjee et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](assigning-roles-to-users-cs.md)
