---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personnalisation du comportement à l’ensemble du site pour les sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre explique comment définir des paramètres sur l’ensemble de votre site Web ou sur un dossier entier, plutôt que sur une seule page.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: f05e05f725d9209bce283ce18659ae5efe4de2ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634623"
---
# <a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personnalisation du comportement à l’ensemble du site pour les sites pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment créer des paramètres côté site pour les pages d’un site Web pages Web ASP.NET (Razor).
> 
> Ce que vous allez apprendre :
> 
> - Comment exécuter du code qui vous permet de définir des valeurs (valeurs globales ou paramètres d’assistance) pour toutes les pages d’un site.
> - Comment exécuter du code qui vous permet de définir des valeurs pour toutes les pages d’un dossier.
> - Comment exécuter du code avant et après le chargement d’une page.
> - Comment envoyer des erreurs vers une page d’erreur centrale.
> - Comment ajouter l’authentification à toutes les pages d’un dossier.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - WebMatrix 3
> - Bibliothèque d’applications auxiliaires Web ASP.NET (package NuGet)
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 3 et Visual Studio 2013 (ou Visual Studio Express 2013 pour le Web), sauf que vous ne pouvez pas utiliser la bibliothèque d’applications auxiliaires Web ASP.NET.

<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Ajout du code de démarrage du site Web pour pages Web ASP.NET

Pour une grande partie du code que vous écrivez dans pages Web ASP.NET, une page individuelle peut contenir tout le code requis pour cette page. Par exemple, si une page envoie un message électronique, il est possible de placer tout le code de cette opération sur une seule page. Cela peut inclure le code permettant d’initialiser les paramètres d’envoi de courrier électronique (c’est-à-dire pour le serveur SMTP) et d’envoyer le message électronique.

Toutefois, dans certaines situations, vous souhaiterez peut-être exécuter du code avant l’exécution d’une page sur le site. Cela est utile pour définir des valeurs qui peuvent être utilisées n’importe où dans le site (appelées *valeurs globales*). Par exemple, certaines applications d’assistance vous obligent à fournir des valeurs telles que les paramètres de messagerie ou les clés de compte. Il peut être pratique de conserver ces paramètres dans les valeurs globales.

Pour ce faire, vous pouvez créer une page nommée *\_AppStart. cshtml* à la racine du site. Si cette page existe, elle est exécutée la première fois qu’une page du site est demandée. Par conséquent, il est recommandé d’exécuter du code pour définir des valeurs globales. (Étant donné que *\_AppStart. cshtml* a un préfixe de trait de soulignement, ASP.net n’envoie pas la page à un navigateur même si les utilisateurs la demandent directement.)

Le diagramme suivant illustre le fonctionnement de la page *\_AppStart. cshtml* . Lorsqu’une demande est envoyée pour une page, et s’il s’agit de la première requête pour une page du site, ASP.NET vérifie tout d’abord si une page *\_AppStart. cshtml* existe. Dans ce cas, le code de la page *\_AppStart. cshtml* s’exécute, puis la page demandée s’exécute.

