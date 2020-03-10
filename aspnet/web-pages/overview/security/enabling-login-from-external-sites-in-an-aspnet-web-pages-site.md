---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Connexion à l’aide de sites externes dans un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment se connecter à votre site pages Web ASP.NET (Razor) à l’aide de Facebook, Google, Twitter, Yahoo et d’autres sites, à savoir comment prendre en charge...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638753"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Connexion à l’aide de sites externes dans un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment se connecter à votre site pages Web ASP.NET (Razor) à l’aide de Facebook, Google, Twitter, Yahoo et d’autres sites, c’est-à-dire comment prendre en charge OAuth et OpenID dans votre site.
> 
> Ce que vous allez apprendre :
> 
> - Comment activer la connexion à partir d’autres sites quand vous utilisez le modèle WebMatrix Starter Site.
> 
> Il s’agit de la fonctionnalité ASP.NET introduite dans l’article :
> 
> - Le programme d’assistance `OAuthWebSecurity`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - WebMatrix 3

Pages Web ASP.NET prend en charge les fournisseurs [OAuth](http://oauth.net/) et [OpenID](http://openid.net/) . À l’aide de ces fournisseurs, vous pouvez permettre aux utilisateurs de se connecter à votre site à l’aide de leurs informations d’identification existantes de Facebook, Twitter, Microsoft et Google. Par exemple, pour se connecter à l’aide d’un compte Facebook, les utilisateurs peuvent simplement choisir une icône Facebook, qui les redirige vers la page de connexion Facebook où elles entrent leurs informations utilisateur. Ils peuvent ensuite associer la connexion Facebook à leur compte sur votre site. L’une des améliorations apportées aux fonctionnalités d’appartenance à des pages Web est que les utilisateurs peuvent associer plusieurs connexions (y compris des connexions à partir de sites de réseau social) à un compte unique sur votre site Web.

Cette image affiche la page de connexion à partir du modèle **Starter Site** , où un utilisateur peut choisir une icône Facebook, Twitter, Google ou Microsoft pour activer la connexion avec un compte externe :

![fournisseurs externes](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Vous pouvez activer l’appartenance OAuth et OpenID en supprimant les commentaires de quelques lignes de code dans le modèle **Starter Site** . Les méthodes et les propriétés que vous utilisez pour travailler avec les fournisseurs OAuth et OpenID se trouvent dans la classe `WebMatrix.Security.OAuthWebSecurity`. Le modèle **Starter Site** comprend une infrastructure d’appartenance complète, complète avec une page de connexion, une base de données d’appartenance et tout le code dont vous avez besoin pour permettre aux utilisateurs de se connecter à votre site à l’aide des informations d’identification locales ou de celles d’un autre site.

Cette section fournit un exemple de la façon de permettre aux utilisateurs de se connecter à partir de sites externes vers un site basé sur le modèle **Starter Site** . Après avoir créé un site de démarrage, procédez comme suit (détails) :

- Pour les sites qui utilisent un fournisseur OAuth (Facebook, Twitter et Microsoft), vous créez une application sur le site externe. Cela vous donne les clés d’application dont vous aurez besoin pour appeler la fonctionnalité de connexion pour ces sites.
- Pour les sites qui utilisent un fournisseur OpenID (Google), vous n’êtes pas obligé de créer une application. Pour tous ces sites, vous devez disposer d’un compte afin de vous connecter et de créer des applications de développement.

    > [!NOTE]
    > Les applications Microsoft acceptent uniquement une URL dynamique pour un site Web de travail. vous ne pouvez donc pas utiliser une URL de site Web locale pour tester les connexions.
- Modifiez quelques fichiers de votre site Web afin de spécifier le fournisseur d’authentification approprié et de soumettre une connexion au site que vous souhaitez utiliser.

Cet article fournit des instructions distinctes pour les tâches suivantes :

- [Activation des connexions Google](#To_enable_Google_logins)
- [Activation des connexions Facebook](#To_enable_Facebook_logins)
- [Activation des connexions Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Activation des connexions Google

1. Créez ou ouvrez un site pages Web ASP.NET basé sur le modèle de Starter Site WebMatrix.
2. Ouvrez la page *\_AppStart. cshtml* et supprimez les marques de commentaire de la ligne de code suivante. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Test de la connexion Google

1. Exécutez la page *default. cshtml* de votre site, puis choisissez le bouton **se connecter** .
2. Sur la page de *connexion* , dans la section **utiliser un autre service pour se connecter** , choisissez le bouton **Google** ou **Yahoo** Submit. Cet exemple utilise la connexion Google. 

    La page web redirige la demande vers la page de connexion Google.

    ![Connexion Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Entrez les informations d’identification d’un compte Google existant.
4. Si Google vous demande si vous souhaitez autoriser *localhost* à utiliser les informations du compte, cliquez sur **autoriser**.

    Le code utilise le jeton Google pour authentifier l’utilisateur, puis renvoie à cette page sur votre site Web. Cette page permet aux utilisateurs d’associer leur connexion Google à un compte existant sur votre site Web, ou d’inscrire un nouveau compte sur votre site pour associer la connexion externe à.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Cliquez sur le bouton **associer** . Le navigateur revient à la page d’hébergement de votre application.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Activation des connexions Facebook

1. Accédez au [site des développeurs Facebook](https://developers.facebook.com/apps) (connectez-vous si vous n’êtes pas déjà connecté).
2. Choisissez le bouton **créer une nouvelle application** , puis suivez les invites pour nommer et créer la nouvelle application.
3. Dans la section, **Sélectionnez la manière dont votre application s’intègre à Facebook**, puis choisissez la section **site Web** .
4. Renseignez le champ **URL du site** avec l’URL de votre site (par exemple, `http://www.example.com`). Le champ **domaine** est facultatif. vous pouvez l’utiliser pour assurer l’authentification pour un domaine entier (par exemple, *example.com*). 

    > [!NOTE]
    > Si vous exécutez un site sur votre ordinateur local avec une URL telle que `http://localhost:12345` (où le nombre est un numéro de port local), vous pouvez ajouter cette valeur au champ **URL du site** pour le test de votre site. Toutefois, chaque fois que le numéro de port de votre site local change, vous devez mettre à jour le champ **URL de site** de votre application.
5. Cliquez sur le bouton **enregistrer les modifications** .
6. Choisissez à nouveau l’onglet **applications** , puis affichez la page de démarrage de votre application.
7. Copiez les valeurs **ID** de l’application et **secret** de l’application pour votre application, puis collez-les dans un fichier texte temporaire. Vous passerez ces valeurs au fournisseur Facebook dans le code de votre site Web.
8. Quittez le site de développement Facebook.

À présent, vous apportez des modifications à deux pages de votre site Web afin que les utilisateurs puissent se connecter au site à l’aide de leurs comptes Facebook.

1. Créez ou ouvrez un site pages Web ASP.NET basé sur le modèle de Starter Site WebMatrix.
2. Ouvrez la page *\_AppStart. cshtml* et supprimez les marques de commentaire du code du fournisseur OAuth Facebook. Le bloc de code non commenté se présente comme suit : 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copiez la valeur de l' **ID d’application** à partir de l’application Facebook comme valeur du paramètre `appId` (à l’intérieur des guillemets).
4. Copiez la valeur de la **clé secrète** de l’application à partir de l’application Facebook comme valeur de paramètre `appSecret`.
5. Enregistrez et fermez le fichier.

### <a name="testing-facebook-login"></a>Test de la connexion Facebook

1. Exécutez la page *default. cshtml* du site et choisissez le bouton de **connexion** .
2. Sur la page de *connexion* , dans la section **utiliser un autre service pour se connecter** , choisissez l’icône **Facebook** . 

    La page web redirige la demande vers la page de connexion à Facebook.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Connectez-vous à un compte Facebook. 

    Le code utilise le jeton Facebook pour vous authentifier, puis retourne à une page où vous pouvez associer votre connexion Facebook à la connexion de votre site. Votre nom d’utilisateur ou votre adresse de messagerie est renseigné dans le champ adresse de **messagerie** du formulaire.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Cliquez sur le bouton **associer** . 

    Le navigateur revient à la page d’hébergement et vous êtes connecté.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Activation des connexions Twitter

1. Accédez au [site des développeurs Twitter](https://dev.twitter.com/).
2. Choisissez le lien **créer une application** , puis connectez-vous au site.
3. Dans le formulaire **créer une application** , renseignez les champs **nom** et **Description** .
4. Dans le champ **site Web** , entrez l’URL de votre site (par exemple, `http://www.example.com`). 

    > [!NOTE]
    > Si vous testez votre site localement (à l’aide d’une URL telle que `http://localhost:12345`), Twitter peut ne pas accepter l’URL. Toutefois, vous pourrez peut-être utiliser l’adresse IP de bouclage locale (par exemple `http://127.0.0.1:12345`). Cela simplifie le processus de test de votre application localement. Toutefois, chaque fois que le numéro de port de votre site local change, vous devez mettre à jour le champ **site Web** de votre application.
5. Dans le champ **URL de rappel** , entrez l’URL de la page de votre site Web à laquelle les utilisateurs doivent revenir après s’être connectés à Twitter. Par exemple, pour envoyer des utilisateurs à la page d’accueil du site de démarrage (qui reconnaîtra leur état de connexion), entrez la même URL que celle que vous avez entrée dans le champ **site Web** .
6. Acceptez les termes du contrat et choisissez le bouton **créer votre application Twitter** .
7. Sur la page d’accueil **mes applications** , choisissez l’application que vous avez créée.
8. Dans l’onglet **Détails** , faites défiler vers le bas et choisissez le bouton **créer un jeton d’accès** .
9. Dans l’onglet **Détails** , copiez les valeurs **clé de consommateur** et secret de **consommateur** de votre application, puis collez-les dans un fichier texte temporaire. Vous transmettez ces valeurs au fournisseur Twitter dans le code de votre site Web.
10. Quittez le site Twitter.

À présent, vous apportez des modifications à deux pages de votre site Web afin que les utilisateurs puissent se connecter au site à l’aide de leurs comptes Twitter.

1. Créez ou ouvrez un site pages Web ASP.NET basé sur le modèle de Starter Site WebMatrix.
2. Ouvrez la page *\_AppStart. cshtml* et supprimez les marques de commentaire du code du fournisseur OAuth Twitter. Le bloc de code non commenté ressemble à ceci : 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copiez la valeur de **clé du client** à partir de l’application Twitter comme valeur du paramètre `consumerKey` (à l’intérieur des guillemets).
4. Copiez la valeur du **secret du client** à partir de l’application Twitter comme valeur du paramètre `consumerSecret`.
5. Enregistrez et fermez le fichier.

### <a name="testing-twitter-login"></a>Test de la connexion Twitter

1. Exécutez la page *default. cshtml* de votre site, puis cliquez sur le bouton de **connexion** .
2. Sur la page de *connexion* , dans la section **utiliser un autre service pour se connecter** , choisissez l’icône **Twitter** . 

    La page web redirige la demande vers une page de connexion Twitter pour l’application que vous avez créée.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Connectez-vous à un compte Twitter.
4. Le code utilise le jeton Twitter pour authentifier l’utilisateur, puis vous renvoie à une page dans laquelle vous pouvez associer votre connexion à votre compte de site Web. Votre nom ou adresse de messagerie est renseigné dans le champ adresse de **messagerie** du formulaire.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Cliquez sur le bouton **associer** . 

    Le navigateur revient à la page d’hébergement et vous êtes connecté.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Personnalisation du comportement à l’échelle du site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Ajout de la sécurité et de l’appartenance à un site pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
