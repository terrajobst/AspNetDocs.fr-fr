---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Envoi de courrier électronique à partir d’un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre explique comment envoyer un message électronique automatisé à partir d’un site Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 23e9717329525fb5a0ed505c9dc94505d4f9dbbe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634714"
---
# <a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Envoi de courrier électronique à partir d’un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment envoyer un message électronique à partir d’un site Web lorsque vous utilisez pages Web ASP.NET (Razor).
> 
> Ce que vous allez apprendre :
> 
> - Comment envoyer un message électronique à partir de votre site Web.
> - Comment joindre un fichier à un message électronique.
> 
> Il s’agit de la fonctionnalité ASP.NET introduite dans l’article :
> 
> - Le programme d’assistance `WebMail`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Envoi de messages électroniques à partir de votre site Web

Il existe toutes sortes de raisons pour lesquelles vous devrez peut-être envoyer des courriers électroniques à partir de votre site Web. Vous pouvez envoyer des messages de confirmation aux utilisateurs, ou vous pouvez envoyer des notifications à vous-même (par exemple, qu’un nouvel utilisateur a inscrit). Le programme d’assistance `WebMail` vous permet d’envoyer facilement des messages électroniques.

Pour utiliser le programme d’assistance `WebMail`, vous devez avoir accès à un serveur SMTP. (SMTP signifie SMTP ( *Simple Mail Transfer Protocol*).) Un serveur SMTP est un serveur de messagerie qui transmet uniquement les messages au serveur &#8212; du destinataire. il s’agit du côté sortant de l’e-mail. Si vous utilisez un fournisseur d’hébergement pour votre site Web, il est probable qu’il vous configure la messagerie électronique et qu’il puisse vous indiquer le nom de votre serveur SMTP. Si vous travaillez à l’intérieur d’un réseau d’entreprise, un administrateur ou votre service informatique peut généralement vous fournir les informations relatives à un serveur SMTP que vous pouvez utiliser. Si vous travaillez chez vous, vous pouvez même être en mesure de tester à l’aide de votre fournisseur de messagerie ordinaire, qui peut vous indiquer le nom de son serveur SMTP. En général, vous avez besoin des éléments suivants :

- Nom du serveur SMTP.
- Numéro de port. Il s’agit presque toujours de 25. Toutefois, votre fournisseur de services Internet peut vous obliger à utiliser le port 587. Si vous utilisez le protocole SSL (Secure Sockets Layer) pour la messagerie électronique, vous aurez peut-être besoin d’un autre port. Contactez votre fournisseur de messagerie.
- Informations d’identification (nom d’utilisateur, mot de passe).

Dans cette procédure, vous créez deux pages. La première page est un formulaire qui permet aux utilisateurs d’entrer une description, comme s’ils remplissaient un formulaire de support technique. La première page envoie ses informations à une deuxième page. Dans la deuxième page, le code extrait les informations de l’utilisateur et envoie un message électronique. Il affiche également un message confirmant que le rapport de problème a été reçu.

