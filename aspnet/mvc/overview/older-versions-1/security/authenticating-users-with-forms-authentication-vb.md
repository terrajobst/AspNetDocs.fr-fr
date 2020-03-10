---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Authentification des utilisateurs avec l’authentification par formulaire (VB) | Microsoft Docs
author: microsoft
description: Découvrez comment utiliser l’attribut [Authorize] pour protéger par mot de passe des pages particulières dans votre application MVC. Vous apprenez également à utiliser l’administration de site Web...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: a2c2140631d59a7f8b21aa73613a92ea5c7a91d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615576"
---
# <a name="authenticating-users-with-forms-authentication-vb"></a>Authentification des utilisateurs avec l’authentification par formulaire (VB)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment utiliser l’attribut [Authorize] pour protéger par mot de passe des pages particulières dans votre application MVC. Vous allez apprendre à utiliser l’outil Administration de site Web pour créer et gérer des utilisateurs et des rôles. Vous apprendrez également à configurer l’emplacement de stockage des informations sur le compte et le rôle d’utilisateur.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez utiliser l’authentification par formulaire pour protéger par mot de passe les vues dans vos applications ASP.NET MVC. Vous allez apprendre à utiliser l’outil Administration de site Web pour créer des utilisateurs et des rôles. Vous apprendrez également comment empêcher des utilisateurs non autorisés d’appeler des actions de contrôleur. Enfin, vous apprendrez à configurer l’emplacement de stockage des noms d’utilisateur et des mots de passe.

#### <a name="using-the-web-site-administration-tool"></a>Utilisation de l’outil Administration de site Web

Avant de faire autre chose, nous devons commencer par créer des utilisateurs et des rôles. Le moyen le plus simple de créer des utilisateurs et des rôles consiste à tirer parti de l’outil d’administration de site Web de Visual Studio 2008. Vous pouvez lancer cet outil en sélectionnant l’option de menu **projet, Configuration ASP.net**. Vous pouvez également lancer l’outil Administration de site Web en cliquant sur l’icône (un peu de danger) du marteau dans le monde qui apparaît en haut de la fenêtre de Explorateur de solutions (voir figure 1).

**Figure 1 : lancement de l’outil Administration de site Web**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Dans l’outil Administration de site Web, vous créez des utilisateurs et des rôles en sélectionnant l’onglet sécurité. cliquez sur le lien **créer un utilisateur** pour créer un nouvel utilisateur nommé Stephen (voir figure 2). Fournissez à l’utilisateur Stephen le mot de passe de votre choix (par exemple, *secret*).

**Figure 2 : création d’un nouvel utilisateur**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Pour créer des rôles, vous devez d’abord activer des rôles et définir un ou plusieurs rôles. Activez les rôles en cliquant sur le lien **activer les rôles** . Ensuite, créez un rôle nommé *administrateurs* en cliquant sur le lien **créer ou gérer des rôles** (voir figure 3).

**Figure 3 : création d’un nouveau rôle**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Enfin, créez un nouvel utilisateur nommé Catherine et associez Catherine au rôle Administrateurs en cliquant sur le lien créer un utilisateur et en sélectionnant administrateurs lors de la création de Catherine (voir figure 4).

**Figure 4 : ajout d’un utilisateur à un rôle**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Lorsque tout est dit et terminé, vous devez avoir deux nouveaux utilisateurs nommés Stephen et Catherine. Vous devez également disposer d’un nouveau rôle nommé administrateurs. Catherine est membre du rôle administrateurs et Stephen ne l’est pas.

#### <a name="requiring-authorization"></a>Demande d’autorisation

Vous pouvez exiger qu’un utilisateur soit authentifié avant que l’utilisateur n’appelle une action de contrôleur en ajoutant l’attribut [Authorize] à l’action. Vous pouvez appliquer l’attribut [Authorize] à une action de contrôleur individuel, ou vous pouvez appliquer cet attribut à une classe de contrôleur entière.

