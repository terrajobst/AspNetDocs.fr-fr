---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Prise en main | Microsoft Docs
author: Rick-Anderson
description: WebMatrix n’est plus recommandé en tant qu’environnement de développement intégré pour pages Web ASP.NET. Utilisez Visual Studio ou Visual Studio Code. Ce guide...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: bb863f8605e6f8faca3b285607b63a3e88e83012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547102"
---
# <a name="getting-started"></a>Commencer

par [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > WebMatrix n’est plus recommandé en tant qu’environnement de développement intégré pour pages Web ASP.NET. Utilisez [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) ou [Visual Studio code](https://code.visualstudio.com/).
> 
> 
> Ce guide et cette application vous donnent une vue d’ensemble de pages Web ASP.NET (version 2 ou ultérieure) et syntaxe Razor, une infrastructure légère pour la création de sites Web dynamiques. Il présente également WebMatrix, un outil permettant de créer des pages et des sites.
> 
> **Niveau**: nouveauté de pages Web ASP.net.  
> **Compétences supposées**: HTML, feuilles de style en cascade de base (CSS).
> 
> Ce que vous allez apprendre dans le premier didacticiel de l’ensemble :
> 
> - Quelle est la technologie pages Web ASP.NET et ce qu’elle fait.
> - Présentation de WebMatrix.
> - Comment installer tout.
> - Comment créer un site Web à l’aide de WebMatrix.
>   
> 
> Fonctionnalités/technologies présentées :
> 
> - Microsoft Web Platform Installer.
> - WebMatrix.
> - pages *. cshtml*
>   
> 
> Mike pape a écrit ce didacticiel. Tom FitzMacken l’a mis à jour pour Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - WebMatrix 3

## <a name="what-should-you-know"></a>Que devez-vous savoir ?

Nous supposons que vous êtes familiarisé avec :

- **HTML**. Aucune expertise approfondie n’est requise. Nous n’expliquerons pas HTML, mais nous n’utilisons pas non plus d’éléments complexes. Nous vous fournirons des liens vers des didacticiels HTML où nous pensons qu’ils sont utiles.
- **Feuilles de style en cascade (CSS)** . Identique à HTML.
- **Idées de base de données de**base. Si vous avez utilisé une feuille de calcul pour les données et que vous avez trié et filtré les données, c’est le niveau d’expertise que nous supposons généralement pour cet ensemble de didacticiels.

Nous partons également du principe que vous êtes intéressé par l’apprentissage de la programmation de base. Pages Web ASP.NET utiliser un langage de programmation C#appelé. Il n’est pas nécessaire que vous ayez un arrière-plan en programmation, juste un intérêt pour celui-ci. Si vous avez déjà écrit du JavaScript dans une page Web avant, vous disposez d’une grande quantité d’informations.

Notez que si vous êtes familiarisé avec la programmation, vous constaterez peut-être que ce didacticiel a été déplacé au départ lentement pendant que nous offrons de nouveaux programmeurs à la vitesse. Au-delà des quelques premiers didacticiels, toutefois, il y aura moins de programmation de base à expliquer et les choses seront déplacées plus rapidement.

## <a name="what-do-you-need"></a>De quoi avez-vous besoin ?

Voici ce dont vous aurez besoin :

- Un ordinateur exécutant Windows 8, Windows 7, Windows Server 2008 ou Windows Server 2012.
- Une connexion Internet en direct.
- Privilèges d’administrateur (requis pour le processus d’installation).

## <a name="what-is-aspnet-web-pages"></a>Qu’est-ce que pages Web ASP.NET ?

Pages Web ASP.NET est une infrastructure que vous pouvez utiliser pour créer des pages Web dynamiques. Une page Web HTML simple est statique ; son contenu est déterminé par le balisage HTML fixe figurant dans la page. Les pages dynamiques comme celles que vous créez avec pages Web ASP.NET vous permettent de créer le contenu de la page à la volée, à l’aide de code.

Les pages dynamiques vous permettent d’effectuer toutes sortes de choses. Vous pouvez demander une entrée à un utilisateur à l’aide d’un formulaire, puis modifier ce que la page affiche ou comment elle ressemble. Vous pouvez prendre les informations d’un utilisateur, l’enregistrer dans une base de données, puis la répertorier ultérieurement. Vous pouvez envoyer un e-mail à partir de votre site. Vous pouvez interagir avec d’autres services sur le Web (par exemple, un service de mappage) et produire des pages qui intègrent les informations de ces sources.

## <a name="what-is-webmatrix"></a>Qu’est-ce que WebMatrix ?

WebMatrix est un outil qui intègre un éditeur de page Web, un utilitaire de base de données, un serveur Web pour les pages de test et des fonctionnalités pour la publication de votre site Web sur Internet. WebMatrix est gratuit et il est facile à installer et facile à utiliser. (Cela fonctionne également pour les pages HTML simples, ainsi que pour d’autres technologies telles que PHP.)

Vous n' *avez pas besoin* d’utiliser WebMatrix pour travailler avec pages Web ASP.net. Vous pouvez créer des pages à l’aide d’un éditeur de texte, par exemple, et tester des pages à l’aide d’un serveur Web auquel vous avez accès. Toutefois, WebMatrix le rend très simple. ces didacticiels utilisent donc WebMatrix tout au long de la procédure.

## <a name="about-these-tutorials"></a>À propos de ces didacticiels

Ce didacticiel est une introduction à l’utilisation de pages Web ASP.NET. Il y a 9 didacticiels au total dans ce didacticiel d’introduction. Il fait partie d’une série de didacticiels qui vous permet de pages Web ASP.NET novice à la création de sites Web de qualité professionnelle.

Ce premier didacticiel a pour fonction de vous montrer les principes fondamentaux de l’utilisation de pages Web ASP.NET. Lorsque vous avez terminé, vous pouvez utiliser des ensembles de didacticiels supplémentaires qui reprennent là où celui-ci se termine et qui explorent les pages Web plus en détail.

Nous sommes délibérément faciles sur les explications approfondies. Et chaque fois que nous présentons quelque chose, pour ce didacticiel, nous choisissons toujours la façon dont nous pensons qu’il est plus facile à comprendre. Les ensembles de didacticiels ultérieurs vont plus loin et vous montrent des approches plus efficaces ou plus flexibles (également plus amusantes). Toutefois, ces didacticiels vous obligent à comprendre les principes de base d’abord.

L’ensemble de didacticiels que vous venez de démarrer aborde les fonctionnalités suivantes de pages Web ASP.NET :

- Présentation et obtention de tous les éléments installés. (Qui se trouve dans le didacticiel que vous lisez.)
- Notions de base de la programmation de pages Web ASP.NET.
- Création d’une base de données.
- Création et traitement d’un formulaire de saisie utilisateur.
- Ajout, mise à jour et suppression de données dans la base de données.

## <a name="what-will-you-create"></a>Que allez-vous créer ?

Cet ensemble de didacticiels et les suivants s’apparentent à un site Web dans lequel vous pouvez répertorier les films de votre choix. Vous serez en mesure d’entrer des films, de les modifier et de les répertorier. Voici quelques-unes des pages que vous allez créer dans ce didacticiel. La première affiche la page de la liste des films que vous allez créer :

![Application de films a fini affichant une liste de films](getting-started/_static/image1.png)

Et voici la page qui vous permet d’ajouter de nouvelles informations de film à votre site :

![Application vidéo terminée avec la page Ajouter un film](getting-started/_static/image2.png)

Le didacticiel suivant définit la build sur cet ensemble et ajoute des fonctionnalités, telles que le chargement d’images, permettant aux utilisateurs de se connecter, d’envoyer des courriers électroniques et d’intégrer des réseaux sociaux.

## <a name="see-this-app-running-on-azure"></a>Voir cette application en cours d’exécution sur Azure

Voulez-vous que le site terminé s’exécute en tant qu’application Web en direct ? Vous pouvez déployer une version complète de l’application sur votre compte Azure en cliquant simplement sur le bouton suivant.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Vous avez besoin d’un compte Azure pour déployer cette solution sur Azure. Si vous n’avez pas encore de compte, vous disposez des options suivantes :

- [Ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : vous recevez des crédits que vous pouvez utiliser pour tester les services Azure payants, et même après leur utilisation, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- [Activer les avantages des abonnés MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

## <a name="installing-everything"></a>Tout installer

Vous pouvez tout installer à l’aide de l’Web Platform Installer de Microsoft. En effet, vous installez le programme d’installation, puis vous l’utilisez pour installer tout le reste.

Pour utiliser les pages Web, vous devez avoir au moins Windows XP avec SP3 installé, ou Windows Server 2008 ou version ultérieure.

Dans la [page pages Web](../../../index.md) du site Web ASP.net, cliquez sur **installer**.

![Site Web ASP.NET qui indique &quot;bouton installer WebMatrix&quot;](getting-started/_static/image3.png)

Vous êtes invité à accepter les termes du contrat de licence et la déclaration de confidentialité avant d’installer WebMatrix.

![accepter le terme pour commencer l’installation](getting-started/_static/image4.png)

Cliquez sur **exécuter** pour démarrer l’installation. (Si vous souhaitez enregistrer le programme d’installation, cliquez sur **Enregistrer** , puis exécutez le programme d’installation à partir du dossier où vous l’avez enregistré.)

![](getting-started/_static/image5.png)

Le Web Platform Installer s’affiche, prêt à installer WebMatrix. Cliquez sur **Suivant**.

![](getting-started/_static/image6.png)

Le processus d’installation détermine ce qu’il doit installer sur votre ordinateur et démarre le processus. En fonction de ce qui doit être installé, le processus peut durer de quelques minutes à plusieurs minutes. Sélectionnez **J’accepte** pour accepter les termes du contrat de licence.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Une fois l’opération terminée, le processus d’installation peut lancer WebMatrix automatiquement. Si ce n’est pas le cas, dans Windows, dans le menu **Démarrer** , lancez **Microsoft WebMatrix**.

Lorsque vous lancez WebMatrix pour la première fois, vous avez la possibilité de vous connecter à Microsoft Azure avec votre compte Microsoft. En vous connectant, vous recevrez 10 applications Web gratuites via Azure. Ces applications Web gratuites offrent un moyen pratique de tester vos applications. Si vous n’avez pas encore de compte Azure, mais que vous disposez d’un abonnement MSDN, vous pouvez [activer les avantages de votre abonnement MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Sinon, vous pouvez créer un compte d’évaluation gratuit en quelques minutes. Pour plus d’informations, consultez [Essai gratuit Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Vous n’avez pas besoin de vous connecter pour continuer ce didacticiel. Si vous ne vous connectez pas maintenant, vous aurez toujours la possibilité de vous connecter ultérieurement. La dernière [rubrique](publishing.md) de cette série de didacticiels explique comment déployer votre site Web sur Azure. par conséquent, vous devez vous connecter pour terminer cette rubrique.

À ce stade, connectez-vous avec votre compte Microsoft ou sélectionnez **pas maintenant** dans le coin inférieur droit.

![Se connecter](getting-started/_static/image7.png)

Pour commencer, vous allez créer un site Web vide et ajouter une page. Dans un didacticiel ultérieur de cet ensemble, vous jouez avec l’un des modèles de site Web intégrés.

Dans la fenêtre Démarrer, cliquez sur **nouveau**.

![Écran de démarrage de WebMatrix](getting-started/_static/image8.png)

Les modèles sont des fichiers et des pages prédéfinis pour différents types de sites Web. Pour afficher tous les modèles qui sont disponibles par défaut, sélectionnez l’option Galerie de modèles.

![Sélectionner la Galerie de modèles](getting-started/_static/image9.png)

Dans la fenêtre **démarrage rapide** , sélectionnez **site vide** dans le groupe **ASP.net** et nommez le nouveau site « WebPagesMovies ».

![Fenêtre de Démarrage rapide WebMatrix avec un modèle de site sélectionné](getting-started/_static/image10.png)

Cliquez sur **Next**.

Si vous vous êtes connecté à votre compte Microsoft, vous aurez la possibilité de créer le site sur Azure. En fonction du nom de votre site, le nom par défaut **WebPagesMovies.azurewebsites.net** est suggéré. Toutefois, le point d’exclamation indique que ce nom n’est pas disponible sur Windows Azure. Pour plus de simplicité, sélectionnez **Ignorer** pour ignorer la création du site Web sur Azure pour le moment. Plus loin dans cette série, nous publierons le site sur Azure.

![créer un site Azure](getting-started/_static/image11.png)

WebMatrix crée et ouvre le site :

![Nouveau site WebPagesMovies ouvert dans WebMatrix](getting-started/_static/image12.png)

En haut, il y a une barre d’outils accès rapide et un ruban. En bas à gauche, vous voyez le sélecteur d’espace de travail dans lequel vous basculez entre les tâches (**site**, **fichiers**, **bases de données**, **rapports**). À droite se trouve le volet de contenu de l’éditeur et des rapports. Et dans la partie inférieure, vous verrez parfois une barre de notification pour les messages.

Vous en apprendrez davantage sur WebMatrix et ses fonctionnalités au fur et à mesure que vous parcourez ces didacticiels.

## <a name="creating-a-web-page"></a>Création d’une page Web

Pour vous familiariser avec WebMatrix et pages Web ASP.NET, vous allez créer une page simple.

Dans le sélecteur d’espace de travail, sélectionnez l’espace de travail **fichiers** . Cet espace de travail vous permet d’utiliser des fichiers et des dossiers. Le volet gauche affiche la structure de fichiers de votre site. Le ruban change pour afficher les tâches liées aux fichiers.

![Espace de travail fichier dans WebMatrix](getting-started/_static/image13.png)

Dans le ruban, cliquez sur la flèche sous **nouveau** , puis cliquez sur **nouveau fichier**.

![Utilisation de la commande &quot;New&quot; dans le ruban pour créer un nouveau fichier](getting-started/_static/image14.png)

WebMatrix affiche la liste des types de fichiers. Sélectionnez **cshtml**, puis dans la zone **nom** , tapez « HelloWorld ». Une page CSHTML est une page pages Web ASP.NET.

![Création d’une nouvelle page CSHTML nommée HelloWorld. cshtml](getting-started/_static/image15.png)

Cliquez sur **OK**.

WebMatrix crée la page et l’ouvre dans l’éditeur.

![Nouvelle page HelloWorld de l’Éditeur WebMatrix](getting-started/_static/image16.png)

Comme vous pouvez le voir, la page contient principalement un balisage HTML ordinaire, à l’exception d’un bloc en haut qui ressemble à ceci :

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

C’est pour ajouter du code, comme vous le verrez bientôt.

Notez que les différentes parties de la page &mdash; les noms d’éléments, les attributs et le texte, plus le bloc en haut, sont tous dans des couleurs différentes. C’est ce que l’on appelle la *mise en surbrillance*de la syntaxe et facilite le maintien de tous les éléments clairs. C’est l’une des fonctionnalités qui facilite l’utilisation des pages Web dans WebMatrix.

Ajoutez du contenu pour les éléments `<head>` et `<body>` comme dans l’exemple suivant. (Si vous le souhaitez, vous pouvez simplement copier le bloc suivant et remplacer l’intégralité de la page existante par ce code.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Dans la barre d’outils accès rapide ou dans le menu **fichier** , cliquez sur **Enregistrer**.

![Bouton enregistrer dans la barre d’outils accès rapide WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Test de la page

Dans l’espace de travail **fichiers** , cliquez avec le bouton droit sur la page *HelloWorld. cshtml* , puis cliquez sur **lancer dans le navigateur**.

![Exécution d’une page à l’aide du bouton exécuter dans le ruban WebMatrix](getting-started/_static/image18.png)

WebMatrix démarre un serveur Web intégré (IIS Express) que vous pouvez utiliser pour tester des pages sur votre ordinateur. (Sans IIS Express dans WebMatrix, vous auriez besoin de publier votre page sur un serveur Web quelque part avant de pouvoir la tester.) La page s’affiche dans votre navigateur par défaut.

![&quot;Hello World&quot; page en cours d’exécution dans le navigateur](getting-started/_static/image19.png)

Notez que lorsque vous testez une page dans WebMatrix, l’URL dans le navigateur est semblable à `http://localhost:33651/HelloWorld.cshtml.` le nom *localhost* fait référence à un serveur local, ce qui signifie que la page est fournie par un serveur Web qui est sur votre ordinateur. Comme indiqué, WebMatrix intègre un programme de serveur Web nommé IIS Express qui s’exécute lorsque vous lancez une page.

Le nombre après *localhost* (par exemple, *localhost : 33651*) fait référence à un *numéro de port* sur votre ordinateur. Il s’agit du numéro du « canal » utilisé par IIS Express pour ce site Web particulier. Le numéro de port est sélectionné au hasard dans la plage comprise entre 1024 et 65536 lorsque vous créez votre site, et il est différent pour chaque site que vous créez. (Lorsque vous testez votre propre site, le numéro de port sera presque certainement un nombre différent de 33561.) En utilisant un port différent pour chaque site Web, IIS Express pouvez conserver directement les sites auxquels il parle.

Plus tard, lorsque vous publierez votre site sur un serveur Web public, vous ne verrez plus *localhost* dans l’URL. À ce stade, vous verrez une URL plus courante comme `http://myhostingsite/mywebsite/HelloWorld.cshtml` ou quelle que soit la page. Vous en apprendrez davantage sur la publication d’un site plus loin dans cette série de didacticiels.

## <a name="adding-some-server-side-code"></a>Ajout de code côté serveur

Fermez le navigateur et revenez à la page dans WebMatrix.

Ajoutez une ligne au bloc de code afin qu’il ressemble au code suivant :

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Il s’agit d’un petit peu de code Razor. Il est probablement évident qu’il obtient la date et l’heure actuelles et place cette valeur dans une *variable* nommée `currentDateTime`. Pour plus d’informations sur les syntaxe Razor, consultez le didacticiel suivant.

Dans le corps de la page, après l’élément `<p>Hello World!</p>`, ajoutez ce qui suit :

[!code-html[Main](getting-started/samples/sample4.html)]

Ce code obtient la valeur que vous placez dans la variable `currentDateTime` en haut et l’insère dans le balisage de la page. Le caractère `@` marque le code pages Web ASP.NET dans la page.

Réexécuter la page (WebMatrix enregistre les modifications pour vous avant d’exécuter la page). Cette fois, vous voyez la date et l’heure dans la page.

![&quot;Hello World&quot; page en cours d’exécution dans le navigateur avec un affichage de temps généré dynamiquement](getting-started/_static/image20.png)

Patientez quelques instants, puis actualisez la page dans le navigateur. L’affichage de la date et de l’heure est mis à jour.

Dans le navigateur, examinez la page source. Elle ressemble à la balise suivante :

[!code-html[Main](getting-started/samples/sample5.html)]

Notez que le bloc `@{ }` en haut n’y figure pas. Notez également que l’affichage de la date et de l’heure affiche une chaîne de caractères réelle (`1/18/2012 2:49:50 PM` ou autre), et non pas `@currentDateTime` comme vous l’avez fait dans la page *. cshtml* . Ce qui est arrivé ici, c’est que lorsque vous avez exécuté la page, ASP.NET a traité tout le code (très peu dans ce cas) qui était marqué avec `@`. Le code produit une sortie et cette sortie a été insérée dans la page.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>C’est ce que pages Web ASP.NET

Lorsque vous lisez ce pages Web ASP.NET produit du contenu Web dynamique, ce que vous avez vu ici est l’idée. La page que vous venez de créer contient le même balisage HTML que vous avez déjà vu. Il peut également contenir du code capable d’effectuer toutes sortes de tâches. Dans cet exemple, il s’agissait de la tâche simple d’obtention de la date et de l’heure actuelles. Comme vous l’avez vu, vous pouvez intercaler du code avec le code HTML pour produire la sortie dans la page. Quand quelqu’un demande une page *. cshtml* dans le navigateur, ASP.net traite la page alors qu’elle est toujours dans les mains du serveur Web. ASP.NET insère la sortie du code (le cas échéant) dans la page au format HTML. Une fois le traitement du code terminé, ASP.NET envoie la page résultante au navigateur. Tout le navigateur obtient le code HTML. Voici un diagramme :

![Déroulement conceptuel de la génération dynamique du code HTML par ASP.NET](getting-started/_static/image21.png)

L’idée est simple, mais il existe de nombreuses tâches intéressantes que le code peut effectuer, et il existe de nombreux moyens intéressants dans lesquels vous pouvez ajouter dynamiquement du contenu HTML à la page. Et les pages ASP.NET *. cshtml* , comme n’importe quelle page HTML, peuvent également inclure du code qui s’exécute dans le navigateur lui-même (code JavaScript et jQuery). Vous y découvrirez tous ces éléments dans ce didacticiel et dans d’autres.

## <a name="coming-up-next"></a>À venir suivant

Dans le didacticiel suivant de cette série, vous explorerez pages Web ASP.NET programmation un peu plus.

## <a name="additional-resources"></a>Ressources supplémentaires

[Créez un site web ASP.net à partir de zéro](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Il s’agit d’un didacticiel qui concerne spécifiquement l’utilisation de WebMatrix (et non pages Web ASP.NET). Il aborde plus en détail certaines des fonctionnalités supplémentaires de WebMatrix que nous n’aborderons pas dans cet ensemble de didacticiels.

> [!div class="step-by-step"]
> [Next](intro-to-web-pages-programming.md)
