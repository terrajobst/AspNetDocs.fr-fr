---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Ajout d’ASP.NET Identity à un projet Web Forms vide ou existant-ASP.NET 4. x
author: raquelsa
description: Ce didacticiel vous montre comment ajouter ASP.NET Identity (le système d’appartenance pour ASP.NET) à une application ASP.NET. Quand vous créez une nouvelle Web Forms ou MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e82951d57f0b8052ee3f6530a7470be7d030206
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584125"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Ajout d’ASP.NET Identity à un projet Web Forms vide ou existant

> Ce didacticiel vous montre comment ajouter des [ASP.net Identity](introduction-to-aspnet-identity.md) (le nouveau système d’appartenance pour ASP.net) à une application ASP.net.
> 
> Lorsque vous créez un nouveau Web Forms ou un projet MVC dans Visual Studio 2017 RTM avec des comptes individuels, Visual Studio installe tous les packages requis et ajoute toutes les classes nécessaires pour vous. Ce didacticiel illustre les étapes permettant d’ajouter ASP.NET Identity la prise en charge de votre projet de Web Forms existant ou d’un nouveau projet vide. Je vais présenter tous les packages NuGet que vous devez installer, ainsi que les classes que vous devez ajouter. Je vais passer en revue les exemples de Web Forms pour l’inscription de nouveaux utilisateurs et la connexion, tout en mettant en surbrillance toutes les API de point d’entrée principales pour la gestion et l’authentification des utilisateurs. Cet exemple utilise l’implémentation par défaut ASP.NET Identity pour le stockage de données SQL qui repose sur Entity Framework. Dans ce didacticiel, nous allons utiliser la base de données locale pour SQL Database.
> 

## <a name="get-started-with-aspnet-identity"></a>Prise en main de ASP.NET Identity