![[image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Définition de valeurs globales pour votre site Web

1. Dans le dossier racine d’un site Web WebMatrix, créez un fichier nommé *\_AppStart. cshtml*. Le fichier doit se trouver à la racine du site.
2. Remplacez le contenu existant par ce qui suit : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Ce code stocke une valeur dans le dictionnaire `AppState`, qui est automatiquement disponible pour toutes les pages du site. Notez que le fichier *\_AppStart. cshtml* ne contient aucune balise. La page exécutera le code, puis redirigera vers la page demandée à l’origine.

    > [!NOTE]
    > Soyez prudent lorsque vous placez du code dans le fichier *\_AppStart. cshtml* . Si des erreurs se produisent dans le code dans le fichier *\_AppStart. cshtml* , le site Web ne démarre pas.
3. Dans le dossier racine, créez une nouvelle page nommée *appname. cshtml*.
4. Remplacez le balisage et le code par défaut par les éléments suivants : 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Ce code extrait la valeur de l’objet `AppState` que vous avez défini dans la page *\_AppStart. cshtml* .
5. Exécutez la page *appname. cshtml* dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) La page affiche la valeur globale. 

    ![[image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Définition des valeurs des applications auxiliaires

Une bonne utilisation du fichier *\_AppStart. cshtml* consiste à définir les valeurs des applications auxiliaires que vous utilisez dans votre site et qui doivent être initialisées. Les paramètres de messagerie pour le programme d’assistance `WebMail`, ainsi que les clés privées et publiques pour le `ReCaptcha` Helper sont des exemples typiques. Dans les cas de ce type, vous pouvez définir les valeurs une fois dans la *\_AppStart. cshtml* , puis elles sont déjà définies pour toutes les pages de votre site.

Cette procédure vous montre comment définir les paramètres de `WebMail` de manière globale. (Pour plus d’informations sur l’utilisation du programme d’assistance `WebMail`, consultez la [page Ajout d’un courrier électronique à un Site pages Web ASP.net](../getting-started/11-adding-email-to-your-web-site.md).)

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas encore fait.
2. Si vous ne disposez pas déjà d’un fichier *\_AppStart. cshtml* , dans le dossier racine d’un site Web, créez un fichier nommé *\_AppStart. cshtml*.
3. Ajoutez les paramètres de `WebMail` suivants au fichier *\_AppStart. cshtml* : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modifiez les paramètres de messagerie suivants dans le code :

   - Définissez `your-SMTP-host` sur le nom du serveur SMTP auquel vous avez accès.
   - Définissez `your-user-name-here` sur le nom d’utilisateur de votre compte de serveur SMTP.
   - Définissez `your-account-password` sur le mot de passe de votre compte de serveur SMTP.
   - Définissez `your-email-address-here` sur votre propre adresse de messagerie. Il s’agit de l’adresse de messagerie à partir de laquelle le message est envoyé. (Certains fournisseurs de messagerie ne vous permettent pas de spécifier une adresse de `From` différente et utilisent votre nom d’utilisateur comme adresse `From`.)

     Pour plus d’informations sur les paramètres SMTP, consultez [Configuration des paramètres de courrier électronique](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) dans l’article [envoi de courriers électroniques à partir d’un site pages Web ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) et [problèmes liés à l’envoi de courrier électronique](https://go.microsoft.com/fwlink/?LinkId=253001#email) dans le [Guide de résolution des problèmes pages Web ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Enregistrez le fichier *\_AppStart. cshtml* et fermez-le.
5. Dans le dossier racine d’un site Web, créez une nouvelle page nommée *TestEmail. cshtml*.
6. Remplacez le contenu existant par ce qui suit : 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Exécutez la page *TestEmail. cshtml* dans un navigateur.
8. Renseignez les champs pour vous envoyer un message électronique, puis cliquez sur **Envoyer**.
9. Vérifiez votre adresse de messagerie pour vous assurer que vous avez reçu le message.

La partie importante de cet exemple est que les paramètres que vous ne modifiez généralement pas, tels que le nom de votre serveur SMTP et vos informations d’identification de messagerie, sont définis dans le fichier *\_AppStart. cshtml* . De cette façon, vous n’avez pas besoin de les redéfinir dans chaque page où vous envoyez un courrier électronique. (Même si, pour une raison quelconque, vous devez modifier ces paramètres, vous pouvez les définir individuellement dans une page.) Dans la page, vous définissez uniquement les valeurs qui changent généralement à chaque fois, comme le destinataire et le corps du message électronique.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Exécution de code avant et après les fichiers dans un dossier

Tout comme vous pouvez utiliser *\_AppStart. cshtml* pour écrire du code avant l’exécution des pages du site, vous pouvez écrire du code qui s’exécute avant (et après) toute page dans un dossier particulier exécuté. Cela est utile pour des tâches telles que la définition de la même page de disposition pour toutes les pages d’un dossier, ou pour vérifier qu’un utilisateur a ouvert une session avant d’exécuter une page dans le dossier.

Pour les pages dans des dossiers particuliers, vous pouvez créer du code dans un fichier nommé *\_PageStart. cshtml*. Le diagramme suivant illustre le fonctionnement de la page *\_PageStart. cshtml* . Lorsqu’une demande est envoyée pour une page, ASP.NET commence par Rechercher une page *\_AppStart. cshtml* et l’exécute. ASP.NET vérifie ensuite s’il existe un *\_page PageStart. cshtml* et, le cas échéant, l’exécute. Il exécute ensuite la page demandée.

À l’intérieur de la page *\_PageStart. cshtml* , vous pouvez spécifier où vous souhaitez que la page demandée s’exécute en incluant une méthode `RunPage`. Cela vous permet d’exécuter du code avant l’exécution de la page demandée, puis de nouveau après. Si vous n’incluez pas `RunPage`, tout le code dans *\_PageStart. cshtml* s’exécute, puis la page demandée s’exécute automatiquement.

![[image]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET vous permet de créer une hiérarchie de *\_fichiers PageStart. cshtml* . Vous pouvez placer un *\_fichier PageStart. cshtml* à la racine du site et dans n’importe quel sous-dossier. Quand une page est demandée, le *\_fichier PageStart. cshtml* au niveau supérieur (le plus proche de la racine du site) est exécuté, suivi du *\_fichier PageStart. cshtml* dans le sous-dossier suivant, et ainsi de suite jusqu’à la dernière structure du sous-dossier jusqu’à ce que la demande atteigne le dossier contenant la page demandée. Une fois que tous les fichiers *\_PageStart. cshtml* applicables ont été exécutés, la page demandée s’exécute.

Par exemple, vous pouvez avoir la combinaison suivante de fichiers *\_PageStart. cshtml* et le fichier *default. cshtml* :

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Lorsque vous exécutez */MyFolder/default.cshtml*, les éléments suivants s’affichent :

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Exécution du code d’initialisation pour toutes les pages d’un dossier

Une bonne utilisation pour *\_fichiers PageStart. cshtml* consiste à initialiser la même page de disposition pour tous les fichiers dans un seul dossier.

1. Dans le dossier racine, créez un nouveau dossier nommé *InitPages*.
2. Dans le dossier *InitPages* de votre site Web, créez un fichier nommé *\_PageStart. cshtml* et remplacez le balisage par défaut par le code suivant : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. À la racine du site Web, créez un dossier nommé *Shared*.
4. Dans le dossier *partagé* , créez un fichier nommé *\_layout1. cshtml* et remplacez le balisage par défaut par le code suivant : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. Dans le dossier *InitPages* , créez un fichier nommé *Content1. cshtml* et remplacez le contenu existant par ce qui suit : 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. Dans le dossier *InitPages* , créez un autre fichier nommé *Content2. cshtml* et remplacez le balisage par défaut par ce qui suit : 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Exécutez *Content1. cshtml* dans un navigateur. 

    ![[image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Lors de l’exécution de la page *Content1. cshtml* , le *\_fichier PageStart. cshtml* définit `Layout` et définit également `PageData["MyBackground"]` sur une couleur. Dans *Content1. cshtml*, la disposition et la couleur sont appliquées.
8. Affichez *Content2. cshtml* dans un navigateur. 

    La disposition est la même, car les deux pages utilisent la même page de disposition et la même couleur que celles initialisées dans *\_PageStart. cshtml*.

## <a name="using-_pagestartcshtml-to-handle-errors"></a>Utilisation de \_PageStart. cshtml pour gérer les erreurs

Une autre bonne utilisation du fichier *\_PageStart. cshtml* consiste à créer un moyen de gérer les erreurs de programmation (exceptions) qui peuvent se produire dans n’importe quelle page *. cshtml* dans un dossier. Cet exemple vous montre une façon de procéder.

1. Dans le dossier racine, créez un dossier nommé *InitCatch*.
2. Dans le dossier *InitCatch* de votre site Web, créez un fichier nommé *\_PageStart. cshtml* et remplacez le balisage existant et le code par ce qui suit : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    Dans ce code, vous essayez d’exécuter la page demandée explicitement en appelant la méthode `RunPage` à l’intérieur d’un bloc `try`. Si des erreurs de programmation se produisent dans la page demandée, le code à l’intérieur du bloc `catch` s’exécute. Dans ce cas, le code redirige vers une page (*Error. cshtml*) et transmet le nom du fichier qui a rencontré l’erreur dans le cadre de l’URL. (Vous allez bientôt créer la page.)
3. Dans le dossier *InitCatch* de votre site Web, créez un fichier nommé *exception. cshtml* et remplacez le balisage existant et le code par ce qui suit : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Dans le cadre de cet exemple, ce que vous effectuez dans cette page est délibérément créé une erreur en tentant d’ouvrir un fichier de base de données qui n’existe pas.
4. Dans le dossier racine, créez un fichier nommé *Error. cshtml* et remplacez le balisage existant et le code par ce qui suit : 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Dans cette page, l’expression `@Request["source"]` récupère la valeur de l’URL et l’affiche.
5. Dans la barre d’outils, cliquez sur **Enregistrer**.
6. Exécutez *exception. cshtml* dans un navigateur. 

    ![[image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Étant donné qu’une erreur se produit dans *exception. cshtml*, la page *\_PageStart. cshtml* redirige vers le fichier *Error. cshtml* , qui affiche le message.

    Pour plus d’informations sur les exceptions, consultez [Introduction à la programmation de pages Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-_pagestartcshtml-to-restrict-folder-access"></a>Utilisation de \_PageStart. cshtml pour limiter l’accès aux dossiers

Vous pouvez également utiliser le fichier *\_PageStart. cshtml* pour limiter l’accès à tous les fichiers d’un dossier.

1. Dans WebMatrix, créez un nouveau site Web à l’aide de l’option **site à partir du modèle** .
2. Dans les modèles disponibles, sélectionnez **Starter Site**.
3. Dans le dossier racine, créez un dossier nommé *AuthenticatedContent*.
4. Dans le dossier *AuthenticatedContent* , créez un fichier nommé *\_PageStart. cshtml* et remplacez le balisage existant et le code par ce qui suit : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Le code commence par empêcher la mise en cache de tous les fichiers du dossier. (Cela est nécessaire pour les scénarios tels que les ordinateurs publics, où vous ne souhaitez pas que les pages mises en cache d’un utilisateur soient disponibles pour l’utilisateur suivant.) Ensuite, le code détermine si l’utilisateur s’est connecté au site avant de pouvoir afficher les pages du dossier. Si l’utilisateur n’est pas connecté, le code redirige vers la page de connexion. La page de connexion peut retourner l’utilisateur à la page demandée à l’origine si vous incluez une valeur de chaîne de requête nommée `ReturnUrl`.
5. Créez une nouvelle page dans le dossier *AuthenticatedContent* nommé *page. cshtml*.
6. Remplacez le balisage par défaut par ce qui suit :  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Exécutez *page. cshtml* dans un navigateur. Le code vous redirige vers une page de connexion. Vous devez vous inscrire avant d’ouvrir une session. Une fois que vous vous êtes inscrit et que vous êtes connecté, vous pouvez accéder à la page et afficher son contenu.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Introduction à la programmation de pages Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
