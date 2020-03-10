---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Présentation du débogage des sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Le débogage est le processus de recherche et de correction des erreurs dans vos pages de codes. Ce chapitre présente des outils et des techniques que vous pouvez utiliser pour déboguer et analyser...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: ae7d871e56326610c043dc20fe6e0919e1b4ac89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624368"
---
# <a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Présentation du débogage de sites pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique les différentes façons de déboguer les pages d’un site Web pages Web ASP.NET (Razor). Le débogage est le processus de recherche et de correction des erreurs dans vos pages de codes.
>
> **Ce que vous allez apprendre :**
>
> - Comment afficher des informations qui permettent d’analyser et de déboguer des pages.
> - Comment utiliser les outils de débogage dans Visual Studio.
>
>
> Voici les fonctionnalités ASP.NET présentées dans l’article :
>
> - Le programme d’assistance `ServerInfo`.
> - `ObjectInfo` Helper.
>
>
> ## <a name="software-versions"></a>Versions de logiciels
>
>
> - Pages Web ASP.NET (Razor) 3
> - Visual Studio 2013
>
>
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2. Vous pouvez utiliser WebMatrix 3, mais le débogueur intégré n’est pas pris en charge.

Un aspect important de la résolution des erreurs et des problèmes dans votre code consiste à les éviter en premier lieu. Pour ce faire, vous pouvez placer des sections de votre code susceptibles de provoquer des erreurs dans `try/catch` blocs. Pour plus d’informations, consultez la section relative à la gestion des erreurs dans [Introduction à la programmation Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

Le `ServerInfo` Helper est un outil de diagnostic qui vous donne une vue d’ensemble des informations sur l’environnement de serveur Web qui héberge votre page. Il affiche également les informations de requête HTTP envoyées lorsqu’un navigateur demande la page. Le `ServerInfo` Helper affiche l’identité de l’utilisateur actuel, le type de navigateur qui a effectué la demande, etc. Ce type d’informations peut vous aider à résoudre les problèmes courants.

1. Créez une nouvelle page Web nommée *du. cshtml*.
2. À la fin de la page, juste avant la balise de fermeture `</body>`, ajoutez `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Vous pouvez ajouter le code `ServerInfo` n’importe où dans la page. Mais l’ajout à la fin conserve sa sortie séparément de l’autre contenu de la page, ce qui en facilite la lecture.

    > [!NOTE]
    >
    > **Important** Vous devez supprimer tout code de diagnostic de vos pages Web avant de déplacer des pages Web vers un serveur de production. Cela s’applique au programme d’assistance `ServerInfo`, ainsi qu’aux autres techniques de diagnostic dans cet article qui impliquent l’ajout de code à une page. Vous ne souhaitez pas que les visiteurs de votre site Web voient les informations sur le nom de votre serveur, les noms d’utilisateur, les chemins d’accès sur votre serveur et les détails similaires, car ce type d’informations peut être utile aux personnes ayant des intentions malveillantes.
3. Enregistrez la page et exécutez-la dans un navigateur.

    ![Débogage-1](introduction-to-debugging/_static/image1.jpg)

    Le programme d’assistance `ServerInfo` affiche quatre tables d’informations dans la page :

   - Configuration du serveur. Cette section fournit des informations sur le serveur Web d’hébergement, y compris le nom de l’ordinateur, la version de ASP.NET que vous exécutez, le nom de domaine et l’heure du serveur.
   - Variables de serveur ASP.NET. Cette section fournit des informations détaillées sur les nombreux détails du protocole HTTP (appelés variables HTTP) et les valeurs qui font partie de chaque requête de page Web.
   - Informations d’exécution HTTP. Cette section fournit des informations détaillées sur la version du Microsoft .NET Framework sous lequel votre page Web s’exécute, le chemin d’accès, les détails sur le cache, etc. (Comme vous l’avez appris dans [Introduction à la programmation web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890), pages Web ASP.net à l’aide de la syntaxe Razor reposent sur la technologie de serveur Web ASP.net de Microsoft, qui est elle-même basée sur une bibliothèque de développement de logiciels complète appelée le .NET Framework.)
   - Variables d’environnement. Cette section fournit la liste de toutes les variables d’environnement locales et leurs valeurs sur le serveur Web.

     La description complète de toutes les informations relatives au serveur et à la demande n’entre pas dans le cadre de cet article, mais vous pouvez voir que le `ServerInfo` Helper renvoie un grand nombre d’informations de diagnostic. Pour plus d’informations sur les valeurs retournées par `ServerInfo`, consultez [variables d’environnement reconnues](https://technet.microsoft.com/library/dd560744(WS.10).aspx) sur le site Web Microsoft TechNet et [variables de serveur IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) sur le site Web MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Incorporation d’expressions de sortie pour afficher les valeurs de page

Une autre façon de voir ce qui se passe dans votre code consiste à incorporer des expressions de sortie dans la page. Comme vous le savez, vous pouvez générer directement la valeur d’une variable en ajoutant des éléments comme `@myVariable` ou `@(subTotal * 12)` à la page. Pour le débogage, vous pouvez placer ces expressions de sortie à des points stratégiques de votre code. Cela vous permet de voir la valeur des variables clés ou le résultat des calculs lors de l’exécution de votre page. Lorsque vous avez terminé le débogage, vous pouvez supprimer les expressions ou les commenter. Cette procédure illustre un moyen classique d’utiliser des expressions incorporées pour faciliter le débogage d’une page.

1. Créez une nouvelle page WebMatrix nommée *OutputExpression. cshtml*.
2. Remplacez le contenu de la page par ce qui suit :

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    L’exemple utilise une instruction `switch` pour vérifier la valeur de la variable `weekday`, puis afficher un autre message de sortie en fonction du jour de la semaine. Dans l’exemple, le bloc `if` dans le premier bloc de code change arbitrairement le jour de la semaine en ajoutant un jour à la valeur actuelle du jour de la semaine. Il s’agit d’une erreur introduite à des fins d’illustration.
3. Enregistrez la page et exécutez-la dans un navigateur.

    La page affiche le message pour le mauvais jour de la semaine. Quel que soit le jour de la semaine, le message s’affiche pendant un jour plus tard. Même si, dans ce cas, vous savez pourquoi le message est désactivé (car le code définit délibérément la valeur de jour incorrecte), en réalité, il est souvent difficile de savoir où les problèmes se posent dans le code. Pour déboguer, vous devez savoir ce qui se passe à la valeur des objets clés et des variables telles que `weekday`.
4. Ajoutez des expressions de sortie en insérant `@weekday` comme indiqué dans les deux emplacements indiqués par les commentaires dans le code. Ces expressions de sortie affichent les valeurs de la variable à ce stade de l’exécution du code.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Enregistrez et exécutez la page dans un navigateur.

    La page affiche d’abord le jour réel de la semaine, puis le jour de la semaine mis à jour qui résulte de l’ajout d’une journée, puis le message résultant de l’instruction `switch`. La sortie des deux expressions variables (`@weekday`) n’a pas d’espace entre les jours, car vous n’avez pas ajouté de balises HTML `<p>` à la sortie ; les expressions sont uniquement destinées au test.

    ![Débogage-2](introduction-to-debugging/_static/image2.jpg)

    Vous pouvez maintenant voir l’origine de l’erreur. Lorsque vous affichez pour la première fois la variable `weekday` dans le code, elle affiche le jour correct. Lorsque vous l’affichez la deuxième fois, après le bloc `if` dans le code, le jour est décalé d’une unité. Vous savez qu’un événement s’est produit entre la première et la deuxième apparition de la variable de jour de la semaine. S’il s’agissait d’un véritable bogue, ce type d’approche vous permettra de limiter l’emplacement du code à l’origine du problème.
6. Corrigez le code dans la page en supprimant les deux expressions de sortie que vous avez ajoutées, et en supprimant le code qui modifie le jour de la semaine. Le bloc de code complet restant se présente comme dans l’exemple suivant :

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Exécutez la page dans un navigateur. Cette fois, vous voyez le message correct affiché pour le jour réel de la semaine.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Utilisation de l’application auxiliaire ObjectInfo pour afficher des valeurs d’objet

Le programme d’assistance `ObjectInfo` affiche le type et la valeur de chaque objet que vous lui transmettez. Vous pouvez l’utiliser pour afficher la valeur des variables et des objets dans votre code (comme vous l’avez fait avec les expressions de sortie dans l’exemple précédent), ainsi que des informations sur les types de données de l’objet.

1. Ouvrez le fichier nommé *OutputExpression. cshtml* que vous avez créé précédemment.
2. Remplacez tout le code de la page par le bloc de code suivant :

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Enregistrez et exécutez la page dans un navigateur.

    ![Débogage-4](introduction-to-debugging/_static/image3.jpg)

    Dans cet exemple, le programme d’assistance `ObjectInfo` affiche deux éléments :

   - Type. Pour la première variable, le type est `DayOfWeek`. Pour la deuxième variable, le type est `String`.
   - La valeur. Dans ce cas, étant donné que vous affichez déjà la valeur de la variable Greeting dans la page, la valeur s’affiche à nouveau lorsque vous transmettez la variable à `ObjectInfo`.

     Pour les objets plus complexes, le programme d’assistance `ObjectInfo` peut afficher &#8212; plus d’informations, mais il peut afficher les types et les valeurs de toutes les propriétés d’un objet.

## <a name="using-debugging-tools-in-visual-studio"></a>Utilisation des outils de débogage dans Visual Studio

Pour une expérience de débogage plus complète, utilisez [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Avec Visual Studio, vous pouvez définir un point d’arrêt dans votre code à la ligne que vous souhaitez inspecter.

![définir un point d’arrêt](introduction-to-debugging/_static/image1.png)

Lorsque vous testez le site Web, le code en cours d’exécution s’arrête au point d’arrêt.

![point d’arrêt atteint](introduction-to-debugging/_static/image2.png)

Vous pouvez examiner les valeurs actuelles des variables et parcourir le code ligne par ligne.

![Voir les valeurs](introduction-to-debugging/_static/image3.png)

Pour plus d’informations sur l’utilisation du débogueur intégré dans Visual Studio pour déboguer les pages Razor ASP.NET, consultez [programmation d’pages Web ASP.net (Razor) à l’aide de Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Pages Web ASP.NET de programmation (Razor) à l’aide de Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Variables de serveur IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Variables d’environnement reconnues](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
