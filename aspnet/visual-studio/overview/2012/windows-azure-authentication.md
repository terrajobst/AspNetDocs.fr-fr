---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Authentification Windows Azure | Microsoft Docs
author: Rick-Anderson
description: Microsoft ASP.NET Tools pour Windows Azure Active Directory facilite l’activation de l’authentification pour les applications Web hébergées sur des sites Web Windows Azure...
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: ce98effe18dd739504fb0d5453bae8a46c3ba102
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457477"
---
# <a name="windows-azure-authentication"></a>Authentification Windows Azure

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Microsoft ASP.NET Tools pour Windows Azure Active Directory facilite l’activation de l’authentification pour les applications Web hébergées sur des [sites Web Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Vous pouvez utiliser l’authentification Windows Azure pour authentifier les utilisateurs Office 365 de votre organisation, les comptes d’entreprise synchronisés à partir de votre Active Directory local ou les utilisateurs créés dans votre propre domaine Windows Azure Active Directory personnalisé. L’activation de l’authentification Windows Azure configure votre application pour authentifier les utilisateurs à l’aide d’un seul locataire [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .
>
> L’outil d’authentification Windows Azure ASP.NET n’est pas pris en charge pour les rôles Web dans un service Cloud, mais nous envisageons de le faire dans une version ultérieure. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) est pris en charge dans les rôles Web Windows Azure.
>
> Pour plus d’informations sur la configuration de la synchronisation entre votre Active Directory sur site et votre client Windows Azure Active Directory, consultez la page [utilisation de AD FS 2,0 pour implémenter et gérer l’authentification unique](https://technet.microsoft.com/library/jj205462.aspx).
>
> Windows Azure Active Directory est actuellement disponible en version [préliminaire gratuite](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="requirements"></a>Conditions requises :

- Visual Studio 2012 ou [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- Extensions [Web Tools pour Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) ou [Extensions web tools pour Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Outils Microsoft ASP.net pour windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) ou [Microsoft ASP.net tools pour windows Azure Active Directory – Visual Studio Express 2012 pour le Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Créer une application Web ASP.NET avec Visual Studio 2012

Vous pouvez créer n’importe quelle application Web avec Visual Studio 2012, ce didacticiel utilise le modèle intranet MVC ASP.NET.

1. Créez une nouvelle application intranet ASP.NET MVC 4 et acceptez toutes les valeurs par défaut. (Il doit s’agir d’une opération in **tra** net et non du projet **RET** net).
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Activer l’authentification Windows Azure (lorsque vous êtes un administrateur général du principe général)

Si vous n’avez pas de locataire Windows Azure Active Directory existant (par exemple, via un compte Office 365 existant), vous pouvez créer un locataire en vous inscrivant pour obtenir un [nouveau compte Windows Azure Active Directory](https://g.microsoftonline.com/0AX00en/5).

1. Dans le menu projet, sélectionnez **activer l’authentification Windows Azure**:

   ![](windows-azure-authentication/_static/image2.png)

2. Entrez le domaine de votre locataire Windows Azure Active Directory (par exemple, contoso.onmicrosoft.com), puis cliquez sur **activer**:

![](windows-azure-authentication/_static/image3.png)

3. Dans la boîte de dialogue authentification Web, connectez-vous en tant qu’administrateur de votre locataire Windows Azure Active Directory :

   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Activer Windows Azure par un non-administrateur du principe

Si vous ne disposez pas de privilèges d’administrateur général pour votre locataire Windows Azure Active Directory, vous pouvez désactiver la case à cocher pour approvisionner l’application.

![](windows-azure-authentication/_static/image6.png)

La boîte de dialogue affiche le **domaine**, l' **ID du principal** de l’application et l' **URL de réponse** , qui sont nécessaires à l’approvisionnement de l’application avec un Azure Active Directory principe. Vous devez fournir ces informations à une personne disposant des privilèges suffisants pour approvisionner l’application. Pour plus d’informations sur l’utilisation de l’applet de commande pour créer manuellement le principal du service[, consultez Comment implémenter l’authentification unique avec Windows Azure Active Directory-Application ASP.net](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) .
Une fois l’application configurée, vous pouvez cliquer sur **continuer pour mettre à jour le fichier Web. config avec les paramètres sélectionnés**. Si vous souhaitez poursuivre le développement de l’application en attendant que l’approvisionnement se produise, vous pouvez cliquer sur **Fermer pour vous souvenir des paramètres du fichier projet**. La prochaine fois que vous appelez activer l’authentification Windows Azure et décochez la case de configuration, vous verrez les mêmes paramètres. vous pouvez cliquer sur **Continuer**, puis sur **appliquer ces paramètres dans le fichier Web. config**.

1. Attendez que votre application soit configurée pour l’authentification Windows Azure et approvisionnée avec Windows Azure Active Directory.
2. Une fois l’authentification Windows Azure activée pour votre application, cliquez sur **Fermer :**

    ![](windows-azure-authentication/_static/image7.png)
3. Appuyez sur F5 pour exécuter votre application. Vous devez automatiquement être redirigé vers la page de connexion. Utilisez les informations d’identification de l’utilisateur Directory-principes pour vous connecter à l’application.

    ![](windows-azure-authentication/_static/image1.jpg)
4. Étant donné que votre application utilise actuellement un certificat de test auto-signé, vous recevrez un avertissement du navigateur indiquant que le certificat n’a pas été émis par une autorité de certification approuvée.

    Cet avertissement peut être ignoré en toute sécurité pendant le développement local en cliquant sur **Continuer sur ce site Web :**

    ![](windows-azure-authentication/_static/image8.png)
5. Vous avez maintenant réussi à vous connecter à votre application à l’aide de l’authentification Windows Azure !

    ![](windows-azure-authentication/_static/image2.jpg)

L’activation de l’authentification Windows Azure apporte les modifications suivantes à votre application :

- Une classe de falsification de requête intersite ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) ( *application\_Start\AntiXsrfConfig.cs* ) est ajoutée à votre projet.
- Les packages NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` sont ajoutés à votre projet.
- Les paramètres de Windows Identity Foundation dans votre application seront configurés pour accepter les jetons de sécurité de votre locataire Windows Azure Active Directory. Cliquez sur l’image ci-dessous pour afficher une vue développée des modifications apportées au fichier *Web. config* .

     ![](windows-azure-authentication/_static/image9.png)
- Un principal de service pour votre application dans votre locataire Windows Azure Active Directory sera approvisionné.
- HTTPs est activé.

## <a name="deploy-the-application-to-windows-azure"></a>Déployer l’application sur Windows Azure

Pour obtenir des instructions complètes, consultez [déploiement d’une application web ASP.net sur un site Web Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Pour publier une application à l’aide de l’authentification Windows Azure sur un site Web Azure :

1. Cliquez avec le bouton droit sur votre application, puis sélectionnez **publier :**

    ![](windows-azure-authentication/_static/image3.jpg)
2. Dans la boîte de dialogue publier le site Web, téléchargez et importez un profil de publication pour votre site Web Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. L’onglet **connexion** affiche l' **URL de destination** (URL publique pour votre application). Cliquez sur **valider la connexion** pour tester votre connexion :

    ![](windows-azure-authentication/_static/image5.jpg)
4. Si vous avez déjà publié sur ce site Web Azure, vous pouvez vérifier le paramètre **Supprimer les fichiers supplémentaires à la destination** pour vous assurer que votre application est correctement publiée. Notez que la case à cocher **activer l’authentification Windows Azure** est activée.

    ![](windows-azure-authentication/_static/image10.png)
5. Facultatif : dans l’onglet **Aperçu** , cliquez sur **Démarrer l’aperçu** pour afficher les fichiers déployés.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Cliquez sur **publier.**

    Vous serez invité à activer l’authentification Windows Azure pour l’ordinateur hôte cible. Cliquez sur **activer** pour continuer :

    ![](windows-azure-authentication/_static/image11.png)
7. Entrez vos informations d’identification d’administrateur pour votre locataire Windows Azure Active Directory :

    ![](windows-azure-authentication/_static/image7.jpg)
8. Une fois que votre application a été correctement publiée, un navigateur s’ouvre sur le site Web publié.

    > [!NOTE]
    > Cette opération peut prendre jusqu’à cinq minutes (généralement moins) pour que votre application soit entièrement configurée avec Windows Azure Active Directory après l’activation de l’authentification Windows Azure pour l’ordinateur hôte cible. Lorsque vous exécutez votre application pour la première fois, si vous recevez l’erreur ACS50001 : la partie de confiance portant le nom « [Realm] » est introuvable, attendez quelques minutes, puis réessayez d’exécuter l’application.
9. Lorsque vous y êtes invité, connectez-vous en tant qu’utilisateur dans votre répertoire :

    ![](windows-azure-authentication/_static/image8.jpg)
10. Vous avez maintenant réussi à vous connecter à votre application hébergée par Azure à l’aide de l’authentification Windows Azure.

     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Problèmes connus

#### <a name="role-based-authorization-fails-when-using-windows-azure-authentication"></a>L’autorisation basée sur les rôles échoue lors de l’utilisation de l’authentification Windows Azure

L’authentification Windows Azure ne fournit pas actuellement la revendication de rôle nécessaire pour permettre l’exécution de l’autorisation basée sur les rôles. Le rôle de l’utilisateur authentifié doit être récupéré manuellement à partir de Windows Azure Active Directory.

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-sts"></a>L’accès à une application avec l’authentification Windows Azure provoque l’erreur « ACS20016 le domaine de l’utilisateur connecté (live.com) ne correspond à aucun domaine autorisé de ce STS »

Si vous êtes déjà connecté à un compte Microsoft (par exemple, hotmail.com, live.com, outlook.com) et que vous tentez d’accéder à une application qui a activé l’authentification Windows Azure, vous pouvez obtenir une réponse d’erreur 400, car le domaine de votre compte Microsoft n’est pas reconnu par Windows Azure Active Directory. Pour vous connecter à l’application, déconnectez-vous d’abord de votre compte Microsoft.

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificate"></a>La connexion à une application avec l’authentification Windows Azure activée et une X509CertificateValidationMode autre que aucun génère des erreurs de validation de certificat pour le certificat accounts.accesscontrol.windows.net

La validation du certificat n’est pas obligatoire et doit être laissée désactivée. L’empreinte numérique du certificat de l’émetteur est validée par WSFederationAuthenticationModule.

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-sts"></a>Lors d’une tentative d’activation de l’authentification Windows Azure, la boîte de dialogue d’authentification Web affiche l’erreur « ACS20016 : le domaine de l’utilisateur connecté (contoso.onmicrosoft.com) ne correspond à aucun domaine autorisé de ce STS. »

Vous pouvez voir cette erreur si vous vous êtes déjà connecté avec un compte Windows Azure Active Directory différent dans le même processus Visual Studio. Déconnectez-vous du compte spécifié ou redémarrez Visual Studio. Si vous avez déjà ouvert une session et sélectionné l’option « maintenir la connexion », vous devrez peut-être effacer les cookies de votre navigateur.

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message"></a>ACS20012 : la demande n’est pas un message de protocole WS-Federation valide

Cela peut se produire si vous êtes déjà connecté avec un autre ID Microsoft à l’un des services Azure. Utilisez une fenêtre de navigateur privée telle que non privée dans IE ou Incognito dans chrome ou désactivez tous les cookies.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Outils Microsoft ASP.net pour Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Fonctionnalités Windows Azure : identité](https://docs.microsoft.com/azure/active-directory/)
- [TechNet : Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory : développer des applications pour votre organisation](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory : développer des applications pour plusieurs organisations](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Comment implémenter l’authentification unique avec Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Authentification unique avec Windows Azure Active Directory : présentation approfondie](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Utiliser AD FS 2,0 pour implémenter et gérer l’authentification unique](https://technet.microsoft.com/library/jj205462.aspx)
