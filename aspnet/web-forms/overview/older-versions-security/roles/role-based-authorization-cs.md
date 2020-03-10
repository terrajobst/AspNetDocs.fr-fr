---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Autorisation basée sur les rôlesC#() | Microsoft Docs
author: rick-anderson
description: Ce didacticiel commence par examiner comment l’infrastructure Roles associe les rôles d’un utilisateur à son contexte de sécurité. Il examine ensuite comment appliquer une URL basée sur les rôles...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 46153ab310bdee814baaa53c372fb92f8a23ce11
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627728"
---
# <a name="role-based-authorization-c"></a>Autorisation basée sur le rôle (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Ce didacticiel commence par examiner comment l’infrastructure Roles associe les rôles d’un utilisateur à son contexte de sécurité. Il examine ensuite comment appliquer les règles d’autorisation d’URL basées sur les rôles. Après cela, nous examinerons l’utilisation de méthodes déclaratives et de programmation pour modifier les données affichées et les fonctionnalités offertes par une page ASP.NET.

## <a name="introduction"></a>Introduction

Dans le <a id="_msoanchor_1"> </a>didacticiel [*sur l’autorisation basée sur l’utilisateur*](../membership/user-based-authorization-cs.md) , nous avons vu comment utiliser l’autorisation d’URL pour spécifier les utilisateurs pouvant accéder à un ensemble de pages particulier. Avec juste un peu de balises dans `Web.config`, nous pourrions demander à ASP.NET d’autoriser uniquement les utilisateurs authentifiés à accéder à une page. Ou nous pouvons décider que seuls les utilisateurs Tito et Bob étaient autorisés, ou indiquer que tous les utilisateurs authentifiés à l’exception de Sam étaient autorisés.

En plus de l’autorisation d’URL, nous avons également examiné les techniques déclaratives et de programmation permettant de contrôler les données affichées et les fonctionnalités offertes par une page en fonction de la visite de l’utilisateur. En particulier, nous avons créé une page qui répertorie le contenu du répertoire actif. N’importe qui peut visiter cette page, mais seuls les utilisateurs authentifiés pouvaient afficher le contenu des fichiers et seul Tito pourrait supprimer les fichiers.

L’application de règles d’autorisation pour chaque utilisateur peut s’étendre à un cauchemar de la comptabilité. Une approche plus facile à gérer consiste à utiliser l’autorisation basée sur les rôles. La bonne nouvelle, c’est que les outils de notre disposition pour l’application des règles d’autorisation fonctionnent aussi bien avec les rôles que pour les comptes d’utilisateur. Les règles d’autorisation d’URL peuvent spécifier des rôles plutôt que des utilisateurs. Le contrôle LoginView, qui restitue une sortie différente pour les utilisateurs authentifiés et anonymes, peut être configuré pour afficher un contenu différent en fonction des rôles de l’utilisateur connecté. Et l’API Roles comprend des méthodes permettant de déterminer les rôles de l’utilisateur connecté.

Ce didacticiel commence par examiner comment l’infrastructure Roles associe les rôles d’un utilisateur à son contexte de sécurité. Il examine ensuite comment appliquer les règles d’autorisation d’URL basées sur les rôles. Après cela, nous examinerons l’utilisation de méthodes déclaratives et de programmation pour modifier les données affichées et les fonctionnalités offertes par une page ASP.NET. C’est parti !

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Comprendre comment les rôles sont associés au contexte de sécurité d’un utilisateur

Chaque fois qu’une demande entre dans le pipeline ASP.NET, elle est associée à un contexte de sécurité, qui comprend des informations identifiant le demandeur. Lors de l’utilisation de l’authentification par formulaire, un ticket d’authentification est utilisé comme jeton d’identité. Comme nous l’avons vu dans la <a id="_msoanchor_2"> </a> [*rubrique vue d’ensemble de*](../introduction/an-overview-of-forms-authentication-cs.md) la configuration de l’authentification par formulaire <a id="_msoanchor_3"> </a>et de l' [*authentification par formulaire et*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) des didacticiels avancés, le `FormsAuthenticationModule` est responsable de la détermination de l’identité du demandeur, qu’il fait pendant l' [événement`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Si un ticket d’authentification valide et non expiré est trouvé, le `FormsAuthenticationModule` le décode pour déterminer l’identité du demandeur. Elle crée un objet `GenericPrincipal` et l’assigne à l’objet `HttpContext.User`. L’objectif d’un principal, comme `GenericPrincipal`, est d’identifier le nom de l’utilisateur authentifié et les rôles auxquels il appartient. Cette fonction est évidente par le fait que tous les objets principal ont une propriété `Identity` et une méthode `IsInRole(roleName)`. Toutefois, l' `FormsAuthenticationModule`ne s’intéresse pas à l’enregistrement des informations de rôle et l’objet `GenericPrincipal` qu’il crée ne spécifie aucun rôle.

Si l’infrastructure de rôles est activée, le [`RoleManagerModule`](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) module http dans après l' `FormsAuthenticationModule` et identifie les rôles de l’utilisateur authentifié lors de l' [`PostAuthenticateRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), qui se déclenche après l’événement `AuthenticateRequest`. Si la demande provient d’un utilisateur authentifié, le `RoleManagerModule` remplace l’objet `GenericPrincipal` créé par le `FormsAuthenticationModule` et le remplace par un [objet`RolePrincipal`](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). La classe `RolePrincipal` utilise l’API Roles pour déterminer les rôles auxquels l’utilisateur appartient.

La figure 1 représente le flux de travail du pipeline ASP.NET lors de l’utilisation de l’authentification par formulaire et de l’infrastructure des rôles. Le `FormsAuthenticationModule` s’exécute en premier, identifie l’utilisateur via son ticket d’authentification et crée un nouvel objet `GenericPrincipal`. Ensuite, le `RoleManagerModule` étapes dans et remplace l’objet `GenericPrincipal` par un objet `RolePrincipal`.

Si un utilisateur anonyme visite le site, ni le `FormsAuthenticationModule` ni le `RoleManagerModule` ne crée un objet principal.

[![les événements de pipeline ASP.NET pour un utilisateur authentifié lors de l’utilisation de l’authentification par formulaire et de l’infrastructure des rôles](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Figure 1**: événements de pipeline ASP.net pour un utilisateur authentifié lors de l’utilisation de l’authentification par formulaire et de l’infrastructure de rôles ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image3.png))

