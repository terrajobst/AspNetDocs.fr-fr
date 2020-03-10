---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Développement d’applications ASP.NET avec Azure Active Directory-ASP.NET 4. x
author: Rick-Anderson
description: Microsoft ASP.NET Tools pour Azure Active Directory facilite l’activation de l’authentification pour les applications Web hébergées sur Azure. Vous pouvez utiliser Azure authenti...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 28425ea8d1312dfc6e14df9677396f2cbcf6f16d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583852"
---
# <a name="developing-aspnet-apps-with-azure-active-directory"></a>Développement d’applications ASP.NET avec Azure Active Directory

par [Rick Anderson](https://twitter.com/RickAndMSFT)

Microsoft ASP.NET Tools pour Azure Active Directory simplifie l’activation de l’authentification pour les applications Web hébergées sur [Azure](https://www.windowsazure.com/home/features/web-sites/). Vous pouvez utiliser l’authentification Azure pour authentifier les utilisateurs Office 365 de votre organisation, les comptes d’entreprise synchronisés à partir de votre Active Directory local ou les utilisateurs créés dans votre propre domaine Azure Active Directory personnalisé. L’activation de l’authentification Windows Azure configure votre application pour authentifier les utilisateurs à l’aide d’un seul [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) locataire.

Ce didacticiel vous montre comment créer une application ASP.NET qui est configurée pour l’authentification avec [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Vous allez également apprendre à appeler le API Graph pour obtenir des informations sur l’utilisateur actuellement connecté et sur la manière de déployer l’application sur Azure.

## <a name="prerequisites"></a>Conditions préalables requises

1. [Visual Studio Express 2013 pour Web](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express) ou [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 ou version ultérieure est requis.
3. Un compte Azure. [Cliquez ici](https://azure.microsoft.com/pricing/free-trial/) pour obtenir une version d’évaluation gratuite si vous n’avez pas encore de compte.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Ajouter un administrateur général à votre Active Directory

1. Connectez-vous au [portail de gestion Azure](https://manage.windowsazure.com/).
2. Tous les comptes Azure contiennent un **répertoire par défaut** , cliquez dessus, puis cliquez sur l’onglet **utilisateurs** en haut de la page (Voir l’image ci-dessous).
3. Cliquez sur Ajouter un utilisateur.
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Créez un utilisateur avec le rôle d' **administrateur général** . Cliquez sur **utilisateurs** dans le menu supérieur, puis cliquez sur le bouton **Ajouter un utilisateur** dans la barre de commandes.
5. Dans la boîte de dialogue **Ajouter un utilisateur** , entrez un nom pour le nouvel utilisateur, puis cliquez sur la flèche droite.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Entrez le nom d’utilisateur et définissez le rôle sur **administrateur général**. Les administrateurs généraux requièrent une adresse de messagerie de secours pour la récupération du mot de passe. Une fois que vous avez terminé, cliquez sur la flèche droite.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Sur la page suivante de la boîte de dialogue, cliquez sur **créer**. Un mot de passe temporaire sera créé pour le nouvel utilisateur et affiché dans la boîte de dialogue.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)

   Enregistrez le mot de passe, vous devez modifier le mot de passe après la première connexion. L’illustration suivante montre le nouveau compte d’administrateur. Vous devez utiliser la Azure Active Directory pour vous connecter à votre application, et non la compte Microsoft également indiquée sur cette page.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Créer une application ASP.NET

Les étapes suivantes utilisent [Visual Studio Express 2013 pour le Web](https://www.microsoft.com/download/details.aspx?id=40747)et nécessitent [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. Dans Visual Studio, cliquez sur **fichier** , puis sur **nouveau projet**. Dans la boîte de dialogue **nouveau projet** , sélectionnez C# le projet Web visuel dans le menu de gauche, puis cliquez sur **OK**. Vous pouvez également décocher la case **Ajouter des application Insights au projet** si vous ne souhaitez pas les fonctionnalités de votre application.
2. Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez **MVC**, puis cliquez sur **modifier l’authentification**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Dans la boîte de dialogue **modifier l’authentification** , sélectionnez **comptes organisationnels**. Ces options peuvent être utilisées pour inscrire automatiquement votre application auprès de Azure AD, ainsi que pour configurer automatiquement votre application pour une intégration à Azure AD. Vous n’êtes pas obligé d’utiliser la boîte de dialogue **modifier l’authentification** pour inscrire et configurer votre application, mais cela facilite grandement la tâche. Si vous utilisez Visual Studio 2012, par exemple, vous pouvez toujours inscrire manuellement l’application dans le Portail de gestion Azure et mettre à jour sa configuration pour l’intégrer à Azure AD.
   Dans les menus déroulants, sélectionnez **Cloud-organisation unique** et **authentification unique, lire les données d’annuaire**. Entrez le domaine de votre répertoire Azure AD, par exemple (dans les images ci-dessous) *aricka0yahoo.onmicrosoft.com*, puis cliquez sur **OK**. Vous pouvez obtenir le nom de domaine à partir de l’onglet domaines pour le répertoire par défaut sur le portail Azure (Voir l’image suivante en dessous).

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)

   L’illustration suivante montre le nom de domaine de la Portail Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)

    > [!NOTE]
    > Vous pouvez éventuellement configurer l’URI d’ID d’application qui sera inscrit dans Azure AD en cliquant sur **plus d’options**. L’URI ID d’application est l’identificateur unique d’une application, qui est inscrite dans Azure AD et utilisée par l’application pour s’identifier lorsqu’elle communique avec Azure AD. Pour plus d’informations sur l’URI ID d’application et d’autres propriétés des applications inscrites, consultez [cette rubrique](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). En cliquant sur la case à cocher sous le champ URI ID d’application, vous pouvez également choisir de remplacer une inscription existante dans Azure AD qui utilise le même URI d’ID d’application.
4. Une fois que vous avez cliqué sur **OK**, une boîte de dialogue de connexion s’affiche et vous devez vous connecter à l’aide d’un compte d’administrateur général (et non de la compte Microsoft associée à votre abonnement). Si vous avez créé un nouveau compte d’administrateur précédemment, vous devez modifier le mot de passe, puis vous reconnecter à l’aide du nouveau mot de passe.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Une fois authentifié, la boîte de dialogue **nouveau projet ASP.net** affiche votre choix d’authentification (**organisation** ) et le répertoire dans lequel la nouvelle application sera inscrite (*aricka0yahoo.onmicrosoft.com* dans l’image ci-dessous). Sous ces informations, activez la case à cocher **hôte dans le Cloud**. Si cette case à cocher est activée, le projet est approvisionné comme une application Web Azure et sera activé pour une publication simplifiée ultérieurement. Cliquez sur **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. La boîte de dialogue **configurer le site Web Azure** s’affiche, à l’aide d’un nom et d’une région de site générés automatiquement. Notez également le compte auquel vous êtes actuellement connecté dans la boîte de dialogue. Vous voulez vous assurer que ce compte est celui auquel votre abonnement Azure est attaché, en général un compte Microsoft.

    > [!NOTE]
    > Ce projet nécessite une base de données. Vous devez sélectionner l’une de vos bases de données existantes ou en créer une nouvelle. Une base de données est nécessaire car le projet utilise déjà un fichier de base de données local pour stocker une petite quantité de données de configuration d’authentification. Lorsque vous déployez l’application sur un site Web Azure, cette base de données n’est pas empaquetée avec le déploiement. vous devez donc en choisir une accessible dans le Cloud. Cliquez sur **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Le projet sera créé et vos options d’authentification et vos options d’application Web seront automatiquement configurées avec le projet. Une fois ce processus terminé, exécutez le projet localement en appuyant sur **^ F5**. Vous devez vous connecter à l’aide de votre compte professionnel. Indiquez le nom d’utilisateur et le mot de passe du compte que vous avez créé précédemment, puis cliquez sur **se connecter**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Une fois la connexion établie, le site ASP.NET indique que vous vous êtes authentifié en affichant le nom d’utilisateur dans le coin supérieur droit de la page.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)

   Si vous recevez l’erreur : la valeur ne peut pas être null ou vide. Nom du paramètre : linkText ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)

   consultez la section [Déboguer](#dbg) à la fin du didacticiel.

## <a name="basics-of-the-graph-api"></a>Notions de base de l’API Graph

[Le API Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) est l’interface de programmation utilisée pour effectuer des opérations CRUD et d’autres opérations sur les objets de votre répertoire Azure ad. Si vous sélectionnez une option de compte professionnel pour l’authentification lors de la création d’un nouveau projet dans Visual Studio 2013, votre application est déjà configurée pour appeler le API Graph. Cette section vous montre brièvement comment fonctionne le API Graph.

1. Dans votre application en cours d’exécution, cliquez sur le nom de l’utilisateur connecté en haut à droite de la page. Cela vous permet de vous connecter à la page de profil utilisateur, qui est une action sur le contrôleur d’hébergement. Vous remarquerez que la table contient des informations utilisateur sur le compte administrateur que vous avez créé précédemment. Ces informations sont stockées dans votre répertoire, et le API Graph est appelé pour récupérer ces informations lors du chargement de la page.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Revenez à Visual Studio et développez le dossier **Controllers** , puis ouvrez le fichier **HomeController.cs** . Vous verrez une action **UserProfile ()** qui contient du code pour récupérer un jeton, puis appeler l’API Graph. Ce code est dupliqué ci-dessous :

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Pour appeler le API Graph, vous devez d’abord récupérer un jeton. Lorsque le jeton est récupéré, sa valeur de chaîne doit être ajoutée à l’en-tête d’autorisation pour toutes les demandes suivantes adressées à l’API Graph. La majeure partie du code ci-dessus gère les détails de l’authentification auprès de Azure AD pour obtenir un jeton, en utilisant le jeton pour effectuer un appel au API Graph, puis en transformant la réponse afin qu’elle puisse être présentée dans la vue.

    La partie la plus pertinente pour la discussion est la ligne en surbrillance suivante : `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Cette ligne représente le nom de l’utilisateur, qui a été désérialisé à partir de la réponse JSON et est présenté dans la vue.

    Vous pouvez appeler l’API Graph à l’aide de HttpClient et gérer vous-même les données brutes, mais il est plus facile d’utiliser la [bibliothèque cliente Graph qui est disponible via NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). La bibliothèque cliente gère les requêtes HTTP brutes et la transformation des données renvoyées pour vous, et facilite grandement l’utilisation de l’API Graph dans un environnement .NET. Consultez les exemples de code API Graph connexes sur [github.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Déploiement de l'application vers Azure

Les étapes suivantes vous montrent comment déployer l’application sur Azure. Dans les étapes précédentes, vous avez connecté votre nouveau projet à une application Web sur Azure. il est donc prêt à être publié en seulement quelques étapes.

1. Dans Visual Studio, cliquez avec le bouton droit sur le projet et sélectionnez **publier**. La boîte de dialogue **publier le site Web** s’affiche avec chaque paramètre déjà configuré. Cliquez sur le bouton **suivant** pour accéder à la page **paramètres** . Vous pouvez être invité à vous authentifier. Veillez à vous authentifier à l’aide de votre compte d’abonnement Azure (généralement un compte Microsoft) et non du compte professionnel que vous avez créé précédemment.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Cochez l’option **activer l’authentification organisationnelle** . Dans le champ **domaine** , entrez le domaine de votre répertoire. Dans la liste déroulante **niveau d’accès** , sélectionnez **authentification unique, lire les données d’annuaire**. Vous remarquerez que la base de données précédente que vous avez utilisée est déjà remplie dans la section **bases de données** . Cliquez sur **Publier**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio va commencer le déploiement de votre site Web, puis une nouvelle fenêtre de navigateur s’affiche. Vous serez peut-être invité à vous authentifier à nouveau dans votre annuaire. Une fois que vous vous êtes authentifié, vous êtes redirigé vers votre nouveau site Web publié sur Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Débogage de l’application

Si vous recevez l’erreur suivante : la valeur ne peut pas être null ou vide. Nom du paramètre : linkText

![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)

Remplacez le code du fichier *Views\Shared\\_LoginPartial. cshtml* par ce qui suit :

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Après avoir exécuté l’application, si l’utilisateur connecté affiche « utilisateur null », déconnectez-vous, puis reconnectez-vous avec le compte Active Directory que vous avez créé précédemment.

L’un des excellents didacticiels à suivre est l’étude approfondie de Rick Rainey [: sites Web Azure et authentification organisationnelle à l’aide de Azure ad](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Plus d'informations

- [Approfondissement : sites Web Azure et authentification organisationnelle à l’aide de Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Présentation de la API Graph Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scénarios d’authentification dans Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Exemples de code Azure AD sur GitHub](https://github.com/AzureADSamples)