![[image]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Pour simplifier cet exemple, le code initialise le `WebMail` application d’assistance dans la page où vous l’utilisez. Toutefois, pour les sites Web réels, il est préférable de placer le code d’initialisation comme celui-ci dans un fichier global, afin d’initialiser le `WebMail` Helper pour tous les fichiers de votre site Web. Pour plus d’informations, consultez [Personnalisation du comportement à l’ensemble du site pour pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).

1. Créez un site Web.
2. Ajoutez une nouvelle page nommée *EmailRequest. cshtml* et ajoutez le balisage suivant : 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Notez que l’attribut `action` de l’élément Form a été défini sur *ProcessRequest. cshtml*. Cela signifie que le formulaire sera soumis à cette page au lieu de revenir à la page actuelle.
3. Ajoutez une nouvelle page nommée *ProcessRequest. cshtml* au site Web et ajoutez le code et le balisage suivants :   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Dans le code, vous pouvez récupérer les valeurs des champs de formulaire qui ont été envoyés à la page. Vous appelez ensuite la méthode de `Send` du programme d’assistance `WebMail` pour créer et envoyer le message électronique. Dans ce cas, les valeurs à utiliser sont constituées de texte que vous concaténez avec les valeurs qui ont été envoyées à partir du formulaire.

    Le code de cette page se trouve à l’intérieur d’un bloc de `try/catch`. Si, pour une raison quelconque, la tentative d’envoi d’un e-mail ne fonctionne pas (par exemple, les paramètres ne sont pas corrects), le code du bloc `catch` s’exécute et définit la variable `errorMessage` sur l’erreur qui s’est produite. (Pour plus d’informations sur les blocs de `try/catch` ou la balise `<text>`, consultez [Introduction à la programmation de pages Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Dans le corps de la page, si la variable `errorMessage` est vide (valeur par défaut), l’utilisateur voit un message indiquant que le message électronique a été envoyé. Si la variable `errorMessage` est définie sur true, l’utilisateur voit un message indiquant qu’un problème est survenu lors de l’envoi du message.

    Notez que dans la partie de la page qui affiche un message d’erreur, il y a un test supplémentaire : `if(debuggingFlag)`. Il s’agit d’une variable que vous pouvez définir sur true si vous rencontrez des problèmes pour envoyer des messages électroniques. Lorsque `debuggingFlag` a la valeur true et qu’un problème est survenu lors de l’envoi d’un courrier électronique, un message d’erreur supplémentaire s’affiche, indiquant les ASP.NET qui ont été signalés lors de la tentative d’envoi du message électronique. Juste AVERTISSEMENT : les messages d’erreur signalés par ASP.NET lorsqu’il ne peut pas envoyer un message électronique peuvent être génériques. Par exemple, si ASP.NET ne parvient pas à contacter le serveur SMTP (par exemple, parce que vous avez effectué une erreur dans le nom du serveur), l’erreur est `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Important** Quand vous recevez un message d’erreur à partir d’un objet exception (`ex` dans le code), ne transmettez *pas* ce message par la suite aux utilisateurs. Les objets d’exception incluent souvent des informations que les utilisateurs ne doivent pas voir et qui peuvent même constituer une faille de sécurité. C’est pourquoi ce code comprend la variable `debuggingFlag` qui est utilisée comme commutateur pour afficher le message d’erreur et pourquoi la variable par défaut est définie sur false. Vous devez définir cette variable sur true (et, par conséquent, afficher le message d’erreur) *uniquement* si vous rencontrez un problème d’envoi de courrier électronique et que vous devez effectuer un débogage. Une fois que vous avez résolu les problèmes, réaffectez à `debuggingFlag` la valeur false.

    Modifiez les paramètres de messagerie suivants dans le code :

   - Définissez `your-SMTP-host` sur le nom du serveur SMTP auquel vous avez accès.
   - Définissez `your-user-name-here` sur le nom d’utilisateur de votre compte de serveur SMTP.
   - Définissez `your-account-password` sur le mot de passe de votre compte de serveur SMTP.
   - Définissez `your-email-address-here` sur votre propre adresse de messagerie. Il s’agit de l’adresse de messagerie à partir de laquelle le message est envoyé. (Certains fournisseurs de messagerie ne vous permettent pas de spécifier une adresse de `From` différente et utilisent votre nom d’utilisateur comme adresse `From`.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Configuration des paramètres de messagerie
     > 
     > Il peut parfois s’avérer difficile de s’assurer que vous disposez des bons paramètres pour le serveur SMTP, le numéro de port, etc. Voici quelques conseils :
     > 
     > - Le nom du serveur SMTP est souvent semblable à `smtp.provider.com` ou `smtp.provider.net`. Toutefois, si vous publiez votre site sur un fournisseur d’hébergement, le nom du serveur SMTP à ce stade peut être `localhost`. En effet, une fois que vous avez publié et que votre site est en cours d’exécution sur le serveur du fournisseur, le serveur de messagerie peut être local du point de vue de votre application. Cette modification des noms de serveurs peut signifier que vous devez modifier le nom du serveur SMTP dans le cadre de votre processus de publication.
     > - Le numéro de port est généralement 25. Toutefois, certains fournisseurs nécessitent que vous utilisiez le port 587 ou un autre port.
     > - Veillez à utiliser les informations d’identification appropriées. Si vous avez publié votre site sur un fournisseur d’hébergement, utilisez les informations d’identification que le fournisseur a spécifiquement indiquées pour la messagerie électronique. Celles-ci peuvent être différentes des informations d’identification que vous utilisez pour publier.
     > - Parfois, vous n’avez pas besoin d’informations d’identification. Si vous envoyez un e-mail à l’aide de votre fournisseur de services Internet personnel, votre fournisseur de messagerie peut déjà connaître vos informations d’identification. Après la publication, vous devrez peut-être utiliser d’autres informations d’identification que lorsque vous testez sur votre ordinateur local.
     > - Si votre fournisseur de messagerie utilise le chiffrement, vous devez définir `WebMail.EnableSsl` sur `true`.
4. Exécutez la page *EmailRequest. cshtml* dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.)
5. Entrez votre nom et une description du problème, puis cliquez sur le bouton **Envoyer** . Vous êtes redirigé vers la page *ProcessRequest. cshtml* , qui confirme votre message et vous envoie un message électronique. 

    ![[image]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Envoi d’un fichier par E-mail

Vous pouvez également envoyer des fichiers joints à des messages électroniques. Dans cette procédure, vous créez un fichier texte et deux pages HTML. Vous utiliserez le fichier texte comme pièce jointe d’un e-mail.

1. Dans le site Web, ajoutez un nouveau fichier texte et nommez-le *MyFile. txt*.
2. Copiez le texte suivant et collez-le dans le fichier : 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Créez une page nommée *sendfile. cshtml* et ajoutez le balisage suivant : 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Créez une page nommée « $ *. cshtml* » et ajoutez le balisage suivant : 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modifiez les paramètres de messagerie suivants dans le code de l’exemple :

    - Définissez `your-SMTP-host` sur le nom d’un serveur SMTP auquel vous avez accès.
    - Définissez `your-user-name-here` sur le nom d’utilisateur de votre compte de serveur SMTP.
    - Définissez `your-email-address-here` sur votre propre adresse de messagerie. Il s’agit de l’adresse de messagerie à partir de laquelle le message est envoyé.
    - Définissez `your-account-password` sur le mot de passe de votre compte de serveur SMTP.
    - Définissez `target-email-address-here` sur votre propre adresse de messagerie. (Comme précédemment, vous envoyez normalement un e-mail à quelqu’un d’autre, mais à des fins de test, vous pouvez l’envoyer à vous-même.)
6. Exécutez la page *sendfile. cshtml* dans un navigateur.
7. Entrez votre nom, une ligne d’objet et le nom du fichier texte à joindre (*MyFile. txt*).
8. Cliquez sur le bouton `Submit`. Comme précédemment, vous êtes redirigé vers la page de l’adresse *. cshtml* qui confirme votre message et vous envoie un message électronique avec le fichier joint.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [ASP.NET Web Pages (Razor) - Guide de résolution des problèmes](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Protocole de transfert de courrier simple](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personnalisation du comportement à l’ensemble du site pour pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
