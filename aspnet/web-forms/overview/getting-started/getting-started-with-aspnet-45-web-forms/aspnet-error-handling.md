---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Gestion des erreurs ASP.NET | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: f420be369801208fa875d9a60e6e154afbe84aa7
ms.sourcegitcommit: b67ffd5b2c5cff01ec4c8eb12a21f693f2e11887
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995306"
---
# <a name="aspnet-error-handling"></a>Gestion des erreurs ASP.NET

par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’exemple de projet WingtipC#Toys ()](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [Télécharger le livre électronique (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les bases de la création d’une application ASP.NET Web Forms à l’aide de ASP.NET 4,5 et Microsoft Visual Studio Express 2013 pour le Web. Un [projet Visual Studio 2013 avec C# le code source](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.

Dans ce didacticiel, vous allez modifier l’exemple d’application Wingtip Toys pour inclure la gestion des erreurs et la journalisation des erreurs. La gestion des erreurs permettra à l’application de gérer correctement les erreurs et d’afficher les messages d’erreur en conséquence. La journalisation des erreurs vous permettra de rechercher et de corriger les erreurs qui se sont produites. Ce didacticiel s’appuie sur le didacticiel précédent sur le «routage d’URL» et fait partie de la série de didacticiels Wingtip Toys.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre:

- Comment ajouter la gestion des erreurs globale à la configuration de l’application.
- Comment ajouter la gestion des erreurs au niveau de l’application, de la page et du code.
- Comment consigner les erreurs pour une révision ultérieure.
- Comment afficher des messages d’erreur qui ne compromettent pas la sécurité.
- Comment implémenter des modules de journalisation des erreurs et des gestionnaires d’erreurs (ELMAH).

## <a name="overview"></a>Vue d'ensemble

Les applications ASP.NET doivent être en mesure de gérer les erreurs qui se produisent pendant l’exécution de manière cohérente. ASP.NET utilise le common language runtime (CLR), qui offre un moyen de notifier les erreurs aux applications de façon uniforme. Lorsqu’une erreur se produit, une exception est levée. Une exception est une erreur, une condition ou un comportement inattendu rencontré par une application.

Dans le .NET Framework, une exception est un objet qui hérite de la classe `System.Exception`. Une exception est levée à partir d'une partie du code où un problème s'est produit. L’exception est passée à la pile des appels jusqu’à un emplacement où l’application fournit du code pour gérer l’exception. Si l’application ne gère pas l’exception, le navigateur est obligé d’afficher les détails de l’erreur.

Il est recommandé de gérer les erreurs dans au niveau du code dans `Try` / / `Catch` `Finally` des blocs au sein de votre code. Essayez de placer ces blocs afin que l’utilisateur puisse corriger les problèmes dans le contexte dans lequel ils se produisent. Si les blocs de gestion des erreurs sont trop éloignés de l’endroit où l’erreur s’est produite, il devient plus difficile de fournir aux utilisateurs les informations dont ils ont besoin pour résoudre le problème.

### <a name="exception-class"></a>Classe d’exception

La classe d’exception est la classe de base dont héritent les exceptions. La plupart des objets exception sont des instances d’une classe dérivée de la classe exception `SystemException` , telles que `IndexOutOfRangeException` la classe, la `ArgumentNullException` classe ou la classe. La classe d’exception a des propriétés, telles `StackTrace` que la propriété `InnerException` , la propriété et `Message` la propriété, qui fournissent des informations spécifiques sur l’erreur qui s’est produite.

### <a name="exception-inheritance-hierarchy"></a>Hiérarchie d’héritage des exceptions

Le runtime a un ensemble de base d’exceptions dérivant de la `SystemException` classe que le runtime lève lorsqu’une exception est rencontrée. La plupart des classes qui héritent de la classe d’exception, telles `IndexOutOfRangeException` que la classe `ArgumentNullException` et la classe, n’implémentent pas de membres supplémentaires. Par conséquent, les informations les plus importantes pour une exception se trouvent dans la hiérarchie des exceptions, le nom de l’exception et les informations contenues dans l’exception.

