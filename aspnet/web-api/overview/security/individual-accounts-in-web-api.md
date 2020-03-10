---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Sécuriser une API Web avec des comptes individuels et une connexion locale dans API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: Cette rubrique montre comment sécuriser une API Web à l’aide de OAuth2 pour l’authentification auprès d’une base de données d’appartenance. Versions logicielles utilisées dans le didacticiel Visual Studio 201...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7492c4aa4c2a0a8aeed64c3462bda8fc51f35a6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555194"
---
# <a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Sécuriser une API Web avec des comptes individuels et une connexion locale dans API Web ASP.NET 2,2

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger l’exemple d’application](https://github.com/MikeWasson/LocalAccountsApp)

> Cette rubrique montre comment sécuriser une API Web à l’aide de OAuth2 pour l’authentification auprès d’une base de données d’appartenance.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [API Web 2,2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2,1](../../../identity/index.md)

Dans Visual Studio 2013, le modèle de projet d’API Web vous donne trois options pour l’authentification :

- **Comptes individuels.** L’application utilise une base de données d’appartenance.
- **Comptes organisationnels.** Les utilisateurs se connectent avec leur Azure Active Directory, Office 365 ou des informations d’identification Active Directory locales.
- **Authentification Windows.** Cette option est destinée aux applications intranet et utilise le module IIS d’authentification Windows.

Pour plus d’informations sur ces options, consultez [création de projets Web ASP.net dans Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Les comptes individuels offrent deux moyens de se connecter à un utilisateur :

- **Connexion locale**. L’utilisateur s’inscrit sur le site, en entrant un nom d’utilisateur et un mot de passe. L’application stocke le hachage de mot de passe dans la base de données d’appartenance. Lorsque l’utilisateur se connecte, le système de ASP.NET Identity vérifie le mot de passe.
- **Connexion sociale**. L’utilisateur se connecte avec un service externe, tel que Facebook, Microsoft ou Google. L’application crée toujours une entrée pour l’utilisateur dans la base de données d’appartenance, mais ne stocke pas les informations d’identification. L’utilisateur s’authentifie en se connectant au service externe.

Cet article examine le scénario de connexion locale. Pour les connexions locales et sociales, l’API Web utilise OAuth2 pour authentifier les demandes. Toutefois, les flux d’informations d’identification sont différents pour les connexions locales et sociales.

Dans cet article, je démontrerai une application simple qui permet à l’utilisateur de se connecter et d’envoyer des appels AJAX authentifiés à une API Web. Vous pouvez télécharger l’exemple de code [ici](https://github.com/MikeWasson/LocalAccountsApp). Le fichier Lisez-moi explique comment créer l’exemple à partir de zéro dans Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

L’exemple d’application utilise Knockout. js pour la liaison de données et jQuery pour l’envoi de demandes AJAX. Je me concentrerai sur les appels AJAX. vous n’avez donc pas besoin de connaître Knockout. js pour cet article.

En cours de route, je décrirai les éléments suivants :

- Ce que fait l’application côté client.
- Ce qui se passe sur le serveur.
- Le trafic HTTP en milieu.

Tout d’abord, nous devons définir une terminologie OAuth2.

- *Ressource*. Certains éléments de données qui peuvent être protégés.
- *Serveur de ressources*. Serveur qui héberge la ressource.
- *Propriétaire*de la ressource. Entité qui peut accorder l’autorisation d’accéder à une ressource. (Généralement l’utilisateur.)
- *Client*: application qui souhaite accéder à la ressource. Dans cet article, le client est un navigateur Web.
- *Jeton d’accès*. Jeton qui accorde l’accès à une ressource.
- *Jeton du porteur*. Type particulier de jeton d’accès, avec la propriété que tout le monde peut utiliser. En d’autres termes, un client n’a pas besoin d’une clé de chiffrement ou d’un autre secret pour utiliser un jeton de porteur. Pour cette raison, les jetons du porteur doivent uniquement être utilisés sur un HTTPs et doivent avoir des délais d’expiration relativement courts.
- *Serveur d’autorisation*. Serveur qui fournit des jetons d’accès.

Une application peut agir à la fois comme serveur d’autorisation et serveur de ressources. Le modèle de projet d’API Web suit ce modèle.

## <a name="local-login-credential-flow"></a>Workflow des informations d’identification de connexion locales

Pour la connexion locale, l’API Web utilise le [Workflow de propriétaire](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) de la ressource défini dans OAuth2.

1. L’utilisateur entre un nom et un mot de passe dans le client.
2. Le client envoie ces informations d’identification au serveur d’autorisation.
3. Le serveur d’autorisation authentifie les informations d’identification et retourne un jeton d’accès.
4. Pour accéder à une ressource protégée, le client comprend le jeton d’accès dans l’en-tête d’autorisation de la requête HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Lorsque vous sélectionnez **des comptes individuels** dans le modèle de projet d’API Web, le projet comprend un serveur d’autorisation qui valide les informations d’identification de l’utilisateur et émet des jetons. Le diagramme suivant montre le même Workflow d’informations d’identification en termes de composants d’API Web.

![](individual-accounts-in-web-api/_static/image4.png)

Dans ce scénario, les contrôleurs d’API Web agissent en tant que serveurs de ressources. Un filtre d’authentification valide les jetons d’accès et l’attribut **[Authorize]** est utilisé pour protéger une ressource. Lorsqu’un contrôleur ou une action possède l’attribut **[Authorize]** , toutes les demandes adressées à ce contrôleur ou cette action doivent être authentifiées. Dans le cas contraire, l’autorisation est refusée et l’API Web renvoie une erreur 401 (non autorisée).

Le serveur d’autorisation et le filtre d’authentification appellent tous les deux un composant [middleware OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) qui gère les détails de OAuth2. Je décrirai la conception plus en détail plus loin dans ce didacticiel.

## <a name="sending-an-unauthorized-request"></a>Envoi d’une demande non autorisée

Pour commencer, exécutez l’application et cliquez sur le bouton **appeler l’API** . Une fois la demande terminée, un message d’erreur doit s’afficher dans la zone de **résultats** . Cela est dû au fait que la demande ne contient pas de jeton d’accès, donc la demande n’est pas autorisée.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Le bouton **appeler l’API** envoie une requête Ajax à ~/API/Values, qui appelle une action de contrôleur d’API Web. Voici la section du code JavaScript qui envoie la requête AJAX. Dans l’exemple d’application, tout le code de l’application JavaScript se trouve dans le fichier Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Tant que l’utilisateur ne se connecte pas, il n’y a aucun jeton de porteur et, par conséquent, aucun en-tête d’autorisation dans la demande. La requête retourne alors une erreur 401.

Voici la requête HTTP. (J’ai utilisé [Fiddler](http://www.telerik.com/fiddler) pour capturer le trafic http.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Réponse HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Notez que la réponse comprend un en-tête WWW-Authenticate avec la stimulation définie sur Bearer. Cela indique que le serveur attend un jeton de porteur.

## <a name="register-a-user"></a>Inscrire un utilisateur

Dans la section **Register** de l’application, entrez un e-mail et un mot de passe, puis cliquez sur le bouton **Register** .

Vous n’avez pas besoin d’utiliser une adresse de messagerie valide pour cet exemple, mais une application réelle confirmerait l’adresse. (Pour plus d’informations [, consultez créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation du mot de passe](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Pour le mot de passe, utilisez un nom tel que « Password1 ! », avec une lettre majuscule, une lettre minuscule, un chiffre et un caractère non alphanumérique. Pour simplifier l’application, j’ai quitté la validation côté client. par conséquent, en cas de problème avec le format du mot de passe, vous obtenez une erreur 400 (demande incorrecte).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Le bouton **Register** envoie une demande de publication à ~/API/Account/Register/. Le corps de la demande est un objet JSON qui contient le nom et le mot de passe. Voici le code JavaScript qui envoie la requête :

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Requête HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Réponse HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Cette requête est gérée par la classe `AccountController`. En interne, `AccountController` utilise ASP.NET Identity pour gérer la base de données d’appartenance.

Si vous exécutez l’application localement à partir de Visual Studio, les comptes d’utilisateur sont stockés dans la base de données locale, dans la table AspNetUsers. Pour afficher les tables dans Visual Studio, cliquez sur le menu **affichage** , sélectionnez **Explorateur de serveurs**, puis **connexions de données**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Recevoir un jeton d’accès

Jusqu’à présent, nous n’avons pas effectué d’OAuth, mais nous allons maintenant voir le serveur d’autorisation OAuth en action, lorsque nous demanderons un jeton d’accès. Dans la zone de **connexion** de l’exemple d’application, entrez l’adresse de messagerie et le mot de passe, puis cliquez sur **se connecter**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Le bouton **se connecter** envoie une demande au point de terminaison de jeton. Le corps de la demande contient les données de formulaire suivantes, codées URL :

- Grant\_type : "password"
- nom d’utilisateur : &lt;l’adresse de messagerie de l’utilisateur&gt;
- mot de passe : &lt;mot de passe&gt;

Voici le code JavaScript qui envoie la requête AJAX :

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Si la demande est réussie, le serveur d’autorisation retourne un jeton d’accès dans le corps de la réponse. Notez que nous stockons le jeton dans le stockage de session, afin de l’utiliser ultérieurement lors de l’envoi de requêtes à l’API. Contrairement à certaines formes d’authentification (telles que l’authentification basée sur les cookies), le navigateur n’inclut pas automatiquement le jeton d’accès dans les demandes suivantes. L’application doit le faire explicitement. C’est une bonne chose, car elle limite les [vulnérabilités de CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Requête HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Vous pouvez voir que la demande contient les informations d’identification de l’utilisateur. Vous *devez* utiliser le protocole HTTPS pour assurer la sécurité de la couche de transport.

Réponse HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Pour une meilleure lisibilité, j’ai mis en retrait le JSON et tronqué le jeton d’accès, ce qui est très long.

Les propriétés `access_token`, `token_type`et `expires_in` sont définies par la spécification OAuth2. Les autres propriétés (`userName`, `.issued`et `.expires`) sont fournies à titre d’information uniquement. Vous pouvez trouver le code qui ajoute ces propriétés supplémentaires dans la méthode `TokenEndpoint`, dans le fichier/Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Envoyer une demande authentifiée

Maintenant que nous avons un jeton de porteur, nous pouvons effectuer une demande authentifiée auprès de l’API. Pour ce faire, définissez l’en-tête d’autorisation dans la demande. Cliquez à nouveau sur le bouton **appeler l’API** pour le voir.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Requête HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Réponse HTTP :

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Se déconnecter

Étant donné que le navigateur ne met pas en cache les informations d’identification ou le jeton d’accès, la déconnexion consiste simplement à oublier le jeton, en le supprimant du stockage de session :

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Fonctionnement du modèle de projet comptes individuels

Lorsque vous sélectionnez **des comptes individuels** dans le modèle de projet d’Application Web ASP.net, le projet comprend les éléments suivants :

- Un serveur d’autorisation OAuth2.
- Point de terminaison d’API Web pour la gestion des comptes d’utilisateur
- Modèle EF pour le stockage des comptes d’utilisateur.

Voici les principales classes d’application qui implémentent ces fonctionnalités :

- `AccountController`. Fournit un point de terminaison d’API Web pour la gestion des comptes d’utilisateur. Le `Register` action est le seul que nous avons utilisé dans ce didacticiel. D’autres méthodes de la classe prennent en charge la réinitialisation de mot de passe, les connexions sociales et d’autres fonctionnalités.
- `ApplicationUser`, défini dans/Models/IdentityModels.cs. Cette classe est le modèle EF pour les comptes d’utilisateur dans la base de données d’appartenance.
- `ApplicationUserManager`, défini dans/App\_Start/IdentityConfig. cs cette classe dérive de [usermanager](https://msdn.microsoft.com/library/dn613290.aspx) et effectue des opérations sur les comptes d’utilisateur, telles que la création d’un utilisateur, la vérification des mots de passe, etc., et conserve automatiquement les modifications apportées à la base de données.
- `ApplicationOAuthProvider`. Cet objet se connecte à l’intergiciel (middleware) OWIN et traite les événements déclenchés par l’intergiciel (middleware). Il dérive de [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configuration du serveur d’autorisation

Dans StartupAuth.cs, le code suivant configure le serveur d’autorisation OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

La propriété `TokenEndpointPath` est le chemin d’accès à l’URL du point de terminaison du serveur d’autorisation. Il s’agit de l’URL utilisée par l’application pour recevoir les jetons du porteur.

La propriété `Provider` spécifie un fournisseur qui se connecte à l’intergiciel (middleware) OWIN et traite les événements déclenchés par l’intergiciel (middleware).

Voici le processus de base lorsque l’application souhaite obtenir un jeton :

1. Pour obtenir un jeton d’accès, l’application envoie une requête à ~/token.
2. L’intergiciel (middleware) OAuth appelle `GrantResourceOwnerCredentials` sur le fournisseur.
3. Le fournisseur appelle la `ApplicationUserManager` pour valider les informations d’identification et créer une identité basée sur les revendications.
4. En cas de tentative, le fournisseur crée un ticket d’authentification, qui est utilisé pour générer le jeton.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

L’intergiciel (middleware) OAuth ne sait rien sur les comptes d’utilisateur. Le fournisseur communique entre l’intergiciel et l’ASP.NET Identity. Pour plus d’informations sur l’implémentation du serveur d’autorisation, consultez [serveur d’autorisation OAuth 2,0 OWIN](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configuration de l’API Web pour utiliser des jetons de porteur

Dans la méthode `WebApiConfig.Register`, le code suivant configure l’authentification pour le pipeline de l’API Web :

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

La classe **HostAuthenticationFilter** active l’authentification à l’aide de jetons de porteur.

La méthode **SuppressDefaultHostAuthentication** indique à l’API Web d’ignorer toute authentification qui se produit avant que la demande n’atteigne le pipeline d’API Web, par le biais d’IIS ou de l’intergiciel (middleware) OWIN. De cette façon, nous pouvons limiter l’API Web pour qu’elle n’effectue l’authentification qu’à l’aide des jetons du porteur.

> [!NOTE]
> En particulier, la partie MVC de votre application peut utiliser l’authentification par formulaire, qui stocke les informations d’identification dans un cookie. L’authentification basée sur les cookies requiert l’utilisation de jetons anti-contrefaçon pour empêcher les attaques CSRF. Il s’agit d’un problème pour les API Web, car il n’existe aucun moyen pratique pour l’API Web d’envoyer le jeton anti-contrefaçon au client. (Pour plus d’informations sur ce problème, consultez [prévention des attaques CSRF dans l’API Web](preventing-cross-site-request-forgery-csrf-attacks.md).) L’appel de **SuppressDefaultHostAuthentication** garantit que l’API Web n’est pas vulnérable aux attaques CSRF à partir des informations d’identification stockées dans les cookies.

Lorsque le client demande une ressource protégée, voici ce qui se produit dans le pipeline de l’API Web :

1. Le filtre **HostAuthentication** appelle l’intergiciel (middleware) OAuth pour valider le jeton.
2. L’intergiciel convertit le jeton en une identité basée sur les revendications.
3. À ce stade, la demande est *authentifiée* , mais elle n’est pas *autorisée*.
4. Le filtre d’autorisation examine l’identité des revendications. Si les revendications autorisent l’utilisateur pour cette ressource, la demande est autorisée. Par défaut, l’attribut **[Authorize]** autorise toutes les demandes authentifiées. Toutefois, vous pouvez autoriser par rôle ou par d’autres revendications. Pour plus d’informations, consultez [authentification et autorisation dans l’API Web](authentication-and-authorization-in-aspnet-web-api.md).
5. Si les étapes précédentes réussissent, le contrôleur retourne la ressource protégée. Dans le cas contraire, le client reçoit une erreur 401 (non autorisée).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Ressources supplémentaires

- [ASP.NET Identity](../../../identity/index.md)
- [Comprendre les fonctionnalités de sécurité dans le modèle Spa pour VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Billet de blog MSDN de Hongye Sun.
- [Discroisement du modèle de comptes individuels de l’API Web-partie 2 : comptes locaux](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Billet de blog de Dominick Baier.
- [Authentification de l’hôte et API Web avec OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Une bonne explication des `SuppressDefaultHostAuthentication` et des `HostAuthenticationFilter` par Brock Allen.
- [Personnalisation des informations de profil dans ASP.net Identity dans les modèles VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Billet de blog MSDN de Pranav Rastogi.
- [Gestion de la durée de vie des demandes pour la classe usermanager dans ASP.net Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Billet de blog MSDN de Suhas Joshi, avec une bonne explication de la classe `UserManager`.
