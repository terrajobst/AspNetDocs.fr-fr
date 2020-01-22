---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Modifier la clé primaire pour les utilisateurs dans ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: Dans Visual Studio 2013, l’application Web par défaut utilise une valeur de chaîne pour la clé pour les comptes d’utilisateur. ASP.NET Identity vous permet de modifier le type de...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519139"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a>Changer la clé principale pour les utilisateurs dans ASP.NET Identity

par [Tom FitzMacken](https://github.com/tfitzmac)

> Dans Visual Studio 2013, l’application Web par défaut utilise une valeur de chaîne pour la clé pour les comptes d’utilisateur. ASP.NET Identity vous permet de modifier le type de la clé pour répondre à vos besoins en matière de données. Par exemple, vous pouvez modifier le type de la clé d’une chaîne en entier.
> 
> Cette rubrique montre comment démarrer avec l’application Web par défaut et remplacer la clé de compte d’utilisateur par un entier. Vous pouvez utiliser les mêmes modifications pour implémenter n’importe quel type de clé dans votre projet. Il montre comment effectuer ces modifications dans l’application Web par défaut, mais vous pouvez appliquer des modifications similaires à une application personnalisée. Il présente les modifications nécessaires lors de l’utilisation de MVC ou de Web Forms.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Visual Studio 2013 avec Update 2 (ou version ultérieure)
> - ASP.NET Identity 2,1 ou version ultérieure

Pour effectuer les étapes de ce didacticiel, vous devez disposer de Visual Studio 2013 Update 2 (ou version ultérieure) et d’une application Web créée à partir du modèle d’application Web ASP.NET. Le modèle a été modifié dans Update 3. Cette rubrique montre comment modifier le modèle dans Update 2 et Update 3.

Cette rubrique contient les sections suivantes :

- [Modifier le type de la clé dans la classe d’utilisateur d’identité](#userclass)
- [Ajouter des classes d’identité personnalisées qui utilisent le type de clé](#customclass)
- [Modifier la classe de contexte et le gestionnaire des utilisateurs pour utiliser le type de clé](#context)
- [Modifier la configuration de démarrage pour utiliser le type de clé](#startup)
- [Pour MVC avec Update 2, modifiez AccountController pour qu’il passe le type de clé](#mvcupdate2)
- [Pour MVC avec Update 3, modifiez AccountController et ManageController pour qu’il passe le type de clé](#mvcupdate3)
- [Pour Web Forms avec Update 2, modifiez les pages de compte pour qu’elles passent le type de clé](#webformsupdate2)
- [Pour Web Forms avec Update 3, modifiez les pages de compte pour qu’elles passent le type de clé](#webformsupdate3)
- [Exécuter l’application](#run)
- [Autres ressources](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Modifier le type de la clé dans la classe d’utilisateur d’identité

Dans votre projet créé à partir du modèle d’application Web ASP.NET, spécifiez que la classe ApplicationUser utilise un entier pour la clé pour les comptes d’utilisateur. Dans IdentityModels.cs, modifiez la classe ApplicationUser pour qu’elle hérite de IdentityUser qui a un type **int** pour le paramètre générique TKey. Vous devez également transmettre les noms de trois classes personnalisées que vous n’avez pas encore implémentées.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Vous avez modifié le type de la clé, mais par défaut, le reste de l’application suppose toujours que la clé est une chaîne. Vous devez indiquer explicitement le type de la clé dans le code qui suppose une chaîne.

Dans la classe **ApplicationUser** , modifiez la méthode **GenerateUserIdentityAsync** pour inclure int, comme indiqué dans le code en surbrillance ci-dessous. Cette modification n’est pas nécessaire pour les projets Web Forms avec le modèle Update 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Ajouter des classes d’identité personnalisées qui utilisent le type de clé

Les autres classes d’identité, telles que IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, au rolestore, sont toujours configurées pour utiliser une clé de chaîne. Créez de nouvelles versions de ces classes qui spécifient un entier pour la clé. Vous n’avez pas besoin de fournir un code d’implémentation très important dans ces classes, vous définissez principalement int comme clé.

Ajoutez les classes suivantes à votre fichier IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Modifier la classe de contexte et le gestionnaire des utilisateurs pour utiliser le type de clé

Dans IdentityModels.cs, modifiez la définition de la classe **ApplicationDbContext** pour utiliser vos nouvelles classes personnalisées et un **int** pour la clé, comme indiqué dans le code en surbrillance.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Le paramètre ThrowIfV1Schema n’est plus valide dans le constructeur. Modifiez le constructeur pour qu’il ne passe pas une valeur ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Ouvrez IdentityConfig.cs, puis modifiez la classe **ApplicationUserManger** pour utiliser votre nouvelle classe de magasin utilisateur pour rendre les données persistantes et un **int** pour la clé.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

Dans le modèle Update 3, vous devez modifier la classe ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Modifier la configuration de démarrage pour utiliser le type de clé

Dans Startup.Auth.cs, remplacez le code OnValidateIdentity, comme indiqué ci-dessous. Notez que la définition getUserIdCallback analyse la valeur de chaîne en un entier.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Si votre projet ne reconnaît pas l’implémentation générique de la méthode **GetUserId** , vous devrez peut-être mettre à jour le package NuGet ASP.net Identity vers la version 2,1

Vous avez apporté de nombreuses modifications aux classes d’infrastructure utilisées par ASP.NET Identity. Si vous essayez de compiler le projet, vous remarquerez un grand nombre d’erreurs. Heureusement, les erreurs restantes sont toutes similaires. La classe Identity attend un entier pour la clé, mais le contrôleur (ou le Web Form) passe une valeur de chaîne. Dans chaque cas, vous devez convertir une chaîne en et un entier en appelant **GetUserId&lt;int&gt;** . Vous pouvez soit utiliser la liste d’erreurs à partir de la compilation, soit suivre les modifications ci-dessous.

Les modifications restantes dépendent du type de projet que vous créez et de la mise à jour que vous avez installée dans Visual Studio. Vous pouvez accéder directement à la section appropriée à l’aide des liens suivants

- [Pour MVC avec Update 2, modifiez AccountController pour qu’il passe le type de clé](#mvcupdate2)
- [Pour MVC avec Update 3, modifiez AccountController et ManageController pour qu’il passe le type de clé](#mvcupdate3)
- [Pour Web Forms avec Update 2, modifiez les pages de compte pour qu’elles passent le type de clé](#webformsupdate2)
- [Pour Web Forms avec Update 3, modifiez les pages de compte pour qu’elles passent le type de clé](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Pour MVC avec Update 2, modifiez AccountController pour qu’il passe le type de clé

Ouvrez le fichier AccountController.cs. Vous devez modifier les méthodes suivantes.

Méthode **ConfirmEmail**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Dissocier** la méthode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

Méthode **Manage (ManageUserViewModel)**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

Méthode **LinkLoginCallback**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

Méthode **RemoveAccountList**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

Méthode **HasPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Vous pouvez maintenant [exécuter l’application](#run) et inscrire un nouvel utilisateur.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Pour MVC avec Update 3, modifiez AccountController et ManageController pour qu’il passe le type de clé

Ouvrez le fichier AccountController.cs. Vous devez modifier la méthode suivante.

Méthode **ConfirmEmail**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

Méthode **SendCode**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Ouvrez le fichier ManageController.cs. Vous devez modifier les méthodes suivantes.

**Index** , méthode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

Méthodes **RemoveLogin**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

Méthode **AddPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

Méthode **EnableTwoFactorAuthentication**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

Méthode **DisableTwoFactorAuthentication**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

Méthodes **VerifyPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

Méthode **RemovePhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** , méthode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** , méthode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

Méthode **ManageLogins**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

Méthode **LinkLoginCallback**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

Méthode **HasPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

Méthode **HasPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Vous pouvez maintenant [exécuter l’application](#run) et inscrire un nouvel utilisateur.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Pour Web Forms avec Update 2, modifiez les pages de compte pour qu’elles passent le type de clé

Pour Web Forms avec Update 2, vous devez modifier les pages suivantes.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Vous pouvez maintenant [exécuter l’application](#run) et inscrire un nouvel utilisateur.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Pour Web Forms avec Update 3, modifiez les pages de compte pour qu’elles passent le type de clé

Pour Web Forms avec Update 3, vous devez modifier les pages suivantes.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Exécuter l’application

Vous avez terminé toutes les modifications requises pour le modèle d’application Web par défaut. Exécutez l’application et inscrivez un nouvel utilisateur. Après avoir inscrit l’utilisateur, vous remarquerez que la table AspNetUsers a une colonne ID qui est un entier.

![nouvelle clé primaire](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Si vous avez déjà créé les tables ASP.NET Identity avec une clé primaire différente, vous devez apporter des modifications supplémentaires. Si possible, supprimez simplement la base de données existante. La base de données sera recréée avec la conception correcte lorsque vous exécuterez l’application Web et ajouterez un nouvel utilisateur. Si la suppression n’est pas possible, exécutez code First migrations pour modifier les tables. Toutefois, la nouvelle clé primaire d’entier n’est pas configurée en tant que propriété d’identité SQL dans la base de données. Vous devez définir manuellement la colonne ID en tant qu’identité.

<a id="other"></a>
## <a name="other-resources"></a>Autres ressources

- [Vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migration d’un site web existant de l’appartenance SQL vers ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migration des données du fournisseur universel pour les profils d’appartenance et utilisateur vers ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Exemple d’application](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) avec la clé primaire modifiée
