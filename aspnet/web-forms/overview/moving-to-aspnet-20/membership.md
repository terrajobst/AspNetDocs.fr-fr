---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Appartenance | Microsoft Docs
author: microsoft
description: L’appartenance à ASP.NET repose sur la réussite du modèle d’authentification par formulaire à partir de ASP.NET 1. x. L’authentification par formulaire ASP.NET offre un moyen pratique d’Incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: da6fc205bd852a818d65425586cec38fdb08d310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642155"
---
# <a name="membership"></a>Appartenance

par [Microsoft](https://github.com/microsoft)

> L’appartenance à ASP.NET repose sur la réussite du modèle d’authentification par formulaire à partir de ASP.NET 1. x. L’authentification par formulaire ASP.NET offre un moyen pratique d’incorporer un formulaire de connexion dans votre application ASP.NET et de valider les utilisateurs par rapport à une base de données ou à un autre magasin de données.

L’appartenance à ASP.NET repose sur la réussite du modèle d’authentification par formulaire à partir de ASP.NET 1. x. L’authentification par formulaire ASP.NET offre un moyen pratique d’incorporer un formulaire de connexion dans votre application ASP.NET et de valider les utilisateurs par rapport à une base de données ou à un autre magasin de données. Les membres de la classe FormsAuthentication permettent de gérer les cookies pour l’authentification, de vérifier s’il y a une connexion valide, de connecter un utilisateur, etc. Toutefois, l’implémentation de l’authentification par formulaire dans une application ASP.NET 1. x peut nécessiter une quantité de code équitable.

L’appartenance à ASP.NET 2,0 est une avancée majeure par rapport à l’utilisation de l’authentification par formulaire uniquement. (L’appartenance est plus fiable lorsqu’elle est couplée à l’authentification par formulaire, mais l’utilisation de l’authentification par formulaire n’est pas obligatoire.) Comme vous le verrez bientôt, vous pouvez utiliser l’appartenance ASP.NET et les contrôles de connexion dans ASP.NET 2,0 pour implémenter un système d’appartenance puissant sans écrire du code.

## <a name="implementing-membership-in-aspnet-20"></a>Implémentation de l’appartenance dans ASP.NET 2,0

L’appartenance est implémentée en quatre étapes. Gardez à l’esprit qu’il existe de nombreuses sous-étapes impliquées, ainsi que la configuration facultative qui peut également être implémentée. Ces étapes sont destinées à illustrer la présentation de la configuration de l’appartenance.

1. Créez votre base de données d’appartenance (si SQL Server est utilisé comme magasin d’appartenance.)
2. Spécifiez les options d’appartenance dans vos fichiers de configuration d’applications. (L’appartenance est activée par défaut.)
3. Déterminez le type de magasin d’appartenance que vous souhaitez utiliser. Les options sont : 

    - Microsoft SQL Server (version 7,0 ou ultérieure)
    - Magasin de Active Directory
    - Fournisseur d’appartenances personnalisé
4. Configurez l’application pour l’authentification par formulaires ASP.NET. Une fois encore, l’appartenance est conçue pour tirer parti de l’authentification par formulaire, mais l’utilisation de l’authentification par formulaire n’est pas obligatoire.
5. Définissez les comptes d’utilisateur pour l’appartenance et configurez les rôles si vous le souhaitez.

## <a name="creating-the-membership-database"></a>Création de la base de données d’appartenance

Si vous utilisez SQL Server 7,0 ou une version ultérieure comme magasin d’appartenance, vous pouvez utiliser l’utilitaire ASPNET\_RegSql (disponible le plus facilement à partir de l’invite de commandes de Visual Studio .NET 2005) pour configurer votre base de données. L’utilitaire ASPNET\_RegSql peut être utilisé en tant qu’outil d’invite de commandes ou à l’aide d’un assistant d’interface utilisateur graphique. La méthode de l’Assistant est le moyen le plus simple de configurer votre base de données. Pour accéder à l’Assistant, exécutez simplement la commande suivante :

`aspnet_regsql W`

Une fois que vous avez exécuté cette commande, l’Assistant Installation de ASP.NET SQL Server s’affiche, comme indiqué ci-dessous.

![](membership/_static/image1.jpg)

**Figure 1**

L’Assistant Installation de ASP.NET SQL Server crée le site Web dans l’instance que vous spécifiez dans l’Assistant. Toutefois, ASP.NET utilise la chaîne de connexion dans le fichier machine. config pour se connecter à votre base de données. Par défaut, cette chaîne de connexion pointe vers une instance SQL Server 2005, donc si vous utilisez une instance SQL Server 2000 ou SQL Server 7,0, vous devrez modifier la chaîne de connexion dans le fichier machine. config. Cette chaîne de connexion est disponible ici :

[!code-xml[Main](membership/samples/sample1.xml)]

Malheureusement, si vous ne modifiez pas la chaîne de connexion, ASP.NET ne vous donnera pas d’erreur descriptive. Il continuera simplement à se plaindre que vous n’avez pas créé la base de données. Dans le cas ci-dessus, j’ai modifié la chaîne de connexion afin qu’elle pointe vers mon 2000 SQL Server local.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Spécification de la configuration et ajout d’utilisateurs et de rôles

L’étape suivante de la configuration de l’appartenance consiste à ajouter les informations nécessaires au fichier Web. config de l’application. Dans ASP.NET 1. x, la modification du fichier Web. config était parfois difficile en raison de l’utilisation de lowerCamelCase et de l’absence d’IntelliSense. Visual Studio .NET 2005 simplifie grandement la tâche grâce à IntelliSense pour les fichiers de configuration, mais ASP.NET 2,0 va encore plus loin en fournissant une interface Web pour la modification des fichiers de configuration.

Vous pouvez lancer l’interface Web en cliquant sur le bouton de configuration ASP.NET dans la barre d’outils Explorateur de solutions, comme indiqué ci-dessous. Vous pouvez également lancer l’interface Web via des fenêtres contextuelles qui s’affichent lorsque les contrôles de connexion sont insérés.

![](membership/_static/image2.jpg)

**Figure 2**

Cela lance l’outil d’administration de site Web ASP.NET illustré ci-dessous. L’administration du site Web ASP.NET est une interface à quatre onglets qui facilite la gestion des paramètres de l’application. Les onglets suivants sont disponibles :

- **Accueil**
- **Sécurité** Configurer les utilisateurs, les rôles et l’accès.
- **Application** Configurer les paramètres de l’application.
- **Fournisseur** Configurez et testez le fournisseur d’appartenances de vos applications.

L’outil Administration de site Web vous permet de créer facilement des utilisateurs, de créer de nouveaux rôles et de gérer des utilisateurs et des rôles. Cette fonctionnalité n’est pas disponible dans l’interface Windows. L’interface Windows vous permet de définir facilement des paramètres d’autorisation et d’ajouter, de supprimer et de gérer des fournisseurs, des fonctionnalités qui ne sont pas dans l’outil Administration de site Web.

Pour lancer l’interface Windows, ouvrez le composant logiciel enfichable Internet Information Services, cliquez avec le bouton droit sur votre application, puis choisissez Propriétés. Cliquez sur l’onglet ASP.NET, puis sur le bouton modifier la configuration. (L’application doit s’exécuter sous ASP.NET 2,0 pour que le bouton modifier la configuration soit activé. Vous pouvez également configurer la version de ASP.NET dans la boîte de dialogue ASP.NET.) La boîte de dialogue Paramètres de configuration de ASP.NET s’affiche comme indiqué ci-dessous.

![](membership/_static/image3.jpg)

**Figure 3**

Sous l’onglet général, les chaînes de connexion et les paramètres d’application sont répertoriés. Tous les paramètres en italique sont définis dans un fichier de configuration parent (machine. config ou Web. config à un niveau supérieur) et les paramètres qui ne sont pas en italique proviennent du fichier de configuration des applications. Si un paramètre est ajouté, supprimé ou modifié au niveau de l’application, ASP.NET ajoute, supprime ou modifie le paramètre au niveau de l’application Web. config au lieu de supprimer le paramètre du fichier de configuration dont il est hérité.

L’onglet authentification est illustré ci-dessous. C’est là que vous allez configurer vos paramètres d’appartenance. Les paramètres d’authentification par formulaire, les fournisseurs d’appartenances et les fournisseurs de rôles peuvent être configurés ici.

![](membership/_static/image4.jpg)

**Figure 4**

## <a name="implementing-membership-in-your-application"></a>Implémentation de l’appartenance à votre application

Le moyen le plus simple d’implémenter l’appartenance à ASP.NET 2,0 dans votre application consiste à utiliser les contrôles d’ouverture de session fournis. Cette méthode vous permet d’implémenter les notions de base de l’appartenance à ASP.NET 2,0 sans écrire de code.

Les contrôles d’ouverture de session suivants sont disponibles dans ASP.NET 2,0 :

## <a name="login-control"></a>Contrôle de connexion

Le contrôle de connexion fournit une interface permettant à un utilisateur de se connecter à votre système d’appartenance. Il vous fournit une zone de texte nom d’utilisateur et mot de passe, ainsi qu’un bouton de connexion. De nombreuses autres fonctionnalités courantes, telles qu’un lien, s’inscrivent pour les personnes qui ne l’ont pas encore fait, une case à cocher qui permet à l’utilisateur de se connecter automatiquement à des visites ultérieures, un lien pour un rappel de mot de passe, etc. Toutes les fonctionnalités du contrôle de connexion sont personnalisables via les propriétés du contrôle.

Dans ASP.NET 1. x, les développeurs devaient écrire une quantité de code équitable pour effectuer une recherche lors de l’utilisation de l’authentification par formulaire. Avec l’appartenance à ASP.NET 2,0, vous pouvez valider les utilisateurs sans écrire de code. ASP.NET effectue automatiquement la recherche de l’utilisateur pour vous. (Si vous utilisez le contrôle de connexion sans utiliser l’appartenance à ASP.NET, vous pouvez utiliser la méthode **OnAuthenticate** pour valider l’utilisateur.)

## <a name="loginview-control"></a>Contrôle LoginView

Le contrôle LoginView est un contrôle basé sur un modèle qui fournit deux modèles par défaut ; AnonymousTemplate et LoggedInTemplate. Le modèle affiché est déterminé par le fait que l’utilisateur soit connecté ou non à votre système d’appartenance. Ce contrôle est généralement utilisé pour afficher un contrôle de connexion lorsqu’un utilisateur ne s’est pas encore connecté, qu’il s’agit d’un contrôle LoginStatus et/ou d’autres contrôles de connexion quand l’utilisateur s’est connecté. Si vous utilisez la gestion des rôles dans votre application ASP.NET, le contrôle LoginView peut afficher un modèle spécifique basé sur le rôle utilisateurs. (En savoir plus sur la gestion des rôles ASP.NET sera abordé plus tard).

## <a name="passwordrecovery-control"></a>Contrôle PasswordRecovery

Le contrôle PasswordRecovery permet aux utilisateurs de recevoir un e-mail avec son mot de passe actuel ou de réinitialiser son mot de passe. Le texte en clair et les mots de passe chiffrés peuvent être récupérés et envoyés par e-mail aux utilisateurs. Si le mot de passe est haché, il ne peut pas être récupéré. Au lieu de cela, l’utilisateur sera obligé d’effectuer une réinitialisation du mot de passe.

## <a name="loginstatus-control"></a>LoginStatus, contrôle

Le contrôle LoginStatus est utilisé pour afficher un indicateur de connexion aux utilisateurs qui ne sont pas connectés et un indicateur de déconnexion pour les utilisateurs qui sont actuellement connectés. La propriété Request. IsAuthenticated est utilisée pour déterminer l’indicateur à afficher. L’indicateur affiché par le contrôle LoginStatus peut être du texte (implémenté via les propriétés **LoginText** et **LogoutText** ) ou des images (implémentées via les propriétés **LoginImageUrl** et **LogoutImageUrl** ).

Lorsqu’un utilisateur se déconnecte via le contrôle LoginStatus, il est redirigé vers l’URL spécifiée par la propriété **LogoutPageUrl** . Si cette propriété n’est pas définie, la page actuelle est actualisée. Étant donné que le site est probablement protégé par l’authentification par formulaire, l’actualisation de la page actuelle redirige l’utilisateur vers la page de connexion du site.

## <a name="loginname-control"></a>Contrôle LoginName

Le contrôle LoginName affiche le nom d’utilisateur de l’utilisateur actuellement connecté au site.

## <a name="createuserwizard-control"></a>Contrôle CreateUserWizard

Le contrôle CreateUserWizard offre aux utilisateurs un moyen pratique de s’inscrire pour votre système d’appartenance. Vous pouvez ajouter des étapes (implémentées sous la forme d’une collection de WizardSteps) par le biais de l’interface illustrée ci-dessous.

![](membership/_static/image5.jpg)

**Figure 5**

CreateUserWizard est un contrôle basé sur un modèle qui dérive de la classe Wizard et fournit les modèles suivants :

- **HeaderTemplate** Ce modèle contrôle l’apparence de l’en-tête de l’Assistant.
- **SidebarTemplate** Ce modèle contrôle l’apparence de l’encadré de l’Assistant.
- **StartNavigationTemplate** Ce modèle contrôle l’apparence de la navigation dans l’Assistant à l’étape de démarrage.
- **StepNavigationTemplate** Ce modèle contrôle l’apparence de la zone de navigation si elle ne se trouve pas dans l’étape de début ou de fin.
- **FinishNavigationTemplate** Ce modèle contrôle l’apparence de la zone de navigation à l’étape terminer.

En outre, pour chaque étape que vous ajoutez à l’Assistant, ASP.NET crée un modèle personnalisé qui contient à la fois un ContentTemplate et un CustomNavigationTemplate pour cette étape. Pour plus d’informations sur la personnalisation de CreateUserWizard, consultez la documentation de VS.NET 2005 :

## <a name="changepassword-control"></a>ChangePassword, contrôle

Le contrôle ChangePassword permet aux utilisateurs de modifier leur mot de passe. Si la propriété DisplayUserName a la valeur true (la valeur par défaut est false), l’utilisateur peut modifier son mot de passe lorsqu’il n’est pas connecté. Si l’utilisateur *est* déjà connecté et que la propriété DisplayUserName a la valeur true, l’utilisateur sera en mesure de modifier le mot de passe d’un autre utilisateur qui n’est pas connecté, à condition de connaître l’ID d’utilisateur de cet utilisateur.

N’oubliez pas que si vous souhaitez que les utilisateurs puissent modifier les mots de passe sans avoir à se connecter, vous devez vous assurer que la page sur laquelle le contrôle ChangePassword est affiché autorise l’accès anonyme. Évidemment, les utilisateurs devront fournir leur ancien mot de passe afin de modifier leur mot de passe.

## <a name="role-management"></a>Gestion des rôles

La gestion des rôles vous permet d’affecter des utilisateurs à un rôle particulier, puis de limiter l’accès à certains fichiers ou dossiers en fonction de ce rôle. La gestion des rôles fournit également une API qui vous permet de déterminer par programme un rôle de personne ou de déterminer tous les utilisateurs dans un rôle particulier et de répondre en conséquence.

La gestion des rôles n’est pas obligatoire dans l’appartenance à ASP.NET, et n’est pas obligatoire pour l’utilisation de la gestion des rôles. Toutefois, les deux compléments sont satisfaisants et il est probable que les développeurs les utiliseront conjointement.

Pour activer la gestion des rôles dans votre application, apportez les modifications suivantes dans votre fichier Web. config :

[!code-xml[Main](membership/samples/sample2.xml)]

Lorsque l’attribut **CacheRolesInCookie** est défini sur true, ASP.NET met en cache une appartenance au rôle utilisateurs dans un cookie sur le client. Cela permet d’effectuer des recherches de rôle sans appels dans le RoleProvider. Lors de l’utilisation de cet attribut, les développeurs sont encouragés à s’assurer que l’attribut **cookieProtection** est défini sur All. (Il s’agit du paramètre par défaut.) Cela garantit le chiffrement des données de cookie et permet de s’assurer que le contenu des cookies n’a pas été modifié. Des rôles peuvent être ajoutés à l’aide de l’outil Administration de site Web. Elle vous permet de définir facilement des rôles, de configurer l’accès aux parties du site en fonction de ces rôles et d’affecter des utilisateurs à des rôles.

![](membership/_static/image6.jpg)

**Figure 6**

Comme indiqué ci-dessus, vous pouvez ajouter de nouveaux rôles en entrant simplement le nom du rôle, puis en cliquant sur Ajouter un rôle. Vous pouvez gérer ou supprimer des rôles existants en cliquant sur le lien approprié dans la liste des rôles existants.

Lorsque vous gérez un rôle, vous pouvez ajouter ou supprimer des utilisateurs comme indiqué ci-dessous.

![](membership/_static/image7.jpg)

**Figure 7**

En activant la case à cocher l’utilisateur est dans le rôle, vous pouvez facilement ajouter un utilisateur à un rôle spécifique. ASP.NET met automatiquement à jour votre base de données d’appartenance avec les entrées appropriées. Vous devez également configurer des règles d’accès pour votre application. Les développeurs ASP.NET 1. x sont familiarisés avec la procédure via l’élément &lt;&gt; d’autorisation dans le fichier Web. config, et cette option est toujours disponible dans ASP.NET 2,0. Toutefois, il est plus facile de gérer les règles d’accès à l’aide de l’outil Administration de site Web, comme indiqué ci-dessous.

![](membership/_static/image8.jpg)

**Figure 8**

Dans ce cas, le dossier administration est mis en surbrillance (ce qui est difficile à voir parce que l’outil le met en surbrillance gris clair) et le rôle administrateurs a reçu l’autorisation d’accès. Tous les autres utilisateurs sont refusés. Vous pouvez cliquer sur l’icône en-tête pour sélectionner une règle, puis utiliser les boutons monter et descendre pour organiser les règles. Comme avec l’élément ASP.NET &lt;Authorization&gt;, les règles sont traitées dans l’ordre dans lequel elles apparaissent. En d’autres termes, si l’ordre des règles dans la capture ci-dessus a été inversé, personne n’a accès au dossier d’administration parce que la première règle qui ASP.NET serait la même que la règle qui refuse tout le monde au dossier.

ASP.NET 2,0 ajoute un fichier Web. config au dossier pour lequel vous spécifiez une règle d’accès. Les règles d’accès peuvent être modifiées par le biais du fichier de configuration ou via l’outil Administration de site Web. En d’autres termes, l’outil Administration de site Web est simplement une interface par le biais de laquelle le fichier de configuration peut être modifié dans un environnement convivial.

## <a name="using-roles-in-code"></a>Utilisation de rôles dans le code

L’API pour la gestion des rôles n’a pas changé depuis la version 1. x. La méthode **IsInRole** est utilisée pour déterminer si un utilisateur est dans un rôle particulier.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET crée également une instance RolePrincipal en tant que membre du contexte actuel. L’objet RolePrincipal peut être utilisé pour obtenir tous les rôles auxquels l’utilisateur appartient, comme suit :

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Utilisation de RoleGroups avec le contrôle LoginView

Maintenant que vous comprenez la gestion des rôles et l’appartenance, vous pouvez discuter brièvement de la façon dont le contrôle LoginView tire parti de cette fonctionnalité dans ASP.NET 2,0. Comme indiqué précédemment, le contrôle LoginView est un contrôle basé sur un modèle qui contient deux modèles par défaut ; AnonymousTemplate et LoggedInTemplate. La boîte de dialogue tâches LoginView est un lien (illustré ci-dessous) qui vous permet de modifier RoleGroups.

![](membership/_static/image9.jpg)

**Figure 9**

Chaque objet RoleGroup contient un tableau de chaînes qui définit les rôles auxquels s’applique RoleGroup. Pour ajouter un nouveau RoleGroup au contrôle LoginView, cliquez sur le lien modifier le RoleGroups. Dans l’image ci-dessus, vous pouvez voir que j’ai ajouté un nouveau RoleGroup pour les administrateurs. En sélectionnant cette RoleGroup (RoleGroup [0]) dans la liste déroulante affichages, je peux configurer un modèle qui sera affiché uniquement aux membres du rôle Administrateurs. Dans l’image ci-dessous, j’ai ajouté un nouveau RoleGroup qui s’applique aux membres du rôle Sales et du rôle distribution. Cela ajoute un deuxième RoleGroup à la liste déroulante des affichages dans la boîte de dialogue tâches LoginView et tout utilisateur ajouté à ce modèle est visible par tous les utilisateurs du rôle de vente ou de distribution.

![](membership/_static/image10.jpg)

**Figure 10**

## <a name="overriding-the-existing-membership-provider"></a>Remplacement du fournisseur d’appartenances existant

Il existe deux façons d’étendre les fonctionnalités de l’appartenance à ASP.NET. Tout d’abord, vous pouvez évidemment modifier les fonctionnalités existantes de la classe SqlMembershipProvider en héritant de celle-ci et en remplaçant ses méthodes. Par exemple, si vous souhaitez implémenter vos propres fonctionnalités lors de la création d’utilisateurs, vous pouvez créer votre propre classe qui hérite de SqlMembershipProvider comme suit :

[!code-csharp[Main](membership/samples/sample5.cs)]

Si, en revanche, vous souhaitez créer votre propre fournisseur (pour stocker vos informations d’appartenance dans une base de données Access, par exemple), vous pouvez créer votre propre fournisseur.

## <a name="creating-your-own-membership-provider"></a>Création de votre propre fournisseur d’appartenances

Pour créer votre propre fournisseur d’appartenances, vous devez d’abord créer une classe qui hérite de la classe MembershipProvider. Si vous utilisez VB.NET, Visual Studio 2005 ajoute les stubs pour toutes les méthodes que vous devez substituer. Si vous utilisez C#, vous pouvez ajouter les stubs.

Vous devrez remplacer les éléments suivants :

- ApplicationName, propriété
- Fonction ChangePassword
- ChangePasswordQuestionAndAnswer fonction)
- CreateUser, fonction
- DeleteUser, fonction
- Propriété EnablePasswordReset
- Propriété EnablePasswordRetrieval
- FindUsersByEmail fonction)
- FindUsersByName fonction)
- GetAllUsers fonction)
- GetNumberOfUsersOnline fonction)
- GetPassword fonction)
- Fonction GetUser
- GetUserNameByEmail fonction)
- Propriété MaxInvalidPasswordAttempts
- Propriété MinRequiredNonAlphanumericCharacters
- Propriété MinRequiredPasswordLength
- Propriété PasswordAttemptWindow
- PasswordFormat, propriété
- Propriété PasswordStrengthRegularExpression
- Propriété RequiresQuestionAndAnswer
- Propriété RequiresUniqueEmail
- ResetPassword fonction)
- Déverrouiller la fonction utilisateur
- UpdateUser fonction)
- ValidateUser fonction)