### <a name="exception-handling-hierarchy"></a>Hiérarchie de gestion des exceptions

Dans une application ASP.NET Web Forms, les exceptions peuvent être gérées en fonction d’une hiérarchie de gestion spécifique. Une exception peut être gérée aux niveaux suivants:

- Niveau de l’application
- Niveau de page
- Niveau de code

Quand une application gère des exceptions, des informations supplémentaires sur l’exception héritée de la classe d’exception peuvent souvent être récupérées et affichées à l’utilisateur. Outre le niveau de l’application, de la page et du code, vous pouvez également gérer les exceptions au niveau du module HTTP et à l’aide d’un gestionnaire personnalisé IIS.

### <a name="application-level-error-handling"></a>Gestion des erreurs au niveau de l’application

Vous pouvez gérer les erreurs par défaut au niveau de l’application, soit en modifiant la configuration de votre application `Application_Error` , soit en ajoutant un gestionnaire dans le fichier *global. asax* de votre application.

Vous pouvez gérer les erreurs par défaut et les erreurs http `customErrors` en ajoutant une section au fichier *Web. config* . La `customErrors` section vous permet de spécifier une page par défaut vers laquelle les utilisateurs sont redirigés lorsqu’une erreur se produit. Elle vous permet également de spécifier des pages individuelles pour les erreurs de code d’État spécifiques.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Malheureusement, lorsque vous utilisez la configuration pour rediriger l’utilisateur vers une autre page, vous n’avez pas les détails de l’erreur qui s’est produite.

Toutefois, vous pouvez intercepter les erreurs qui se produisent n’importe où dans votre application `Application_Error` en ajoutant du code au gestionnaire dans le fichier *global. asax* .

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Gestion des événements d’erreur au niveau de la page

Un gestionnaire au niveau de la page retourne l’utilisateur à la page où l’erreur s’est produite, mais étant donné que les instances des contrôles ne sont pas conservées, il n’y aura plus aucune information sur la page. Pour fournir les détails de l’erreur à l’utilisateur de l’application, vous devez spécifiquement écrire les détails de l’erreur dans la page.

En général, vous utilisez un gestionnaire d’erreurs au niveau de la page pour enregistrer des erreurs non gérées ou pour faire passer l’utilisateur à une page qui peut afficher des informations utiles.

Cet exemple de code montre un gestionnaire pour l’événement d’erreur dans une page Web ASP.NET. Ce gestionnaire intercepte toutes les exceptions qui ne sont pas `try` déjà gérées / `catch` dans les blocs de la page.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Une fois que vous avez géré une erreur, vous devez l’effacer `ClearError` en appelant la méthode de l'`HttpServerUtility` objet serveur (classe). dans le cas contraire, vous verrez une erreur qui s’est produite précédemment.

### <a name="code-level-error-handling"></a>Gestion des erreurs au niveau du code

L’instruction try-catch se compose d’un bloc try suivi d’une ou de plusieurs clauses catch, qui spécifient des gestionnaires pour différentes exceptions. Quand une exception est levée, le common language runtime (CLR) recherche l’instruction catch qui gère cette exception. Si la méthode en cours d’exécution ne contient pas de bloc catch, le CLR examine la méthode qui a appelé la méthode actuelle, et ainsi de suite, en haut de la pile des appels. Si aucun bloc catch n’est trouvé, le CLR affiche un message d’exception non gérée à l’utilisateur et arrête l’exécution du programme.

L’exemple de code suivant montre un moyen courant d' `try` utiliser / / `catch` `finally` pour gérer les erreurs.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Dans le code ci-dessus, le bloc try contient le code qui doit être protégé contre une exception possible. Le bloc est exécuté jusqu’à ce qu’une exception soit levée ou que le bloc soit correctement effectué. Si une `FileNotFoundException` exception ou une `IOException` exception se produit, l’exécution est transférée vers une autre page. Ensuite, le code contenu dans le bloc finally est exécuté, qu’une erreur se soit produite ou non.