### <a name="caching-role-information-in-a-cookie"></a>Mise en cache des informations de rôle dans un cookie

La méthode `IsInRole(roleName)` de l’objet `RolePrincipal` appelle `Roles.GetRolesForUser` pour obtenir les rôles pour l’utilisateur afin de déterminer si l’utilisateur est membre de *roleName*. Lors de l’utilisation du `SqlRoleProvider`, une requête est générée dans la base de données du magasin de rôles. Lors de l’utilisation de règles d’autorisation d’URL basée sur les rôles, la méthode `IsInRole` de l' `RolePrincipal`est appelée à chaque demande adressée à une page protégée par les règles d’autorisation d’URL basées sur les rôles. Au lieu de devoir rechercher les informations de rôle dans la base de données à chaque demande, l’infrastructure de rôles comprend une option permettant de mettre en cache les rôles de l’utilisateur dans un cookie.

Si le Framework de rôles est configuré pour mettre en cache les rôles de l’utilisateur dans un cookie, le `RoleManagerModule` crée le cookie pendant l' [événement de`EndRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)du pipeline ASP.net. Ce cookie est utilisé dans les demandes suivantes dans le `PostAuthenticateRequest`, c’est-à-dire lors de la création de l’objet `RolePrincipal`. Si le cookie est valide et n’a pas expiré, les données du cookie sont analysées et utilisées pour remplir les rôles de l’utilisateur, ce qui évite aux `RolePrincipal` d’avoir à appeler la classe `Roles` pour déterminer les rôles de l’utilisateur. La figure 2 illustre ce flux de travail.

[![les informations de rôle de l’utilisateur peuvent être stockées dans un cookie pour améliorer les performances](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Figure 2**: les informations de rôle de l’utilisateur peuvent être stockées dans un cookie pour améliorer les performances ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image6.png))

Par défaut, le mécanisme de cookie du cache de rôles est désactivé. Vous pouvez l’activer via le balisage de configuration `<roleManager>` dans `Web.config`. Nous avons abordé l’utilisation de l' [élément`<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx) pour spécifier des <a id="_msoanchor_4"> </a>fournisseurs de rôles dans le didacticiel de [*création et de gestion des rôles*](creating-and-managing-roles-cs.md) . vous devez donc déjà avoir cet élément dans le fichier `Web.config` de votre application. Les paramètres de cookie du cache de rôle sont spécifiés en tant qu’attributs de l’élément `<roleManager>` et sont résumés dans le tableau 1.

> [!NOTE]
> Les paramètres de configuration listés dans le tableau 1 spécifient les propriétés du cookie de cache de rôle qui en résulte. Pour plus d’informations sur les cookies, leur fonctionnement et leurs différentes propriétés, lisez [ce didacticiel sur les cookies](http://www.quirksmode.org/js/cookies.html).

| <strong>Propriété</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Description</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Valeur booléenne qui indique si la mise en cache des cookies est utilisée. La valeur par défaut est `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Nom du cookie de cache de rôle. La valeur par défaut est «. ASPXROLES»».                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Chemin d’accès du cookie de nom de rôle. L’attribut path permet à un développeur de limiter l’étendue d’un cookie à une hiérarchie de répertoires particulière. La valeur par défaut est « / », ce qui indique au navigateur d’envoyer le cookie de ticket d’authentification à toute demande adressée au domaine.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indique les techniques utilisées pour protéger le cookie de cache de rôle. Les valeurs autorisées sont les suivantes : `All` (valeur par défaut); `Encryption`; `None`; et `Validation`. Reportez-vous à l' <a id="_anchor_5"> </a>étape 3 du didacticiel configuration de l' [*authentification par formulaire et Rubriques avancées*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) pour plus d’informations sur ces niveaux de protection.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Valeur booléenne qui indique si une connexion SSL est requise pour transmettre le cookie d’authentification. La valeur par défaut est `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Valeur booléenne qui indique si le délai d’expiration du cookie est réinitialisé chaque fois que l’utilisateur visite le site pendant une seule session. La valeur par défaut est `false`. Cette valeur est pertinente uniquement lorsque `createPersistentCookie` est défini sur `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Spécifie la durée, en minutes, au terme de laquelle le cookie de ticket d’authentification expire. La valeur par défaut est `30`. Cette valeur est pertinente uniquement lorsque `createPersistentCookie` est défini sur `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Valeur booléenne qui spécifie si le cookie de cache de rôle est un cookie de session ou un cookie persistant. Si `false` (valeur par défaut), un cookie de session est utilisé, qui est supprimé lorsque le navigateur est fermé. Si `true`, un cookie persistant est utilisé ; elle expire `cookieTimeout` nombre de minutes après sa création ou après la visite précédente, en fonction de la valeur de `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Spécifie la valeur de domaine du cookie. La valeur par défaut est une chaîne vide, ce qui amène le navigateur à utiliser le domaine à partir duquel il a été émis (par exemple, www.yourdomain.com). Dans ce cas, le cookie n’est <strong>pas</strong> envoyé lors de la demande de sous-domaines, tels que admin.yourdomain.com. Si vous souhaitez que le cookie soit passé à tous les sous-domaines, vous devez personnaliser l’attribut `domain`, en lui affectant la valeur « yourdomain.com ».                                                                                                                                                 |
|    `maxCachedResults`     | Spécifie le nombre maximal de noms de rôles mis en cache dans le cookie. La valeur par défaut est 25. Le `RoleManagerModule` ne crée pas de cookie pour les utilisateurs qui appartiennent à plus de `maxCachedResults` rôles. Par conséquent, la méthode `IsInRole` de l’objet `RolePrincipal` utilise la classe `Roles` pour déterminer les rôles de l’utilisateur. La raison pour laquelle `maxCachedResults` existe est que de nombreux agents utilisateur n’autorisent pas les cookies d’une taille supérieure à 4 096 octets. Ce plafond est donc destiné à réduire la probabilité de dépassement de cette limite de taille. Si vous avez des noms de rôles extrêmement longs, vous souhaiterez peut-être spécifier une plus petite `maxCachedResults` valeur ; contrariwise, si vous avez des noms de rôles extrêmement courts, vous pouvez probablement augmenter cette valeur. |

**Tableau 1 :** Options de configuration du cookie de cache de rôle

Configurons notre application pour utiliser des cookies de cache de rôle non persistant. Pour ce faire, mettez à jour l’élément `<roleManager>` dans `Web.config` pour inclure les attributs liés au cookie suivants :

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

J’ai mis à jour l’élément `<roleManager>` en ajoutant trois attributs : `cacheRolesInCookie`, `createPersistentCookie`et `cookieProtection`. En définissant `cacheRolesInCookie` sur `true`, le `RoleManagerModule` met désormais automatiquement en cache les rôles de l’utilisateur dans un cookie plutôt que d’avoir à rechercher les informations de rôle de l’utilisateur à chaque demande. Je définis explicitement les attributs `createPersistentCookie` et `cookieProtection` sur `false` et `All`, respectivement. Techniquement, je n’ai pas besoin de spécifier des valeurs pour ces attributs, car je les ai attribués à leurs valeurs par défaut, mais je les ai placées ici pour indiquer clairement que je n’utilise pas de cookies persistants et que le cookie est à la fois chiffré et validé.

C’est aussi simple que cela ! Désormais, l’infrastructure des rôles met en cache les rôles des utilisateurs dans les cookies. Si le navigateur de l’utilisateur ne prend pas en charge les cookies, ou si leurs cookies sont supprimés ou perdus, il n’y a pas grand intérêt : l’objet `RolePrincipal` utilisera simplement la classe `Roles` dans le cas où aucun cookie (ou un cookie non valide ou expiré) n’est disponible.

> [!NOTE]
> Le groupe patterns &amp; Practices de Microsoft déconseille à l’aide de cookies persistants cache. Étant donné que le cookie de cache de rôle est suffisant pour prouver l’appartenance à un rôle, si un pirate peut accéder à un cookie d’utilisateur valide, il peut emprunter l’identité de cet utilisateur. La probabilité que cela se produise augmente si le cookie est conservé dans le navigateur de l’utilisateur. Pour plus d’informations sur cette recommandation de sécurité, ainsi que sur d’autres problèmes de sécurité, reportez-vous à la [liste des questions de sécurité pour ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx).

## <a name="step-1-defining-role-based-url-authorization-rules"></a>Étape 1 : définition des règles d’autorisation d’URL basées sur les rôles

Comme indiqué dans le <a id="_msoanchor_6"> </a>didacticiel sur l' [*autorisation basée sur l’utilisateur*](../membership/user-based-authorization-cs.md) , l’autorisation d’URL offre un moyen de limiter l’accès à un ensemble de pages d’utilisateur ou de rôle par rôle. Les règles d’autorisation d’URL sont orthographiées dans `Web.config` à l’aide de l' [élément`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) avec des éléments enfants `<allow>` et `<deny>`. Outre les règles d’autorisation relatives aux utilisateurs présentées dans les didacticiels précédents, chaque `<allow>` et `<deny>` élément enfant peut également inclure les éléments suivants :