Il s’agit d’une liste à implémenter C# en tant que développeur. Il peut s’avérer plus facile de créer la classe dans VB.NET sans implémentation, puis d’utiliser .NET Reflector ou un outil similaire pour convertir le C#code en.

La chaîne de connexion et les autres propriétés doivent être définies sur leurs valeurs par défaut dans la méthode Initialize. (La méthode Initialize est déclenchée lorsque le fournisseur est chargé au moment de l’exécution.) Le deuxième paramètre de la méthode Initialize est de type System. Collections. Specialized. NameValueCollection et est une référence au &lt;ajouter&gt; élément associé à votre fournisseur personnalisé dans le fichier Web. config. Cette entrée ressemble à ceci :

[!code-xml[Main](membership/samples/sample6.xml)]

Voici un exemple de la méthode Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Pour valider l’utilisateur lors de l’envoi de votre formulaire de connexion, vous devez utiliser la méthode ValidateUser. Cette méthode se déclenche lorsque l’utilisateur clique sur le bouton de connexion dans le contrôle de connexion. Vous allez placer votre code qui effectue la recherche de l’utilisateur dans cette méthode.

Comme vous pouvez le voir, l’écriture de votre propre fournisseur d’appartenances n’est pas difficile et vous permet d’étendre cette fonctionnalité puissante de ASP.NET 2,0.