## <a name="adding-error-logging-support"></a>Ajout de la prise en charge de l’enregistrement des erreurs

Avant d’ajouter la gestion des erreurs à l’exemple d’application Wingtip Toys, vous allez ajouter la prise `ExceptionUtility` en charge de la journalisation des erreurs en ajoutant une classe au dossier *logique* . En procédant ainsi, chaque fois que l’application gère une erreur, les détails de l’erreur sont ajoutés au fichier journal des erreurs.

1. Cliquez avec le bouton droit sur le dossier *logique* , puis sélectionnez **Ajouter**  - &gt; **un nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le groupe modèles de **code** **visuel C#**   - &gt; sur la gauche. Ensuite, sélectionnez **classe**dans la liste du milieu, puis nommez-la **ExceptionUtility.cs**.
3. Sélectionnez **Ajouter**. Le nouveau fichier de classe s’affiche.
4. Remplacez le code existant par le code ci-dessous :  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Quand une exception se produit, l’exception peut être écrite dans un fichier journal d’exception en `LogException` appelant la méthode. Cette méthode prend deux paramètres, l’objet exception et une chaîne qui contient des détails sur la source de l’exception. Le journal des exceptions est écrit dans le fichier *ErrorLog. txt* du *dossier\_Application Data* .

### <a name="adding-an-error-page"></a>Ajout d’une page d’erreur

Dans l’exemple d’application Wingtip Toys, une page est utilisée pour afficher les erreurs. La page d’erreur est conçue pour afficher un message d’erreur sécurisé pour les utilisateurs du site. Toutefois, si l’utilisateur est un développeur qui effectue une requête HTTP qui est servie localement sur l’ordinateur où le code vit, des détails supplémentaires sur l’erreur s’affichent sur la page d’erreur.

1. Cliquez avec le bouton droit sur le nom du projet (**Wingtip Toys**) dans **Explorateur de solutions** , puis sélectionnez **Ajouter**  - &gt; **un nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le groupe modèles **Web** **visuels C#**   - &gt; sur la gauche. Dans la liste du milieu, sélectionnez **Web Form avec page maître**et nommez-le **pageerreur. aspx**.
3. Cliquez sur **Ajouter**.
4. Sélectionnez le fichier *site. Master* comme page maître, puis choisissez **OK**.
5. Remplacez le balisage existant par le code suivant:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Remplacez le code existant du code-behind (*ErrorPage.aspx.cs*) afin qu’il apparaisse comme suit:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Lorsque la page d’erreur s’affiche, `Page_Load` le gestionnaire d’événements est exécuté. Dans le `Page_Load` gestionnaire, l’emplacement de la première gestion de l’erreur est déterminé. Ensuite, la dernière erreur qui s’est produite est déterminée par `GetLastError` l’appel de la méthode de l’objet serveur. Si l’exception n’existe plus, une exception générique est créée. Ensuite, si la requête HTTP a été effectuée localement, tous les détails de l’erreur s’affichent. Dans ce cas, seul l’ordinateur local exécutant l’application Web verra les détails de l’erreur. Une fois les informations sur l’erreur affichées, l’erreur est ajoutée au fichier journal et l’erreur est effacée du serveur.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Affichage des messages d’erreur non gérés pour l’application

En ajoutant une `customErrors` section au fichier *Web. config* , vous pouvez rapidement gérer des erreurs simples qui se produisent dans l’ensemble de l’application. Vous pouvez également spécifier comment gérer les erreurs en fonction de leur valeur de code d’État, telle que 404-Fichier introuvable.

#### <a name="update-the-configuration"></a>Mettre à jour la configuration

Mettez à jour la configuration en `customErrors` ajoutant une section au fichier *Web. config* .

1. Dans **Explorateur de solutions**, recherchez et ouvrez le fichier *Web. config* à la racine de l’exemple d’application Wingtip Toys.
2. Ajoutez la `customErrors` section au fichier *Web. config* dans le `<system.web>` nœud comme suit:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Enregistrez le fichier *Web. config* .

