---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Modification du code ASP.NET Web Forms dans Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 3473ad476fbbebc58e12586334b4600f57cf17ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632747"
---
# <a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Édition du code Web Forms ASP.NET dans Visual Studio 2013

par [Erik Reitan](https://github.com/Erikre)

Dans de nombreuses pages Web Forms ASP.NET, vous écrivez du code C#dans Visual Basic, ou dans un autre langage. L’éditeur de code de Visual Studio peut vous aider à écrire du code rapidement tout en vous aidant à éviter les erreurs. En outre, l’éditeur vous permet de créer du code réutilisable pour réduire la quantité de travail que vous devez effectuer.

Cette procédure pas à pas illustre différentes fonctionnalités de l’éditeur de code Visual Studio.

Pendant cette procédure pas à pas, vous allez apprendre à :

- Corriger les erreurs de codage en ligne.
- Refactoriser et renommer le code.
- Renommez les variables et les objets.
- Insérer des extraits de code.

## <a name="prerequisites"></a>Conditions préalables requises

Pour exécuter cette procédure pas à pas, vous avez besoin des éléments suivants :

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Le .NET Framework est installé automatiquement. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 et Microsoft Visual Studio Express 2013 pour le Web sont souvent appelés Visual Studio dans cette série de didacticiels.  
    >   
    > Si vous utilisez Visual Studio, cette procédure pas à pas suppose que vous avez sélectionné la collection de paramètres **développement Web** la première fois que vous avez démarré Visual Studio. Pour plus d’informations, consultez [Comment : sélectionner les paramètres de l’environnement de développement Web](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Création d’un projet d’application Web et d’une page

<a id="sectionToggle0"></a>

Dans cette partie de la procédure pas à pas, vous allez créer un projet d’application Web et lui ajouter une nouvelle page.

### <a name="to-create-a-web-application-project"></a>Pour créer un projet d’application Web

1. Ouvrez Microsoft Visual Studio.
2. Dans le menu **Fichier**, sélectionnez **Nouveau projet**.  
    ![menu fichier](code-editing-in-web-forms-pages/_static/image1.png)

    La boîte de dialogue **Nouveau projet** s’affiche.
3. Sélectionnez les **modèles** -&gt; le groupe **Visual C#**  -&gt; de modèles **Web** sur la gauche.
4. Choisissez le modèle **Application Web ASP.NET** dans la colonne centrale.
5. Nommez votre projet ***BasicWebApp*** et cliquez sur le bouton **OK** .   
![Boîte de dialogue Nouveau projet](code-editing-in-web-forms-pages/_static/image2.png)
6. Ensuite, sélectionnez le modèle **Web Forms** , puis cliquez sur le bouton **OK** pour créer le projet.  
![Boîte de dialogue New ASP.NET Project](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio crée un projet qui comprend des fonctionnalités prégénérées basées sur le modèle de Web Forms.

## <a name="creating-a-new-aspnet-web-forms-page"></a>Création d’une nouvelle page de Web Forms ASP.NET

Quand vous créez une application de Web Forms à l’aide du modèle de projet d' **application Web ASP.net** , Visual Studio ajoute une page ASP.net (page Web Forms) nommée *default. aspx*, ainsi que plusieurs autres fichiers et dossiers. Vous pouvez utiliser la page *default. aspx* comme page d’hébergement de votre application Web. Toutefois, pour cette procédure pas à pas, vous allez créer et utiliser une nouvelle page.

### <a name="to-add-a-page-to-the-web-application"></a>Pour ajouter une page à l’application Web

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le nom de l’application Web (dans ce didacticiel, le nom de l’application est **BasicWebSite**), puis cliquez sur **Ajouter** -&gt; **nouvel élément**.   
La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le **groupe C# Visual** -&gt; des modèles **Web** sur la gauche. Ensuite, sélectionnez **Web Form** dans la liste du milieu, puis nommez-le *FirstWebPage. aspx*.   
    ![Boîte de dialogue Ajouter un nouvel élément](code-editing-in-web-forms-pages/_static/image4.png)
3. Cliquez sur **Ajouter** pour ajouter la page de Web Forms à votre projet.  
 Visual Studio crée la page et l’ouvre.
4. Ensuite, définissez cette nouvelle page comme page de démarrage par défaut. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la nouvelle page nommée *FirstWebPage. aspx* , puis sélectionnez **définir comme page de démarrage**. La prochaine fois que vous exécuterez cette application pour tester notre progression, vous verrez automatiquement cette nouvelle page dans le navigateur.

## <a name="correcting-inline-coding-errors"></a>Correction des erreurs de codage en ligne

L’éditeur de code dans Visual Studio vous permet d’éviter les erreurs lors de l’écriture du code, et si vous avez effectué une erreur, l’éditeur de code vous aide à corriger l’erreur. Dans cette partie de la procédure pas à pas, vous allez écrire une ligne de code qui illustre les fonctionnalités de correction d’erreur dans l’éditeur.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Pour corriger les erreurs de codage simples dans Visual Studio

1. En mode **conception** , double-cliquez sur la page vierge pour créer un gestionnaire pour l’événement de **chargement** de la page.   
   Vous utilisez le gestionnaire d’événements uniquement comme emplacement pour écrire du code.
2. Dans le gestionnaire, tapez la ligne suivante qui contient une erreur et appuyez sur **entrée**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Lorsque vous appuyez sur **entrée**, l’éditeur de code place les traits de soulignement vert et rouge (appelez communément &quot;lignes&quot; ondulées) sous les zones du code qui présentent des problèmes. Un trait de soulignement vert indique un avertissement. Un trait de soulignement rouge indique une erreur que vous devez corriger. 

    Maintenez le pointeur de la souris sur `myStr` pour afficher une info-bulle qui vous indique l’avertissement. Placez également le pointeur de la souris sur le trait de soulignement rouge pour afficher le message d’erreur.

    L’illustration suivante montre le code avec les traits de soulignement.

    ![Texte de bienvenue dans Mode Création](code-editing-in-web-forms-pages/_static/image5.png "Texte de bienvenue en mode Design")  
   L’erreur doit être corrigée en ajoutant un point-virgule `;` à la fin de la ligne. L’avertissement vous avertit simplement que vous n’avez pas encore utilisé la variable `myStr`.  

    > [!NOTE] 
    > 
    > Vous pouvez afficher les paramètres de mise en forme de code actuels dans Visual Studio en sélectionnant **outils** -**Options** de &gt; -&gt; **les polices et les couleurs**.

## <a name="refactoring-and-renaming"></a>Refactorisation et changement de nom

La refactorisation est une méthodologie logicielle qui implique la restructuration de votre code pour le rendre plus facile à comprendre et à gérer, tout en préservant ses fonctionnalités. Un exemple simple peut être d’écrire du code dans un gestionnaire d’événements pour obtenir des données d’une base de données. Lorsque vous développez votre page, vous découvrez que vous devez accéder aux données à partir de différents gestionnaires. Par conséquent, vous refactorisez le code de la page en créant une méthode d’accès aux données dans la page et en insérant des appels à la méthode dans les gestionnaires.

L’éditeur de code comprend des outils pour vous aider à effectuer diverses tâches de refactorisation. Dans cette procédure pas à pas, vous allez utiliser deux techniques de refactorisation : renommer des variables et extraire des méthodes. Les autres options de refactorisation incluent l’encapsulation de champs, la promotion de variables locales en paramètres de méthode et la gestion des paramètres de méthode. La disponibilité de ces options de refactorisation dépend de l’emplacement dans le code.

### <a name="refactoring-code"></a>Refactorisation du code

Un scénario de refactorisation courant consiste à créer (extraire) une méthode à partir d’un code qui se trouve à l’intérieur d’un autre membre, tel qu’une méthode. Cela réduit la taille du membre d’origine et rend le code extrait réutilisable.

Dans cette partie de la procédure pas à pas, vous allez écrire du code simple, puis en extraire une méthode. La refactorisation est prise C#en charge pour. vous allez donc créer une C# page qui utilise comme langage de programmation.

### <a name="to-extract-a-method-in-a-c-page"></a>Pour extraire une méthode dans une C# page

1. Basculez en mode **Design** .
2. Dans la **boîte à outils**, sous l’onglet **standard** , faites glisser un contrôle [bouton](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sur la page.
3. Double-cliquez sur le contrôle **Button** pour créer un gestionnaire pour son événement [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) , puis ajoutez le code en surbrillance suivant :

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Le code crée un objet **ArrayList** , utilise une boucle pour le charger avec des valeurs, puis utilise une autre boucle pour afficher le contenu de l’objet **ArrayList** .
4. Appuyez sur **CTRL + F5** pour exécuter la page, puis cliquez sur le **bouton** pour vous assurer que la sortie suivante s’affiche :   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Revenez à l’éditeur de code, puis sélectionnez les lignes suivantes dans le gestionnaire d’événements.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Cliquez avec le bouton droit sur la sélection, cliquez sur **Refactoriser**, puis choisissez **extraire la méthode**. 

    La boîte de dialogue **extraire la méthode** s’affiche.
7. Dans la zone **nouveau nom** de la méthode, tapez **DisplayArray**, puis cliquez sur **OK**. 

    L’éditeur de code crée une nouvelle méthode nommée `DisplayArray`et place un appel à la nouvelle méthode dans le gestionnaire de **clic** où la boucle était à l’origine.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Appuyez sur **CTRL + F5** pour réexécuter la page et cliquez sur le **bouton**.

    La page fonctionne de la même façon qu’auparavant. La méthode `DisplayArray` peut désormais être appelée à partir de n’importe où dans la classe page.

## <a name="renaming-variables"></a>Attribution d’un nouveau nom aux variables

Lorsque vous utilisez des variables, ainsi que des objets, vous souhaiterez peut-être les renommer une fois qu’ils sont déjà référencés dans votre code. Toutefois, l’attribution d’un nouveau nom aux variables et aux objets peut entraîner l’arrêt du code si vous omettez de renommer l’une des références. Par conséquent, vous pouvez utiliser la refactorisation pour effectuer l’attribution d’un nouveau nom.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Pour utiliser la refactorisation pour renommer une variable

1. Dans le gestionnaire d’événements **Click** , recherchez la ligne suivante :

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Cliquez avec le bouton droit sur le nom de la variable `alist`, choisissez **Refactoriser**, puis **Renommer**.

    La boîte de dialogue **Renommer** s’affiche.
3. Dans la zone **nouveau nom** , tapez **ArrayList1** et assurez-vous que la case à cocher **aperçu des modifications** de la référence a été sélectionnée. Cliquez ensuite sur **OK**.

    La boîte de dialogue **aperçu des modifications** s’affiche et affiche une arborescence qui contient toutes les références à la variable que vous renommez.
4. Cliquez sur **appliquer** pour fermer la boîte de dialogue **aperçu des modifications** .

    Les variables qui font spécifiquement référence à l’instance que vous avez sélectionnée sont renommées. Notez, toutefois, que la variable `alist` dans la ligne suivante n’est pas renommée.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    La variable `alist` de cette ligne n’est pas renommée, car elle ne représente pas la même valeur que la variable `alist` que vous avez renommée. La variable `alist` dans la déclaration `DisplayArray` est une variable locale pour cette méthode. Cela illustre le fait que l’utilisation de la refactorisation pour renommer des variables est différente de l’exécution d’une action de recherche et remplacement dans l’éditeur ; la refactorisation renomme les variables avec une connaissance de la sémantique de la variable avec laquelle elle travaille.

## <a name="inserting-snippets"></a>Insertion d'extraits de code

Comme il existe de nombreuses tâches de codage que Web Forms les développeurs doivent souvent effectuer, l’éditeur de code fournit une bibliothèque d’extraits de code, ou des blocs de code préécrit. Vous pouvez insérer ces extraits de code dans votre page.

Chaque langage que vous utilisez dans Visual Studio présente de légères différences dans la façon d’insérer des extraits de code. Pour plus d’informations sur l’insertion d’extraits de code, consultez [Visual Basic des extraits de code IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx). Pour plus d’informations sur l’insertion d' C#extraits de code dans Visual, consultez [extraits de code visuels C# ](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Étapes suivantes

Cette procédure pas à pas a illustré les fonctionnalités de base de l’éditeur de code Visual Studio 2010 pour corriger les erreurs dans votre code, refactorisation du code, attribution d’un nouveau nom aux variables et insertion d’extraits de code dans votre code. Les fonctionnalités supplémentaires de l’éditeur peuvent faciliter et accélérer le développement d’applications. Vous pouvez, par exemple, décider d'effectuer les opérations suivantes :

- En savoir plus sur les fonctionnalités d’IntelliSense, telles que la modification des options IntelliSense, la gestion des extraits de code et la recherche d’extraits de code en ligne. Pour plus d'informations, consultez [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Découvrez comment créer vos propres extraits de code. Pour plus d’informations, consultez [création et utilisation d’extraits de code IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx) .
- En savoir plus sur les fonctionnalités spécifiques à Visual Basic des extraits de code IntelliSense, tels que la personnalisation des extraits de code et la résolution des problèmes. Pour plus d’informations, consultez [Visual Basic des extraits de code IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx)
- En savoir plus sur C#les fonctionnalités spécifiques à IntelliSense, telles que la refactorisation et les extraits de code. Pour plus d’informations, [consultez C# Visual IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
