---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Fonctionnement des fonctionnalités de débogage ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La possibilité de déboguer le code est une compétence que chaque développeur doit avoir dans son arsenal, quelle que soit la technologie qu’il utilise. Alors que de nombreux développeurs sont...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 08ced380f3551407d757524dbc84b5feeeb5482b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629506"
---
# <a name="understanding-aspnet-ajax-debugging-capabilities"></a>Présentation des fonctionnalités de débogage d’ASP.NET AJAX

par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> La possibilité de déboguer le code est une compétence que chaque développeur doit avoir dans son arsenal, quelle que soit la technologie qu’il utilise. Alors que de nombreux développeurs sont habitués à utiliser Visual Studio .NET ou Web Developer Express pour déboguer des C# applications ASP.net qui utilisent VB.net ou du code, certaines ne savent pas qu’elles sont également extrêmement utiles pour déboguer du code côté client tel que JavaScript. Le même type de technique utilisé pour déboguer des applications .NET peut également être appliqué aux applications compatibles AJAX et plus particulièrement aux applications AJAX ASP.NET.

## <a name="debugging-aspnet-ajax-applications"></a>Débogage d’applications ASP.NET AJAX

Dan Wahlin

La possibilité de déboguer le code est une compétence que chaque développeur doit avoir dans son arsenal, quelle que soit la technologie qu’il utilise. Il va sans dire que la compréhension des différentes options de débogage disponibles peut économiser beaucoup de temps sur un projet et peut-être même quelques tracas. Alors que de nombreux développeurs sont habitués à utiliser Visual Studio .NET ou Web Developer Express pour déboguer des C# applications ASP.net qui utilisent VB.net ou du code, certaines ne savent pas qu’elles sont également extrêmement utiles pour déboguer du code côté client tel que JavaScript. Le même type de technique utilisé pour déboguer des applications .NET peut également être appliqué aux applications compatibles AJAX et plus particulièrement aux applications AJAX ASP.NET.

Dans cet article, vous allez découvrir comment Visual Studio 2008 et plusieurs autres outils peuvent être utilisés pour déboguer des applications ASP.NET AJAX afin de localiser rapidement les bogues et autres problèmes. Cette discussion inclut des informations sur l’activation d’Internet Explorer 6 ou version ultérieure pour le débogage, l’utilisation de Visual Studio 2008 et l’Explorateur de scripts pour parcourir le code, ainsi que l’utilisation d’autres outils gratuits, tels que l’application auxiliaire pour le développement Web. Vous apprendrez également à déboguer des applications ASP.NET AJAX dans Firefox à l’aide d’une extension nommée Firebug qui vous permet d’effectuer un pas à pas détaillé du code JavaScript directement dans le navigateur sans autre outil. Enfin, vous allez découvrir les classes de la bibliothèque AJAX ASP.NET qui peuvent vous aider dans diverses tâches de débogage, telles que le suivi et les instructions d’assertion de code.

Avant d’essayer de déboguer les pages affichées dans Internet Explorer, vous devez effectuer quelques étapes de base pour activer le débogage. Jetons un coup d’œil à certaines exigences de configuration de base qui doivent être effectuées pour commencer.

## <a name="configuring-internet-explorer-for-debugging"></a>Configuration d’Internet Explorer pour le débogage

La plupart des gens ne s’intéressent pas à la détection de problèmes JavaScript sur un site Web affiché avec Internet Explorer. En fait, l’utilisateur moyen ne sait même pas quoi faire s’il a vu un message d’erreur. Par conséquent, les options de débogage sont désactivées par défaut dans le navigateur. Toutefois, il est très simple de désactiver le débogage et de le mettre en service à l’utilisation lorsque vous développez de nouvelles applications AJAX.

Pour activer la fonctionnalité de débogage, accédez à outils options Internet dans le menu Internet Explorer, puis sélectionnez l’onglet Avancé. Dans la section navigation, vérifiez que les éléments suivants sont désactivés :

- Désactiver le débogage de script (Internet Explorer)
- Désactiver le débogage de script (autre)

Bien que cela ne soit pas obligatoire, si vous essayez de déboguer une application, vous souhaiterez probablement que toutes les erreurs JavaScript de la page soient immédiatement visibles et évidentes. Vous pouvez forcer l’affichage de toutes les erreurs avec une boîte de message en activant la case à cocher « Afficher une notification sur chaque erreur de script ». Bien qu’il s’agit d’une option intéressante pour activer le développement d’une application, cela peut rapidement devenir ennuyeux si vous utilisez d’autres sites Web, car vos chances de rencontrer des erreurs JavaScript sont assez bonnes.

La figure 1 montre ce que la boîte de dialogue avancée d’Internet Explorer doit afficher une fois qu’elle a été correctement configurée pour le débogage.

[![la configuration d’Internet Explorer pour le débogage.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figure 1**: configuration d’Internet Explorer pour le débogage.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))

