---
uid: web-api/overview/security/external-authentication-services
title: Services d’authentification externes avec API Web ASP.NETC#() | Microsoft Docs
author: rmcmurray
description: Décrit l’utilisation des services d’authentification externes dans API Web ASP.NET.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555474"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a>Services d’authentification externes avec API Web ASP.NETC#()

Visual Studio 2017 et ASP.NET 4.7.2 développent les options de sécurité pour les [applications à page unique](../../../single-page-application/index.md) (Spa) et les services d' [API Web](../../index.md) pour s’intégrer aux services d’authentification externes, qui incluent plusieurs services d’authentification de réseaux sociaux et OAuth/OpenID : comptes Microsoft, Twitter, Facebook et Google.  

### <a name="in-this-walkthrough"></a>Dans cette procédure pas à pas

- [Utilisation des services d’authentification externes](#USING)
- [Création de l’exemple d’application Web](#SAMPLE)
- [Activation de l’authentification Facebook](#FACEBOOK)
- [Activation de l’authentification Google](#GOOGLE)
- [Activation de l’authentification Microsoft](#MICROSOFT)
- [Activation de l’authentification Twitter](#TWITTER)
- [Informations supplémentaires](#MOREINFO)

    - [Combinaison de services d’authentification externes](#COMBINE)
    - [Configuration de IIS Express pour utiliser un nom de domaine complet](#FQDN)
    - [Comment obtenir les paramètres de votre application pour l’authentification Microsoft](#OBTAIN)
    - [Facultatif : désactiver l’inscription locale](#DISABLE)

### <a name="prerequisites"></a>Conditions préalables requises

Pour suivre les exemples de cette procédure pas à pas, vous devez disposer des éléments suivants :

- Visual Studio 2017
- Un compte de développeur avec l’identificateur d’application et la clé secrète pour l’un des services d’authentification de réseaux sociaux suivants :

  - Comptes Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
  - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
  - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))
  - Google ([https://developers.google.com/](https://developers.google.com))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Utilisation des services d’authentification externes

L’abondance de services d’authentification externes actuellement disponibles pour les développeurs Web permet de réduire le temps de développement lors de la création de nouvelles applications Web. Les utilisateurs Web ont généralement plusieurs comptes existants pour les services Web populaires et les sites Web des réseaux sociaux. par conséquent, lorsqu’une application Web implémente les services d’authentification à partir d’un service Web externe ou d’un site Web de réseaux sociaux, elle enregistre le temps de développement a été consacré à la création d’une implémentation d’authentification. L’utilisation d’un service d’authentification externe évite aux utilisateurs finaux d’avoir à créer un autre compte pour votre application Web et à se souvenir également d’un autre nom d’utilisateur et d’un mot de passe.

Par le passé, les développeurs avaient deux possibilités : créer leur propre implémentation d’authentification, ou apprendre à intégrer un service d’authentification externe dans leurs applications. Au niveau le plus simple, le diagramme suivant illustre un simple workflow de requête pour un agent utilisateur (navigateur Web) qui demande des informations à partir d’une application Web configurée pour utiliser un service d’authentification externe :

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

Dans le schéma précédent, l’agent utilisateur (ou le navigateur Web dans cet exemple) effectue une demande à une application Web, qui redirige le navigateur Web vers un service d’authentification externe. L’agent utilisateur envoie ses informations d’identification au service d’authentification externe, et si l’agent utilisateur s’est correctement authentifié, le service d’authentification externe redirige l’agent utilisateur vers l’application Web d’origine avec une forme de jeton que le l’agent utilisateur sera envoyé à l’application Web. L’application Web utilise le jeton pour vérifier que l’agent utilisateur a été correctement authentifié par le service d’authentification externe, et l’application Web peut utiliser le jeton pour collecter plus d’informations sur l’agent utilisateur. Une fois que l’application a terminé le traitement des informations de l’agent utilisateur, l’application Web renverra la réponse appropriée à l’agent utilisateur en fonction de ses paramètres d’autorisation.

Dans ce deuxième exemple, l’agent utilisateur négocie avec l’application Web et le serveur d’autorisation externe, et l’application Web effectue une communication supplémentaire avec le serveur d’autorisation externe pour récupérer des informations supplémentaires sur l’utilisateur. agent

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

Visual Studio 2017 et ASP.NET 4.7.2 facilitent l’intégration des services d’authentification externes pour les développeurs en fournissant une intégration intégrée pour les services d’authentification suivants :

- Facebook
- Google
- Comptes Microsoft (comptes Windows Live ID)
- Twitter

Les exemples de cette procédure pas à pas montrent comment configurer chacun des services d’authentification externes pris en charge à l’aide du nouveau modèle d’application Web ASP.NET fourni avec Visual Studio 2017.

> [!NOTE]
> Si nécessaire, vous devrez peut-être ajouter votre nom de domaine complet aux paramètres de votre service d’authentification externe. Cette exigence est basée sur des contraintes de sécurité pour certains services d’authentification externes qui nécessitent que le nom de domaine complet dans les paramètres de votre application corresponde au nom de domaine complet utilisé par vos clients. (Les étapes de cette opération varient considérablement pour chaque service d’authentification externe ; vous devrez consulter la documentation de chaque service d’authentification externe pour voir si cela est nécessaire et comment configurer ces paramètres.) Si vous devez configurer IIS Express pour utiliser un nom de domaine complet pour tester cet environnement, consultez la section [configuration de IIS Express pour utiliser un nom de domaine complet](#FQDN) plus loin dans cette procédure pas à pas.

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a>Créer un exemple d’application Web

Les étapes suivantes vous guideront dans la création d’un exemple d’application à l’aide du modèle d’application Web ASP.NET, et vous utiliserez cet exemple d’application pour chacun des services d’authentification externes plus loin dans cette procédure pas à pas.

Démarrez Visual Studio 2017, puis sélectionnez **nouveau projet** dans la page de démarrage. Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

Quand la boîte **de dialogue Nouveau projet** s’affiche, sélectionnez **installé** et développez **visuel C#** . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.net (.NET Framework)** . Entrez un nom pour votre projet et cliquez sur **OK**.

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

Lorsque le **nouveau projet ASP.net** s’affiche, sélectionnez le modèle **application à page unique** , puis cliquez sur **créer un projet**.

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

Attendez que Visual Studio 2017 crée votre projet.

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

Lorsque Visual Studio 2017 a terminé la création de votre projet, ouvrez le fichier *Startup.auth.cs* situé dans le dossier de démarrage de l' **application\_** .

Lorsque vous créez le projet pour la première fois, aucun des services d’authentification externes n’est activé dans le fichier *Startup.auth.cs* ; l’exemple suivant illustre ce que votre code peut ressembler, avec les sections mises en surbrillance pour l’emplacement où vous activez un service d’authentification externe et tous les paramètres appropriés afin d’utiliser des comptes Microsoft, Twitter, Facebook ou l’authentification Google avec votre application ASP.NET :

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Quand vous appuyez sur F5 pour générer et déboguer votre application Web, un écran de connexion s’affiche, dans lequel vous verrez qu’aucun service d’authentification externe n’a été défini.

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

Dans les sections suivantes, vous allez apprendre à activer chacun des services d’authentification externes fournis avec ASP.NET dans Visual Studio 2017.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Activation de l’authentification Facebook

L’utilisation de l’authentification Facebook vous oblige à créer un compte de développeur Facebook, et votre projet aura besoin d’un ID d’application et d’une clé secrète de Facebook pour fonctionner. Pour plus d’informations sur la création d’un compte de développeur Facebook et l’obtention de votre ID d’application et de votre clé secrète, consultez [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Une fois que vous avez obtenu votre ID d’application et votre clé secrète, procédez comme suit pour activer l’authentification Facebook pour votre application Web :

1. Lorsque votre projet est ouvert dans Visual Studio 2017, ouvrez le fichier *Startup.auth.cs* .

2. Recherchez la section de code d’authentification Facebook :

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Supprimez le &quot;//&quot; caractères pour supprimer les marques de commentaire des lignes de code en surbrillance, puis ajoutez votre ID d’application et votre clé secrète. Une fois que vous avez ajouté ces paramètres, vous pouvez recompiler votre projet :

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Quand vous appuyez sur F5 pour ouvrir votre application Web dans votre navigateur Web, vous verrez que Facebook a été défini en tant que service d’authentification externe :

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. Lorsque vous cliquez sur le bouton **Facebook** , votre navigateur est redirigé vers la page de connexion à Facebook :

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. Une fois que vous avez entré vos informations d’identification Facebook et cliqué sur **se connecter**, votre navigateur Web est redirigé vers votre application Web, ce qui vous invite à entrer le **nom d’utilisateur** que vous souhaitez associer à votre compte Facebook :

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. Une fois que vous avez entré votre nom d’utilisateur et cliqué sur le bouton s’inscrire, votre application Web affiche la **page** d' **installation** par défaut de votre compte Facebook :

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Activation de l’authentification Google

L’utilisation de l’authentification Google vous oblige à créer un compte de développeur Google et votre projet nécessitera un ID d’application et une clé secrète de Google pour fonctionner. Pour plus d’informations sur la création d’un compte de développeur Google et l’obtention de votre ID d’application et de votre clé secrète, consultez [https://developers.google.com](https://developers.google.com).

Pour activer l’authentification Google pour votre application Web, procédez comme suit :

1. Lorsque votre projet est ouvert dans Visual Studio 2017, ouvrez le fichier *Startup.auth.cs* .

2. Recherchez la section de code d’authentification Google :

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Supprimez le &quot;//&quot; caractères pour supprimer les marques de commentaire des lignes de code en surbrillance, puis ajoutez votre ID d’application et votre clé secrète. Une fois que vous avez ajouté ces paramètres, vous pouvez recompiler votre projet :

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Lorsque vous appuyez sur F5 pour ouvrir votre application Web dans votre navigateur Web, vous verrez que Google a été défini en tant que service d’authentification externe :

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. Lorsque vous cliquez sur le bouton **Google** , votre navigateur est redirigé vers la page de connexion Google :

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. Après avoir entré vos informations d’identification Google et cliqué sur **se connecter**, Google vous invite à vérifier que votre application Web dispose des autorisations d’accès à votre compte Google :

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. Lorsque vous cliquez sur **accepter**, votre navigateur Web est redirigé vers votre application Web, ce qui vous invite à entrer le **nom d’utilisateur** que vous souhaitez associer à votre compte Google :

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. Une fois que vous avez entré votre nom d’utilisateur et cliqué sur le bouton s’inscrire, votre application Web affiche la **page** d' **installation** par défaut de votre compte Google :

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Activation de l’authentification Microsoft

L’authentification Microsoft vous oblige à créer un compte de développeur et nécessite un ID client et une clé secrète client pour fonctionner. Pour plus d’informations sur la création d’un compte de développeur Microsoft et sur l’obtention de votre ID client et la clé secrète client, consultez [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).

Une fois que vous avez obtenu votre clé de consommateur et le secret de consommateur, procédez comme suit pour activer l’authentification Microsoft pour votre application Web :

1. Lorsque votre projet est ouvert dans Visual Studio 2017, ouvrez le fichier *Startup.auth.cs* .

2. Recherchez la section d’authentification Microsoft de code :

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Supprimez le &quot;//&quot; caractères pour supprimer les marques de commentaire des lignes de code en surbrillance, puis ajoutez votre ID client et votre clé secrète client. Une fois que vous avez ajouté ces paramètres, vous pouvez recompiler votre projet :

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Lorsque vous appuyez sur F5 pour ouvrir votre application Web dans votre navigateur Web, vous verrez que Microsoft a été défini comme un service d’authentification externe :

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. Lorsque vous cliquez sur le bouton **Microsoft** , votre navigateur est redirigé vers la page de connexion Microsoft :

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. Après avoir entré vos informations d’identification Microsoft et cliqué sur **se connecter**, vous êtes invité à vérifier que votre application Web dispose des autorisations nécessaires pour accéder à votre compte Microsoft :

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. Lorsque vous cliquez sur **Oui**, votre navigateur Web est redirigé vers votre application Web, ce qui vous invite à entrer le **nom d’utilisateur** que vous souhaitez associer à votre compte Microsoft :

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. Une fois que vous avez entré votre nom d’utilisateur et cliqué sur le bouton s’inscrire, votre application Web affiche la **page** d' **installation** par défaut de votre compte Microsoft :

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Activation de l’authentification Twitter

L’authentification Twitter vous oblige à créer un compte de développeur et nécessite une clé de consommateur et un secret de consommateur pour fonctionner. Pour plus d’informations sur la création d’un compte de développeur Twitter et sur l’obtention de votre clé de consommateur et le secret de consommateur, consultez [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Une fois que vous avez obtenu votre clé de consommateur et le secret de consommateur, procédez comme suit pour activer l’authentification Twitter pour votre application Web :

1. Lorsque votre projet est ouvert dans Visual Studio 2017, ouvrez le fichier *Startup.auth.cs* .

2. Recherchez la section de code d’authentification Twitter :

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Supprimez le &quot;//&quot; caractères pour supprimer les marques de commentaire des lignes de code en surbrillance, puis ajoutez votre clé de consommateur et le secret de consommateur. Une fois que vous avez ajouté ces paramètres, vous pouvez recompiler votre projet :

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Quand vous appuyez sur F5 pour ouvrir votre application Web dans votre navigateur Web, vous verrez que Twitter a été défini en tant que service d’authentification externe :

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. Lorsque vous cliquez sur le bouton **Twitter** , votre navigateur est redirigé vers la page de connexion à Twitter :

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. Une fois que vous avez entré vos informations d’identification Twitter et cliqué sur **autoriser l’application**, votre navigateur Web est redirigé vers votre application Web, ce qui vous invite à entrer le **nom d’utilisateur** que vous souhaitez associer à votre compte Twitter :

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. Une fois que vous avez entré votre nom d’utilisateur et cliqué sur le bouton s’inscrire, votre application Web affiche la **page** d' **installation** par défaut de votre compte Twitter :

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Informations supplémentaires

Pour plus d’informations sur la création d’applications qui utilisent OAuth et OpenID, consultez les URL suivantes :

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Combinaison de services d’authentification externes

Pour une plus grande flexibilité, vous pouvez définir simultanément plusieurs services d’authentification externes. cela permet aux utilisateurs de votre application Web d’utiliser un compte à partir de n’importe quel service d’authentification externe activé :

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a>Configurer IIS Express pour utiliser un nom de domaine complet

Certains fournisseurs d’authentification externes ne prennent pas en charge le test de votre application à l’aide d’une adresse HTTP comme `http://localhost:port/`. Pour contourner ce problème, vous pouvez ajouter un mappage de nom de domaine complet (FQDN) statique à votre fichier HOSTs et configurer vos options de projet dans Visual Studio 2017 afin d’utiliser le nom de domaine complet (FQDN) à des fins de test ou de débogage. Pour ce faire, procédez comme suit :

- Ajoutez un nom de domaine complet statique qui mappe votre fichier HOSTs :

  1. Ouvrez une invite de commandes avec élévation de privilèges dans Windows.
  2. Tapez la commande suivante :

      <kbd>bloc-notes%WinDir%\system32\drivers\etc\hosts</kbd>
  3. Ajoutez une entrée semblable à la suivante au fichier HOSTs :

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Enregistrez et fermez votre fichier HOSTs.

- Configurez votre projet Visual Studio pour utiliser le nom de domaine complet :

  1. Lorsque votre projet est ouvert dans Visual Studio 2017, cliquez sur le menu **projet** , puis sélectionnez les propriétés de votre projet. Par exemple, vous pouvez sélectionner **Propriétés de WebApplication1**.
  2. Sélectionnez l’onglet **Web** .
  3. Entrez votre nom de domaine complet pour l' <strong>URL du projet</strong>. Par exemple, vous entrez <kbd><http://www.wingtiptoys.com></kbd> si c’était le mappage de nom de domaine complet que vous avez ajouté à votre fichier hosts.

- Configurez IIS Express pour utiliser le nom de domaine complet de votre application :

    1. Ouvrez une invite de commandes avec élévation de privilèges dans Windows.
    2. Tapez la commande suivante pour passer au dossier IIS Express :

        <kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Tapez la commande suivante pour ajouter le nom de domaine complet à votre application :

        <kbd>appcmd. exe set config-section : System. applicationHost/sites/+&quot;[name = 'Webapplication1 ']. Bindings. [Protocol = 'http', bindingInformation = ' * : 80 : www. wingtiptoys. com']&quot;/commit : apphost</kbd>

  Où **WebApplication1** est le nom de votre projet et **bindingInformation** contient le numéro de port et le nom de domaine complet que vous souhaitez utiliser pour vos tests.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Comment obtenir les paramètres de votre application pour l’authentification Microsoft

La liaison d’une application à Windows Live pour l’authentification Microsoft est un processus simple. Si vous n’avez pas encore lié une application à Windows Live, vous pouvez utiliser les étapes suivantes :

1. Accédez à [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) et entrez le nom et le mot de passe de votre compte Microsoft lorsque vous y êtes invité, puis cliquez sur **se connecter**:

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. Sélectionnez **Ajouter une application** et entrez le nom de votre application lorsque vous y êtes invité, puis cliquez sur **créer**:

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. Sélectionnez votre application sous **nom** et sa page Propriétés de l’application s’affiche.

4. Entrez le domaine de redirection pour votre application. Copiez l’ID de l' **application** et, sous secrets de l' **application**, sélectionnez **générer le mot de passe**. Copiez le mot de passe qui s’affiche. L’ID de l’application et le mot de passe sont votre ID client et votre clé secrète client. Sélectionnez **OK** , puis **Enregistrer**.

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Facultatif : désactiver l’inscription locale

La fonctionnalité d’inscription locale ASP.NET actuelle n’empêche pas les programmes automatisés (robots) de créer des comptes membres. par exemple, en utilisant une technologie de validation et de prévention des robots comme [captcha](../../../web-pages/overview/security/16-adding-security-and-membership.md). Pour cette raison, vous devez supprimer le formulaire de connexion locale et le lien d’inscription sur la page de connexion. Pour ce faire, ouvrez la page *\_login. cshtml* dans votre projet, puis commentez les lignes pour le panneau de connexion local et le lien d’inscription. La page résultante doit ressembler à l’exemple de code suivant :

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Une fois que le panneau de connexion local et le lien d’inscription ont été désactivés, votre page de connexion affiche uniquement les fournisseurs d’authentification externes que vous avez activés :

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