La `customErrors` section spécifie le mode, qui a la valeur «on». Elle spécifie également `defaultRedirect`le, qui indique à l’application la page à laquelle accéder lorsqu’une erreur se produit. En outre, vous avez ajouté un élément d’erreur spécifique qui spécifie comment gérer une erreur 404 lorsqu’une page est introuvable. Plus loin dans ce didacticiel, vous allez ajouter une gestion des erreurs supplémentaire qui capturera les détails d’une erreur au niveau de l’application.

#### <a name="running-the-application"></a>Exécution de l'application

Vous pouvez exécuter l’application maintenant pour voir les itinéraires mis à jour.

1. Appuyez sur **F5** pour exécuter l’exemple d’application Wingtip Toys.  
 Le navigateur s’ouvre et affiche la page *default. aspx* .
2. Entrez l’URL suivante dans le navigateur (veillez à utiliser le numéro de port):  
    `https://localhost:44300/NoPage.aspx`
3. Examinez le *pageerreur. aspx* affiché dans le navigateur. 

    ![Gestion des erreurs ASP.NET-erreur de page introuvable](aspnet-error-handling/_static/image1.png)

Lorsque vous demandez la page *nopage. aspx* , qui n’existe pas, la page d’erreur affiche le message d’erreur simple et les informations d’erreur détaillées si des détails supplémentaires sont disponibles. Toutefois, si l’utilisateur a demandé une page inexistante à partir d’un emplacement distant, la page d’erreur affiche uniquement le message d’erreur en rouge.

### <a name="including-an-exception-for-testing-purposes"></a>Inclusion d’une exception à des fins de test

Pour vérifier le fonctionnement de votre application lorsqu’une erreur se produit, vous pouvez créer délibérément des conditions d’erreur dans ASP.NET. Dans l’exemple d’application Wingtip Toys, vous allez lever une exception de test lors du chargement de la page par défaut pour voir ce qui se passe.

1. Ouvrez le code-behind de la page *default. aspx* dans Visual Studio.   
   La page code-behind *default.aspx.cs* s’affiche.
2. Dans le `Page_Load` gestionnaire, ajoutez le code afin que le gestionnaire apparaisse comme suit:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Il est possible de créer différents types d’exceptions. Dans le code ci-dessus, vous créez `InvalidOperationException` un lorsque la page *default. aspx* est chargée.

#### <a name="running-the-application"></a>Exécution de l'application

Vous pouvez exécuter l’application pour voir comment l’application gère l’exception.

1. Appuyez sur **CTRL + F5** pour exécuter l’exemple d’application Wingtip Toys.  
 L’application lève l’exception InvalidOperationException. 

    > [!NOTE] 
    > 
    > Vous devez appuyer sur **CTRL + F5** pour afficher la page sans rupture dans le code pour afficher la source de l’erreur dans Visual Studio.
2. Examinez le *pageerreur. aspx* affiché dans le navigateur. 

    ![Gestion des erreurs ASP.NET-page d’erreur](aspnet-error-handling/_static/image2.png)

Comme vous pouvez le voir dans les détails de l’erreur, l’exception a `customError` été interceptée par la section dans le fichier *Web. config* .

### <a name="adding-application-level-error-handling"></a>Ajout de la gestion des erreurs au niveau de l’application

Au lieu d’intercepter l’exception `customErrors` à l’aide de la section du fichier *Web. config* , où vous obtenez peu d’informations sur l’exception, vous pouvez intercepter l’erreur au niveau de l’application et récupérer les détails de l’erreur.

1. Dans **Explorateur de solutions**, recherchez et ouvrez le fichier *global.asax.cs* .
2. Ajoutez un gestionnaire d' **Erreurs d’application\_** pour qu’il apparaisse comme suit:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Lorsqu’une erreur se produit dans l’application, `Application_Error` le gestionnaire est appelé. Dans ce gestionnaire, la dernière exception est récupérée et examinée. Si l’exception n’est pas gérée et que l’exception contient les détails de l’exception interne `InnerException` (c’est-à-dire que n’est pas null), l’application transfère l’exécution à la page d’erreur où les détails de l’exception sont affichés.

