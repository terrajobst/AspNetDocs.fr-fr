---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Créer une application MVC 5 avec Facebook, Twitter, LinkedIn et l’authentification Google OAuth2 (C#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application Web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2,0 avec les informations d’identification d’un authenti externe...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457685"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Créer une application ASP.NET MVC 5 avec authentification OAuth2 Facebook, Twitter, LinkedIn et Google (C#)

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel vous montre comment créer une application Web ASP.NET MVC 5 qui permet aux utilisateurs de se connecter à l’aide d' [OAuth 2,0](http://oauth.net/2/) avec les informations d’identification d’un fournisseur d’authentification externe, tel que Facebook, Twitter, LinkedIn, Microsoft ou Google. Pour plus de simplicité, ce didacticiel se concentre sur l’utilisation des informations d’identification de Facebook et Google.
> 
> L’activation de ces informations d’identification dans vos sites Web constitue un avantage significatif, car des millions d’utilisateurs disposent déjà de comptes avec ces fournisseurs externes. Ces utilisateurs peuvent être plus enclins à s’inscrire à votre site s’ils n’ont pas à créer et à mémoriser un nouvel ensemble d’informations d’identification.
> 
> Voir aussi [application ASP.NET MVC 5 avec SMS et E-mail authentification à deux facteurs](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Ce didacticiel montre également comment ajouter des données de profil pour l’utilisateur et comment utiliser l’API d’appartenance pour ajouter des rôles. Ce didacticiel a été rédigé par [Rick Anderson](https://blogs.msdn.com/rickAndy) (veuillez suivre sur Twitter : [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

<a id="start"></a>
## <a name="getting-started"></a>Mise en route

Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installez Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure. Pour obtenir de l’aide sur Dropbox, GitHub, LinkedIn, Instagram, buffer, Salesforce, STEAM, Stack Exchange, TripIt, Twitch, Twitter, Yahoo ! et bien plus encore, consultez cet [exemple de projet](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Vous devez installer Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure pour utiliser Google OAuth 2 et déboguer localement sans avertissements SSL.

Cliquez sur **nouveau projet** dans la page de **démarrage** , ou utilisez le menu et sélectionnez **fichier**, puis **nouveau projet**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Création de votre première application

Cliquez sur **nouveau projet**, sélectionnez **visuel C#**  sur la gauche, puis **Web** , puis sélectionnez **application Web ASP.net**. Nommez votre projet « MvcAuth », puis cliquez sur **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

Dans la boîte de dialogue **nouveau projet ASP.net** , cliquez sur **MVC**. Si l’authentification n’est pas un **compte d’utilisateur individuel**, cliquez sur le bouton **modifier l’authentification** et sélectionnez les **comptes d’utilisateur individuels**. En vérifiant l' **hôte dans le Cloud**, l’application est très facile à héberger dans Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Si vous avez sélectionné **hôte dans le Cloud**, complétez la boîte de dialogue Configurer.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Utiliser NuGet pour mettre à jour l’intergiciel OWIN le plus récent

Utilisez le gestionnaire de package NuGet pour mettre à jour l' [intergiciel OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Dans le menu de gauche, sélectionnez **mises à jour** . Vous pouvez cliquer sur le bouton **mettre à jour tout** ou vous pouvez rechercher uniquement les packages OWIN (illustrés dans l’image suivante) :

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Dans l’image ci-dessous, seuls les packages OWIN sont affichés :

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

À partir de la console du gestionnaire de package (PMC), vous pouvez entrer la commande `Update-Package`, qui met à jour tous les packages.

Appuyez sur **F5** ou sur **CTRL + F5** pour exécuter l’application. Dans l’image ci-dessous, le numéro de port est 1234. Lorsque vous exécutez l’application, vous verrez un numéro de port différent.

Selon la taille de la fenêtre de votre navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour afficher les liens **page d’hébergement**, **à propos**de, **contacter**, **inscrire** et **ouvrir une session** .

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configuration de SSL dans le projet

Pour vous connecter à des fournisseurs d’authentification tels que Google et Facebook, vous devez configurer IIS-Express pour utiliser SSL. Il est important de continuer à utiliser SSL après la connexion et de ne pas revenir à HTTP, votre cookie de connexion est tout aussi secret que votre nom d’utilisateur et votre mot de passe, et sans utiliser SSL, vous l’envoyez en texte clair sur le réseau. En outre, vous avez déjà pris le temps d’effectuer le protocole de transfert et de sécuriser le canal (ce qui est la majeure partie de la façon dont HTTPs est plus lent que HTTP) avant l’exécution du pipeline MVC. par conséquent, la redirection vers le protocole HTTP Après que vous êtes connecté ne rend pas la requête actuelle ou future les requêtes sont beaucoup plus rapides.

1. Dans **Explorateur de solutions**, cliquez sur le projet **MvcAuth** .
2. Appuyez sur la touche F4 pour afficher les propriétés du projet. Vous pouvez également sélectionner la **fenêtre Propriétés**dans le menu **affichage** .
3. Remplacez **SSL activé** par true.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copiez l’URL SSL (qui sera `https://localhost:44300/`, sauf si vous avez créé d’autres projets SSL).
5. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **MvcAuth** , puis sélectionnez **Propriétés**.
6. Sélectionnez l’onglet **Web** , puis collez l’URL SSL dans la zone **URL du projet** . Enregistrez le fichier (CTL + S). Vous aurez besoin de cette URL pour configurer les applications d’authentification Facebook et Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Ajoutez l’attribut [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) au contrôleur `Home` pour exiger que toutes les demandes utilisent le protocole HTTPS. Une approche plus sécurisée consiste à ajouter le filtre [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) à l’application. Consultez la section &quot;protéger l’application avec SSL et l’attribut Authorize&quot; dans mon didacticiel [créer une application ASP.NET MVC avec auth et SQL DB et déployer sur Azure App service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Une partie du contrôleur d’hébergement est illustrée ci-dessous.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Appuyez sur Ctrl+F5 pour exécuter l’application. Si vous avez installé le certificat par le passé, vous pouvez ignorer le reste de cette section et passer à la [création d’une application Google pour OAuth 2 et connecter l’application au projet](#goog). sinon, suivez les instructions pour approuver le certificat auto-signé que IIS Express a généré.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Lisez la boîte de dialogue **avertissement de sécurité** , puis cliquez sur **Oui** si vous souhaitez installer le certificat représentant localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE affiche la *page d’accueil* sans avertissement SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome accepte également le certificat et affiche le contenu HTTPs sans avertissement. Firefox utilise son propre magasin de certificats, donc un avertissement s’affiche. Pour notre application, vous pouvez cliquer sur **je comprends**en toute sécurité les risques.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Création d’une application Google pour OAuth 2 et connexion de l’application au projet

> [!WARNING]
> Pour obtenir les instructions actuelles de Google OAuth, consultez [configuration de l’authentification Google dans ASP.net Core](/aspnet/core/security/authentication/social/google-logins).

1. Accédez à la [Console développeur de Google](https://console.developers.google.com/).
2. Si vous n’avez pas encore créé de projet, sélectionnez **informations d’identification** dans l’onglet de gauche, puis sélectionnez **créer**.
3. Dans l’onglet de gauche, cliquez sur **informations d’identification**.
4. Cliquez sur **créer des informations d’identification** , puis sur **ID client OAuth**. 

    1. Dans la boîte de dialogue **créer un ID client** , conservez l' **application Web** par défaut pour le type d’application.
    2. Définissez les origines de **JavaScript autorisées** sur l’URL SSL que vous avez utilisée ci-dessus (`https://localhost:44300/`, sauf si vous avez créé d’autres projets SSL).
    3. Définissez l' **URI de redirection autorisée** sur :  
         `https://localhost:44300/signin-google`
5. Cliquez sur l’élément de menu de l’écran de consentement OAuth, puis définissez votre adresse de messagerie et le nom de votre produit. Une fois le formulaire rempli, cliquez sur **Enregistrer**.
6. Cliquez sur l’élément de menu bibliothèque, recherchez **Google + API**, cliquez dessus, puis appuyez sur Activer.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   L’image ci-dessous montre les API activées.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. À partir du gestionnaire d’API des API Google, accédez à l’onglet **informations d’identification** pour obtenir l' **ID client**. Téléchargez pour enregistrer un fichier JSON avec des secrets d’application. Copiez et collez le **ClientID** et le **ClientSecret** dans la méthode `UseGoogleAuthentication` trouvée dans le fichier *Startup.auth.cs* dans le dossier *App_Start* . Les valeurs **ClientID** et **ClientSecret** présentées ci-dessous sont des exemples et ne fonctionnent pas.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont ajoutés au code ci-dessus pour que l’exemple reste simple. Consultez [meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles dans ASP.net et Azure App service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Appuyez sur **Ctrl+F5** pour générer et exécuter l’application. Cliquez sur le lien **Ouvrir une session** .  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. Sous **utiliser un autre service pour se connecter**, cliquez sur **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Si vous manquez l’une des étapes ci-dessus, vous obtiendrez une erreur HTTP 401. Revérifiez les étapes ci-dessus. Si vous manquez un paramètre requis (par exemple, **nom du produit**), ajoutez l’élément manquant et enregistrez-le. le fonctionnement de l’authentification peut prendre quelques minutes.
10. Vous serez redirigé vers le site Google sur lequel vous allez entrer vos informations d’identification.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Après avoir entré vos informations d’identification, vous êtes invité à accorder des autorisations pour l’application Web que vous venez de créer :
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Cliquez sur **Accepter**. Vous êtes maintenant redirigé vers la page de **Registre** de l’application MvcAuth où vous pouvez inscrire votre compte Google. Vous avez la possibilité de changer le nom d'inscription local utilisé pour votre compte Gmail, mais, en règle générale, les utilisateurs préfèrent conserver l'alias de messagerie par défaut (c'est-à-dire celui que vous avez utilisé pour l'authentification). Cliquez sur **Inscrire**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Création de l’application dans Facebook et connexion de l’application au projet

> [!WARNING]
> Pour obtenir les instructions d’authentification OAuth2 Facebook actuelles, consultez [configuration de l’authentification Facebook](/aspnet/core/security/authentication/social/facebook-logins)

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examiner les données d’appartenance

Dans le menu **affichage** , cliquez sur **Explorateur de serveurs**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Développez **DefaultConnection (MvcAuth)** , développez **tables**, cliquez avec le bouton droit sur **AspNetUsers** , puis cliquez sur **afficher les données**de la table.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![données de la table aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Ajout de données de profil à la classe d’utilisateur

Dans cette section, vous allez ajouter la date de naissance et la ville d’habitation aux données utilisateur lors de l’inscription, comme illustré dans l’image suivante.

![reg avec la ville d’habitation et bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Ouvrez le fichier *Models\IdentityModels.cs* et ajoutez les propriétés date de naissance et ville d’hébergement :

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Ouvrez le fichier *Models\AccountViewModels.cs* et les propriétés définir la date de naissance et la ville d’hébergement dans `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Ouvrez le fichier *Controllers\AccountController.cs* et ajoutez le code correspondant à la date de naissance et à la ville d’habitation dans la méthode d’action `ExternalLoginConfirmation` comme indiqué ci-dessous :

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Ajoutez la date de naissance et la ville d’habitation au fichier *Views\Account\ExternalLoginConfirmation.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Supprimez la base de données d’appartenance pour pouvoir inscrire à nouveau votre compte Facebook auprès de votre application, puis vérifiez que vous pouvez ajouter les informations relatives à la nouvelle date de naissance et au profil de ville d’hébergement.

Dans **Explorateur de solutions**, cliquez sur l’icône **Afficher tous les fichiers** , cliquez avec le bouton droit sur *ajouter\_Data\aspnet-MvcAuth-&lt;date&gt;. mdf* , puis cliquez sur **supprimer**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Dans le menu **Outils** , cliquez sur gestionnaire de **package NuGet**, puis sur **console du gestionnaire de package** (PMC). Entrez les commandes suivantes dans le PMC.

1. Activer-migrations
2. Ajouter une initialisation de migration
3. Mettre à jour-base de données

Exécutez l’application et utilisez FaceBook et Google pour vous connecter et inscrire certains utilisateurs.

## <a name="examine-the-membership-data"></a>Examiner les données d’appartenance

Dans le menu **affichage** , cliquez sur **Explorateur de serveurs**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Cliquez avec le bouton droit sur **AspNetUsers** et cliquez sur **afficher les données**de la table.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

Les champs `HomeTown` et `BirthDate` sont affichés ci-dessous.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Déconnexion de votre application et connexion avec un autre compte

Si vous vous connectez à votre application avec Facebook,, puis vous déconnectez et essayez de vous reconnecter avec un autre compte Facebook (à l’aide du même navigateur), vous serez immédiatement connecté au compte Facebook précédent que vous avez utilisé. Pour utiliser un autre compte, vous devez accéder à Facebook et vous déconnecter de Facebook. La même règle s’applique à tout autre fournisseur d’authentification tiers. Vous pouvez également vous connecter avec un autre compte à l’aide d’un autre navigateur.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir des instructions Yahoo et LinkedIn, consultez [Présentation des fournisseurs de sécurité OAuth Yahoo et LinkedIn pour OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser. Pour obtenir les boutons de connexion aux réseaux sociaux, consultez boutons de connexion Pretty-Jerrie pour ASP.NET MVC 5.

Suivez le didacticiel [créer une application ASP.NET MVC avec auth et SQL DB et déployer sur Azure App service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), qui poursuit ce didacticiel et présente les éléments suivants :

1. Comment déployer votre application sur Azure.
2. Comment sécuriser votre application avec des rôles.
3. Comment sécuriser votre application avec le [requirehttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) et [autoriser](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) les filtres.
4. Comment utiliser l’API d’appartenance pour ajouter des utilisateurs et des rôles.

N’hésitez pas à nous faire part de vos commentaires sur ce didacticiel et sur ce que nous pourrions améliorer. Vous pouvez également demander de nouvelles rubriques pour [afficher le code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Vous pouvez même demander et voter sur les nouvelles fonctionnalités à ajouter à ASP.NET. Par exemple, vous pouvez voter pour un outil pour [créer et gérer des utilisateurs et des rôles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Pour une bonne explication du fonctionnement des services d’authentification externes ASP.NET, consultez [services d’authentification externes](https://asp.net/web-api/overview/security/external-authentication-services)de Robert McMurray. L’article de Robert est également détaillé dans l’activation de l’authentification Microsoft et Twitter. L’excellent [didacticiel EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) de Tom Dykstra montre comment utiliser les Entity Framework.
