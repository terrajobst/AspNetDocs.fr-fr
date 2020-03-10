---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: Authentification des utilisateurs avec l’authentificationC#Windows () | Microsoft Docs
author: microsoft
description: Découvrez comment utiliser l’authentification Windows dans le contexte d’une application MVC. Vous allez apprendre à activer l’authentification Windows dans le co Web de votre application...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: bb3909bff2791c15a8737fc12cac69f79b55733f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624123"
---
# <a name="authenticating-users-with-windows-authentication-c"></a>Authentification des utilisateurs avec l’authentification Windows (C#)

par [Microsoft](https://github.com/microsoft)

> Découvrez comment utiliser l’authentification Windows dans le contexte d’une application MVC. Vous allez apprendre à activer l’authentification Windows dans le fichier de configuration Web de votre application et à configurer l’authentification auprès d’IIS. Enfin, vous allez apprendre à utiliser l’attribut [Authorize] pour limiter l’accès aux actions de contrôleur à des utilisateurs ou des groupes Windows particuliers.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez tirer parti des fonctionnalités de sécurité intégrées à Internet Information Services pour protéger les vues dans vos applications MVC. Vous apprenez à autoriser les actions de contrôleur à être appelées uniquement par des utilisateurs ou utilisateurs Windows particuliers qui sont membres de groupes Windows particuliers.

L’utilisation de l’authentification Windows est utile lorsque vous créez un site Web interne de l’entreprise (un site intranet) et que vous souhaitez que vos utilisateurs puissent utiliser leurs noms d’utilisateur et mots de passe Windows standard lors de l’accès au site Web. Si vous créez un site Web orienté vers l’extérieur (site Web Internet), envisagez plutôt d’utiliser l’authentification par formulaire.

#### <a name="enabling-windows-authentication"></a>Activation de l’authentification Windows

Lorsque vous créez une nouvelle application ASP.NET MVC, l’authentification Windows n’est pas activée par défaut. L’authentification par formulaire est le type d’authentification par défaut activé pour les applications MVC. Vous devez activer l’authentification Windows en modifiant le fichier de configuration Web (Web. config) de votre application MVC. Recherchez la section&gt; d’authentification &lt;et modifiez-la pour qu’elle utilise Windows au lieu de l’authentification par formulaire comme suit :

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Lorsque vous activez l’authentification Windows, votre serveur Web est responsable de l’authentification des utilisateurs. En règle générale, il existe deux types de serveurs Web différents que vous utilisez lors de la création et du déploiement d’une application ASP.NET MVC.

Tout d’abord, lors du développement d’une application MVC, vous utilisez le serveur Web de développement ASP.NET inclus dans Visual Studio. Par défaut, le serveur Web de développement ASP.NET exécute toutes les pages dans le contexte du compte Windows actuel (quel que soit le compte que vous avez utilisé pour vous connecter à Windows).

Le serveur Web de développement ASP.NET prend également en charge l’authentification NTLM. Vous pouvez activer l’authentification NTLM en cliquant avec le bouton droit sur le nom de votre projet dans la fenêtre Explorateur de solutions et en sélectionnant Propriétés. Ensuite, sélectionnez l’onglet Web et cochez la case NTLM (voir figure 1).

**Figure 1 : activation de l’authentification NTLM pour le serveur Web de développement ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Pour une application Web de production, à la main, vous utilisez IIS en tant que serveur Web. IIS prend en charge plusieurs types d’authentifications, notamment :

- Authentification de base : définie dans le cadre du protocole HTTP 1,0. Envoie des noms d’utilisateur et des mots de passe en texte clair (encodé en base64) sur Internet. -Authentification Digest : envoie un hachage d’un mot de passe, au lieu du mot de passe lui-même, sur Internet. -Authentification Windows intégrée (NTLM) : le meilleur type d’authentification à utiliser dans les environnements intranet à l’aide de Windows. -Authentification par certificat : active l’authentification à l’aide d’un certificat côté client. Le certificat est mappé à un compte d’utilisateur Windows.

> [!NOTE] 
> 
> Pour obtenir une vue d’ensemble plus détaillée de ces différents types d’authentification, consultez [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).

Vous pouvez utiliser Internet Information Services Manager pour activer un type particulier d’authentification. N’oubliez pas que tous les types d’authentification ne sont pas disponibles dans le cas de chaque système d’exploitation. En outre, si vous utilisez IIS 7,0 avec Windows Vista, vous devez activer les différents types d’authentification Windows avant qu’ils n’apparaissent dans le gestionnaire de Internet Information Services. Ouvrez le **panneau de configuration, programmes, programmes et fonctionnalités, activez ou désactivez les fonctionnalités Windows**, puis développez le nœud Internet Information Services (voir figure 2).

**Figure 2 : activation des fonctionnalités de Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

À l’aide de Internet Information Services, vous pouvez activer ou désactiver différents types d’authentification. Par exemple, la figure 3 illustre la désactivation de l’authentification anonyme et l’activation de l’authentification Windows intégrée (NTLM) lors de l’utilisation d’IIS 7,0.

**Figure 3 : activation de l’authentification Windows intégrée**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorisation des utilisateurs et des groupes Windows

Après avoir activé l’authentification Windows, vous pouvez utiliser l’attribut [Authorize] pour contrôler l’accès aux contrôleurs ou aux actions du contrôleur. Cet attribut peut être appliqué à un contrôleur MVC entier ou à une action de contrôleur particulière.

Par exemple, le contrôleur d’hébergement de la liste 1 expose trois actions nommées index (), CompanySecrets () et StephenSecrets (). N’importe qui peut appeler l’action index (). Toutefois, seuls les membres du groupe administrateurs locaux Windows peuvent appeler l’action CompanySecrets (). Enfin, seul l’utilisateur de domaine Windows nommé Stephen (dans le domaine Redmond) peut appeler l’action StephenSecrets ().

**Liste 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> En raison du contrôle de compte d’utilisateur (UAC) Windows, lorsque vous travaillez avec Windows Vista ou Windows Server 2008, le groupe Administrateurs local se comporte différemment des autres groupes. L’attribut [Authorize] ne reconnaîtra pas correctement un membre du groupe Administrateurs local, sauf si vous modifiez les paramètres de contrôle de compte d’utilisateur de votre ordinateur.

Exactement ce qui se produit lorsque vous tentez d’appeler une action de contrôleur sans avoir les autorisations appropriées dépend du type d’authentification activé. Par défaut, lorsque vous utilisez la Serveur de développement ASP.NET, vous recevez simplement une page vierge. La page est fournie avec un état de réponse http **401 non autorisé** .

Si, en revanche, vous utilisez IIS alors que l’authentification anonyme est désactivée et que l’authentification de base est activée, vous continuez à recevoir une invite de la boîte de dialogue de connexion chaque fois que vous demandez la page protégée (voir la figure 4).

**Figure 4 : boîte de dialogue de connexion à l’authentification de base**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Récapitulatif

Ce didacticiel a expliqué comment vous pouvez utiliser l’authentification Windows dans le contexte d’une application ASP.NET MVC. Vous avez appris à activer l’authentification Windows dans le fichier de configuration Web de votre application et à configurer l’authentification auprès d’IIS. Enfin, vous avez appris à utiliser l’attribut [Authorize] pour limiter l’accès aux actions de contrôleur à des utilisateurs ou des groupes Windows particuliers.

> [!div class="step-by-step"]
> [Précédent](authenticating-users-with-forms-authentication-cs.md)
> [Suivant](preventing-javascript-injection-attacks-cs.md)