Une fois que le débogage a été activé, vous verrez un nouvel élément de menu s’afficher dans le menu affichage nommé débogueur de script. Deux options sont disponibles : ouvrir et arrêter à la prochaine instruction. Lorsque l’option ouvrir est sélectionnée, vous êtes invité à déboguer la page dans Visual Studio 2008 (Notez que Visual Web Developer Express peut également être utilisé pour le débogage). Si Visual Studio .NET est en cours d’exécution, vous pouvez choisir d’utiliser cette instance ou de créer une nouvelle instance. Lorsque l’option arrêter à la prochaine instruction est sélectionnée, vous êtes invité à déboguer la page lors de l’exécution du code JavaScript. Si le code JavaScript s’exécute dans l’événement onLoad de la page, vous pouvez actualiser la page pour déclencher une session de débogage. Si du code JavaScript est exécuté après un clic sur un bouton, le débogueur s’exécute immédiatement après un clic sur le bouton.

> [!NOTE]
> Si vous exécutez Windows Vista avec l’Access Control utilisateur (UAC) activée, et que Visual Studio 2008 est configuré pour s’exécuter en tant qu’administrateur, Visual Studio ne parvient pas à s’attacher au processus lorsque vous êtes invité à l’attacher. Pour contourner ce problème, démarrez d’abord Visual Studio, puis utilisez cette instance pour déboguer.

Bien que la section suivante montre comment déboguer une page AJAX ASP.NET directement à partir de Visual Studio 2008, l’utilisation de l’option de débogueur de script d’Internet Explorer est utile quand une page est déjà ouverte et que vous souhaitez l’examiner plus en détail.

## <a name="debugging-with-visual-studio-2008"></a>Débogage avec Visual Studio 2008

Visual Studio 2008 fournit des fonctionnalités de débogage que les développeurs du monde entier utilisent quotidiennement pour déboguer des applications .NET. Le débogueur intégré vous permet d’effectuer un pas à pas détaillé dans le code, d’afficher les données d’objet, d’observer des variables spécifiques, de surveiller la pile des appels plus bien plus encore. En plus de déboguer des VB.NET C# ou du code, le débogueur est également utile pour déboguer des applications ASP.NET AJAX et vous permet d’effectuer un pas à pas détaillé dans le code JavaScript ligne par ligne. Les détails qui suivent se focalisent sur les techniques qui peuvent être utilisées pour déboguer des fichiers de script côté client plutôt que de fournir un processus de débogage sur le processus global de débogage d’applications à l’aide de Visual Studio 2008.

Le processus de débogage d’une page dans Visual Studio 2008 peut être démarré de plusieurs façons différentes. Tout d’abord, vous pouvez utiliser l’option du débogueur de script d’Internet Explorer mentionnée dans la section précédente. Cela fonctionne bien quand une page est déjà chargée dans le navigateur et que vous souhaitez commencer à le déboguer. Vous pouvez également cliquer avec le bouton droit sur une page. aspx dans le Explorateur de solutions et sélectionner définir comme page de démarrage dans le menu. Si vous êtes habitué à déboguer les pages ASP.NET, vous avez probablement fait cela auparavant. Une fois que vous avez appuyé sur la touche F5, vous pouvez déboguer la page. Toutefois, bien que vous puissiez généralement définir un point d’arrêt là où vous voulez C# dans VB.net ou dans du code, ce n’est pas toujours le cas avec JavaScript comme vous le verrez ensuite.

*Scripts incorporés et externes*

Le débogueur Visual Studio 2008 traite JavaScript Embedded dans une page différente des fichiers JavaScript externes. Avec les fichiers de script externes, vous pouvez ouvrir le fichier et définir un point d’arrêt sur n’importe quelle ligne de votre choix. Vous pouvez définir des points d’arrêt en cliquant dans la zone grise située à gauche de la fenêtre de l’éditeur de code. Lorsque JavaScript est incorporé directement dans une page à l’aide de la balise `<script>`, la définition d’un point d’arrêt en cliquant dans la zone grisée n’est pas une option. Toute tentative de définition d’un point d’arrêt sur une ligne de script incorporé génère un avertissement indiquant qu’il ne s’agit pas d’un emplacement valide pour un point d’arrêt.

Vous pouvez contourner ce problème en déplaçant le code dans un fichier. js externe et en le référençant à l’aide de l’attribut SRC de la balise de&gt; de script &lt;:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

Que se passe-t-il si le déplacement du code dans un fichier externe n’est pas une option ou nécessite plus de travail que nécessaire ? Bien que vous ne puissiez pas définir un point d’arrêt à l’aide de l’éditeur, vous pouvez ajouter l’instruction du débogueur directement dans le code où vous souhaitez démarrer le débogage. Vous pouvez également utiliser la classe Sys. Debug disponible dans la bibliothèque AJAX ASP.NET pour forcer le débogage à démarrer. Vous en apprendrez davantage sur la classe Sys. Debug plus loin dans cet article.

Un exemple d’utilisation du mot clé `debugger` est présenté dans la liste 1. Cet exemple force le débogueur à s’arrêter juste avant qu’un appel à une fonction de mise à jour soit effectué.