- Un rôle particulier
- Liste de rôles délimités par des virgules

Par exemple, les règles d’autorisation d’URL accordent l’accès à ces utilisateurs dans les rôles administrateurs et superviseurs, mais refusent l’accès à tous les autres :

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

L’élément `<allow>` dans le balisage ci-dessus indique que les rôles administrateurs et superviseurs sont autorisés ; l’élément `<deny>` indique que *tous* les utilisateurs sont refusés.

Nous allons configurer notre application afin que les pages `ManageRoles.aspx`, `UsersAndRoles.aspx`et `CreateUserWizardWithRoles.aspx` soient accessibles uniquement aux utilisateurs du rôle administrateurs, tandis que la page `RoleBasedAuthorization.aspx` reste accessible à tous les visiteurs.

Pour ce faire, commencez par ajouter un fichier `Web.config` dans le dossier `Roles`.

[![ajouter un fichier Web. config au répertoire Roles](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Figure 3**: ajouter un fichier de `Web.config` au répertoire `Roles` ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image9.png))

Ensuite, ajoutez le balisage de configuration suivant à `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

L’élément `<authorization>` de la section `<system.web>` indique que seuls les utilisateurs appartenant au rôle administrateurs peuvent accéder aux ressources ASP.NET dans le répertoire `Roles`. L’élément `<location>` définit un autre ensemble de règles d’autorisation d’URL pour la page `RoleBasedAuthorization.aspx`, ce qui permet à tous les utilisateurs de visiter la page.

Après avoir enregistré les modifications apportées à `Web.config`, connectez-vous en tant qu’utilisateur qui n’est pas dans le rôle administrateurs, puis essayez d’accéder à l’une des pages protégées. Le `UrlAuthorizationModule` détectera que vous n’êtes pas autorisé à accéder à la ressource demandée. par conséquent, le `FormsAuthenticationModule` vous redirigera vers la page de connexion. La page de connexion vous redirigera vers la page `UnauthorizedAccess.aspx` (voir figure 4). Cette redirection finale de la page de connexion vers `UnauthorizedAccess.aspx` se produit en raison du code que nous avons ajouté à la page de <a id="_msoanchor_7"> </a>connexion à l’étape 2 du didacticiel [*sur l’autorisation basée sur l’utilisateur*](../membership/user-based-authorization-cs.md) . En particulier, la page de connexion redirige automatiquement tous les utilisateurs authentifiés vers `UnauthorizedAccess.aspx` si la chaîne de chaîne contient un paramètre `ReturnUrl`, car ce paramètre indique que l’utilisateur est arrivé sur la page de connexion après la tentative d’affichage d’une page qu’il n’était pas autorisé à afficher.

[![seuls les utilisateurs du rôle administrateurs peuvent afficher les pages protégées.](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Figure 4**: seuls les utilisateurs du rôle administrateurs peuvent afficher les pages protégées ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image12.png))

Déconnectez-vous, puis connectez-vous en tant qu’utilisateur qui se trouve dans le rôle Administrateurs. Vous devriez maintenant être en mesure d’afficher les trois pages protégées.

[![Tito pouvez visiter la page UsersAndRoles. aspx, car il se trouve dans le rôle Administrateurs](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Figure 5**: Tito peut visiter la page `UsersAndRoles.aspx`, car il se trouve dans le rôle Administrateurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image15.png))

> [!NOTE]
> Lorsque vous spécifiez des règles d’autorisation d’URL pour des rôles ou des utilisateurs, il est important de garder à l’esprit que les règles sont analysées l’une après l’autre, du haut vers le haut. Dès qu’une correspondance est trouvée, l’accès est accordé ou refusé à l’utilisateur, selon si la correspondance a été trouvée dans un élément `<allow>` ou `<deny>`. **Si aucune correspondance n’est trouvée, l’utilisateur est autorisé à y accéder.** Par conséquent, si vous souhaitez restreindre l’accès à un ou plusieurs comptes d’utilisateur, il est impératif d’utiliser un élément `<deny>` comme dernier élément de la configuration d’autorisation d’URL. **Si vos règles d’autorisation d’URL n’incluent pas d'** **élément`<deny>`, l’accès est accordé à tous les utilisateurs.** Pour une discussion plus approfondie sur la façon dont les règles d’autorisation d’URL sont analysées, reportez-vous à la section « Présentation de l’utilisation des règles d’autorisation pour accorder <a id="_msoanchor_8"> </a>ou refuser `UrlAuthorizationModule` l’accès » du didacticiel [*sur l’autorisation basée sur l’utilisateur*](../membership/user-based-authorization-cs.md) .

## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Étape 2 : limitation des fonctionnalités en fonction des rôles de l’utilisateur actuellement connecté

L’autorisation d’URL vous permet de spécifier facilement des règles d’autorisation grossistes qui indiquent les identités autorisées et celles qui ne sont pas autorisées à afficher une page particulière (ou toutes les pages d’un dossier et de ses sous-dossiers). Toutefois, dans certains cas, nous pouvons autoriser tous les utilisateurs à visiter une page, mais limiter les fonctionnalités de la page en fonction des rôles de l’utilisateur visité. Cela peut impliquer l’indication ou le masquage des données en fonction du rôle de l’utilisateur, ou l’offre de fonctionnalités supplémentaires aux utilisateurs qui appartiennent à un rôle particulier.

Ces règles d’autorisation basées sur les rôles affinées peuvent être implémentées de façon déclarative ou par programme (ou par le biais d’une combinaison des deux). Dans la section suivante, nous verrons comment implémenter l’autorisation de granularité fine déclarative via le contrôle LoginView. Après cela, nous explorerons les techniques de programmation. Avant de pouvoir examiner l’application de règles d’autorisation affinées, toutefois, nous devons tout d’abord créer une page dont les fonctionnalités dépendent du rôle de l’utilisateur qui la visite.

Nous allons créer une page qui répertorie tous les comptes d’utilisateur du système dans un GridView. Le contrôle GridView inclut le nom d’utilisateur, l’adresse de messagerie, la date de la dernière connexion et les commentaires de l’utilisateur. Outre l’affichage des informations de chaque utilisateur, le contrôle GridView inclut des fonctionnalités de modification et de suppression. Nous allons commencer par créer cette page avec les fonctionnalités de modification et de suppression disponibles pour tous les utilisateurs. Dans les sections « utilisation du contrôle LoginView » et « limitation par programmation », nous verrons comment activer ou désactiver ces fonctionnalités en fonction du rôle de l’utilisateur visitant.

> [!NOTE]
> La page ASP.NET que nous allons créer utilise un contrôle GridView pour afficher les comptes d’utilisateur. Dans la mesure où cette série de didacticiels se concentre sur l’authentification par formulaire, l’autorisation, les comptes d’utilisateurs et les rôles, je ne souhaite pas consacrer trop de temps à discuter du fonctionnement interne du contrôle GridView. Bien que ce didacticiel fournisse des instructions pas à pas pour configurer cette page, il n’aborde pas en détail les raisons pour lesquelles certains choix ont été effectués, ou les effets spécifiques sur la sortie rendue. Pour un examen approfondi du contrôle GridView, consultez la série de didacticiels mon *[utilisation de données dans ASP.NET 2,0](../../data-access/index.md)* .

Commencez par ouvrir la page `RoleBasedAuthorization.aspx` dans le dossier `Roles`. Faites glisser un contrôle GridView de la page sur le concepteur et définissez sa `ID` sur `UserGrid`. Dans un moment, nous allons écrire du code qui appelle la méthode `Membership.GetAllUsers` et lie l’objet `MembershipUserCollection` résultant au GridView. Le `MembershipUserCollection` contient un objet `MembershipUser` pour chaque compte d’utilisateur dans le système ; les objets `MembershipUser` ont des propriétés comme `UserName`, `Email`, `LastLoginDate`, etc.

Avant d’écrire le code qui lie les comptes d’utilisateur à la grille, commençons par définir les champs du contrôle GridView. À partir de la balise active de GridView, cliquez sur le lien « modifier les colonnes » pour ouvrir la boîte de dialogue champs (voir figure 6). À partir de là, décochez la case « générer automatiquement les champs » dans le coin inférieur gauche. Étant donné que ce GridView doit inclure des fonctionnalités de modification et de suppression, ajoutez une propriété CommandField et affectez à ses propriétés `ShowEditButton` et `ShowDeleteButton` la valeur true. Ensuite, ajoutez quatre champs pour afficher les propriétés `UserName`, `Email`, `LastLoginDate`et `Comment`. Utilisez un BoundField pour les deux propriétés en lecture seule (`UserName` et `LastLoginDate`) et TemplateFields pour les deux champs modifiables (`Email` et `Comment`).

Faire en sorte que le premier BoundField affiche la propriété `UserName` ; définissez ses propriétés `HeaderText` et `DataField` sur « UserName ». Ce champ ne sera pas modifiable. par conséquent, affectez la valeur true à sa propriété `ReadOnly`. Configurez le `LastLoginDate` BoundField en affectant à son `HeaderText` la valeur « Last Login » et à sa `DataField` la valeur « LastLoginDate ». Nous allons mettre en forme la sortie de BoundField pour que seule la date soit affichée (au lieu de la date et de l’heure). Pour ce faire, affectez à la propriété `HtmlEncode` de BoundField la valeur false et à sa propriété `DataFormatString` la valeur «{0:d}». Affectez également la valeur true à la propriété `ReadOnly`.

Définissez les propriétés de `HeaderText` des deux TemplateFields sur « email » et « comment ».

[![les champs du contrôle GridView peuvent être configurés via la boîte de dialogue champs](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Figure 6**: les champs de GridView peuvent être configurés via la boîte de dialogue champs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image18.png))

Nous devons maintenant définir les `ItemTemplate` et `EditItemTemplate` pour les TemplateFields « email » et « comment ». Ajoutez un contrôle Web Label à chaque `ItemTemplate` s et liez leurs propriétés `Text` aux propriétés `Email` et `Comment`, respectivement.

Pour la « messagerie » TemplateField, ajoutez une zone de texte nommée `Email` à son `EditItemTemplate` et liez sa propriété `Text` à la propriété `Email` à l’aide de la liaison de liaison bidirectionnelle. Ajoutez un RequiredFieldValidator et un RegularExpressionValidator au `EditItemTemplate` pour vous assurer qu’un visiteur qui modifie la propriété de l’E-mail a entré une adresse e-mail valide. Pour le « commentaire » TemplateField, ajoutez une zone de texte multiligne nommée `Comment` à son `EditItemTemplate`. Définissez les propriétés `Columns` et `Rows` de la zone de texte sur 40 et 4, respectivement, puis liez sa propriété `Text` à la propriété `Comment` à l’aide de la liaison de liaison bidirectionnelle.

Une fois ces TemplateFields configurés, leur balisage déclaratif doit ressembler à ce qui suit :

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Lors de la modification ou de la suppression d’un compte d’utilisateur, nous devons connaître la valeur de la propriété `UserName` de cet utilisateur. Définissez la propriété `DataKeyNames` de GridView sur « UserName » afin que ces informations soient disponibles via la collection de `DataKeys` de GridView.

Enfin, ajoutez un contrôle ValidationSummary à la page et affectez à sa propriété `ShowMessageBox` la valeur true et à sa propriété `ShowSummary` la valeur false. Avec ces paramètres, le ValidationSummary affiche une alerte côté client si l’utilisateur tente de modifier un compte d’utilisateur avec une adresse de messagerie manquante ou non valide.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Nous avons maintenant terminé le balisage déclaratif de cette page. La tâche suivante consiste à lier l’ensemble des comptes d’utilisateur au contrôle GridView. Ajoutez une méthode nommée `BindUserGrid` à la classe code-behind de la page `RoleBasedAuthorization.aspx` qui lie la `MembershipUserCollection` retournée par `Membership.GetAllUsers` à la `UserGrid` GridView. Appelez cette méthode à partir du gestionnaire d’événements `Page_Load` sur la première page visitée.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Une fois ce code en place, accédez à la page via un navigateur. Comme le montre la figure 7, vous devriez voir un GridView qui répertorie des informations sur chaque compte d’utilisateur dans le système.

[![le UserGrid GridView répertorie des informations sur chaque utilisateur dans le système](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Figure 7**: la `UserGrid` GridView répertorie des informations sur chaque utilisateur du système ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image21.png))

> [!NOTE]
> Le `UserGrid` GridView répertorie tous les utilisateurs dans une interface non paginée. Cette interface de grille simple ne convient pas aux scénarios où il y a plusieurs dizaines d’utilisateurs. L’une des options consiste à configurer le contrôle GridView pour activer la pagination. La méthode `Membership.GetAllUsers` a deux surcharges : une qui n’accepte aucun paramètre d’entrée et retourne tous les utilisateurs et une qui accepte des valeurs entières pour l’index de la page et la taille de la page, et retourne uniquement le sous-ensemble spécifié des utilisateurs. La deuxième surcharge peut être utilisée pour parcourir les utilisateurs de manière plus efficace, car elle retourne simplement le sous-ensemble précis des comptes d’utilisateur, et non pas *tous* . Si vous avez des milliers de comptes d’utilisateur, vous pouvez envisager d’utiliser une interface basée sur des filtres, qui affiche uniquement les utilisateurs dont le nom d’utilisateur commence par un caractère sélectionné, par exemple. Le [`Membership.FindUsersByName method`](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) est idéal pour la création d’une interface utilisateur basée sur des filtres. Nous examinerons la création d’une telle interface dans un prochain didacticiel.

Le contrôle GridView offre une prise en charge intégrée de la modification et de la suppression lorsque le contrôle est lié à un contrôle de source de données correctement configuré, tel que SqlDataSource ou ObjectDataSource. Toutefois, les données de la `UserGrid` GridView sont liées par programmation ; par conséquent, nous devons écrire du code pour effectuer ces deux tâches. En particulier, nous devons créer des gestionnaires d’événements pour les événements `RowEditing`, `RowCancelingEdit`, `RowUpdating`et `RowDeleting` de GridView, qui sont déclenchés lorsqu’un visiteur clique sur les boutons Edit, Cancel, Update ou DELETE du contrôle GridView.

Commencez par créer les gestionnaires d’événements pour les événements `RowEditing`, `RowCancelingEdit`et `RowUpdating` du GridView, puis ajoutez le code suivant :

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

Les gestionnaires d’événements `RowEditing` et `RowCancelingEdit` définissent simplement la propriété `EditIndex` du GridView, puis relient la liste des comptes d’utilisateur à la grille. Les choses intéressantes se produisent dans le gestionnaire d’événements `RowUpdating`. Ce gestionnaire d’événements démarre en s’assurant que les données sont valides, puis récupère la valeur `UserName` du compte d’utilisateur modifié à partir de la collection de `DataKeys`. Les zones de texte `Email` et `Comment` dans les deux `EditItemTemplate`s « TemplateFields » sont ensuite référencées par programmation. Leurs propriétés de `Text` contiennent l’adresse de messagerie et le commentaire modifiés.

Pour mettre à jour un compte d’utilisateur par le biais de l’API d’appartenance, nous devons d’abord obtenir les informations de l’utilisateur, à l’aide d’un appel à `Membership.GetUser(userName)`. Les propriétés `Email` et `Comment` de l’objet `MembershipUser` retournées sont ensuite mises à jour avec les valeurs entrées dans les deux zones de texte de l’interface de modification. Enfin, ces modifications sont enregistrées avec un appel à [`Membership.UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). Le gestionnaire d’événements `RowUpdating` se termine en rétablissant le contrôle GridView à son interface de pré-édition.