#### <a name="running-the-application"></a>Exécution de l'application

Vous pouvez exécuter l’application pour voir les détails supplémentaires sur l’erreur fournis par la gestion de l’exception au niveau de l’application.

1. Appuyez sur **CTRL + F5** pour exécuter l’exemple d’application Wingtip Toys.  
 L’application lève `InvalidOperationException` .
2. Examinez le *pageerreur. aspx* affiché dans le navigateur. 

    ![Gestion des erreurs ASP.NET-erreur au niveau de l’application](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Ajout de la gestion des erreurs au niveau de la page

Vous pouvez ajouter la gestion des erreurs au niveau de la page à une page en `ErrorPage` utilisant l’ajout `@Page` d’un attribut à la directive de la page `Page_Error` , ou en ajoutant un gestionnaire d’événements au code-behind d’une page. Dans cette section, vous allez ajouter un `Page_Error` gestionnaire d’événements qui va transférer l’exécution à la page *pageerreur. aspx* .

1. Dans **Explorateur de solutions**, recherchez et ouvrez le fichier *default.aspx.cs* .
2. Ajoutez un `Page_Error` gestionnaire afin que le code-behind apparaisse comme suit:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Lorsqu’une erreur se produit sur la page, `Page_Error` le gestionnaire d’événements est appelé. Dans ce gestionnaire, la dernière exception est récupérée et examinée. Si un `InvalidOperationException` se produit, `Page_Error` le gestionnaire d’événements transfère l’exécution à la page d’erreur où les détails de l’exception sont affichés.

#### <a name="running-the-application"></a>Exécution de l'application

Vous pouvez exécuter l’application maintenant pour voir les itinéraires mis à jour.

1. Appuyez sur **CTRL + F5** pour exécuter l’exemple d’application Wingtip Toys.  
 L’application lève `InvalidOperationException` .
2. Examinez le *pageerreur. aspx* affiché dans le navigateur. 

    ![Gestion des erreurs ASP.NET-erreur au niveau de la page](aspnet-error-handling/_static/image4.png)
3. Fermez la fenêtre de votre navigateur.

### <a name="removing-the-exception-used-for-testing"></a>Suppression de l’exception utilisée pour le test

Pour permettre à l’exemple d’application Wingtip Toys de fonctionner sans lever l’exception que vous avez ajoutée précédemment dans ce didacticiel, supprimez l’exception.

1. Ouvrez le code-behind de la page *default. aspx* .
2. Dans le `Page_Load` gestionnaire, supprimez le code qui lève l’exception afin que le gestionnaire apparaisse comme suit:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Ajout de la journalisation des erreurs au niveau du code

Comme mentionné plus haut dans ce didacticiel, vous pouvez ajouter des instructions try/catch pour tenter d’exécuter une section de code et gérer la première erreur qui se produit. Dans cet exemple, vous allez uniquement écrire les détails de l’erreur dans le fichier journal des erreurs pour que l’erreur puisse être révisée ultérieurement.

1. Dans **Explorateur de solutions**, dans le dossier *logique* , recherchez et ouvrez le fichier *PayPalFunctions.cs* .
2. Mettez à `HttpCall` jour la méthode afin que le code apparaisse comme suit:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Le code ci-dessus `LogException` appelle la méthode contenue dans `ExceptionUtility` la classe. Vous avez ajouté le fichier de classe *ExceptionUtility.cs* au dossier *logique* , plus haut dans ce didacticiel. La méthode `LogException` accepte deux paramètres. Le premier paramètre est l’objet exception. Le deuxième paramètre est une chaîne utilisée pour identifier la source de l’erreur.

### <a name="inspecting-the-error-logging-information"></a>Inspection des informations de journalisation des erreurs

Comme mentionné précédemment, vous pouvez utiliser le journal des erreurs pour déterminer les erreurs de votre application qui doivent être corrigées en premier. Bien entendu, seules les erreurs qui ont été interceptées et écrites dans le journal des erreurs sont enregistrées.

1. Dans **Explorateur de solutions**, recherchez et ouvrez le fichier *ErrorLog. txt* dans le *dossier\_Application Data* .   
 Vous devrez peut-être sélectionner l’option «**Afficher tous les fichiers**» oul’option «Actualiser» à partir du haut de **Explorateur de solutions** pour afficher le fichier *ErrorLog. txt* .
2. Examinez le journal des erreurs affiché dans Visual Studio: 

    ![Gestion des erreurs ASP.NET-ErrorLog. txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Messages d’erreur sécurisés

Il est **important de noter** que lorsque votre application affiche des messages d’erreur, elle ne doit pas fournir d’informations qu’un utilisateur malveillant pourrait trouver utile pour attaquer votre application. Par exemple, si votre application tente sans succès d’écrire dans une base de données, elle ne doit pas afficher de message d’erreur incluant le nom d’utilisateur qu’elle utilise. Pour cette raison, un message d’erreur générique en rouge s’affiche pour l’utilisateur. Tous les détails d’erreur supplémentaires sont affichés uniquement au développeur sur l’ordinateur local.

## <a name="using-elmah"></a>Utilisation de ELMAH

ELMAH (modules d’enregistrement des erreurs et gestionnaires) est une fonctionnalité de journalisation des erreurs que vous connectez à votre application ASP.NET en tant que package NuGet. ELMAH fournit les fonctionnalités suivantes:

- Journalisation des exceptions non gérées.
- Une page Web pour afficher le journal complet des exceptions non gérées recodées.
- Une page Web pour afficher les détails complets de chaque exception journalisée.
- Une notification par courrier électronique de chaque erreur au moment où elle se produit.
- Flux RSS des 15 dernières erreurs du journal.

Avant de pouvoir utiliser ELMAH, vous devez l’installer. Il est facile d’utiliser le programme d’installation du package *NuGet* . Comme mentionné plus haut dans cette série de didacticiels, NuGet est une extension Visual Studio qui facilite l’installation et la mise à jour des bibliothèques et des outils open source dans Visual Studio.

1. Dans Visual Studio, dans le menu **Outils** , sélectionnez **Gestionnaire** > de package NuGet**gérer les packages NuGet pour la solution**. 

    ![Gestion des erreurs ASP.NET-gérer les packages NuGet pour la solution](aspnet-error-handling/_static/image6.png)
2. La boîte de dialogue **gérer les packages NuGet** s’affiche dans Visual Studio.
3. Dans la boîte de dialogue **gérer les packages NuGet** , développez **en ligne** sur la gauche, puis sélectionnez **NuGet.org**. Ensuite, recherchez et installez le package **ELMAH** à partir de la liste des packages disponibles en ligne. 

    ![Gestion des erreurs ASP.NET-package NuGet ELMA](aspnet-error-handling/_static/image7.png)
4. Vous devez disposer d’une connexion Internet pour télécharger le package.
5. Dans la boîte de dialogue **Sélectionner les projets** , assurez-vous que la sélection **WingtipToys** est sélectionnée, puis cliquez sur **OK**. 

    ![Gestion des erreurs ASP.NET-boîte de dialogue Sélectionner les projets](aspnet-error-handling/_static/image8.png)
6. Si nécessaire, cliquez sur **Fermer** dans la boîte de dialogue **gérer les packages NuGet** .
7. Si Visual Studio demande de recharger tous les fichiers ouverts, sélectionnez «**Oui pour tout**».
8. Le package ELMAH ajoute des entrées pour lui-même dans le fichier *Web. config* à la racine de votre projet. Si Visual Studio vous demande si vous souhaitez recharger le fichier *Web. config* modifié, cliquez sur **Oui**.

ELMAH est maintenant prêt à stocker toutes les erreurs non gérées qui se produisent.

### <a name="viewing-the-elmah-log"></a>Affichage du journal ELMAH

L’affichage du journal ELMAH est simple, mais vous allez d’abord créer une exception non gérée qui sera enregistrée dans le journal ELMAH.

1. Appuyez sur **CTRL + F5** pour exécuter l’exemple d’application Wingtip Toys.
2. Pour écrire une exception non gérée dans le journal ELMAH, accédez à l’URL suivante dans votre navigateur (à l’aide de votre numéro de port):  
    `https://localhost:44300/NoPage.aspx`La page d’erreur s’affiche.
3. Pour afficher le journal ELMAH, accédez à l’URL suivante dans votre navigateur (à l’aide de votre numéro de port):  
    `https://localhost:44300/elmah.axd`

    ![Gestion des erreurs ASP.NET-journal des erreurs ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à gérer les erreurs au niveau de l’application, au niveau de la page et au niveau du code. Vous avez également appris comment enregistrer des erreurs gérées et non gérées pour les examiner ultérieurement. Vous avez ajouté l’utilitaire ELMAH pour fournir la journalisation des exceptions et la notification à votre application à l’aide de NuGet. En outre, vous avez appris à l’importance des messages d’erreur sécurisés.

## <a name="tutorial-series-conclusion"></a>Conclusion de la série de didacticiels

Merci pour les opérations suivantes. J’espère que cet ensemble de didacticiels vous a aidé à vous familiariser avec ASP.NET Web Forms. Pour plus d’informations sur les fonctionnalités Web Forms disponibles dans ASP.NET 4,5 et Visual Studio 2013, consultez [ASP.net et Web Tools pour Visual Studio 2013 notes de publication](../../../../visual-studio/overview/2013/release-notes.md). En outre, veillez à consulter le didacticiel mentionné dans la section **étapes suivantes** et defintely essayer la [version d’évaluation gratuite d’Azure](https://azure.microsoft.com/pricing/free-trial/).

![Merci-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur le déploiement de votre application Web sur Microsoft Azure, consultez [déployer une application secure ASP.NET Web Forms avec appartenance, OAuth et SQL Database sur un site Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Essai gratuit

[Microsoft Azure-essai gratuit](https://azure.microsoft.com/pricing/free-trial/)  
 La publication de votre site Web sur Microsoft Azure vous permet de gagner du temps, de la maintenance et des dépenses. Il s’agit d’un processus rapide pour déployer votre application Web sur Azure. Lorsque vous devez gérer et surveiller votre application Web, Azure propose un large éventail d’outils et de services. Gérez les données, le trafic, l’identité, les sauvegardes, la messagerie, le support et les performances dans Azure. Tout cela est fourni dans une approche très économique.

## <a name="additional-resources"></a>Ressources supplémentaires

[Journalisation des détails des erreurs avec ASP.NET Health Monitoring](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Remerciements

Je souhaite remercier les personnes suivantes qui ont apporté des contributions significatives au contenu de cette série de didacticiels:

- [Alberto Poblacion, MCT &amp; MVP, Espagne](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Pays-Bas](http://blog.alexthissen.nl/) (Twitter: [@alexthissen](http://twitter.com/alexthissen))
- [Andre Tournier, États-Unis](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slovénie](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brésil](http://msmvps.com/blogs/bsonnino) (Twitter: [@bsonnino](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brésil](http://www.carloscds.net/)
- [Dave Campbell, États-Unis](http://www.wynapse.com/) (Twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (Twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Michael dièses, États-Unis](http://www.930solutions.com/) (Twitter: [@mrsharps](http://twitter.com/mrsharps))
- Mike pape
- [Mitchel vendeurs, États-Unis](http://www.mitchelsellers.com/) (Twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Contributions de la communauté

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Exemple de code lié à Visual Studio 2012 sur MSDN: [Navigation dans Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Exemple de code lié à Visual Studio 2012 sur MSDN: [Série de didacticiels ASP.NET 4,5 Web Forms dans Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo-contributeur Microsoft technical audience (Twitter: @driazevedo)  
  Traduction de Visual Studio 2012: [Iniciando com ASP.NET Web Forms 4,5-partie 1-Introdução e Visão geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Précédent](url-routing.md)