**Liste 1. Utilisation du mot clé Debugger pour forcer l’arrêt du débogueur Visual Studio .NET.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Une fois l’instruction du débogueur atteinte, vous êtes invité à déboguer la page à l’aide de Visual Studio .NET et vous pouvez commencer à exécuter le code pas à pas. En procédant ainsi, vous pouvez rencontrer un problème d’accès aux fichiers de script de la bibliothèque AJAX ASP.NET utilisés dans la page. nous allons donc examiner l’utilisation de Visual Studio. Explorateur de scripts du réseau.

## <a name="using-visual-studio-net-windows-to-debug"></a>Utilisation de Windows Visual Studio .NET pour déboguer

Une fois qu’une session de débogage a démarré et que vous commencez à parcourir le code à l’aide de la clé F11 par défaut, vous pouvez rencontrer la boîte de dialogue d’erreur illustrée dans la figure 2, sauf si tous les fichiers de script utilisés dans la page sont ouverts et disponibles pour le débogage.

[![boîte de dialogue d’erreur qui s’affiche quand aucun code source n’est disponible pour le débogage.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figure 2**: boîte de dialogue d’erreur qui s’affiche quand aucun code source n’est disponible pour le débogage.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))

Cette boîte de dialogue s’affiche, car Visual Studio .NET ne vous permet pas de savoir comment accéder au code source de certains scripts référencés par la page. Bien que cela puisse être très frustrant au début, il existe un correctif simple. Une fois que vous avez démarré une session de débogage et atteint un point d’arrêt, accédez à la fenêtre de débogage de l’Explorateur de scripts Windows dans le menu de Visual Studio 2008 ou utilisez la touche d’accès rapide Ctrl + Alt + N.

> [!NOTE]
> Si le menu Explorateur de scripts ne s’affiche pas, accédez à **outils** > **personnaliser** > **commandes** dans le menu .net de Visual Studio. Recherchez l’entrée de **débogage** dans la section catégories, puis cliquez dessus pour afficher toutes les entrées de menu disponibles. Dans la liste commandes, faites défiler vers le haut jusqu’à Explorateur de scripts, puis faites-le glisser vers le menu fenêtres de débogage dans mentionné précédemment. Cela rendra l’entrée de menu de l’Explorateur de scripts disponible chaque fois que vous exécutez Visual Studio .NET.

L’Explorateur de scripts peut être utilisé pour afficher tous les scripts utilisés dans une page et les ouvrir dans l’éditeur de code. Une fois l’Explorateur de scripts ouvert, double-cliquez sur la page. aspx en cours de débogage pour l’ouvrir dans la fenêtre de l’éditeur de code. Effectuez la même action pour tous les autres scripts affichés dans l’Explorateur de scripts. Une fois que tous les scripts sont ouverts dans la fenêtre de code, vous pouvez appuyer sur F11 (et utiliser les autres raccourcis de débogage) pour parcourir votre code. La figure 3 montre un exemple de l’Explorateur de scripts. Il répertorie le fichier actuel en cours de débogage (Demo. aspx), ainsi que deux scripts personnalisés et deux scripts injectés dynamiquement dans la page par ASP.NET AJAX ScriptManager.

[![l’Explorateur de scripts permet d’accéder facilement aux scripts utilisés dans une page.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figure 3**. L’Explorateur de scripts permet d’accéder facilement aux scripts utilisés dans une page.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))

Plusieurs autres fenêtres peuvent également servir à fournir des informations utiles au fur et à mesure que vous parcourez le code dans une page. Par exemple, vous pouvez utiliser la fenêtre variables locales pour voir les valeurs des différentes variables utilisées dans la page, la fenêtre exécution pour évaluer des variables ou conditions spécifiques et afficher la sortie. Vous pouvez également utiliser la fenêtre sortie pour afficher les instructions de trace écrites à l’aide de la fonction sys. Debug. trace (qui sera traitée plus loin dans cet article) ou la fonction Debug. writeln d’Internet Explorer.

Au fur et à mesure que vous parcourez le code à l’aide du débogueur, vous pouvez faire défiler des variables dans le code pour afficher la valeur qui leur est assignée. Toutefois, le débogueur de script n’affiche parfois rien quand vous placez le pointeur de la souris sur une variable JavaScript donnée. Pour afficher la valeur, mettez en surbrillance l’instruction ou la variable que vous essayez d’afficher dans la fenêtre de l’éditeur de code, puis placez le pointeur de la souris dessus. Bien que cette technique ne fonctionne pas dans toutes les situations, il peut arriver que vous puissiez voir la valeur sans avoir à regarder dans une fenêtre de débogage différente telle que la fenêtre variables locales.

