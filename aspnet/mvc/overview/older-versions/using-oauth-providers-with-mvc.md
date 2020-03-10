---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Utilisation de fournisseurs OAuth avec MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application Web ASP.NET MVC 4 qui permet aux utilisateurs de se connecter avec les informations d’identification d’un fournisseur externe, tel que facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5dfd1305376a62f4987caea242ca0f6aac1018e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539080"
---
# <a name="using-oauth-providers-with-mvc-4"></a>Utilisation de fournisseurs OAuth avec MVC 4

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment créer une application Web ASP.NET MVC 4 qui permet aux utilisateurs de se connecter avec les informations d’identification d’un fournisseur externe, tel que Facebook, Twitter, Microsoft ou Google, puis d’intégrer certaines des fonctionnalités de ces fournisseurs dans votre application Web. Pour plus de simplicité, ce didacticiel se concentre sur l’utilisation des informations d’identification de Facebook.
> 
> Pour utiliser des informations d’identification externes dans une application Web ASP.NET MVC 5, consultez [créer une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et l’authentification OpenID](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> L’activation de ces informations d’identification dans vos sites Web constitue un avantage significatif, car des millions d’utilisateurs disposent déjà de comptes avec ces fournisseurs externes. Ces utilisateurs peuvent être plus enclins à s’inscrire à votre site s’ils n’ont pas à créer et à mémoriser un nouvel ensemble d’informations d’identification. En outre, une fois qu’un utilisateur s’est connecté via l’un de ces fournisseurs, vous pouvez incorporer des opérations sociales à partir du fournisseur.

## <a name="what-youll-build"></a>Ce que vous allez générer

Ce didacticiel présente deux objectifs principaux :

1. Permet à un utilisateur de se connecter avec les informations d’identification d’un fournisseur OAuth.
2. Récupérez les informations de compte du fournisseur et intégrez ces informations à l’inscription du compte de votre site.

Bien que les exemples de ce didacticiel se concentrent sur l’utilisation de Facebook comme fournisseur d’authentification, vous pouvez modifier le code pour utiliser l’un des fournisseurs. Les étapes à suivre pour implémenter un fournisseur sont très similaires à celles que vous verrez dans ce didacticiel. Vous ne remarquerez que des différences importantes lors des appels directs à l’ensemble d’API du fournisseur.

## <a name="prerequisites"></a>Conditions préalables requises

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) ou [Microsoft Visual Studio Express 2012 pour le Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Ou

- Microsoft Visual Studio 2010 SP1 ou [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

En outre, cette rubrique suppose que vous avez des connaissances de base sur ASP.NET MVC et Visual Studio. Si vous avez besoin d’une introduction à ASP.NET MVC 4, consultez [Introduction à ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Créer le projet

Dans Visual Studio, créez une nouvelle application Web ASP.NET MVC 4 et nommez-la &quot;OAuthMVC&quot;. Vous pouvez cibler .NET Framework 4,5 ou 4.

![créer un projet](using-oauth-providers-with-mvc/_static/image1.png)

Dans la fenêtre nouveau projet ASP.NET MVC 4, sélectionnez **application Internet** et laissez le moteur d’affichage **Razor** .

![sélectionner une application Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Activer un fournisseur

Lorsque vous créez une application Web MVC 4 avec le modèle d’application Internet, le projet est créé avec un fichier nommé AuthConfig.cs dans le dossier de démarrage de l’application\_.

![Fichier AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Le fichier AuthConfig contient le code permettant d’inscrire des clients pour des fournisseurs d’authentification externes. Par défaut, ce code est commenté, donc aucun des fournisseurs externes n’est activé.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Vous devez supprimer les marques de commentaire de ce code pour utiliser le client d’authentification externe. Vous supprimez les marques de commentaire des fournisseurs que vous souhaitez inclure dans votre site. Pour ce didacticiel, vous allez uniquement activer les informations d’identification Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Notez que dans l’exemple ci-dessus, la méthode comprend des chaînes vides pour les paramètres d’inscription. Si vous essayez d’exécuter l’application maintenant, l’application lève une exception d’argument, car les chaînes vides ne sont pas autorisées pour les paramètres. Pour fournir des valeurs valides, vous devez inscrire votre site Web auprès des fournisseurs externes, comme indiqué dans la section suivante.

## <a name="registering-with-an-external-provider"></a>Inscription auprès d’un fournisseur externe

Pour authentifier les utilisateurs avec les informations d’identification d’un fournisseur externe, vous devez inscrire votre site Web auprès du fournisseur. Lorsque vous inscrivez votre site, vous recevez les paramètres (tels que clé ou ID et secret) à inclure lors de l’inscription du client. Vous devez disposer d’un compte avec les fournisseurs que vous souhaitez utiliser.

Ce didacticiel n’affiche pas toutes les étapes que vous devez effectuer pour vous inscrire auprès de ces fournisseurs. Les étapes ne sont généralement pas difficiles. Pour inscrire correctement votre site, suivez les instructions fournies sur ces sites. Pour commencer à inscrire votre site, consultez le site du développeur pour :

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Lors de l’inscription de votre site auprès de Facebook, vous pouvez fournir &quot;localhost&quot; pour le domaine de site et `&quot; http://localhost/&quot;` pour l’URL, comme indiqué dans l’image ci-dessous. L’utilisation de localhost fonctionne avec la plupart des fournisseurs, mais ne fonctionne pas actuellement avec le fournisseur Microsoft. Pour le fournisseur Microsoft, vous devez inclure une URL de site Web valide.

![inscrire un site](using-oauth-providers-with-mvc/_static/image4.png)

Dans l’image précédente, les valeurs de l’ID d’application, de la clé secrète de l’application et de l’adresse e-mail du contact ont été supprimées. Lorsque vous inscrivez votre site, ces valeurs sont présentes. Vous devez noter les valeurs pour ID d’application et secret de l’application, car vous les ajouterez à votre application.

## <a name="creating-test-users"></a>Création d’utilisateurs de test

Si vous n’envisagez pas d’utiliser un compte Facebook existant pour tester votre site, vous pouvez ignorer cette section.

Vous pouvez facilement créer des utilisateurs de test pour votre application dans la page de gestion des applications Facebook. Vous pouvez utiliser ces comptes de test pour vous connecter à votre site. Pour créer des utilisateurs de test, cliquez sur le lien **rôles** dans le volet de navigation gauche, puis sur le lien **créer** .

![créer des utilisateurs de test](using-oauth-providers-with-mvc/_static/image5.png)

Le site Facebook crée automatiquement le nombre de comptes de test que vous demandez.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Ajout de l’ID d’application et du secret à partir du fournisseur

Maintenant que vous avez reçu l’ID et le secret de Facebook, revenez au fichier AuthConfig et ajoutez-les en tant que valeurs de paramètre. Les valeurs indiquées ci-dessous ne sont pas des valeurs réelles.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Se connecter avec des informations d’identification externes

C’est tout ce que vous devez faire pour activer les informations d’identification externes dans votre site. Exécutez votre application, puis cliquez sur le lien de connexion dans le coin supérieur droit. Le modèle reconnaît automatiquement que vous avez inscrit Facebook en tant que fournisseur et comprend un bouton pour le fournisseur. Si vous inscrivez plusieurs fournisseurs, un bouton est automatiquement inclus pour chacun d’eux.

![connexion externe](using-oauth-providers-with-mvc/_static/image6.png)

Ce didacticiel ne traite pas de la personnalisation des boutons de connexion aux fournisseurs externes. Pour plus d’informations, consultez [Personnalisation de l’interface utilisateur de connexion lors de l’utilisation d’OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Cliquez sur le bouton Facebook pour vous connecter avec les informations d’identification Facebook. Lorsque vous sélectionnez l’un des fournisseurs externes, vous êtes redirigé vers ce site et invité par ce service à se connecter.

L’image suivante montre l’écran de connexion de Facebook. Il remarque que vous utilisez votre compte Facebook pour vous connecter à un site nommé oauthmvcexample.

![authentification Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Une fois connecté avec les informations d’identification Facebook, une page informe l’utilisateur que le site aura accès aux informations de base.

![demander l’autorisation](using-oauth-providers-with-mvc/_static/image8.png)

Une fois que vous avez sélectionné **accéder à l’application**, l’utilisateur doit s’inscrire au site. L’illustration suivante montre la page d’inscription après qu’un utilisateur s’est connecté avec des informations d’identification Facebook. Le nom d’utilisateur est généralement pré-rempli avec un nom du fournisseur.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Cliquez sur **s’inscrire** pour terminer l’inscription. Fermez le navigateur.

Vous pouvez voir que le nouveau compte a été ajouté à votre base de données. Dans Explorateur de serveurs, ouvrez la base de données **DefaultConnection** et ouvrez le dossier **tables** .

![tables de base de données](using-oauth-providers-with-mvc/_static/image10.png)

Cliquez avec le bouton droit sur la table **UserProfile** et sélectionnez **afficher les données**de la table.

![afficher les données](using-oauth-providers-with-mvc/_static/image11.png)

Vous verrez le nouveau compte que vous avez ajouté. Examinez les données dans la **page web\_table OAuthMembership** . Vous verrez plus de données relatives au fournisseur externe pour le compte que vous venez d’ajouter.

Si vous souhaitez uniquement activer l’authentification externe, vous avez terminé. Toutefois, vous pouvez intégrer les informations du fournisseur dans le nouveau processus d’inscription des utilisateurs, comme indiqué dans les sections suivantes.

## <a name="create-models-for-additional-user-information"></a>Créer des modèles pour des informations utilisateur supplémentaires

Comme vous l’avez remarqué dans les sections précédentes, vous n’avez pas besoin de récupérer des informations supplémentaires pour que l’inscription du compte intégré fonctionne. Toutefois, la plupart des fournisseurs externes renvoient des informations supplémentaires sur l’utilisateur. Les sections suivantes montrent comment conserver ces informations et les enregistrer dans une base de données. Plus précisément, vous conservez des valeurs pour le nom complet de l’utilisateur, l’URI de la page Web personnelle de l’utilisateur et une valeur qui indique si Facebook a vérifié le compte.

Vous utiliserez [migrations code First](https://msdn.microsoft.com/data/jj591621) pour ajouter une table pour stocker des informations utilisateur supplémentaires. Vous ajoutez la table à une base de données existante. vous devez donc d’abord créer un instantané de la base de données active. En créant un instantané de la base de données actuelle, vous pouvez créer ultérieurement une migration qui contient uniquement la nouvelle table. Pour créer un instantané de la base de données active :

1. Ouvrir la **console du gestionnaire de package**
2. Exécuter la commande **Enable-migrations**
3. Exécutez la commande **Add-migration initial – IgnoreChanges**
4. Exécutez la commande **Update-Database**

À présent, vous allez ajouter les nouvelles propriétés. Dans le dossier Models, ouvrez le fichier AccountModels.cs et recherchez la classe RegisterExternalLoginModel. La classe RegisterExternalLoginModel contient les valeurs renvoyées par le fournisseur d’authentification. Ajoutez des propriétés nommées FullName et Link, comme indiqué ci-dessous.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

En outre, dans AccountModels.cs, ajoutez une nouvelle classe appelée ExtraUserInformation. Cette classe représente la nouvelle table qui sera créée dans la base de données.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Dans la classe UsersContext, ajoutez le code en surbrillance ci-dessous pour créer une propriété DbSet pour la nouvelle classe.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Vous êtes maintenant prêt à créer la table. Rouvrez la console du gestionnaire de package et cette fois-ci :

1. Exécutez la commande **Add-migration AddExtraUserInformation**
2. Exécutez la commande **Update-Database**

La nouvelle table existe maintenant dans la base de données.

## <a name="retrieve-the-additional-data"></a>Récupérer les données supplémentaires

Il existe deux façons de récupérer des données utilisateur supplémentaires. La première consiste à conserver les données utilisateur qui sont renvoyées, par défaut, pendant la demande d’authentification. La seconde consiste à appeler spécifiquement l’API du fournisseur et à demander plus d’informations. Les valeurs de FullName et Link sont renvoyées automatiquement par Facebook. Valeur qui indique si Facebook a vérifié que le compte est récupéré via un appel à l’API Facebook. Tout d’abord, vous allez renseigner les valeurs de FullName et Link, puis vous obtiendrez la valeur vérifiée.

Pour récupérer les données utilisateur supplémentaires, ouvrez le fichier **AccountController.cs** dans le dossier **Controllers** .

Ce fichier contient la logique de journalisation, d’inscription et de gestion des comptes. En particulier, notez les méthodes appelées **ExternalLoginCallback** et **ExternalLoginConfirmation**. Dans ces méthodes, vous pouvez ajouter du code pour personnaliser les opérations de connexion externes pour votre application. La première ligne de la méthode **ExternalLoginCallback** contient :

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Les données utilisateur supplémentaires sont retournées dans la propriété **ExtraData** de l’objet **AuthenticationResult** retourné à partir de la méthode **VerifyAuthentication** . Le client Facebook contient les valeurs suivantes dans la propriété **ExtraData** :

- ID
- name
- link
- gender
- AccessToken

Les autres fournisseurs auront des données similaires, mais légèrement différentes, dans la propriété ExtraData.

Si l’utilisateur est nouveau sur votre site, vous allez récupérer des données supplémentaires et transmettre ces données à l’affichage de confirmation. Le dernier bloc de code de la méthode est exécuté uniquement si l’utilisateur est nouveau sur votre site. Remplacez la ligne suivante :

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Avec cette ligne :

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Cette modification comprend simplement des valeurs pour les propriétés FullName et Link.

Dans la méthode **ExternalLoginConfirmation** , modifiez le code comme indiqué ci-dessous pour enregistrer les informations supplémentaires sur l’utilisateur.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Ajustement de la vue

Les données utilisateur supplémentaires que vous récupérez à partir du fournisseur seront affichées dans la page d’inscription.

Dans le dossier **Views**/**Account** , ouvrez **ExternalLoginConfirmation. cshtml**. Sous le champ existant pour nom d’utilisateur, ajoutez des champs pour FullName, Link et PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Vous êtes maintenant presque prêt à exécuter l’application et à inscrire un nouvel utilisateur avec les informations supplémentaires enregistrées. Vous devez disposer d’un compte qui n’est pas déjà inscrit sur le site. Vous pouvez soit utiliser un compte de test différent, soit supprimer les lignes dans les tables **UserProfile** et **Web\_** les tables OAuthMembership pour le compte que vous souhaitez réutiliser. En supprimant ces lignes, vous êtes assuré que le compte est inscrit de nouveau.

Exécutez l’application et inscrivez le nouvel utilisateur. Notez que cette fois, la page de confirmation contient plus de valeurs.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Une fois l’inscription terminée, fermez le navigateur. Recherchez dans la base de données les nouvelles valeurs de la table **ExtraUserInformation** .

## <a name="install-nuget-package-for-facebook-api"></a>Installer le package NuGet pour l’API Facebook

Facebook fournit une [API](https://developers.facebook.com/docs/reference/apis/) que vous pouvez appeler pour effectuer des opérations. Vous pouvez appeler l’API Facebook en dirigeant l’envoi de requêtes HTTP ou en installant un package NuGet qui facilite l’envoi de ces demandes. L’utilisation d’un package NuGet est présentée dans ce didacticiel, mais l’installation du package NuGet n’est pas essentielle. Ce didacticiel montre comment utiliser le package du C# Kit de développement logiciel (SDK) Facebook. D’autres packages NuGet permettent d’appeler l’API Facebook.

Dans la fenêtre **gérer les packages NuGet** , sélectionnez le C# package du kit de développement logiciel (SDK) Facebook.

![installer le package](using-oauth-providers-with-mvc/_static/image13.png)

Vous allez utiliser le kit C# de développement logiciel (SDK) Facebook pour appeler une opération qui requiert le jeton d’accès pour l’utilisateur. La section suivante montre comment récupérer le jeton d’accès.

## <a name="retrieve-access-token"></a>Récupérer le jeton d’accès

La plupart des fournisseurs externes renvoient un jeton d’accès après que les informations d’identification de l’utilisateur ont été vérifiées. Ce jeton d’accès est très important, car il vous permet d’appeler des opérations qui sont uniquement disponibles pour les utilisateurs authentifiés. Par conséquent, il est essentiel de récupérer et de stocker le jeton d’accès lorsque vous souhaitez fournir davantage de fonctionnalités.

Selon le fournisseur externe, le jeton d’accès peut ne pas être valide pour une durée limitée. Pour vous assurer que vous disposez d’un jeton d’accès valide, vous devez le récupérer chaque fois que l’utilisateur se connecte et le stocker en tant que valeur de session au lieu de l’enregistrer dans une base de données.

Dans la méthode **ExternalLoginCallback** , le jeton d’accès est également renvoyé dans la propriété **ExtraData** de l’objet **AuthenticationResult** . Ajoutez le code en surbrillance à **ExternalLoginCallback** pour enregistrer le jeton d’accès dans l’objet **session** . Ce code est exécuté chaque fois que l’utilisateur se connecte avec un compte Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Bien que cet exemple récupère un jeton d’accès à partir de Facebook, vous pouvez récupérer le jeton d’accès de n’importe quel fournisseur externe via la même clé nommée &quot;AccessToken&quot;.

## <a name="logging-off"></a>Déconnexion

La méthode de **déconnexion** par défaut déconnecte l’utilisateur de votre application, mais ne déconnecte pas l’utilisateur du fournisseur externe. Pour déconnecter l’utilisateur de Facebook et empêcher le jeton de persister une fois que l’utilisateur s’est déconnecté, vous pouvez ajouter le code en surbrillance suivant à la méthode de **fermeture** de session dans le AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

La valeur que vous fournissez dans le paramètre `next` est l’URL à utiliser une fois que l’utilisateur s’est déconnecté de Facebook. Lorsque vous testez sur votre ordinateur local, vous devez fournir le numéro de port correct pour votre site local. En production, vous devez fournir une page par défaut, par exemple contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Récupérer les informations utilisateur qui requièrent le jeton d’accès

Maintenant que vous avez stocké le jeton d’accès et installé C# le package du kit de développement logiciel (SDK) Facebook, vous pouvez les utiliser ensemble pour demander des informations utilisateur supplémentaires à Facebook. Dans la méthode **ExternalLoginConfirmation** , créez une instance de la classe **FacebookClient** en passant la valeur du jeton d’accès. Demandez la valeur de la propriété **vérifiée** pour l’utilisateur authentifié actuel. La propriété **vérifié** indique si Facebook a validé le compte par d’autres moyens, tels que l’envoi d’un message à un téléphone portable. Enregistrez cette valeur dans la base de données.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Vous devrez à nouveau supprimer les enregistrements de la base de données de l’utilisateur ou utiliser un autre compte Facebook.

Exécutez l’application et inscrivez le nouvel utilisateur. Examinez la table **ExtraUserInformation** pour voir la valeur de la propriété vérifié.

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez créé un site intégré à Facebook pour l’authentification utilisateur et les données d’inscription. Vous avez appris le comportement par défaut qui est configuré pour l’application Web MVC 4 et comment personnaliser ce comportement par défaut.

## <a name="related-topics"></a>Rubriques connexes

- [Créer une application ASP.NET MVC avec authentification et base de données SQL et la déployer dans Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
