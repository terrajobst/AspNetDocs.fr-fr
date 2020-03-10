---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Ajout de la sécurité et de l’appartenance à un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre vous montre comment sécuriser votre site Web afin que certaines pages ne soient disponibles que pour les personnes qui se connectent. (Vous verrez également comment créer des pages...
ms.author: riande
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 0be3e767a42939a3c343f6d4a730eb1d9a6b367c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628386"
---
# <a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Ajout de la sécurité et de l’appartenance à un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment sécuriser un site Web pages Web ASP.NET (Razor) de sorte que certaines pages ne soient disponibles que pour les personnes qui se connectent. (Vous verrez également comment créer des pages auxquelles tout le monde peut accéder.)
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer un site Web avec une page d’inscription et une page de connexion afin que, pour certaines pages, vous puissiez limiter l’accès aux seuls membres.
> - Comment créer des pages publiques et membres uniquement.
> - Comment définir des rôles, qui sont des groupes qui ont des autorisations de sécurité différentes sur votre site et comment affecter des utilisateurs à un rôle.
> - Comment utiliser CAPTCHA pour empêcher les programmes automatisés (robots) de créer des comptes de membres.
>   
> 
> Voici les fonctionnalités ASP.NET présentées dans l’article :
> 
> - Modèle de **Starter Site** WebMatrix.
> - `WebSecurity` Helper et `Roles` classe.
> - Le programme d’assistance `ReCaptcha`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - WebMatrix 3
> - Bibliothèque d’applications auxiliaires Web ASP.NET

Vous pouvez configurer votre site Web pour que les utilisateurs puissent se connecter &#8212; à celui-ci, afin que le site prenne en charge l' *appartenance*. Cela peut être utile pour de nombreuses raisons. Par exemple, votre site peut avoir des pages qui doivent être disponibles uniquement pour les membres. Dans certains cas, vous pouvez avoir besoin que les utilisateurs se connectent pour vous envoyer des commentaires ou conserver un commentaire.

Même si votre site Web prend en charge l’appartenance, les utilisateurs ne sont pas nécessairement obligés de se connecter avant de pouvoir utiliser certaines des pages du site. Les utilisateurs qui ne sont pas connectés sont appelés *utilisateurs anonymes*.

Un utilisateur peut s’inscrire sur votre site Web et peut alors se connecter au site. Le site Web requiert un nom d’utilisateur (une adresse de messagerie) et un mot de passe pour confirmer que les utilisateurs sont bien ceux qu’ils revendiquent. Ce processus de connexion et de confirmation de l’identité d’un utilisateur est appelé *authentification*.

Vous pouvez configurer la sécurité et l’appartenance de différentes manières :

- Si vous utilisez WebMatrix, un moyen simple consiste à créer un nouveau site basé sur le modèle **Starter Site** . Ce modèle est déjà configuré pour la sécurité et l’appartenance, et contient déjà une page d’inscription, une page de connexion, etc.

    Le site créé par le modèle a également la possibilité de permettre aux utilisateurs de se connecter à l’aide d’un site externe comme Facebook, Google ou Twitter.
- Si vous souhaitez ajouter la sécurité à un site existant, ou si vous ne souhaitez pas utiliser le modèle **Starter Site** , vous pouvez créer votre propre page d’inscription, votre page de connexion, etc.

Cet article se concentre sur la première option &mdash; comment ajouter la sécurité à l’aide du modèle **Starter Site** . Il fournit également des informations de base sur la façon d’implémenter votre propre sécurité, puis fournit des liens vers des informations supplémentaires sur la procédure à suivre. Vous y trouverez également des informations sur l’activation des connexions externes, qui sont décrites plus en détail dans un article distinct.

## <a name="creating-website-security-using-the-starter-site-template"></a>Création de la sécurité de site Web à l’aide du modèle Starter Site

Dans WebMatrix, vous pouvez utiliser le modèle **Starter Site** pour créer un site Web qui contient les éléments suivants :

- Base de données utilisée pour stocker les noms d’utilisateur et les mots de passe de vos membres.
- Page d’inscription dans laquelle les utilisateurs anonymes (nouveaux) peuvent s’inscrire.
- Page de connexion et de déconnexion.
- Une page de récupération et de réinitialisation du mot de passe.

La procédure suivante décrit comment créer le site et le configurer.

1. Démarrez WebMatrix et, dans la page **démarrage rapide** , sélectionnez **site à partir d’un modèle**.
2. Sélectionnez le modèle **Starter Site** , puis cliquez sur **OK**. WebMatrix crée un nouveau site.
3. Dans le volet gauche, cliquez sur le sélecteur d’espace de travail **fichiers** .
4. Dans le dossier racine de votre site Web, ouvrez le fichier *\_AppStart. cshtml* , qui est un fichier spécial utilisé pour contenir les paramètres globaux. Elle contient des instructions qui sont commentées à l’aide des caractères `//` :

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Ces instructions configurent le programme d’assistance `WebMail`, qui peut être utilisé pour envoyer des messages électroniques. Le système d’appartenance peut utiliser la messagerie électronique pour envoyer des messages de confirmation quand les utilisateurs s’inscrivent ou lorsqu’ils souhaitent modifier leur mot de passe. (Par exemple, une fois que les utilisateurs s’inscrivent, ils reçoivent un message électronique incluant un lien sur lequel ils peuvent cliquer pour terminer le processus d’inscription.)

    L’envoi de courrier électronique requiert l’accès à un serveur SMTP, comme décrit dans la rubrique [Ajout d’un courrier électronique à un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202899). Vous allez stocker les paramètres de messagerie dans ce fichier central *\_AppStart. cshtml* afin de ne pas avoir à les coder à plusieurs reprises dans chaque page qui peut envoyer du courrier électronique. (Vous n’avez pas besoin de configurer les paramètres SMTP pour configurer une base de données d’inscription ; vous n’avez besoin que des paramètres SMTP si vous souhaitez valider les utilisateurs à partir de leur alias de messagerie et permettre aux utilisateurs de réinitialiser un mot de passe oublié.)
5. Supprimez les marques de commentaire des instructions en supprimant des `//` de devant chacune d’elles.

    Si vous ne souhaitez pas configurer de confirmation par courrier électronique, vous pouvez ignorer cette étape et l’étape suivante. Si les valeurs SMTP ne sont pas définies, le nouveau compte est immédiatement disponible sans e-mail de confirmation.
6. Modifiez les paramètres de messagerie suivants dans le code :

   - Définissez `WebMail.SmtpServer` sur le nom du serveur SMTP auquel vous avez accès.
   - Laissez `WebMail.EnableSsl` défini sur `true`. Ce paramètre sécurise les informations d’identification qui sont envoyées au serveur SMTP en les chiffrant.
   - Définissez `WebMail.UserName` sur le nom d’utilisateur de votre compte de serveur SMTP.
   - Définissez `WebMail.Password` sur le mot de passe de votre compte de serveur SMTP.
   - Définissez `WebMail.From` sur votre propre adresse de messagerie. Il s’agit de l’adresse de messagerie à partir de laquelle le message est envoyé.

     > [!NOTE] 
     > 
     > **Conseil** Pour plus d’informations sur les valeurs de ces propriétés, consultez [Configuration des paramètres de messagerie](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) dans personnalisation du comportement à l' [ensemble du site pour pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Enregistrez et fermez *\_AppStart. cshtml*.
8. Exécutez la page *default. cshtml* dans un navigateur.

    ![sécurité-adhésion-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Si vous voyez une erreur qui vous indique qu’une propriété doit être une instance de `ExtendedMembershipProvider`, il est possible que le site ne soit pas configuré pour utiliser le SimpleMembership (pages Web ASP.NET Membership System). Cela peut parfois se produire si le serveur d’un fournisseur d’hébergement est configuré différemment de votre serveur local. Pour résoudre ce problème, ajoutez l’élément suivant au fichier *Web. config* du site :
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Ajoutez cet élément en tant qu’enfant de l’élément `<configuration>` et en tant qu’homologue de l’élément `<system.web>`.
9. Dans le coin supérieur droit de la page, cliquez sur le lien **Register** . La page *Register. cshtml* s’affiche.
10. Entrez un nom d’utilisateur et un mot de passe, puis cliquez sur **inscrire**.

    ![sécurité-adhésion-3](16-adding-security-and-membership/_static/image2.png)

    Lorsque vous avez créé le site Web à partir du modèle **Starter Site** , une base de données nommée *StarterSite. sdf* a été créée dans le dossier de données de l' *application\_* du site. Lors de l’inscription, vos informations utilisateur sont ajoutées à la base de données. Si vous définissez les valeurs SMTP, un message est envoyé à l’adresse de messagerie que vous avez utilisée afin que vous puissiez terminer l’inscription.

    ![sécurité-adhésion-4](16-adding-security-and-membership/_static/image3.png)
11. Accédez à votre programme de messagerie et recherchez le message qui contient votre code de confirmation et un lien hypertexte vers le site.
12. Cliquez sur le lien hypertexte pour activer votre compte. Le lien hypertexte de confirmation ouvre une page de confirmation de l’inscription.

    ![sécurité-adhésion-5](16-adding-security-and-membership/_static/image4.png)
13. Cliquez sur le lien **connexion** , puis connectez-vous à l’aide du compte que vous avez inscrit.

      Une fois que vous êtes connecté, les liens de **connexion** et d' **inscription** sont remplacés par un lien de **déconnexion** . Votre nom de connexion s’affiche sous forme de lien. (Le lien vous permet d’accéder à une page dans laquelle vous pouvez modifier votre mot de passe.)

      ![sécurité-adhésion-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Par défaut, les pages Web ASP.NET envoient des informations d’identification au serveur en texte clair (sous forme de texte explicite). Un site de production doit utiliser le protocole HTTP sécurisé (https://, également connu sous le nom de SSL ( *Secure Sockets Layer* ) pour chiffrer les informations sensibles échangées avec le serveur. Vous pouvez avoir besoin d’envoyer des messages électroniques à l’aide de SSL en définissant `WebMail.EnableSsl=true` comme dans l’exemple précédent. Pour plus d’informations sur SSL, consultez [sécurisation des communications Web : certificats, SSL et https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Fonctionnalités d’appartenance supplémentaires dans le site

Votre site contient d’autres fonctionnalités qui permettent aux utilisateurs de gérer leurs comptes. Les utilisateurs peuvent effectuer les opérations suivantes :

- Modifier leurs mots de passe. Une fois connecté, ils peuvent cliquer sur le nom d’utilisateur (qui est un lien). Cela permet d’accéder à une page où ils peuvent créer un nouveau mot de passe (*compte/ChangePassword. cshtml*).
- Récupérez un mot de passe oublié. La page de connexion contient un lien (**vous avez oublié votre mot de passe ?** ) qui amène les utilisateurs à accéder à une page (*compte/ForgotPassword. cshtml*) où ils peuvent entrer une adresse de messagerie. Le site leur envoie un message électronique contenant un lien sur lequel ils peuvent cliquer afin de définir un nouveau mot de passe (*compte/PasswordReset. cshtml*).

Vous pouvez également permettre aux utilisateurs de se connecter à l’aide d’un site externe, comme expliqué ultérieurement.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Création d’une page réservée aux membres

Pour le moment, tout le monde peut accéder à n’importe quelle page de votre site Web. Toutefois, vous souhaiterez peut-être avoir des pages qui sont disponibles uniquement pour les personnes qui se sont connectées (c’est-à-dire, aux membres). ASP.NET vous permet de créer des pages accessibles uniquement par les membres connectés. En règle générale, si les utilisateurs anonymes essaient d’accéder à une page réservée aux membres, vous les redirigez vers la page de connexion.

Dans cette procédure, vous allez créer un dossier qui contiendra des pages qui sont uniquement disponibles pour les utilisateurs connectés.

1. À la racine du site, créez un nouveau dossier. (Dans le ruban, cliquez sur la flèche sous **nouveau** , puis choisissez **nouveau dossier**.)
2. Nommez les nouveaux *membres*du dossier.
3. Dans le dossier *members* , créez une nouvelle page et nommez-la *MembersInformation. cshtml*.
4. Remplacez le contenu existant par le code et le balisage suivants :

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Ce code teste la propriété `IsAuthenticated` de l’objet `WebSecurity`, qui retourne `true` si l’utilisateur s’est connecté. Si l’utilisateur n’est pas connecté, le code appelle `Response.Redirect` pour envoyer l’utilisateur à la page *login. cshtml* dans le dossier *Account* .

    L’URL de la redirection comprend une `returnUrl` valeur de chaîne de requête qui utilise `Request.Url.LocalPath` pour définir le chemin d’accès de la page actuelle. Si vous définissez la valeur `returnUrl` dans la chaîne de requête comme celle-ci (et si l’URL de renvoi est un chemin d’accès local), la page de connexion renvoie les utilisateurs à cette page après leur connexion.

    Le code définit également *\_page SiteLayout. cshtml* comme sa page de disposition. (Pour plus d’informations sur les pages de disposition, consultez [création d’une disposition cohérente dans les Sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Exécutez le site. Si vous êtes toujours connecté, cliquez sur le bouton **déconnexion** en haut de la page.
6. Dans le navigateur, demandez la page */members/MembersInformation*. Par exemple, l’URL peut se présenter comme suit :

    `http://localhost:38366/Members/MembersInformation`

    (Le numéro de port (38366) sera probablement différent dans votre URL.)

    Vous êtes redirigé vers la page *login. cshtml* , car vous n’êtes pas connecté.
7. Connectez-vous à l’aide du compte que vous avez créé précédemment. Vous êtes redirigé vers la page *MembersInformation* . Étant donné que vous êtes connecté, cette fois, vous voyez le contenu de la page.

Pour sécuriser l’accès à plusieurs pages, procédez comme suit :

- Ajoutez la vérification de sécurité à chaque page.
- Créez une page *\_PageStart. cshtml* dans le dossier dans lequel vous conservez les pages protégées, puis ajoutez la vérification de sécurité à cet emplacement. La page *\_PageStart. cshtml* agit comme une sorte de page globale pour toutes les pages du dossier. Cette technique est expliquée plus en détail dans personnalisation du comportement à l' [ensemble du site pour pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Création de la sécurité pour les groupes d’utilisateurs (rôles)

Si votre site comporte un grand nombre de membres, il n’est pas judicieux de vérifier l’autorisation pour chaque utilisateur individuellement avant de les laisser voir une page. Au lieu de cela, vous pouvez créer des groupes ou des *rôles*auxquels appartiennent des membres individuels. Vous pouvez ensuite vérifier les autorisations en fonction du rôle. Dans cette section, vous allez créer un rôle d'&quot; admin &quot;, puis créer une page accessible aux utilisateurs qui appartiennent à ce rôle.

Le système d’appartenance ASP.NET est configuré pour prendre en charge les rôles. Toutefois, à la différence de l’inscription d’appartenance et de la connexion, le modèle **Starter Site** ne contient pas de pages qui vous aident à gérer les rôles. (La gestion des rôles est une tâche administrative plutôt qu’une tâche utilisateur.) Toutefois, vous pouvez ajouter des groupes directement dans la base de données d’appartenance dans WebMatrix.

1. Dans WebMatrix, cliquez sur le sélecteur d’espace **de travail bases de données** .
2. Dans le volet gauche, ouvrez le nœud *StarterSite. sdf* , ouvrez le nœud **tables** , puis double-cliquez sur la table *webpages\_Roles* .

    ![sécurité-adhésion-7](16-adding-security-and-membership/_static/image6.png)
3. Ajoutez un rôle nommé &quot;&quot;d’administration. Le champ *RoleId* est renseigné automatiquement. (Il s’agit de la clé primaire et a été défini comme un champ d’identification, comme expliqué dans [Introduction à l’utilisation d’une base de données dans des Sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Prenez note de la valeur du champ *RoleId* . (S’il s’agit du premier rôle que vous définissez, il sera 1.)

    ![sécurité-adhésion-8](16-adding-security-and-membership/_static/image7.png)
5. Fermez le tableau *pages web\_des rôles* .
6. Ouvrez la table *UserProfile* .
7. Prenez note de la valeur *userid* d’un ou plusieurs utilisateurs dans la table, puis fermez la table.
8. Ouvrez la table *webpages\_UserInRoles* et entrez un *ID d’utilisateur* et une valeur *RoleID* dans la table. Par exemple, pour placer l’utilisateur 2 dans le rôle &quot;admin&quot;, entrez les valeurs suivantes :

    ![sécurité-adhésion-9](16-adding-security-and-membership/_static/image8.png)
9. Fermez la table *webpages\_UsersInRoles* .

    Maintenant que vous avez défini des rôles, vous pouvez configurer une page accessible aux utilisateurs qui appartiennent à ce rôle.
10. Dans le dossier racine du site Web, créez une nouvelle page nommée *AdminError. cshtml* et remplacez le contenu existant par le code suivant. Il s’agit de la page vers laquelle les utilisateurs sont redirigés s’ils ne sont pas autorisés à accéder à une page.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Dans le dossier racine du site Web, créez une nouvelle page nommée *AdminOnly. cshtml* et remplacez le code existant par le code suivant :

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    La méthode `Roles.IsUserInRole` retourne `true` si l’utilisateur actuel est membre du rôle spécifié (dans ce cas, le rôle « Admin »).
12. Exécutez *default. cshtml* dans un navigateur, mais ne vous connectez pas. (Si vous êtes déjà connecté, déconnectez-vous.)
13. Dans la barre d’adresse du navigateur, ajoutez *AdminOnly* dans l’URL. (En d’autres termes, demandez le fichier *AdminOnly. cshtml* .) Vous êtes redirigé vers la page *AdminError. cshtml* , car vous n’êtes pas actuellement connecté en tant qu’utilisateur dans le rôle&quot; administrateur &quot;.
14. Revenez à *default. cshtml* et connectez-vous en tant que l’utilisateur que vous avez ajouté au rôle&quot; admin &quot;.
15. Accédez à la page *AdminOnly. cshtml* . Cette fois, vous voyez la page.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Empêcher les programmes automatisés de rejoindre votre site Web

La page de connexion n’arrêtera pas l’inscription de programmes automatisés (parfois appelés *robots Web* *ou robots*) auprès de votre site Web. Cette procédure décrit comment activer un test ReCaptcha pour la page d’inscription.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Inscrivez votre site Web à ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Une fois l’inscription terminée, vous obtenez une clé publique et une clé privée.
2. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas déjà fait.
3. Dans le dossier *Account* , ouvrez le fichier nommé *Register. cshtml*.
4. Dans le code en haut de la page, recherchez les lignes suivantes et supprimez les marques de commentaire en supprimant les `//` les caractères de commentaire :

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Remplacez `PRIVATE_KEY` par votre propre clé privée ReCaptcha.
6. Dans le balisage de la page, supprimez le `@*` et `*@` les caractères de commentaire autour des lignes suivantes dans le balisage de la page :

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Remplacez `PUBLIC_KEY` par votre clé.
8. Si vous ne l’avez pas encore supprimée, supprimez l’élément `<div>` qui contient le texte commençant par « pour activer la vérification de CAPTCHA... ». (Supprimez l’intégralité de l’élément `<div>` et son contenu.)

9. Exécutez *default. cshtml* dans un navigateur. Si vous êtes connecté au site, cliquez sur le lien de **déconnexion** .
10. Cliquez sur le lien **Register** et testez l’inscription à l’aide du test CAPTCHA.

     ![sécurité-adhésion-10](16-adding-security-and-membership/_static/image9.png)

Pour plus d’informations sur le `ReCaptcha` Helper, consultez [utilisation d’un CATPCHA pour empêcher les programmes automatisés (robots) d’utiliser votre site Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Permettre aux utilisateurs de se connecter à l’aide d’un site externe

Le modèle **Starter Site** comprend du code et des balises qui permettent aux utilisateurs de se connecter à l’aide de Facebook, Windows Live, Twitter, Google ou Yahoo. Par défaut, cette fonctionnalité n’est pas activée. La procédure générale d’utilisation permettant aux utilisateurs de se connecter à l’aide de ces fournisseurs externes est la suivante :

- Choisissez les sites externes que vous souhaitez prendre en charge.
- Si nécessaire, accédez à ce site et configurez une application de connexion. (Par exemple, vous devez le faire afin d’autoriser les connexions Facebook.)
- Dans votre site, configurez le fournisseur. Dans la plupart des cas, il vous suffit de supprimer les marques de commentaire de code dans le fichier *\_AppStart. cshtml* .
- Ajoutez une balise à la page d’inscription qui permet aux utilisateurs de se connecter au site externe pour se connecter. Vous pouvez généralement copier le balisage dont vous avez besoin et modifier légèrement le texte.

Vous trouverez des instructions pas à pas dans la rubrique [activation de la connexion à partir de sites externes dans un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=251969).

Une fois qu’un utilisateur se connecte à partir d’un autre site, il revient à votre site et *associe* cette connexion à votre site. En effet, cette action crée une entrée d’appartenance dans votre site pour la connexion externe de l’utilisateur. Cela vous permet d’utiliser les fonctions d’appartenance normales (telles que les rôles) avec la connexion externe.

## <a name="adding-security-to-an-existing-website"></a>Ajout de la sécurité à un site Web existant

La procédure décrite plus haut dans cet article repose sur l’utilisation du modèle **Starter Site** comme base pour la sécurité de site Web. S’il n’est pas pratique de démarrer à partir du modèle de **site Starter** ou de copier les pages pertinentes à partir d’un site basé sur ce modèle, vous pouvez implémenter le même type de sécurité dans votre propre site en le codant vous-même. Vous créez les mêmes types de pages (inscription, connexion, etc.), puis vous utilisez des applications et des applications pour configurer l’appartenance.

Le processus de base est décrit dans le billet de blog [la méthode la plus simple pour implémenter la sécurité ASP.net Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). La majeure partie du travail est effectuée à l’aide des méthodes et propriétés suivantes de l’application auxiliaire `WebSecurity` :

- [WebSecurty. userexists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity. CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Ces méthodes vous permettent de déterminer si une personne est déjà inscrite et de les inscrire.
- [WebSecurty. IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Cette propriété vous permet de déterminer si l’utilisateur actuel est connecté. Cela est utile pour rediriger les utilisateurs vers une page de connexion s’ils ne se sont pas déjà connectés.
- [WebSecurity. Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity. logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Ces méthodes déconnectent un utilisateur.
- [WebSecurity. CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Cette propriété est utile pour afficher le nom de connexion de l’utilisateur actuel (si l’utilisateur est connecté).
- [WebSecurity. ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Cette méthode est utile si vous configurez la confirmation de l’inscription par courrier électronique. (Les détails sont décrits dans le billet de blog [à l’aide de la fonctionnalité de confirmation pour pages Web ASP.net sécurité](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Pour gérer les rôles, vous pouvez utiliser les [rôles](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) et les classes d' [appartenance](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) , comme décrit dans le billet de blog.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Personnalisation du comportement à l’échelle du site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sécurisation des communications Web : certificats, SSL et https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [La méthode la plus simple pour implémenter la sécurité Razor ASP.net](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) et l' [utilisation de la fonctionnalité de confirmation pour pages Web ASP.net la sécurité](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Il s’agit de billets de blog qui décrivent comment implémenter des fonctionnalités d’appartenance ASP.NET sans utiliser le modèle **Starter Site** .
- [Activation de la connexion à partir de sites externes dans un site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969)
- Informations de référence sur l' [API de classe WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- Référence de l’API de la [classe SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- Référence de l’API de la [classe SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