Vous pouvez consulter un didacticiel vidéo décrivant certaines des fonctionnalités abordées dans [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Débogage à l’aide du programme d’assistance pour le développement Web

Bien que Visual Studio 2008 (et Visual Web Developer Express 2008) soient des outils de débogage très efficaces, il existe des options supplémentaires qui peuvent également être utilisées plus légères. L’un des outils les plus récents à publier est l’application auxiliaire pour le développement Web. Le Nikhil Kothari de Microsoft (l’un des principaux architectes ASP.NET AJAX chez Microsoft) a écrit cet excellent outil qui peut effectuer de nombreuses tâches différentes à partir du débogage simple pour afficher les messages de requête et de réponse HTTP. L’application auxiliaire de développement Web peut être téléchargée sur [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

L’application auxiliaire de développement Web peut être utilisée directement dans Internet Explorer, ce qui facilite son utilisation. Pour commencer, sélectionnez Outils application d’assistance pour le développement Web dans le menu Internet Explorer. L’outil s’ouvre dans la partie inférieure du navigateur, ce qui est intéressant, car vous n’avez pas besoin de quitter le navigateur pour effectuer plusieurs tâches telles que la journalisation des messages de requête et de réponse HTTP. La figure 4 montre l’application d’aide au développement Web en action.

[Application d’assistance pour le développement Web ![](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figure 4**: application d’assistance pour le développement Web ([cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))

Le programme d’assistance du développement Web n’est pas un outil que vous allez utiliser pour parcourir le code ligne par ligne, comme dans Visual Studio 2008. Toutefois, il peut être utilisé pour afficher la sortie de trace, évaluer facilement des variables dans un script ou explorer des données à l’intérieur d’un objet JSON. Elle est également très utile pour l’affichage des données qui sont passées vers et à partir d’une page AJAX ASP.NET et d’un serveur.

Une fois que l’application auxiliaire de développement Web est ouverte dans Internet Explorer, le débogage de script doit être activé en sélectionnant script activer le débogage de script à partir du menu d’assistance du développement Web, comme indiqué plus haut dans la figure 4. Cela permet à l’outil d’intercepter les erreurs qui se produisent lors de l’exécution d’une page. Il permet également d’accéder facilement aux messages de trace qui sont générés dans la page. Pour afficher les informations de trace ou exécuter des commandes de script pour tester différentes fonctions au sein d’une même page, sélectionnez script afficher la console de script dans le menu d’assistance du développement Web. Cela permet d’accéder à une fenêtre de commande et à une fenêtre immédiate simple.

*Affichage des messages de trace et des données d’objets JSON*

La fenêtre exécution peut être utilisée pour exécuter des commandes de script ou même charger ou enregistrer des scripts utilisés pour tester différentes fonctions dans une page. La fenêtre commande affiche les messages de trace ou de débogage écrits par la page affichée. La liste 2 montre comment écrire un message de trace à l’aide de la fonction Debug. writeln d’Internet Explorer.

**Liste 2. Écriture d’un message de trace côté client à l’aide de la classe Debug.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Si la propriété LastName contient la valeur Doe, le programme d’assistance du développement Web affiche le message « Person Name : DOE » dans la fenêtre de commande de la console de script (en supposant que le débogage a été activé). L’application auxiliaire de développement Web ajoute également un objet debugService de niveau supérieur dans des pages qui peuvent être utilisées pour écrire des informations de trace ou afficher le contenu d’objets JSON. La liste 3 montre un exemple d’utilisation de la fonction de trace de la classe debugService.

**Liste 3. Utilisation de la classe debugService de l’application d’assistance pour le développement Web pour écrire un message de trace.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Une fonctionnalité intéressante de la classe debugService est qu’elle fonctionne même si le débogage n’est pas activé dans Internet Explorer, ce qui facilite l’accès aux données de trace lorsque l’application auxiliaire du développement Web est en cours d’exécution. Lorsque l’outil n’est pas utilisé pour déboguer une page, les instructions de trace sont ignorées, car l’appel à Window. debugService retourne la valeur false.

La classe debugService autorise également l’affichage des données d’objet JSON à l’aide de la fenêtre d’inspecteur du programme d’aide au développement Web. La liste 4 crée un objet JSON simple contenant des données Person. Une fois l’objet créé, un appel est effectué à la fonction d’inspection de la classe debugService pour permettre l’inspection visuelle de l’objet JSON.

**Liste 4. Utilisation de la fonction debugService. Inspect pour afficher les données d’objet JSON.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

L’appel de la fonction GetPerson () dans la page ou dans la fenêtre exécution entraînera l’affichage de la fenêtre de boîte de dialogue de l’inspecteur d’objet, comme illustré à la figure 5. Les propriétés de l’objet peuvent être modifiées dynamiquement en les mettant en surbrillance, en modifiant la valeur affichée dans la zone de texte valeur, puis en cliquant sur le lien mettre à jour. L’utilisation de l’inspecteur d’objet facilite l’affichage des données d’objets JSON et l’expérimentation de différentes valeurs pour les propriétés.

*Erreurs de débogage*

En plus de permettre l’affichage des données de trace et des objets JSON, l’application auxiliaire de développement Web peut également faciliter le débogage des erreurs dans une page. Si une erreur se produit, vous êtes invité à passer à la ligne de code suivante ou à déboguer le script (voir figure 6). La fenêtre d’erreur de script affiche la pile des appels complète, ainsi que les numéros de ligne pour vous permettre d’identifier facilement où se trouvent les problèmes dans un script.

[![à l’aide de la fenêtre de l’inspecteur d’objets pour afficher un objet JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figure 5**: utilisation de la fenêtre de l’inspecteur d’objet pour afficher un objet JSON.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))

La sélection de l’option de débogage vous permet d’exécuter des instructions de script directement dans la fenêtre exécution de l’application d’assistance de développement Web pour afficher la valeur des variables, écrire des objets JSON, ainsi que d’autres informations. Si la même action qui a déclenché l’erreur est réexécutée et que Visual Studio 2008 est disponible sur l’ordinateur, vous serez invité à démarrer une session de débogage pour pouvoir parcourir le code ligne par ligne, comme indiqué dans la section précédente.

[![boîte de dialogue d’erreur de script du programme d’assistance du développement Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figure 6**: boîte de dialogue d’erreur de script du programme d’assistance pour le développement Web ([cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))

*Inspection des messages de demande et de réponse*

Lors du débogage des pages AJAX ASP.NET, il est souvent utile de consulter les messages de demande et de réponse envoyés entre une page et un serveur. L’affichage du contenu dans des messages vous permet de voir si les données appropriées sont transmises, ainsi que la taille des messages. L’application auxiliaire de développement Web fournit une excellente fonctionnalité d’enregistrement des messages HTTP qui facilite l’affichage des données sous forme de texte brut ou dans un format plus lisible.

Pour afficher les messages de demande et de réponse AJAX ASP.NET, l’enregistreur d’événements HTTP doit être activé en sélectionnant HTTP activer la journalisation HTTP dans le menu d’assistance du développement Web. Une fois activé, tous les messages envoyés à partir de la page active peuvent être affichés dans la visionneuse du journal HTTP, accessible en sélectionnant HTTP Show HTTP logs.

Bien que l’affichage du texte brut envoyé dans chaque message de demande/réponse soit certainement utile (et une option dans l’application auxiliaire de développement Web), il est souvent plus facile d’afficher les données de message dans un format plus graphique. Une fois la journalisation HTTP activée et les messages enregistrés, les données des messages peuvent être consultées en double-cliquant sur le message dans la visionneuse du journal HTTP. Cela vous permet d’afficher tous les en-têtes associés à un message, ainsi que le contenu réel du message. La figure 7 montre un exemple de message de demande et de réponse affiché dans la fenêtre de la visionneuse du journal HTTP.

[![à l’aide de la visionneuse du journal HTTP pour afficher les données des messages de demande et de réponse.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figure 7**: utilisation de la visionneuse du journal http pour afficher les données des messages de demande et de réponse.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))

La visionneuse du journal HTTP analyse automatiquement les objets JSON et les affiche à l’aide d’une arborescence, ce qui permet d’afficher rapidement et facilement les données de propriété de l’objet. Quand un UpdatePanel est utilisé dans une page AJAX ASP.NET, la visionneuse décompose chaque partie du message dans des parties individuelles, comme illustré à la figure 8. Il s’agit d’une fonctionnalité intéressante qui facilite grandement la visualisation et la compréhension du contenu du message par rapport à l’affichage des données brutes du message.

[![un message de réponse UpdatePanel affiché à l’aide de la visionneuse du journal HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figure 8**: message de réponse UpdatePanel affiché à l’aide de la visionneuse du journal http.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))

Il existe plusieurs autres outils qui peuvent être utilisés pour afficher les messages de demande et de réponse en plus de l’application auxiliaire de développement Web. Une autre option intéressante est Fiddler, disponible gratuitement sur [http://www.fiddlertool.com](http://www.fiddlertool.com). Bien que Fiddler ne soit pas abordé ici, il s’agit également d’une bonne option lorsque vous devez examiner minutieusement les en-têtes et les données des messages.

## <a name="debugging-with-firefox-and-firebug"></a>Débogage avec Firefox et Firebug

Internet Explorer est toujours le navigateur le plus couramment utilisé, tandis que d’autres navigateurs tels que Firefox sont devenus très populaires et sont utilisés de plus en plus. Par conséquent, vous souhaiterez afficher et déboguer vos pages ASP.NET AJAX dans Firefox et Internet Explorer pour vous assurer que vos applications fonctionnent correctement. Même si Firefox ne peut pas être directement lié à Visual Studio 2008 pour le débogage, il possède une extension appelée Firebug qui peut être utilisée pour déboguer des pages. Vous pouvez télécharger gratuitement le Firebug en accédant à [http://www.getfirebug.com](http://www.getfirebug.com).

Firebug fournit un environnement de débogage complet qui peut être utilisé pour parcourir le code ligne par ligne, accéder à tous les scripts utilisés dans une page, afficher les structures DOM, afficher les styles CSS et même suivre les événements qui se produisent dans une page. Une fois installé, vous pouvez accéder à Firebug en sélectionnant Outils Firebug ouvrir Firebug dans le menu Firefox. À l’instar du développement Web, Firebug est utilisé directement dans le navigateur, bien qu’il puisse également être utilisé en tant qu’application autonome.

Une fois Firebug en cours d’exécution, les points d’arrêt peuvent être définis sur n’importe quelle ligne d’un fichier JavaScript, que le script soit incorporé dans une page ou non. Pour définir un point d’arrêt, commencez par charger la page appropriée que vous souhaitez déboguer dans Firefox. Une fois la page chargée, sélectionnez le script à déboguer dans la liste déroulante scripts de Firebug. Tous les scripts utilisés par la page s’affichent. Un point d’arrêt est défini en cliquant dans la zone grise du plateau de l’emplacement de la ligne où le point d’arrêt doit être le même que dans Visual Studio 2008.

Une fois qu’un point d’arrêt a été défini dans Firebug, vous pouvez effectuer l’action requise pour exécuter le script qui doit être débogué, par exemple en cliquant sur un bouton ou en actualisant le navigateur pour déclencher l’événement onLoad. L’exécution s’arrêtera automatiquement sur la ligne contenant le point d’arrêt. La figure 9 illustre un exemple de point d’arrêt qui a été déclenché dans Firebug.

[![la gestion des points d’arrêt dans Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figure 9**: gestion des points d’arrêt dans Firebug.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))

Une fois qu’un point d’arrêt est atteint, vous pouvez effectuer un pas à pas détaillé ou un pas à pas sortant du code à l’aide des boutons fléchés. Au fur et à mesure que vous parcourez le code, les variables de script s’affichent dans la partie droite du débogueur, ce qui vous permet de voir les valeurs et d’accéder aux objets. Firebug comprend également une liste déroulante pile des appels pour afficher les étapes d’exécution du script qui ont conduit à la ligne en cours de débogage.

Firebug comprend également une fenêtre de console qui peut être utilisée pour tester différentes instructions de script, évaluer des variables et afficher la sortie de trace. Vous y accédez en cliquant sur l’onglet de la console en haut de la fenêtre Firebug. La page en cours de débogage peut également être « inspectée » pour afficher sa structure et son contenu DOM en cliquant sur l’onglet inspecter. Lorsque vous placez le pointeur de la souris sur les différents éléments DOM affichés dans la fenêtre de l’inspecteur, la partie appropriée de la page est mise en surbrillance, ce qui vous permet de voir facilement où l’élément est utilisé dans la page. Les valeurs d’attribut associées à un élément donné peuvent être modifiées « Live » pour expérimenter l’application de différentes largeurs, styles, etc. à un élément. Il s’agit d’une fonctionnalité intéressante qui vous évite d’avoir à basculer constamment entre l’éditeur de code source et le navigateur Firefox pour voir comment les modifications simples affectent une page.

La figure 10 illustre un exemple d’utilisation de l’Inspecteur DOM pour localiser une zone de texte nommée txtCountry dans la page. L’inspecteur de Firebug peut également être utilisé pour afficher les styles CSS utilisés dans une page, ainsi que les événements qui se produisent, tels que le suivi des mouvements de souris, des clics de bouton, et plus encore.

[![à l’aide de l’Inspecteur DOM de Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figure 10**: utilisation de l’Inspecteur DOM de Firebug.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))

Firebug offre une méthode légère pour déboguer rapidement une page directement dans Firefox, ainsi qu’un excellent outil pour inspecter les différents éléments de la page.

## <a name="debugging-support-in-aspnet-ajax"></a>Prise en charge du débogage dans ASP.NET AJAX

La bibliothèque AJAX ASP.NET comprend de nombreuses classes différentes qui peuvent être utilisées pour simplifier le processus d’ajout de fonctionnalités AJAX dans une page Web. Vous pouvez utiliser ces classes pour localiser des éléments dans une page et les manipuler, ajouter de nouveaux contrôles, appeler des services Web et même gérer des événements. La bibliothèque ASP.NET AJAX contient également des classes qui peuvent être utilisées pour améliorer le processus de débogage des pages. Dans cette section, vous allez découvrir la classe Sys. Debug et voir comment elle peut être utilisée dans les applications.

*Utilisation de la classe Sys. Debug*

La classe Sys. Debug (une classe JavaScript située dans l’espace de noms sys) peut être utilisée pour exécuter plusieurs fonctions différentes, notamment l’écriture de la sortie de trace, l’exécution d’assertions de code et le forçage du code pour qu’elle puisse être déboguée. Il est largement utilisé dans les fichiers de débogage de la bibliothèque AJAX ASP.NET (installés à l’emplacement C:\Program Files\Microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 par défaut) pour effectuer des tests conditionnels ( appelés assertions) qui garantissent que les paramètres sont passés correctement aux fonctions, que les objets contiennent les données attendues et écrivent des instructions de trace.

La classe Sys. Debug expose plusieurs fonctions différentes qui peuvent être utilisées pour gérer le suivi, les assertions de code ou les échecs, comme indiqué dans le tableau 1.

**Tableau 1. Fonctions de la classe Sys. Debug.**

| **Nom de la fonction** | **Description** |
| --- | --- |
| assert(condition, message, displayCaller) | Déclare que le paramètre condition a la valeur true. Si la condition testée a la valeur false, une boîte de message est utilisée pour afficher la valeur du paramètre de message. Si le paramètre displayCaller a la valeur true, la méthode affiche également des informations sur l’appelant. |
| clearTrace() | Efface les instructions de sortie des opérations de suivi. |
| échec (message) | Provoque l’arrêt de l’exécution du programme et l’arrêt du débogueur. Le paramètre de message peut être utilisé pour indiquer la raison de l’échec. |
| trace (message) | Écrit le paramètre de message dans la sortie de trace. |
| traceDump(object, name) | Renvoie les données d’un objet dans un format lisible. Le paramètre Name peut être utilisé pour fournir une étiquette pour le vidage de trace. Tous les sous-objets dans l’objet qui est vidé sont écrits par défaut. |

Le suivi côté client peut être utilisé à peu près de la même façon que les fonctionnalités de suivi disponibles dans ASP.NET. Il permet d’afficher facilement différents messages sans interrompre le déroulement de l’application. La liste 5 illustre un exemple d’utilisation de la fonction sys. Debug. trace pour écrire dans le journal des traces. Cette fonction prend simplement le message qui doit être écrit en tant que paramètre.

**Liste 5. Utilisation de la fonction sys. Debug. Trace.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Si vous exécutez le code affiché dans la liste 5, vous ne verrez pas de sortie de trace dans la page. La seule façon de le voir est d’utiliser une fenêtre de console disponible dans Visual Studio .NET, l’application d’assistance pour le développement Web ou le Firebug. Si vous souhaitez voir la sortie de suivi dans la page, vous devez ajouter une balise TextArea et lui attribuer un ID TraceConsole comme indiqué ci-dessous :

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Toutes les instructions sys. Debug. trace de la page seront écrites dans le TextArea TraceConsole.

Dans les cas où vous souhaitez afficher les données contenues dans un objet JSON, vous pouvez utiliser la fonction traceDump de la classe Sys. Debug. Cette fonction accepte deux paramètres, y compris l’objet qui doit être vidé dans la console de trace et un nom qui peut être utilisé pour identifier l’objet dans la sortie de trace. La liste 6 illustre un exemple d’utilisation de la fonction traceDump.

**Liste 6. À l’aide de la fonction sys. Debug. traceDump.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

La figure 11 illustre la sortie de l’appel de la fonction sys. Debug. traceDump. Notez qu’en plus d’écrire les données de l’objet Person, il écrit également les données de l’objet Address.

Outre le suivi, la classe Sys. Debug peut également être utilisée pour effectuer des assertions de code. Les assertions sont utilisées pour vérifier que des conditions spécifiques sont respectées lorsqu’une application est en cours d’exécution. La version de débogage des scripts de la bibliothèque AJAX ASP.NET contient plusieurs instructions Assert pour tester diverses conditions.

La liste 7 montre un exemple d’utilisation de la fonction sys. Debug. Assert pour tester une condition. Le code teste si l’objet Address est null avant de mettre à jour un objet Person.

[![sortie de la fonction sys. Debug. traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figure 11**: sortie de la fonction sys. Debug. traceDump.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))

**Liste 7. À l’aide de la fonction Debug. Assert.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Trois paramètres sont passés, y compris la condition à évaluer, le message à afficher si l’assertion retourne la valeur false et si les informations sur l’appelant doivent être affichées ou non. Dans les cas où une assertion échoue, le message s’affiche, ainsi que les informations de l’appelant si le troisième paramètre a la valeur true. La figure 12 illustre un exemple de la boîte de dialogue d’échec qui s’affiche si l’assertion indiquée dans la liste 7 échoue.

La dernière fonction à couvrir est sys. Debug. Fail. Lorsque vous souhaitez forcer le code à échouer sur une ligne particulière dans un script, vous pouvez ajouter un appel sys. Debug. Fail plutôt que l’instruction du débogueur généralement utilisée dans les applications JavaScript. La fonction sys. Debug. Fail accepte un paramètre de chaîne unique qui représente la raison de l’échec, comme indiqué ci-dessous :

[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]

[![un message d’échec sys. Debug. Assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figure 12**: message d’échec sys. Debug. Assert.  ([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))

Quand une instruction sys. Debug. Fail est rencontrée pendant l’exécution d’un script, la valeur du paramètre de message est affichée dans la console d’une application de débogage telle que Visual Studio 2008 et vous êtes invité à déboguer l’application. Cela peut être très utile dans le cas où vous ne pouvez pas définir un point d’arrêt avec Visual Studio 2008 sur un script inline, mais que vous souhaitez que le code s’arrête sur une ligne particulière afin que vous puissiez inspecter la valeur des variables.

*Fonctionnement de la propriété ScriptMode du contrôle ScriptManager*

La bibliothèque ASP.NET AJAX comprend des versions de script Debug et Release qui sont installées dans C:\Program Files\Microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 par défaut. Les scripts de débogage sont correctement mis en forme, faciles à lire et plusieurs appels sys. Debug. Assert sont éparpillés, tandis que les scripts de version comportent des espaces blancs supprimés et utilisent la classe Sys. Debug avec modération pour réduire leur taille globale.

Le contrôle ScriptManager ajouté aux pages AJAX ASP.NET lit l’attribut de débogage de l’élément de compilation dans Web. config pour déterminer les versions des scripts de bibliothèque à charger. Toutefois, vous pouvez contrôler si les scripts Debug ou Release sont chargés (scripts de bibliothèque ou vos propres scripts personnalisés) en modifiant la propriété ScriptMode. ScriptMode accepte une énumération ScriptMode dont les membres incluent auto, Debug, Release et Inherit.

ScriptMode utilise par défaut la valeur auto, ce qui signifie que ScriptManager vérifie l’attribut de débogage dans Web. config. Lorsque debug a la valeur false, ScriptManager charge la version Release des scripts de la bibliothèque AJAX ASP.NET. Lorsque debug a la valeur true, la version Debug des scripts sera chargée. La modification de la propriété ScriptMode en Release ou debug force le ScriptManager à charger les scripts appropriés, quelle que soit la valeur de l’attribut debug dans Web. config. La liste 8 illustre un exemple d’utilisation du contrôle ScriptManager pour charger des scripts de débogage à partir de la bibliothèque AJAX ASP.NET.

**Liste 8. Chargement des scripts de débogage à l’aide de ScriptManager**.

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Vous pouvez également charger différentes versions (Debug ou Release) de vos propres scripts personnalisés en utilisant la propriété scripts de ScriptManager avec le composant ScriptReference, comme indiqué dans la liste 9.

**Liste 9. Chargement de scripts personnalisés à l’aide de ScriptManager.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Si vous chargez des scripts personnalisés à l’aide du composant ScriptReference, vous devez notifier le ScriptManager lorsque le chargement du script est terminé en ajoutant le code suivant au bas du script :

[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

Le code présenté dans la liste 9 indique à ScriptManager de rechercher une version de débogage du script person pour qu’il recherche automatiquement Person. Debug. js au lieu de Person. js. Si le fichier Person. Debug. js est introuvable, une erreur est générée.

Dans les cas où vous souhaitez qu’une version Debug ou Release d’un script personnalisé soit chargée en fonction de la valeur de la propriété ScriptMode définie sur le contrôle ScriptManager, vous pouvez définir la propriété ScriptMode du contrôle ScriptReference sur inherit. Cela entraîne le chargement de la version appropriée du script personnalisé en fonction de la propriété ScriptMode de ScriptManager, comme indiqué dans la liste 10. Étant donné que la propriété ScriptMode du contrôle ScriptManager est définie sur Debug, le script Person. Debug. js est chargé et utilisé dans la page.

**Liste 10. Héritage de ScriptMode du ScriptManager pour les scripts personnalisés.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

En utilisant la propriété ScriptMode de manière appropriée, vous pouvez déboguer plus facilement les applications et simplifier le processus global. Les scripts de version de la bibliothèque AJAX ASP.NET sont plutôt difficiles à parcourir et à lire, car la mise en forme du code a été supprimée, tandis que les scripts de débogage sont mis en forme spécifiquement à des fins de débogage.

## <a name="conclusion"></a>Conclusion

La technologie ASP.NET AJAX de Microsoft fournit une base solide pour créer des applications compatibles AJAX qui peuvent améliorer l’expérience globale de l’utilisateur final. Toutefois, comme pour toute technologie de programmation, les bogues et autres problèmes liés aux applications se produisent certainement. En connaissant les différentes options de débogage disponibles, vous pouvez gagner beaucoup de temps et générer un produit plus stable.

Dans cet article, vous avez introduit plusieurs techniques différentes pour déboguer des pages ASP.NET AJAX, notamment Internet Explorer avec Visual Studio 2008, le programme d’assistance pour le développement Web et Firebug. Ces outils peuvent simplifier le processus global de débogage dans la mesure où vous pouvez accéder aux données variables, parcourir le code ligne par ligne et afficher les instructions de trace. Outre les différents outils de débogage décrits, vous avez également vu comment la classe Sys. Debug de la bibliothèque AJAX ASP.NET peut être utilisée dans une application et comment la classe ScriptManager peut être utilisée pour charger des versions Debug ou Release de scripts.

## <a name="bio"></a>Équivalence

Dan Wahlin (professionnel Microsoft le plus précieux pour ASP.NET et les services Web XML) est un formateur de développement .NET et un consultant en architecture à la formation technique d’interface ([www.interfacett.com)](http://www.interfacett.com). Dan a créé le site Web XML for ASP.NET Developers ([www.XMLforASP.net](http://www.XMLforASP.NET)), se trouve sur le Bureau du conférencier INETA et participe à plusieurs conférences. Dan co-auteur professionnel Windows DNA (Wrox), ASP.NET : conseils, didacticiels et code (Sams), ASP.NET 1,1 Insider solutions, professionnel ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 hackers MVP et auteur XML for ASP.NET Developers (Sams). Lorsqu’il n’écrit pas de code, d’articles ou de livres, Dan aime écrire et enregistrer de la musique et écouter du golf et du basket avec sa femme et ses enfants.

Scott Cate travaille avec les technologies Web de Microsoft depuis 1997 et est le Président de myKB.com ([www.myKB.com](http://www.myKB.com)), où il se spécialise dans l’écriture d’applications ASP.net axées sur les solutions logicielles de la base de connaissances. Scott peut être contacté par e-mail à [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-asp-net-ajax-web-services.md)