Par exemple, le contrôleur de la liste 1 expose une action nommée CompanySecrets (). Étant donné que cette action est décorée avec l’attribut [Authorize], cette action ne peut pas être appelée, sauf si un utilisateur est authentifié.

**Liste 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Si vous appelez l’action CompanySecrets () en entrant l’URL/Home/CompanySecrets dans la barre d’adresses de votre navigateur et que vous n’êtes pas un utilisateur authentifié, vous êtes redirigé vers la vue de connexion automatiquement (voir figure 5).

**Figure 5 : vue de la connexion**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Vous pouvez utiliser la vue de connexion pour entrer votre nom d’utilisateur et votre mot de passe. Si vous n’êtes pas un utilisateur inscrit, vous pouvez cliquer sur le lien **Enregistrer** pour accéder à la vue de Registre (voir figure 6). Vous pouvez utiliser la vue Register pour créer un nouveau compte d’utilisateur.

**Figure 6 : vue Registre**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Une fois que vous êtes connecté, vous pouvez voir la vue CompanySecrets (voir la figure 7). Par défaut, vous pouvez continuer à vous connecter jusqu’à ce que vous fermiez la fenêtre de votre navigateur.

**Figure 7 : vue CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorisation par nom d’utilisateur ou rôle d’utilisateur

Vous pouvez utiliser l’attribut [Authorize] pour restreindre l’accès à une action de contrôleur à un ensemble spécifique d’utilisateurs ou à un ensemble particulier de rôles d’utilisateur. Par exemple, le contrôleur d’hébergement modifié de la liste 2 contient deux nouvelles actions nommées StephenSecrets () et AdministratorSecrets ().

**Liste 2 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Seul un utilisateur avec le nom d’utilisateur Stephen peut appeler l’action StephenSecrets (). Tous les autres utilisateurs sont redirigés vers la vue de connexion. La propriété Users accepte une liste de noms de comptes d’utilisateur séparés par des virgules.

Seuls les utilisateurs du rôle administrateurs peuvent appeler l’action AdministratorSecrets (). Par exemple, étant donné que Catherine est membre du groupe administrateurs, elle peut appeler l’action AdministratorSecrets (). Tous les autres utilisateurs sont redirigés vers la vue de connexion. La propriété Roles accepte une liste séparée par des virgules de noms de rôles.

#### <a name="configuring-authentication"></a>Configuration de l’authentification

À ce stade, vous vous demandez peut-être où le compte d’utilisateur et les informations de rôle sont stockés. Par défaut, les informations sont stockées dans une base de données SQL Express (RANU) nommée ASPNETDB. mdf située dans le dossier de données de l’application\_de votre application MVC. Cette base de données est générée automatiquement par l’infrastructure ASP.NET lorsque vous commencez à utiliser l’appartenance.

Pour afficher la base de données ASPNETDB. mdf dans la fenêtre Explorateur de solutions, vous devez d’abord sélectionner l’option de menu projet, afficher tous les fichiers.

L’utilisation de la base de données SQL Express par défaut est correcte lors du développement d’une application. Toutefois, il est probable que vous ne souhaitiez pas utiliser la base de données ASPNETDB. mdf par défaut pour une application de production. Dans ce cas, vous pouvez modifier l’emplacement de stockage des informations de compte d’utilisateur en effectuant les deux étapes suivantes :

1. Ajoutez les objets de base de données Services d’application à votre base de données de production-modifiez votre chaîne de connexion d’application pour qu’elle pointe vers votre base de données de production.

La première étape consiste à ajouter tous les objets de base de données nécessaires (tables et procédures stockées) à votre base de données de production. Le moyen le plus simple d’ajouter ces objets à une nouvelle base de données est de tirer parti de l’Assistant Installation de ASP.NET SQL Server (voir figure 8). Vous pouvez lancer cet outil en ouvrant l’invite de commandes Visual Studio 2008 à partir du groupe de programmes Microsoft Visual Studio 2008 et en exécutant la commande suivante à partir de l’invite de commandes :

