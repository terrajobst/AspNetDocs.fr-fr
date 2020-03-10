---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Validation et paiement avec PayPal | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78565407"
---
# <a name="checkout-and-payment-with-paypal"></a>Commande et paiement avec PayPal

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’exemple de projet WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour le Web. Un [projet Visual Studio 2013 avec C# le code source](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.

Ce didacticiel explique comment modifier l’exemple d’application Wingtip Toys pour inclure l’autorisation de l’utilisateur, l’inscription et le paiement à l’aide de PayPal. Seuls les utilisateurs qui sont connectés auront l’autorisation d’acheter des produits. La fonctionnalité d’inscription des utilisateurs intégrée du modèle de projet ASP.NET 4,5 Web Forms contient déjà la plupart des éléments dont vous avez besoin. Vous allez ajouter la fonctionnalité de validation PayPal Express. Dans ce didacticiel, vous allez utiliser l’environnement de test de développeur PayPal, afin de ne pas transférer de fonds réels. À la fin du didacticiel, vous allez tester l’application en sélectionnant les produits à ajouter au panier, en cliquant sur le bouton Checkout et en transférant les données vers le site Web de test PayPal. Sur le site Web de test PayPal, vous allez confirmer vos informations d’expédition et de paiement, puis revenir à l’exemple d’application local Wingtip Toys pour confirmer et terminer l’achat.

Plusieurs processeurs de paiement tiers expérimentés sont spécialisés dans les achats en ligne qui répondent à l’évolutivité et à la sécurité. Les développeurs ASP.NET doivent prendre en compte les avantages de l’utilisation d’une solution de paiement tierce avant d’implémenter une solution d’achat et d’achat.

> [!NOTE] 
> 
> L’exemple d’application Wingtip Toys a été conçu pour illustrer des concepts et des fonctionnalités ASP.NET spécifiques disponibles pour les développeurs Web ASP.NET. Cet exemple d’application n’a pas été optimisé pour toutes les circonstances possibles en ce qui concerne l’évolutivité et la sécurité.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment restreindre l’accès à des pages spécifiques dans un dossier.
- Comment créer un panier d’achat connu à partir d’un panier d’achat anonyme.
- Comment activer SSL pour le projet.
- Comment ajouter un fournisseur OAuth au projet.
- Comment utiliser PayPal pour acheter des produits à l’aide de l’environnement de test PayPal.
- Comment afficher les détails d’un compte PayPal dans un contrôle **DetailsView** .
- Comment mettre à jour la base de données de l’application Wingtip Toys avec les détails obtenus à partir de PayPal.

## <a name="adding-order-tracking"></a>Ajout du suivi des commandes

Dans ce didacticiel, vous allez créer deux nouvelles classes pour suivre les données de la commande qu’un utilisateur a créée. Les classes effectuent le suivi des données relatives aux informations d’expédition, au total des achats et à la confirmation du paiement.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Ajouter les classes de modèle Order et OrderDetail

Plus haut dans cette série de didacticiels, vous avez défini le schéma pour les catégories, les produits et les éléments de panier d’achat en créant les classes `Category`, `Product`et `CartItem` dans le dossier *modèles* . À présent, vous allez ajouter deux nouvelles classes pour définir le schéma de l’ordre des produits et les détails de la commande.

1. Dans le dossier **Models** , ajoutez une nouvelle classe nommée *Order.cs*.   
   Le nouveau fichier de classe s’affiche dans l’éditeur.
2. Remplacez le code par défaut par ce qui suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Ajoutez une classe *OrderDetail.cs* au dossier *Models* .
4. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Les classes `Order` et `OrderDetail` contiennent le schéma permettant de définir les informations de commande utilisées pour l’achat et l’expédition.

En outre, vous devrez mettre à jour la classe de contexte de base de données qui gère les classes d’entité et qui fournit l’accès aux données à la base de données. Pour ce faire, vous allez ajouter la commande et `OrderDetail` classes de modèle nouvellement créées à `ProductContext` classe.

1. Dans **Explorateur de solutions**, recherchez et ouvrez le fichier *ProductContext.cs* .
2. Ajoutez le code en surbrillance au fichier *ProductContext.cs* comme indiqué ci-dessous :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Comme mentionné précédemment dans cette série de didacticiels, le code du fichier *ProductContext.cs* ajoute l’espace de noms `System.Data.Entity` afin que vous ayez accès à toutes les fonctionnalités principales du Entity Framework. Cette fonctionnalité offre la possibilité d’interroger, d’insérer, de mettre à jour et de supprimer des données en utilisant des objets fortement typés. Le code ci-dessus dans la classe `ProductContext` ajoute Entity Framework accès aux classes `Order` et `OrderDetail` récemment ajoutées.

## <a name="adding-checkout-access"></a>Ajout d’un accès d’extraction

L’exemple d’application Wingtip Toys permet aux utilisateurs anonymes d’examiner et d’ajouter des produits à un panier d’achat. Toutefois, lorsque des utilisateurs anonymes choisissent d’acheter les produits qu’ils ont ajoutés au panier d’achat, ils doivent se connecter au site. Une fois qu’ils sont connectés, ils peuvent accéder aux pages réservées de l’application Web qui gèrent l’extraction et le processus d’achat. Ces pages restreintes sont contenues dans le dossier d' *extraction* de l’application.

### <a name="add-a-checkout-folder-and-pages"></a>Ajouter un dossier et des pages d’extraction

Vous allez maintenant créer le dossier d' *extraction* et les pages qu’il verra dans le processus de validation. Vous allez mettre à jour ces pages plus loin dans ce didacticiel.

1. Cliquez avec le bouton droit sur le nom du projet (**Wingtip Toys**) dans **Explorateur de solutions** , puis sélectionnez **Ajouter un nouveau dossier**. 

    ![Valider et payer avec PayPal-nouveau dossier](checkout-and-payment-with-paypal/_static/image1.png)
2. Nommez l' *extraction*du nouveau dossier.
3. Cliquez avec le bouton droit sur le dossier d' *extraction* , puis sélectionnez **Ajouter**-&gt;**nouvel élément**. 

    ![Valider et payer avec PayPal-nouvel élément](checkout-and-payment-with-paypal/_static/image2.png)
4. La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
5. Sélectionnez le **groupe C# Visual** -&gt; des modèles **Web** sur la gauche. Ensuite, dans le volet central, sélectionnez **Web Form avec page maître**et nommez-le *CheckoutStart. aspx*. 

    ![Récupération et paiement avec PayPal-boîte de dialogue Ajouter un nouvel élément](checkout-and-payment-with-paypal/_static/image3.png)
6. Comme précédemment, sélectionnez le fichier *site. Master* comme page maître.
7. Ajoutez les pages supplémentaires suivantes au dossier *Checkout* en procédant de la même façon que ci-dessus :   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel. aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Ajouter un fichier Web. config

En ajoutant un nouveau fichier *Web. config* au dossier d' *extraction* , vous pourrez restreindre l’accès à toutes les pages contenues dans le dossier.

1. Cliquez avec le bouton droit sur le dossier *Checkout* , puis sélectionnez **Ajouter** -&gt; **nouvel élément**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le **groupe C# Visual** -&gt; des modèles **Web** sur la gauche. Ensuite, dans le volet central, sélectionnez **fichier de configuration Web**, acceptez le nom par défaut de *Web. config*, puis sélectionnez **Ajouter**.
3. Remplacez le contenu XML existant dans le fichier *Web.config* par le code suivant :  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Enregistrez le fichier *Web.config* .

Le fichier *Web. config* spécifie que tous les utilisateurs inconnus de l’application Web doivent se voir refuser l’accès aux pages contenues dans le dossier d' *extraction* . Toutefois, si l’utilisateur a inscrit un compte et qu’il a ouvert une session, il s’agit d’un utilisateur connu qui aura accès aux pages du dossier d' *extraction* .

Il est important de noter que la configuration ASP.NET suit une hiérarchie, où chaque fichier *Web. config* applique des paramètres de configuration au dossier dans lequel il se trouve et à tous les répertoires enfants situés sous celui-ci.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Activation du protocole SSL pour le projet

 SSL (Secure Sockets Layer) est un protocole défini pour autoriser les serveurs et clients web à communiquer de manière plus sécurisée grâce au chiffrement. Lorsque SSL n'est pas utilisé, les données envoyées entre le client et le serveur peuvent être soumises à la détection des paquets par quiconque dispose d'un accès physique au réseau. En outre, plusieurs schémas d'authentification ne sont pas sécurisés sur le HTTP standard. En particulier, l'authentification de base et l'authentification par formulaires envoient des informations d'identification non chiffrées. Pour être sécurisés, ces schémas d'authentification doivent utiliser SSL. 

1. Dans **Explorateur de solutions**, cliquez sur le projet **WingtipToys** , puis appuyez sur **F4** pour afficher la fenêtre **Propriétés** .
2. Remplacez **SSL activé** par `true`.
3. Copiez l’ **URL SSL** afin de pouvoir l’utiliser ultérieurement.   
 L’URL SSL sera `https://localhost:44300/`, sauf si vous avez déjà créé des sites Web SSL (comme indiqué ci-dessous).   
    ![Propriétés de projet](checkout-and-payment-with-paypal/_static/image4.png)
4. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **WingtipToys** , puis cliquez sur **Propriétés**.
5. Dans l’onglet de gauche, cliquez sur **Web**.
6. Modifiez l' **URL du projet** pour utiliser l' **URL SSL** que vous avez enregistrée précédemment.   
    ![Propriétés du projet Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Enregistrez la page en appuyant sur **Ctrl+S**.
8. Appuyez sur **Ctrl+F5** pour exécuter l’application. Visual Studio affiche une option qui vous permet d'éviter les avertissements SSL.
9. Cliquez sur **Oui** pour approuver le certificat IIS Express SSL et continuer.   
    ![Détails du certificat IIS Express SSL](checkout-and-payment-with-paypal/_static/image6.png)  
 Un avertissement de sécurité s’affiche.
10. Cliquez sur **Oui** pour installer le certificat sur votre hôte local (localhost).   
    ![Boîte de dialogue Avertissement de sécurité](checkout-and-payment-with-paypal/_static/image7.png)  
 La fenêtre du navigateur s’affiche.

Vous pouvez maintenant tester facilement votre application Web localement à l’aide de SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Ajout d'un fournisseur OAuth 2.0

ASP.NET Web Forms fournit des options améliorées pour l’appartenance et l’authentification. Ces améliorations incluent OAuth. OAuth est un protocole ouvert qui permet une autorisation sécurisée dans une méthode simple et standard à partir d’applications Web, mobiles et de bureau. Le modèle de Web Forms ASP.NET utilise OAuth pour exposer Facebook, Twitter, Google et Microsoft en tant que fournisseurs d’authentification. Bien que ce didacticiel utilise uniquement Google comme fournisseur d’authentification, vous pouvez facilement modifier le code pour utiliser l’un des fournisseurs. Les étapes à suivre pour implémenter d’autres fournisseurs sont très similaires à celles que vous verrez dans ce didacticiel.

En plus de l’authentification, ce didacticiel va également utiliser des rôles pour l’implémentation d’autorisations. Seuls les utilisateurs que vous ajoutez au rôle `canEdit` pourront créer, modifier ou supprimer des contacts.

> [!NOTE] 
> 
> Les applications Windows Live acceptent uniquement une URL dynamique pour un site Web de travail. vous ne pouvez donc pas utiliser une URL de site Web locale pour tester les connexions.

Les étapes suivantes vous permettent d'ajouter un fournisseur d'authentification Google.

1. Ouvrez le fichier *App\_Start\Startup.auth.cs* .
2. Supprimez les caractères de commentaire de la méthode `app.UseGoogleAuthentication()` de telle sorte qu’elle ait l’aspect suivant : 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Accédez à la [Console développeur de Google](https://console.developers.google.com/). Vous devez également vous connecter avec votre compte de messagerie de développeur Google (gmail.com). Si vous n’avez pas de compte Google, sélectionnez le lien **Créer un compte** .   
   Vous accédez alors à **Google Developers Console**.   
    ![Console développeur de Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Cliquez sur le bouton **créer un projet** et entrez un nom de projet et un ID (vous pouvez utiliser les valeurs par défaut). Ensuite, cliquez sur la **case à cocher accord** et sur le bouton **créer** .  

    ![Google-nouveau projet](checkout-and-payment-with-paypal/_static/image9.png)

   En quelques secondes, le projet est créé et votre navigateur affiche la nouvelle page de projets.
5. Dans l’onglet de gauche, cliquez sur **api &amp; auth**, puis cliquez sur **informations d’identification**.
6. Cliquez sur **créer un nouvel ID client** sous **OAuth**.   
   La boîte de dialogue **Créer un identifiant client** s’affiche.   
    ![Google - Créer un identifiant client](checkout-and-payment-with-paypal/_static/image10.png)
7. Dans la boîte de dialogue **créer un ID client** , conservez l' **application Web** par défaut pour le type d’application.
8. Définissez les **origines de JavaScript autorisées** sur l’URL SSL que vous avez utilisée précédemment dans ce didacticiel (`https://localhost:44300/`, sauf si vous avez créé d’autres projets SSL).   
   Cette URL est l'origine de votre application. Pour cet exemple, vous entrerez uniquement l'URL de test de l'hôte local (localhost). Toutefois, vous pouvez entrer plusieurs URL pour tenir compte de localhost et de production.
9. Définissez les **URI de redirection autorisés** comme suit : 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Cette valeur est l’URI que le protocole OAuth ASP.NET utilise pour communiquer avec le serveur OAuth Google. Souvenez-vous de l’URL SSL que vous avez utilisée précédemment (`https://localhost:44300/`, sauf si vous avez créé d’autres projets SSL).
10. Cliquez sur le bouton **créer un ID client** .
11. Dans le menu de gauche de la console des développeurs Google, cliquez sur l’élément de menu de l' **écran de consentement** , puis définissez votre adresse de messagerie et le nom de votre produit. Une fois le formulaire rempli, cliquez sur **Enregistrer**.
12. Cliquez sur l’élément de menu **API** , faites défiler la liste et cliquez sur le bouton **désactivé** en regard de **Google + API**.   
    L’acceptation de cette option active l’API Google +.
13. Vous devez également mettre à jour le package NuGet **Microsoft. Owin** vers la version 3.0.0.   
    Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** , puis sélectionnez **gérer les packages NuGet pour la solution**.  
    Dans la fenêtre **gérer les packages NuGet** , recherchez et mettez à jour le package **Microsoft. Owin** vers la version 3.0.0.
14. Dans Visual Studio, mettez à jour la méthode `UseGoogleAuthentication` de la page *Startup.auth.cs* en copiant et en collant l' **ID client** et la **clé secrète client** dans la méthode. Les valeurs **ID client** et **clé secrète client** présentées ci-dessous sont des exemples et ne fonctionnent pas. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Appuyez sur **Ctrl+F5** pour générer et exécuter l’application. Cliquez sur le lien **Ouvrir une session** .
16. Sous **utiliser un autre service pour se connecter**, cliquez sur **Google**.  
    ![Ouvrir une session](checkout-and-payment-with-paypal/_static/image11.png)
17. Si vous devez entrer vos informations d’identification, vous êtes redirigé vers le site Google approprié.  
    ![Google - Se connecter](checkout-and-payment-with-paypal/_static/image12.png)
18. Une fois que vous avez entré vos informations d’identification, vous êtes invité à accorder des autorisations d’accès à l’application Web que vous venez de créer.  
    ![Compte de service de projet par défaut](checkout-and-payment-with-paypal/_static/image13.png)
19. Cliquez sur **Accepter**. Vous êtes alors redirigé vers la page de **Registre** de l’application **WingtipToys** dans laquelle vous pouvez inscrire votre compte Google.  
    ![S’inscrire avec un compte Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Vous avez la possibilité de changer le nom d'inscription local utilisé pour votre compte Gmail, mais, en règle générale, les utilisateurs préfèrent conserver l'alias de messagerie par défaut (c'est-à-dire celui que vous avez utilisé pour l'authentification). Cliquez sur **se connecter** comme indiqué ci-dessus.

### <a name="modifying-login-functionality"></a>Modification de la fonctionnalité de connexion

Comme mentionné précédemment dans cette série de didacticiels, la plupart des fonctionnalités d’inscription de l’utilisateur ont été incluses dans le modèle ASP.NET Web Forms par défaut. À présent, vous allez modifier les pages *login. aspx* et *Register. aspx* par défaut pour appeler la méthode `MigrateCart`. La méthode `MigrateCart` associe un utilisateur qui vient d’être connecté à un panier d’achat anonyme. En associant l’utilisateur et le panier d’achat, l’exemple d’application Wingtip Toys peut conserver le panier d’achat de l’utilisateur entre les visites.

1. Dans **Explorateur de solutions**, recherchez et ouvrez le dossier *Account* .
2. Modifiez la page code-behind nommée *login.aspx.cs* pour inclure le code mis en surbrillance en jaune, afin qu’elle apparaisse comme suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Enregistrez le fichier *login.aspx.cs* .

Pour le moment, vous pouvez ignorer l’avertissement indiquant qu’il n’existe aucune définition pour la méthode `MigrateCart`. Vous l’ajouterez un peu plus tard dans ce didacticiel.

Le fichier code-behind *login.aspx.cs* prend en charge une méthode de connexion. En examinant la page Login. aspx, vous verrez que cette page comprend un bouton « se connecter » qui, lorsque vous cliquez sur, déclenche le gestionnaire de `LogIn` sur le code-behind.

Lorsque la méthode `Login` sur *login.aspx.cs* est appelée, une nouvelle instance du panier d’achat nommé `usersShoppingCart` est créée. L’ID du panier d’achat (GUID) est récupéré et défini sur la variable `cartId`. Ensuite, la méthode `MigrateCart` est appelée, passant la `cartId` et le nom de l’utilisateur connecté à cette méthode. Lors de la migration du panier d’achat, le GUID utilisé pour identifier le panier d’achat anonyme est remplacé par le nom d’utilisateur.

En plus de modifier le fichier code-behind *login.aspx.cs* pour migrer le panier d’achat lorsque l’utilisateur se connecte, vous devez également modifier le *fichier code-behind Register.aspx.cs* pour migrer le panier lorsque l’utilisateur crée un nouveau compte et se connecte.

1. Dans le dossier *Account* , ouvrez le fichier code-behind nommé *Register.aspx.cs*.
2. Modifiez le fichier code-behind en incluant le code en jaune, afin qu’il apparaisse comme suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Enregistrez le fichier *Register.aspx.cs* . Une fois encore, ignorez l’avertissement concernant la méthode `MigrateCart`.

Notez que le code que vous avez utilisé dans le gestionnaire d’événements `CreateUser_Click` est très similaire au code que vous avez utilisé dans la méthode `LogIn`. Lorsque l’utilisateur s’inscrit ou se connecte au site, un appel à la méthode `MigrateCart` est effectué.

## <a name="migrating-the-shopping-cart"></a>Migration du panier d’achat

Maintenant que vous avez mis à jour le processus de connexion et d’inscription, vous pouvez ajouter le code pour migrer le panier d’achat à l’aide de la méthode `MigrateCart`.

1. Dans **Explorateur de solutions**, recherchez le dossier *logique* et ouvrez le fichier de classe *ShoppingCartActions.cs* .
2. Ajoutez le code mis en surbrillance en jaune au code existant dans le fichier *ShoppingCartActions.cs* , afin que le code du fichier *ShoppingCartActions.cs* apparaisse comme suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

La méthode `MigrateCart` utilise le cartId existant pour rechercher le panier d’achat de l’utilisateur. Ensuite, le code parcourt tous les éléments du panier d’achat et remplace la propriété `CartId` (telle que spécifiée par le schéma `CartItem`) par le nom d’utilisateur connecté.

### <a name="updating-the-database-connection"></a>Mise à jour de la connexion de base de données

Si vous suivez ce didacticiel à l’aide de l’exemple d’application Wingtip Toys **prédéfini** , vous devez recréer la base de données d’appartenance par défaut. En modifiant la chaîne de connexion par défaut, la base de données d’appartenance sera créée lors de la prochaine exécution de l’application.

1. Ouvrez le fichier *Web. config* à la racine du projet.
2. Mettez à jour la chaîne de connexion par défaut afin qu’elle apparaisse comme suit :   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Intégration de PayPal

PayPal est une plateforme de facturation basée sur le Web qui accepte les paiements effectués par les commerçants en ligne. Ce didacticiel explique ensuite comment intégrer la fonctionnalité d’extraction Express de PayPal à votre application. L’extraction Express permet à vos clients d’utiliser PayPal pour payer les articles qu’ils ont ajoutés à leur panier d’achat.

### <a name="create-paypal-test-accounts"></a>Créer des comptes de test PayPal

Pour utiliser l’environnement de test PayPal, vous devez créer et vérifier un compte de test de développeur. Vous allez utiliser le compte test des développeurs pour créer un compte test de l’acheteur et un compte test de vendeur. Les informations d’identification du compte test de développeur permettront également à l’exemple d’application Wingtip Toys d’accéder à l’environnement de test PayPal.

1. Dans un navigateur, accédez au site de test des développeurs PayPal :   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Si vous n’avez pas de compte de développeur PayPal, créez un nouveau compte en cliquant sur s' **inscrire**et suivez les étapes d’inscription. Si vous disposez déjà d’un compte de développeur PayPal, connectez-vous en cliquant sur **se connecter**. Vous aurez besoin de votre compte de développeur PayPal pour tester l’exemple d’application Wingtip Toys plus loin dans ce didacticiel.
3. Si vous venez de vous inscrire à votre compte de développeur PayPal, vous devrez peut-être vérifier votre compte de développeur PayPal avec PayPal. Vous pouvez vérifier votre compte en suivant les étapes que PayPal a envoyées à votre compte de messagerie. Une fois que vous avez vérifié votre compte de développeur PayPal, reconnectez-vous au site de test des développeurs PayPal.
4. Une fois que vous êtes connecté au site de développement PayPal avec votre compte de développeur PayPal, vous devez créer un compte de test PayPal Buyer si vous n’en avez pas déjà un. Pour créer un compte test de l’acheteur, sur le site PayPal, cliquez sur l’onglet **applications** , puis sur **comptes sandbox**.   
 La page **comptes de test bac à sable (sandbox)** s’affiche.   

    > [!NOTE] 
    > 
    > Le site de développement PayPal fournit déjà un compte de test commerçant.

    ![Validation et paiement avec les comptes de test PayPal-sandbox](checkout-and-payment-with-paypal/_static/image15.png)
5. Dans la page comptes de test sandbox, cliquez sur **créer un compte**.
6. Sur la page **créer un compte de test** , choisissez un e-mail et un mot de passe de compte test de l’acheteur de votre choix.   

    > [!NOTE] 
    > 
    > À la fin de ce didacticiel, vous aurez besoin des adresses e-mail et du mot de passe de l’acheteur pour tester l’exemple d’application Wingtip Toys.

    ![Validation et paiement avec les comptes de test PayPal-sandbox](checkout-and-payment-with-paypal/_static/image16.png)
7. Créez le compte test de l’acheteur en cliquant sur le bouton **créer un compte** .  
 La page **comptes de test bac à sable (sandbox)** s’affiche. 

    ![Validation et paiement avec les comptes PayPal-PayPal](checkout-and-payment-with-paypal/_static/image17.png)
8. Dans la page **comptes de test sandbox** , cliquez sur le compte de messagerie **facilitateur** .  
    Les options de **Profil** et de **notification** s’affichent.
9. Sélectionnez l’option **Profil** , puis cliquez sur **informations d’identification** de l’API pour afficher les informations d’identification de votre API pour le compte de test commerçant.
10. Copiez les informations d’identification de l’API de TEST dans le bloc-notes.

Vous aurez besoin de vos informations d’identification d’API de TEST Classic (nom d’utilisateur, mot de passe et signature) pour effectuer des appels d’API à partir de l’exemple d’application Wingtip Toys vers l’environnement de test PayPal. Vous allez ajouter les informations d’identification à l’étape suivante.

### <a name="add-paypal-class-and-api-credentials"></a>Ajouter la classe PayPal et les informations d’identification de l’API

Vous allez placer la majorité du code PayPal dans une seule et même classe. Cette classe contient les méthodes utilisées pour communiquer avec PayPal. Vous allez également ajouter vos informations d’identification PayPal à cette classe.

1. Dans l’exemple d’application Wingtip Toys dans Visual Studio, cliquez avec le bouton droit sur le dossier **logique** , puis sélectionnez **Ajouter** -&gt; **nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sous **Visual C#**  dans le volet **installé** sur la gauche, sélectionnez **code**.
3. Dans le volet central, sélectionnez **classe**. Nommez cette nouvelle classe **PayPalFunctions.cs**.
4. Cliquez sur **Ajouter**.  
   Le nouveau fichier de classe s’affiche dans l’éditeur.
5. Remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Ajoutez les informations d’identification de l’API Merchant (nom d’utilisateur, mot de passe et signature) que vous avez affichées précédemment dans ce didacticiel afin de pouvoir effectuer des appels de fonction vers l’environnement de test PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> Dans cet exemple d’application, vous ajoutez simplement des informations d' C# identification à un fichier (. cs). Toutefois, dans une solution implémentée, envisagez de chiffrer vos informations d’identification dans un fichier de configuration.

La classe NVPAPICaller contient la majorité des fonctionnalités PayPal. Le code de la classe fournit les méthodes nécessaires pour effectuer un test d’achat à partir de l’environnement de test PayPal. Les trois fonctions PayPal suivantes sont utilisées pour effectuer des achats :

- Fonction `SetExpressCheckout`
- Fonction `GetExpressCheckoutDetails`
- Fonction `DoExpressCheckoutPayment`

La méthode `ShortcutExpressCheckout` collecte les informations sur les achats de test et les détails du produit à partir du panier d’achat, puis appelle la fonction `SetExpressCheckout` PayPal. La méthode `GetCheckoutDetails` confirme les détails de l’achat et appelle la fonction `GetExpressCheckoutDetails` PayPal avant d’effectuer le test. La méthode `DoCheckoutPayment` termine l’achat de test à partir de l’environnement de test en appelant la fonction `DoExpressCheckoutPayment` PayPal. Le code restant prend en charge les méthodes et processus PayPal, tels que l’encodage de chaînes, le décodage de chaînes, le traitement des tableaux et la détermination des informations d’identification.

> [!NOTE] 
> 
> PayPal vous permet d’inclure des informations d’achat facultatives basées sur [la spécification de l’API PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). En étendant le code de l’exemple d’application Wingtip Toys, vous pouvez inclure des informations de localisation, des descriptions de produits, des taxes, un numéro de service client, ainsi que de nombreux autres champs facultatifs.

Notez que les URL de retour et d’annulation spécifiées dans la méthode **ShortcutExpressCheckout** utilisent un numéro de port.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Lorsque Visual Web Developer exécute un projet Web à l’aide de SSL, le port 44300 est généralement utilisé pour le serveur Web. Comme indiqué ci-dessus, le numéro de port est 44300. Lorsque vous exécutez l’application, vous pouvez voir un numéro de port différent. Votre numéro de port doit être correctement défini dans le code afin que vous puissiez exécuter l’exemple d’application Wingtip Toys à la fin de ce didacticiel. La section suivante de ce didacticiel explique comment récupérer le numéro de port de l’hôte local et mettre à jour la classe PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Mettre à jour le numéro de port LocalHost dans la classe PayPal

L’exemple d’application Wingtip Toys achète des produits en accédant au site de test PayPal et en revenant à votre instance locale de l’exemple d’application Wingtip Toys. Pour que PayPal retourne à l’URL correcte, vous devez spécifier le numéro de port de l’exemple d’application en cours d’exécution dans le code PayPal mentionné ci-dessus.

1. Dans **Explorateur de solutions** , cliquez avec le bouton droit sur le nom du projet (**WingtipToys**) et sélectionnez **Propriétés**.
2. Dans la colonne de gauche, sélectionnez l’onglet **Web** .
3. Récupérez le numéro de port dans la zone **URL du projet** .
4. Si nécessaire, mettez à jour les `returnURL` et `cancelURL` de la classe PayPal (`NVPAPICaller`) dans le fichier *PayPalFunctions.cs* pour utiliser le numéro de port de votre application Web :   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

À présent, le code que vous avez ajouté correspond au port attendu pour votre application Web locale. PayPal sera en mesure de retourner à l’URL correcte sur votre ordinateur local.

### <a name="add-the-paypal-checkout-button"></a>Ajouter le bouton PayPal Checkout

Maintenant que les fonctions principales PayPal ont été ajoutées à l’exemple d’application, vous pouvez commencer à ajouter le balisage et le code nécessaires à l’appel de ces fonctions. Tout d’abord, vous devez ajouter le bouton d’extraction que l’utilisateur verra sur la page du panier d’achat.

1. Ouvrez le fichier *ShoppingCart. aspx* .
2. Faites défiler le fichier jusqu’en bas et recherchez le `<!--Checkout Placeholder -->` commentaire.
3. Remplacez le commentaire par un contrôle `ImageButton` afin que le marquage soit remplacé comme suit :  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. Dans le fichier *ShoppingCart.aspx.cs* , après l' `UpdateBtn_Click` gestionnaire d’événements près de la fin du fichier, ajoutez le gestionnaire d’événements `CheckOutBtn_Click` :  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Dans le fichier *ShoppingCart.aspx.cs* , ajoutez également une référence au `CheckoutBtn`, afin que le bouton New image soit référencé comme suit :  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Enregistrez les modifications apportées au fichier *ShoppingCart. aspx* et au fichier *ShoppingCart.aspx.cs* .
7. Dans le menu, sélectionnez **Déboguer**-&gt;**générer WingtipToys**.  
   Le projet sera régénéré avec le contrôle **ImageButton** qui vient d’être ajouté.

### <a name="send-purchase-details-to-paypal"></a>Envoyer les détails de l’achat à PayPal

Quand l’utilisateur clique sur le bouton **Checkout** sur la page du panier d’achat (*ShoppingCart. aspx*), il commence le processus d’achat. Le code suivant appelle la première fonction PayPal nécessaire pour acheter des produits.

1. Dans le dossier *Checkout* , ouvrez le fichier code-behind nommé *CheckoutStart.aspx.cs*.   
   Veillez à ouvrir le fichier code-behind.
2. Remplacez le code existant par le code ci-dessous :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Lorsque l’utilisateur de l’application clique sur le bouton **Checkout** sur la page du panier d’achat, le navigateur accède à la page *CheckoutStart. aspx* . Lors du chargement de la page *CheckoutStart. aspx* , la méthode `ShortcutExpressCheckout` est appelée. À ce stade, l’utilisateur est transféré vers le site Web de test PayPal. Sur le site PayPal, l’utilisateur entre ses informations d’identification PayPal, passe en revue les détails de l’achat, accepte le contrat PayPal et retourne à l’exemple d’application Wingtip Toys où se termine la méthode de `ShortcutExpressCheckout`. Lorsque la méthode `ShortcutExpressCheckout` est terminée, elle redirige l’utilisateur vers la page *CheckoutReview. aspx* spécifiée dans la méthode `ShortcutExpressCheckout`. Cela permet à l’utilisateur de passer en revue les détails de la commande à partir de l’exemple d’application Wingtip Toys.

### <a name="review-order-details"></a>Consulter les détails de la commande

Après avoir renvoyé à partir de PayPal, la page *CheckoutReview. aspx* de l’exemple d’application Wingtip Toys affiche les détails de la commande. Cette page permet à l’utilisateur de passer en revue les détails de la commande avant d’acheter les produits. La page *CheckoutReview. aspx* doit être créée comme suit :

1. Dans le dossier *Checkout* , ouvrez la page nommée *CheckoutReview. aspx*.
2. Remplacez le balisage existant par le code suivant :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Ouvrez la page code-behind nommée *CheckoutReview.aspx.cs* et remplacez le code existant par ce qui suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Le contrôle **DetailsView** est utilisé pour afficher les détails des commandes qui ont été retournés par Paypal. En outre, le code ci-dessus enregistre les détails de la commande dans la base de données Wingtip Toys sous la forme d’un objet `OrderDetail`. Quand l’utilisateur clique sur le bouton **terminer l’ordre** , il est redirigé vers la page *CheckoutComplete. aspx* .

> [!NOTE] 
> 
> **Conseil**
> 
> Dans le balisage de la page *CheckoutReview. aspx* , Notez que la balise `<ItemStyle>` est utilisée pour modifier le style des éléments dans le contrôle **DetailsView** près du bas de la page. En affichant la page en **mode Design** (en sélectionnant **conception** dans l’angle inférieur gauche de Visual Studio), puis en sélectionnant le contrôle **DetailsView** et en sélectionnant la **balise active** (l’icône représentant une flèche en haut à droite du contrôle), vous pouvez voir les **Tâches DetailsView**.
> 
> ![Valider et payer avec PayPal-modifier les champs](checkout-and-payment-with-paypal/_static/image18.png)
> 
> En sélectionnant **modifier les champs**, la boîte de dialogue **champs** s’affiche. Dans cette boîte de dialogue, vous pouvez facilement contrôler les propriétés visuelles, telles que **ItemStyle**, du contrôle **DetailsView** .
> 
> ![Boîte de dialogue valider et payer avec PayPal-Fields](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Terminer l’achat

La page *CheckoutComplete. aspx* effectue l’achat auprès de Paypal. Comme indiqué ci-dessus, l’utilisateur doit cliquer sur le bouton **terminer l’ordre** pour que l’application accède à la page *CheckoutComplete. aspx* .

1. Dans le dossier *Checkout* , ouvrez la page nommée *CheckoutComplete. aspx*.
2. Remplacez le balisage existant par le code suivant :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Ouvrez la page code-behind nommée *CheckoutComplete.aspx.cs* et remplacez le code existant par ce qui suit :   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Lorsque la page *CheckoutComplete. aspx* est chargée, la méthode `DoCheckoutPayment` est appelée. Comme indiqué précédemment, la méthode `DoCheckoutPayment` termine l’achat à partir de l’environnement de test PayPal. Une fois que PayPal a terminé l’achat de la commande, la page *CheckoutComplete. aspx* affiche une transaction de paiement `ID` à l’acheteur.

### <a name="handle-cancel-purchase"></a>Gérer annuler l’achat

Si l’utilisateur décide d’annuler l’achat, il est dirigé vers la page *CheckoutCancel. aspx* , où il verra que son ordre a été annulé.

1. Ouvrez la page nommée *CheckoutCancel. aspx* dans le dossier *Checkout* .
2. Remplacez le balisage existant par le code suivant :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Gérer les erreurs d’achat

Les erreurs survenues pendant le processus d’achat sont gérées par la page *CheckoutError. aspx* . Le code-behind de la page *CheckoutStart. aspx* , la page *CheckoutReview. aspx* et la page *CheckoutComplete. aspx* redirigent chacun vers la page *CheckoutError. aspx* si une erreur se produit.

1. Ouvrez la page nommée *CheckoutError. aspx* dans le dossier *Checkout* .
2. Remplacez le balisage existant par le code suivant :   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

La page *CheckoutError. aspx* s’affiche avec les détails de l’erreur lorsqu’une erreur se produit pendant le processus d’extraction.

## <a name="running-the-application"></a>Exécution de l'application

Exécutez l’application pour voir comment acheter des produits. Notez que vous allez exécuter dans l’environnement de test PayPal. Aucun argent réel n’est échangé.

1. Assurez-vous que tous vos fichiers sont enregistrés dans Visual Studio.
2. Ouvrez un navigateur Web et accédez à [https://developer.paypal.com](https://developer.paypal.com/).
3. Connectez-vous avec votre compte de développeur PayPal que vous avez créé précédemment dans ce didacticiel.  
   Pour le sandbox de développement PayPal, vous devez être connecté à [https://developer.paypal.com](https://developer.paypal.com/) pour tester l’extraction Express. Cela s’applique uniquement aux tests du bac à sable (sandbox) PayPal, et non à l’environnement Live de PayPal.
4. Dans Visual Studio, appuyez sur **F5** pour exécuter l’exemple d’application Wingtip Toys.  
   Une fois la base de données reconstruite, le navigateur s’ouvre et affiche la page *default. aspx* .
5. Ajoutez trois produits différents au panier d’achat en sélectionnant la catégorie de produit, par exemple « cars », puis en cliquant sur **Ajouter au panier** en regard de chaque produit.  
   Le panier affiche le produit que vous avez sélectionné.
6. Cliquez sur le bouton **PayPal** pour valider. 

    ![Validation et paiement avec PayPal-cart](checkout-and-payment-with-paypal/_static/image20.png)

   L’extraction nécessite que vous disposiez d’un compte d’utilisateur pour l’exemple d’application Wingtip Toys.
7. Cliquez sur le lien **Google** à droite de la page pour vous connecter avec un compte de messagerie Gmail.com existant.  
   Si vous n’avez pas de compte gmail.com, vous pouvez en créer un à des fins de test sur [www.gmail.com](https://www.gmail.com/). Vous pouvez également utiliser un compte local standard en cliquant sur « s’inscrire ». 

    ![Valider et payer avec PayPal-log in](checkout-and-payment-with-paypal/_static/image21.png)
8. Connectez-vous avec votre compte et votre mot de passe Gmail. 

    ![Valider et payer avec la connexion PayPal-gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Cliquez sur le bouton **se connecter** pour inscrire votre compte Gmail auprès du nom d’utilisateur de l’exemple d’application Wingtip Toys. 

    ![Validation et paiement avec PayPal-inscrire un compte](checkout-and-payment-with-paypal/_static/image23.png)
10. Sur le site de test PayPal, ajoutez l’adresse de messagerie et le mot de passe de l' **acheteur** que vous avez créés précédemment dans ce didacticiel, puis cliquez sur le bouton **se connecter** . 

    ![Validation et paiement avec PayPal-connexion PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Acceptez la stratégie PayPal, puis cliquez sur le bouton **accepter et continuer** .  
    Notez que cette page s’affiche uniquement la première fois que vous utilisez ce compte PayPal. Notez à nouveau qu’il s’agit d’un compte de test, aucun argent n’est échangé. 

    ![Validation et paiement avec la stratégie PayPal-PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Passez en revue les informations de commande sur la page de vérification de l’environnement de test PayPal, puis cliquez sur **Continuer**. 

    ![Validation et paiement avec PayPal-informations de vérification](checkout-and-payment-with-paypal/_static/image26.png)
13. Sur la page *CheckoutReview. aspx* , vérifiez le montant de la commande et affichez l’adresse de livraison générée. Ensuite, cliquez sur le bouton **terminer l’ordre** . 

    ![Validation et paiement avec l’évaluation de l’ordre PayPal](checkout-and-payment-with-paypal/_static/image27.png)
14. La page **CheckoutComplete. aspx** s’affiche avec un ID de transaction de paiement. 

    ![Validation et paiement avec PayPal-Checkout terminé](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Vérification de la base de données

En passant en revue les données mises à jour dans l’exemple de base de données d’application Wingtip Toys après l’exécution de l’application, vous pouvez voir que l’application a correctement enregistré l’achat des produits.

Vous pouvez inspecter les données contenues dans le fichier de base de données *wingtiptoys. mdf* à l’aide de la fenêtre de **Explorateur de base de données** (**Explorateur de serveurs** fenêtre dans Visual Studio) comme vous l’avez fait précédemment dans cette série de didacticiels.

1. Fermez la fenêtre du navigateur si elle est toujours ouverte.
2. Dans Visual Studio, sélectionnez l’icône **Afficher tous les fichiers** en haut de **Explorateur de solutions** pour vous permettre de développer le dossier de données de l' **application\_** .
3. Développez le dossier données de l' **application\_** .  
 Vous devrez peut-être sélectionner l’icône **Afficher tous les fichiers** du dossier.
4. Cliquez avec le bouton droit sur le fichier de base de données *wingtiptoys. mdf* , puis sélectionnez **ouvrir**.  
    **Explorateur de serveurs** s’affiche.
5. Développez le dossier **Tables** .
6. Cliquez avec le bouton droit sur la table **Orders**, puis sélectionnez **afficher les données**de la table.  
 La table **Orders** s’affiche.
7. Passez en revue la colonne **PaymentTransactionID** pour confirmer la réussite des transactions. 

    ![Valider et payer avec la base de données de vérification PayPal](checkout-and-payment-with-paypal/_static/image29.png)
8. Fermez la fenêtre table **Orders** .
9. Dans la Explorateur de serveurs, cliquez avec le bouton droit sur la table **OrderDetails** et sélectionnez **afficher les données**de la table.
10. Passez en revue les valeurs `OrderId` et `Username` dans la table **OrderDetails** . Notez que ces valeurs correspondent aux valeurs `OrderId` et `Username` incluses dans la table **Orders** .
11. Fermez la fenêtre table **OrderDetails** .
12. Cliquez avec le bouton droit sur le fichier de base de données Wingtip Toys (*wingtiptoys. mdf*), puis sélectionnez **Fermer la connexion**.
13. Si vous ne voyez pas la fenêtre de **Explorateur de solutions** , cliquez sur **Explorateur de solutions** en bas de la fenêtre de **Explorateur de serveurs** pour afficher à nouveau le **Explorateur de solutions** .

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez ajouté des schémas de commande et de détail de commande pour effectuer le suivi de l’achat de produits. Vous avez également intégré la fonctionnalité PayPal à l’exemple d’application Wingtip Toys.

## <a name="additional-resources"></a>Ressources supplémentaires

[Présentation de la configuration ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Déployer une application ASP.NET Web Forms sécurisée avec appartenance, OAuth et SQL Database à Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-essai gratuit](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Exclusion de responsabilité

Ce didacticiel contient un exemple de code. Ces exemples de code sont fournis « en l’État » sans garantie d’aucune sorte. En conséquence, Microsoft ne garantit pas la précision, l’intégrité ou la qualité de l’exemple de code. Vous acceptez d’utiliser l’exemple de code à vos propres risques. En aucun cas, Microsoft ne pourra être tenu responsable d’un exemple de code, de contenu, y compris mais non limité à, d’erreurs ou d’omissions dans un exemple de code, de contenu ou de toute perte ou détérioration d’un type résultant de l’utilisation d’un exemple de code. Vous en êtes informé et vous acceptez d’indemniser, d’enregistrer et de détenir les personnes qui ne sont pas inoffensives de toute perte, réclamation de perte, blessure ou dommage de quelque nature que ce soit, y compris, sans limitation, celles provoquées par le matériel que vous publiez, transmettez, utilisez ou recourez à, mais sans s’y limiter, les vues qui y sont exprimées.

> [!div class="step-by-step"]
> [Précédent](shopping-cart.md)
> [Suivant](membership-and-administration.md)