1. Commencez par installer et exécuter [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Sélectionnez **nouveau projet** dans la page de démarrage, ou utilisez le menu et sélectionnez **fichier**, puis **nouveau projet**.
3. Dans le volet gauche, développez **visuel C#** , puis sélectionnez **Web**, puis **application Web ASP.net (.NET Framework)** . Nommez votre projet « WebFormsIdentity » et sélectionnez **OK**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **vide** .
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Notez que le bouton **modifier l’authentification** est désactivé et qu’aucune prise en charge de l’authentification n’est fournie dans ce modèle. Les modèles Web Forms, MVC et API Web vous permettent de sélectionner l’approche d’authentification.

## <a name="add-identity-packages-to-your-app"></a>Ajouter des packages d’identité à votre application

Dans Explorateur de solutions, cliquez avec le bouton droit sur votre projet et sélectionnez **gérer les packages NuGet**. Recherchez et installez le package **Microsoft. Aspnet. Identity. EntityFramework** . 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Notez que ce package installe les packages de dépendances : **EntityFramework** et **Microsoft ASP.net principal d’identité**.

## <a name="add-a-web-form-to-register-users"></a>Ajouter un formulaire Web pour inscrire des utilisateurs

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur votre projet et sélectionnez **Ajouter**, puis **Web Form**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. Dans la boîte de dialogue **spécifier le nom de l’élément** , nommez le nouveau **registre**Web Form, puis sélectionnez **OK** .
3. Remplacez le balisage dans le fichier *Register. aspx* généré par le code ci-dessous. Les modifications du code apparaissent en surbrillance. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Il s’agit simplement d’une version simplifiée du fichier *Register. aspx* créé lorsque vous créez un projet de Web Forms ASP.net. Le balisage ci-dessus ajoute des champs de formulaire et un bouton pour inscrire un nouvel utilisateur.
4. Ouvrez le fichier *Register.aspx.cs* et remplacez le contenu du fichier par le code suivant :

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Le code ci-dessus est une version simplifiée du fichier *Register.aspx.cs* créé lorsque vous créez un projet de Web Forms ASP.net.
    > 2. La classe *IdentityUser* est l’implémentation d’EntityFramework par défaut de l’interface *IUser* . L’interface *IUser* est l’interface minimale pour un utilisateur sur ASP.net Identity noyau.
    > 3. La classe *UserStore* est l’implémentation d’EntityFramework par défaut d’un magasin d’utilisateurs. Cette classe implémente les interfaces minimales du ASP.NET Identity Core : *IUserStore*, *IUserLoginStore*, *IUserClaimStore* et *IUserRoleStore*.
    > 4. La classe *usermanager* expose les API associées à l’utilisateur, qui enregistre automatiquement les modifications apportées au *UserStore*.
    > 5. La classe *IdentityResult* représente le résultat d’une opération d’identité.
5. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur votre projet et sélectionnez **Ajouter**, **Ajoutez le dossier ASP.net** , puis **application\_données**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Ouvrez le fichier *Web. config* et ajoutez une entrée de chaîne de connexion pour la base de données que nous utiliserons pour stocker les informations utilisateur. La base de données sera créée au moment de l’exécution par EntityFramework pour les entités d’identité. La chaîne de connexion est similaire à celle créée pour vous lorsque vous créez un projet de Web Forms. Le code en surbrillance montre le balisage que vous devez ajouter :

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Pour Visual Studio 2015 ou une version ultérieure, remplacez `(localdb)\v11.0` par `(localdb)\MSSQLLocalDB` dans votre chaîne de connexion.
    
7. Cliquez avec le bouton droit sur fichier *Register. aspx* dans votre projet, puis sélectionnez **définir comme page de démarrage**. Appuyez sur CTRL + F5 pour générer et exécuter l’application Web. Entrez un nouveau nom d’utilisateur et un nouveau mot de passe, puis sélectionnez **inscrire**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity prend en charge la validation et, dans cet exemple, vous pouvez vérifier le comportement par défaut sur les validateurs d’utilisateur et de mot de passe provenant du package d’identité principal. Le validateur par défaut de l’utilisateur (`UserValidator`) a une propriété `AllowOnlyAlphanumericUserNames` dont la valeur par défaut est définie sur `true`. Le validateur par défaut pour le mot de passe (`MinimumLengthValidator`) garantit que le mot de passe comporte au moins 6 caractères. Ces validateurs sont des propriétés sur `UserManager` qui peuvent être substituées si vous souhaitez avoir une validation personnalisée.

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Vérifiez la base de données d’identités de base de données locale et les tables générées par Entity Framework

1. Dans le menu **affichage** , sélectionnez **Explorateur de serveurs**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Développez **DefaultConnection (WebFormsIdentity)** , développez **tables**, cliquez avec le bouton droit sur **AspNetUsers** , puis sélectionnez **afficher les données**de la table.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Configurer l’application pour l’authentification OWIN

À ce stade, nous avons ajouté uniquement la prise en charge de la création d’utilisateurs. Maintenant, nous allons montrer comment nous pouvons ajouter l’authentification pour se connecter à un utilisateur. ASP.NET Identity utilise l’intergiciel (middleware) d’authentification Microsoft OWIN pour l’authentification par formulaire. OWIN cookie Authentication est un mécanisme d’authentification basé sur les cookies et les revendications qui peut être utilisé par n’importe quelle infrastructure hébergée sur [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) ou IIS. Avec ce modèle, les mêmes packages d’authentification peuvent être utilisés dans plusieurs frameworks, notamment ASP.NET MVC et Web Forms. Pour plus d’informations sur le projet Katana et sur la façon d’exécuter un intergiciel dans un hôte agnostique, consultez [prise en main avec le projet Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Installer des packages d’authentification dans votre application

1. Dans Explorateur de solutions, cliquez avec le bouton droit sur votre projet et sélectionnez **gérer les packages NuGet**. Recherchez et installez le package ***Microsoft. Aspnet. Identity. Owin*** . 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Recherchez et installez le package ***Microsoft. Owin. Host. SystemWeb*** .

    > [!NOTE]
    > Le package **Microsoft. Aspnet. Identity. Owin** contient un ensemble de classes d’extension Owin pour gérer et configurer l’intergiciel (middleware) d’authentification Owin à consommer par ASP.net Identity packages principaux.
    > Le package **Microsoft. Owin. Host. SystemWeb** contient un serveur Owin qui permet aux applications Owin de s’exécuter sur IIS à l’aide du pipeline de demande ASP.net. Pour plus d’informations [, consultez middleware OWIN dans le pipeline intégré IIS](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Ajouter des classes de configuration de démarrage et d’authentification OWIN

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur votre projet, sélectionnez **Ajouter**, puis **Ajouter un nouvel élément**. Dans la boîte de dialogue zone de texte de recherche, tapez «*Owin*». Nommez la classe «*Startup*», puis sélectionnez **Ajouter**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Dans le fichier Startup.cs, ajoutez le code en surbrillance ci-dessous pour configurer l’authentification de cookie OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Cette classe contient l’attribut `OwinStartup` pour la spécification de la classe de démarrage OWIN. Chaque application OWIN a une classe Startup dans laquelle vous spécifiez les composants du pipeline d’application. Pour plus d’informations sur ce modèle, consultez [détection de classe de démarrage OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) .

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Ajouter des Web Forms pour l’inscription et la connexion des utilisateurs

1. Ouvrez le fichier *Register.aspx.cs* et ajoutez le code suivant qui connecte l’utilisateur lorsque l’inscription est réussie.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Étant donné que les ASP.NET Identity et l’authentification des cookies OWIN sont des systèmes basés sur les revendications, l’infrastructure exige que le développeur de l’application génère un [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) pour l’utilisateur. ClaimsIdentity contient des informations sur toutes les revendications pour l’utilisateur, telles que les rôles auxquels l’utilisateur appartient. Vous pouvez également ajouter d’autres revendications pour l’utilisateur à ce niveau.
    > - Vous pouvez connecter l’utilisateur à l’aide du AuthenticationManager à partir de OWIN et en appelant `SignIn` et en passant le ClaimsIdentity comme indiqué ci-dessus. Ce code permet de connecter l’utilisateur et de générer également un cookie. Cet appel est analogue à [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilisé par le module [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
2. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur votre projet, sélectionnez **Ajouter**, puis **Web Form**. Nommez la **connexion**Web Form.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Remplacez le contenu du fichier *login. aspx* par le code suivant :

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Remplacez le contenu du fichier *login.aspx.cs* par le code suivant :

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - Le `Page_Load` vérifie à présent l’état de l’utilisateur actuel et entreprend une action en fonction de son état `Context.User.Identity.IsAuthenticated`.
    >   **Afficher le nom d’utilisateur connecté** : le Framework d’identité Microsoft ASP.net a ajouté des méthodes d’extension sur [System. Security. principal. IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) qui vous permet d’obtenir le `UserName` et `UserId` pour l’utilisateur connecté. Ces méthodes d’extension sont définies dans l’assembly `Microsoft.AspNet.Identity.Core`. Ces méthodes d’extension remplacent [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Méthode de connexion : `This` méthode remplace la méthode `CreateUser_Click` précédente dans cet exemple et connecte désormais l’utilisateur après avoir créé l’utilisateur.   
    >   L’infrastructure Microsoft OWIN a ajouté des méthodes d’extension sur `System.Web.HttpContext` qui vous permettent d’obtenir une référence à une `IOwinContext`. Ces méthodes d’extension sont définies dans `Microsoft.Owin.Host.SystemWeb` assembly. La classe `OwinContext` expose une propriété `IAuthenticationManager` qui représente les fonctionnalités d’intergiciel (middleware) d’authentification disponibles sur la requête actuelle. Vous pouvez connecter l’utilisateur à l’aide de la `AuthenticationManager` à partir de OWIN et en appelant `SignIn` et en passant le `ClaimsIdentity` comme indiqué ci-dessus. Étant donné que ASP.NET Identity et l’authentification des cookies OWIN sont des systèmes basés sur les revendications, l’infrastructure requiert que l’application génère un `ClaimsIdentity` pour l’utilisateur. Le `ClaimsIdentity` contient des informations sur toutes les revendications pour l’utilisateur, telles que les rôles auxquels l’utilisateur appartient. Vous pouvez également ajouter d’autres revendications pour l’utilisateur à ce niveau. ce code se connecte à l’utilisateur et génère également un cookie. Cet appel est analogue à [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) utilisé par le module [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
    > - méthode `SignOut` : obtient une référence au `AuthenticationManager` à partir de OWIN et appelle `SignOut`. Cela est analogue à la méthode [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) utilisée par le module [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
5. Appuyez sur **CTRL + F5** pour générer et exécuter l’application Web. Entrez un nouveau nom d’utilisateur et un nouveau mot de passe, puis sélectionnez **inscrire**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Remarque : à ce stade, le nouvel utilisateur est créé et connecté.
6. Sélectionnez le bouton **se déconnecter** . Vous êtes redirigé vers le formulaire de connexion.
7. Entrez un nom d’utilisateur ou un mot de passe non valide, puis sélectionnez le bouton **se connecter** . 
   La méthode `UserManager.Find` retourne la valeur null et le message d’erreur suivant s’affiche : « *nom d’utilisateur ou mot de passe non valide* ».
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