RegSql ASPNET\_

**Figure 8 – Assistant Installation de ASP.NET SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

L’Assistant Installation de ASP.NET SQL Server vous permet de sélectionner une base de données SQL Server sur votre réseau et d’installer tous les objets de base de données requis par les services d’application ASP.NET. Il n’est pas nécessaire que le serveur de base de données se trouve sur votre ordinateur local.

> [!NOTE]
> Si vous ne souhaitez pas utiliser l’Assistant Installation de ASP.NET SQL Server, vous pouvez trouver des scripts SQL pour ajouter les objets de base de données des services d’application dans le dossier suivant :
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727

Après avoir créé les objets de base de données nécessaires, vous devez modifier la connexion de base de données utilisée par votre application MVC. Modifiez la chaîne de connexion ApplicationServices dans votre fichier de configuration Web (Web. config) pour qu’elle pointe vers la base de données de production. Par exemple, la connexion modifiée dans la liste 3 pointe vers une base de données nommée MyProductionDB (la chaîne de connexion ApplicationServices d’origine a été commentée).

**Liste 3 – Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configuration des autorisations de base de données

Si vous utilisez la sécurité intégrée pour vous connecter à votre base de données, vous devez ajouter le compte d’utilisateur Windows approprié en tant que connexion à votre base de données. Le compte correct varie selon que vous utilisez le Serveur de développement ASP.NET ou Internet Information Services en tant que serveur Web. Le compte d’utilisateur correct dépend également de votre système d’exploitation.

Si vous utilisez le Serveur de développement ASP.NET (serveur Web par défaut utilisé par Visual Studio), votre application s’exécute dans le contexte de votre compte d’utilisateur Windows. Dans ce cas, vous devez ajouter votre compte d’utilisateur Windows en tant que connexion au serveur de base de données.

Si vous utilisez Internet Information Services, vous avez également besoin d’ajouter le compte ASPNET ou le compte autorité NT/SERVICE réseau en tant que connexion au serveur de base de données. Si vous utilisez Windows XP, ajoutez le compte ASPNET en tant que connexion à votre base de données. Si vous utilisez un système d’exploitation plus récent, tel que Windows Vista ou Windows Server 2008, ajoutez le compte autorité NT/SERVICE réseau en tant que connexion à la base de données.

Vous pouvez ajouter un nouveau compte d’utilisateur à votre base de données à l’aide de Microsoft SQL Server Management Studio (voir figure 9).

**Figure 9 : création d’un compte de connexion Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Une fois que vous avez créé la connexion requise, vous devez mapper la connexion à un utilisateur de base de données avec les rôles de base de données appropriés. Double-cliquez sur la connexion, puis sélectionnez l’onglet Mappage de l’utilisateur. Sélectionnez un ou plusieurs rôles de base de données des services d’application. Par exemple, pour authentifier les utilisateurs, vous devez activer l’appartenance\_ASPNET\_rôle de base de données BasicAccess. Pour créer de nouveaux utilisateurs, vous devez activer l’appartenance\_ASPNET\_rôle de base de données FullAccess (voir figure 10).

**Figure 10 : ajout des rôles de base de données Services d’application**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à utiliser l’authentification par formulaire lors de la création d’une application ASP.NET MVC. Tout d’abord, vous avez appris à créer des utilisateurs et des rôles en tirant parti de l’outil Administration de site Web. Ensuite, vous avez appris à utiliser l’attribut [Authorize] pour empêcher des utilisateurs non autorisés d’appeler des actions de contrôleur. Enfin, vous avez appris à configurer votre application MVC pour stocker les informations d’utilisateur et de rôle dans une base de données de production.

> [!div class="step-by-step"]
> [Précédent](preventing-javascript-injection-attacks-cs.md)
> [Suivant](authenticating-users-with-windows-authentication-vb.md)