Ensuite, créez le `RowDeleting` gestionnaire d’événements, puis ajoutez le code suivant :

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

Le gestionnaire d’événements ci-dessus commence par saisir la valeur `UserName` de la collection de `DataKeys` du contrôle GridView ; Cette valeur `UserName` est ensuite transmise à la [méthode`DeleteUser`](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)de la classe d’appartenance. La méthode `DeleteUser` supprime le compte d’utilisateur du système, y compris les données d’appartenance associées (telles que les rôles auxquels cet utilisateur appartient). Après la suppression de l’utilisateur, la `EditIndex` de la grille est définie sur-1 (au cas où l’utilisateur cliquait sur supprimer alors qu’une autre ligne était en mode édition) et la méthode `BindUserGrid` est appelée.

> [!NOTE]
> Le bouton supprimer ne nécessite aucun type de confirmation de la part de l’utilisateur avant de supprimer le compte d’utilisateur. Je vous encourage à ajouter une forme de confirmation de l’utilisateur pour réduire le risque de suppression accidentelle d’un compte. L’une des méthodes les plus simples pour confirmer une action consiste à utiliser une boîte de dialogue de confirmation côté client. Pour plus d’informations sur cette technique, consultez [Ajout d’une confirmation côté client lors de la suppression](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

Vérifiez que cette page fonctionne comme prévu. Vous devez être en mesure de modifier l’adresse de messagerie et le commentaire de l’utilisateur, ainsi que de supprimer un compte d’utilisateur. Étant donné que la page `RoleBasedAuthorization.aspx` est accessible à tous les utilisateurs, et même aux visiteurs anonymes, vous pouvez consulter cette page et modifier et supprimer des comptes d’utilisateur. Nous allons mettre à jour cette page afin que seuls les utilisateurs des rôles superviseurs et administrateurs puissent modifier l’adresse de messagerie et le commentaire d’un utilisateur, et que seuls les administrateurs puissent supprimer un compte d’utilisateur.

La section « utilisation du contrôle LoginView » examine l’utilisation du contrôle LoginView pour afficher des instructions spécifiques au rôle de l’utilisateur. Si une personne du rôle Administrateurs accède à cette page, nous vous présenterons des instructions sur la modification et la suppression des utilisateurs. Si un utilisateur du rôle Superviseur atteint cette page, nous afficherons des instructions sur la modification des utilisateurs. Et si le visiteur est anonyme ou s’il ne se trouve pas dans le rôle superviseurs ou administrateurs, nous affichons un message expliquant qu’il ne peut pas modifier ou supprimer les informations du compte d’utilisateur. Dans la section « fonctionnalités de limitation par programmation », nous allons écrire du code qui affiche ou masque par programmation les boutons modifier et supprimer en fonction du rôle de l’utilisateur.

### <a name="using-the-loginview-control"></a>Utilisation du contrôle LoginView

Comme nous l’avons vu dans les didacticiels précédents, le contrôle LoginView est utile pour afficher des interfaces différentes pour les utilisateurs authentifiés et anonymes, mais le contrôle LoginView peut également être utilisé pour afficher différents balisages en fonction des rôles de l’utilisateur. Nous allons utiliser un contrôle LoginView pour afficher des instructions différentes en fonction du rôle de l’utilisateur visitant.

Commencez par ajouter un LoginView au-dessus du `UserGrid` GridView. Comme nous l’avons vu précédemment, le contrôle LoginView contient deux modèles intégrés : `AnonymousTemplate` et `LoggedInTemplate`. Entrez un bref message dans ces deux modèles pour informer l’utilisateur qu’il ne peut pas modifier ou supprimer des informations utilisateur.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

En plus des `AnonymousTemplate` et des `LoggedInTemplate`, le contrôle LoginView peut inclure des *RoleGroups*, qui sont des modèles spécifiques aux rôles. Chaque RoleGroup contient une seule propriété, `Roles`, qui spécifie les rôles auxquels le RoleGroup s’applique. La propriété `Roles` peut être définie sur un rôle unique (comme « administrateurs ») ou sur une liste de rôles délimités par des virgules (par exemple, « administrateurs, superviseurs »).

Pour gérer RoleGroups, cliquez sur le lien « Edit RoleGroups » à partir de la balise active du contrôle pour afficher l’éditeur de collections RoleGroup. Ajoutez deux nouveaux RoleGroups. Définissez la propriété `Roles` du premier RoleGroup sur « administrateurs » et la deuxième sur « superviseurs ».

[![gérer les modèles spécifiques aux rôles de LoginView par le biais de l’éditeur de collections RoleGroup](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Figure 8**: gérer les modèles spécifiques aux rôles de LoginView par le biais de l’éditeur de collections RoleGroup ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image24.png))

Cliquez sur OK pour fermer l’éditeur de collections RoleGroup. Cela met à jour le balisage déclaratif de LoginView pour inclure une section `<RoleGroups>` avec un élément enfant `<asp:RoleGroup>` pour chaque RoleGroup défini dans l’éditeur de collections RoleGroup. En outre, la liste déroulante « views » dans la balise active du LoginView, qui a initialement répertorié uniquement les `AnonymousTemplate` et `LoggedInTemplate`, comprend également le RoleGroups ajouté.

Modifiez le RoleGroups afin que les utilisateurs du rôle superviseurs aient des instructions sur la modification des comptes d’utilisateur, tandis que les utilisateurs du rôle administrateurs sont des instructions pour la modification et la suppression. Après avoir apporté ces modifications, le balisage déclaratif de votre LoginView doit ressembler à ce qui suit.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Après avoir apporté ces modifications, enregistrez la page, puis accédez-y dans un navigateur. Tout d’abord, visitez la page en tant qu’utilisateur anonyme. Vous devez voir le message «vous n’êtes pas connecté au système. Par conséquent, vous ne pouvez pas modifier ou supprimer des informations utilisateur.» Connectez-vous ensuite en tant qu’utilisateur authentifié, mais qui n’est ni dans le rôle Superviseur ni dans le rôle Administrateurs. Cette fois, vous devriez voir le message «vous n’êtes pas membre du rôle superviseurs ou administrateurs. Par conséquent, vous ne pouvez pas modifier ou supprimer des informations utilisateur.»

Ensuite, connectez-vous en tant qu’utilisateur membre du rôle superviseurs. Cette fois, vous devriez voir le message spécifique au rôle des superviseurs (voir la figure 9). Et si vous vous connectez en tant qu’utilisateur dans le rôle administrateurs, vous devez voir le message spécifique au rôle Administrateurs (voir la figure 10).

[![Bruce affiche le message spécifique au rôle des superviseurs.](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Figure 9**: Bruce affiche le message spécifique au rôle des superviseurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image27.png))

[![Tito affiche le message spécifique au rôle Administrateurs](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Figure 10**: Tito est affiché dans le message d’administrateur spécifique ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image30.png))

Comme les captures d’écran des figures 9 et 10 s’affichent, le LoginView affiche uniquement un modèle, même si plusieurs modèles sont appliqués. Bruce et Tito sont tous deux connectés aux utilisateurs, mais le LoginView affiche uniquement le RoleGroup correspondant et non le `LoggedInTemplate`. En outre, Tito appartient aux rôles administrateurs et superviseurs, alors que le contrôle LoginView affiche le modèle propre au rôle administrateurs au lieu des superviseurs.

La figure 11 illustre le flux de travail utilisé par le contrôle LoginView pour déterminer le modèle à restituer. Notez que si plusieurs RoleGroup sont spécifiés, le modèle LoginView affiche le *premier* RoleGroup qui correspond. En d’autres termes, si nous avions placé les superviseurs RoleGroup comme premier RoleGroup et les administrateurs comme deuxième, alors, lorsque Tito a visité cette page, il voit le message des superviseurs.

[![le flux de travail du contrôle LoginView pour déterminer le modèle à afficher](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Figure 11**: flux de travail du contrôle LoginView pour déterminer le modèle à afficher ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Limitation des fonctionnalités par programmation

Tandis que le contrôle LoginView affiche des instructions différentes en fonction du rôle de l’utilisateur visitant la page, les boutons modifier et annuler restent visibles pour tous. Nous devons masquer par programmation les boutons modifier et supprimer pour les visiteurs anonymes et les utilisateurs qui ne sont ni le rôle Superviseur ni le rôle Administrateurs. Nous devons masquer le bouton Supprimer pour tous les utilisateurs qui ne sont pas des administrateurs. Pour ce faire, nous allons écrire un peu de code qui référence par programmation le LinkButtons de modification et de suppression de CommandField et définit leurs propriétés de `Visible` sur `false`, si nécessaire.

Le moyen le plus simple de référencer par programmation des contrôles dans une CommandField consiste à le convertir d’abord en modèle. Pour ce faire, cliquez sur le lien « modifier les colonnes » dans la balise active de GridView, sélectionnez le CommandField dans la liste des champs actuels, puis cliquez sur le lien « convertir ce champ en TemplateField ». Cela transforme le CommandField en TemplateField avec un `ItemTemplate` et `EditItemTemplate`. Le `ItemTemplate` contient le LinkButtons de modification et de suppression, tandis que le `EditItemTemplate` héberge les LinkButtons de mise à jour et d’annulation.

[![convertir CommandField en TemplateField](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Figure 12**: convertir CommandField en TemplateField ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image36.png))

Mettez à jour le LinkButtons de modification et de suppression dans la `ItemTemplate`, en affectant aux propriétés de `ID` les valeurs de `EditButton` et `DeleteButton`, respectivement.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Chaque fois que les données sont liées au GridView, le GridView énumère les enregistrements dans sa propriété `DataSource` et génère un objet `GridViewRow` correspondant. À mesure que chaque objet `GridViewRow` est créé, l’événement `RowCreated` est déclenché. Afin de masquer les boutons modifier et supprimer pour les utilisateurs non autorisés, nous devons créer un gestionnaire d’événements pour cet événement et référencer par programmation le LinkButtons de modification et de suppression, en définissant leurs propriétés `Visible` en conséquence.

Créez un gestionnaire d’événements `RowCreated` événement, puis ajoutez le code suivant :

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

N’oubliez pas que l’événement `RowCreated` se déclenche pour *toutes* les lignes GridView, y compris l’en-tête, le pied de page, l’interface de pagineur, et ainsi de suite. Nous voulons uniquement faire référence par programmation au LinkButtons de modification et de suppression si nous traitons une ligne de données qui n’est pas en mode édition (puisque la ligne en mode édition a des boutons mettre à jour et annuler au lieu de modifier et supprimer). Cette vérification est gérée par l’instruction `if`.

Si nous traitons une ligne de données qui n’est pas en mode édition, les LinkButtons de modification et de suppression sont référencés et leurs propriétés `Visible` sont définies en fonction des valeurs booléennes retournées par la méthode `IsInRole(roleName)` de l’objet `User`. L’objet User fait référence au principal créé par le `RoleManagerModule`; par conséquent, la méthode `IsInRole(roleName)` utilise l’API Roles pour déterminer si le visiteur actuel appartient à *roleName*.

> [!NOTE]
> Nous aurions pu utiliser la classe Roles directement, en remplaçant l’appel à `User.IsInRole(roleName)` par un appel à la [méthode`Roles.IsUserInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). J’ai décidé d’utiliser la méthode `IsInRole(roleName)` de l’objet principal dans cet exemple car elle est plus efficace que l’utilisation directe de l’API Roles. Plus haut dans ce didacticiel, nous avons configuré le gestionnaire de rôles pour mettre en cache les rôles de l’utilisateur dans un cookie. Les données de cookie mises en cache sont utilisées uniquement lorsque la méthode `IsInRole(roleName)` du principal est appelée ; les appels directs à l’API de rôles impliquent toujours un voyage vers le magasin de rôles. Même si les rôles ne sont pas mis en cache dans un cookie, l’appel de la méthode `IsInRole(roleName)` de l’objet principal est généralement plus efficace, car lorsqu’il est appelé pour la première fois pendant une demande, il met en cache les résultats. L’API Roles, en revanche, n’effectue pas de mise en cache. Étant donné que l’événement `RowCreated` est déclenché une fois pour chaque ligne du contrôle GridView, l’utilisation d' `User.IsInRole(roleName)` implique un seul voyage vers le magasin de rôles, tandis que `Roles.IsUserInRole(roleName)` nécessite *n* TRIPS, où *n* est le nombre de comptes d’utilisateur affichés dans la grille.

La propriété `Visible` du bouton modifier est définie sur `true` si l’utilisateur visitant cette page se trouve dans le rôle administrateurs ou superviseurs. Sinon, elle a la valeur `false`. La propriété `Visible` du bouton supprimer est définie sur `true` uniquement si l’utilisateur est dans le rôle Administrateurs.

Testez cette page par le biais d’un navigateur. Si vous visitez la page en tant que visiteur anonyme ou en tant qu’utilisateur qui n’est ni un superviseur ni un administrateur, le CommandField est vide. Il existe toujours, mais comme un petit éclat sans les boutons modifier ou supprimer.

> [!NOTE]
> Il est possible de masquer le CommandField ensemble lorsqu’un non-superviseur et un non-administrateur visitent la page. Je laisse cela en tant qu’exercice pour le lecteur.

[![les boutons modifier et supprimer sont masqués pour les non-superviseurs et les non-administrateurs](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Figure 13**: les boutons modifier et supprimer sont masqués pour les non-superviseurs et les non-administrateurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image39.png))

Si un utilisateur qui appartient au rôle superviseurs (mais pas au rôle Administrateurs) est visité, il voit uniquement le bouton modifier.

[![lorsque le bouton modifier est disponible pour les superviseurs, le bouton supprimer est masqué.](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Figure 14**: lorsque le bouton modifier est disponible pour les superviseurs, le bouton supprimer est masqué ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image42.png))

Et si un administrateur visite, elle a accès aux boutons modifier et supprimer.

[![les boutons modifier et supprimer sont uniquement disponibles pour les administrateurs](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Figure 15**: les boutons modifier et supprimer sont uniquement disponibles pour les administrateurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image45.png))

## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Étape 3 : application de règles d’autorisation basées sur les rôles à des classes et des méthodes

À l’étape 2, nous avons limité les fonctionnalités d’édition aux utilisateurs des rôles superviseurs et administrateurs et supprimer les fonctionnalités uniquement aux administrateurs. Pour ce faire, vous devez masquer les éléments d’interface utilisateur associés pour les utilisateurs non autorisés par le biais de techniques de programmation. De telles mesures ne garantissent pas qu’un utilisateur non autorisé ne pourra pas effectuer d’action privilégiée. Il peut y avoir des éléments d’interface utilisateur ajoutés ultérieurement ou que nous avons oublié de masquer pour des utilisateurs non autorisés. Un pirate peut également découvrir une autre façon d’obtenir la page ASP.NET pour exécuter la méthode souhaitée.

Un moyen simple de garantir qu’une fonctionnalité particulière n’est pas accessible par un utilisateur non autorisé consiste à décorer cette classe ou méthode avec l' [attribut`PrincipalPermission`](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Lorsque le Runtime .NET utilise une classe ou exécute l’une de ses méthodes, il vérifie que le contexte de sécurité actuel dispose de l’autorisation. L’attribut `PrincipalPermission` fournit un mécanisme permettant de définir ces règles.

Nous avons examiné l’utilisation de l’attribut `PrincipalPermission` dans <a id="_msoanchor_9"> </a>le didacticiel [*sur l’autorisation basée sur l’utilisateur*](../membership/user-based-authorization-cs.md) . Plus précisément, nous avons vu comment décorer les `SelectedIndexChanged` du GridView et `RowDeleting` gestionnaire d’événements afin qu’ils ne puissent être exécutés que par les utilisateurs authentifiés et Tito, respectivement. L’attribut `PrincipalPermission` fonctionne également aussi bien avec les rôles.

Nous allons démontrer l’utilisation de l’attribut `PrincipalPermission` sur les gestionnaires d’événements `RowUpdating` et `RowDeleting` de GridView pour interdire l’exécution des utilisateurs non autorisés. Il vous suffit d’ajouter l’attribut approprié à chaque définition de fonction :

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

L’attribut du gestionnaire d’événements `RowUpdating` impose que seuls les utilisateurs appartenant aux rôles administrateurs ou superviseurs puissent exécuter le gestionnaire d’événements, où l’attribut sur le gestionnaire d’événements `RowDeleting` limite l’exécution aux utilisateurs du rôle Administrateurs.

> [!NOTE]
> L’attribut `PrincipalPermission` est représenté sous la forme d’une classe dans l’espace de noms `System.Security.Permissions`. Veillez à ajouter une instruction `using System.Security.Permissions` en haut de votre fichier de classe code-behind pour importer cet espace de noms.

Si, en quelque sorte, un non-administrateur tente d’exécuter le gestionnaire d’événements `RowDeleting` ou si un non-superviseur ou un non-administrateur tente d’exécuter le gestionnaire d’événements `RowUpdating`, le Runtime .NET déclenche une `SecurityException`.

[![si le contexte de sécurité n’est pas autorisé à exécuter la méthode, une exception SecurityException est levée](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Figure 16**: si le contexte de sécurité n’est pas autorisé à exécuter la méthode, une `SecurityException` est levée ([cliquez pour afficher l’image en taille réelle](role-based-authorization-cs/_static/image48.png))

En plus des pages ASP.NET, de nombreuses applications ont également une architecture qui comprend différentes couches, telles que la logique métier et les couches d’accès aux données. Ces couches sont généralement implémentées en tant que bibliothèques de classes et offrent des classes et des méthodes pour l’exécution de la logique métier et des fonctionnalités liées aux données. L’attribut `PrincipalPermission` est utile pour appliquer des règles d’autorisation à ces couches également.

Pour plus d’informations sur l’utilisation de l’attribut `PrincipalPermission` pour définir des règles d’autorisation sur des classes et des méthodes, reportez-vous au billet de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/) [Ajout de règles d’autorisation à des couches de données et métier à l’aide de `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment spécifier des règles d’autorisation grossière et fine en fonction des rôles de l’utilisateur. ASP. La fonctionnalité d’autorisation d’URL du réseau permet à un développeur de pages de spécifier les identités auxquelles l’accès aux pages est autorisé ou refusé. Comme nous l’avons vu dans <a id="_msoanchor_10"> </a>le didacticiel sur les [*autorisations basées*](../membership/user-based-authorization-cs.md) sur l’utilisateur, les règles d’autorisation d’URL peuvent être appliquées pour chaque utilisateur. Elles peuvent également être appliquées en fonction du rôle, comme nous l’avons vu à l’étape 1 de ce didacticiel.

Les règles d’autorisation fine grain peuvent être appliquées de façon déclarative ou par programme. À l’étape 2, nous avons examiné l’utilisation de la fonctionnalité RoleGroups du contrôle LoginView pour afficher une sortie différente en fonction des rôles de l’utilisateur visitant. Nous avons également examiné comment déterminer par programmation si un utilisateur appartient à un rôle spécifique et comment ajuster les fonctionnalités de la page en conséquence.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Ajout de règles d’autorisation à des couches de données et métier à l’aide de `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Examen de l’appartenance, des rôles et du profil de ASP.NET 2.0 : utilisation des rôles](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Liste des questions de sécurité pour ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentation technique pour l’élément `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs ouvrages ASP/ASP. NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web de Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est *[Sams vous apprend vous-même ASP.NET 2,0 en 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott peut être contacté au [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à l’adresse [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel incluent les Banerjee et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Dans ce cas, insérez une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](assigning-roles-to-users-cs.md)
> [Suivant](creating-and-managing-roles-vb.md)
