---
title: Prise en main des composants de Razor
author: guardrex
description: Découvrez comment bien démarrer avec les composants de Razor en créant et modifiant un projet de composants de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: a9ada603e5ed4e0e75c4aebc5105c331118666e6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036386"
---
# <a name="get-started-with-razor-components"></a>Prise en main des composants de Razor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Conditions préalables :

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Pour créer votre premier projet de composants de Razor dans Visual Studio :

1. Sélectionnez **fichier** > **nouveau projet** > **Web** > **Application Web ASP.NET Core**.
1. Assurez-vous que **.NET Core** et **ASP.NET Core 3.0** sont sélectionnés en haut.
1. Choisissez le **Razor composants** modèle et sélectionnez **OK**.

   ![Boîte de dialogue nouvelle application](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. Appuyez sur **F5** pour exécuter l'application.

Félicitations ! Vous venez d’exécuter votre première application Razor composants !

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.


[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli/)

Conditions préalables :

* [Afficher un aperçu de .NET core SDK 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Pour créer votre premier projet Razor composants à partir d’une invite de commandes :

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. Dans un navigateur, accédez à `https://localhost:5001`.

Félicitations ! Vous venez d’exécuter votre première application Razor composants !

---

## <a name="razor-components-project"></a>Projet de composants de Razor

La solution créée par le modèle de composants de Razor contient deux projets :

* *WebApplication1.Server* &ndash; le projet serveur est un projet ASP.NET Core configuré pour héberger l’application de composants de Razor.
* *WebApplication1.App* &ndash; le projet d’interface utilisateur web côté client qui utilise les composants de Razor.

La logique de l’interface utilisateur dans le *WebApplication1.App* projet est séparé du reste de l’application en raison d’une restriction technique dans ASP.NET Core 3.0 Preview 2. L’extension de fichier Razor (*.cshtml*) utilisé pour les composants de Razor est également utilisée pour les vues de Pages Razor et MVC. Actuellement, les composants de Razor et Razor Pages/MVC étant modèles différents de compilation, les fichiers Razor composants Razor sont maintenues séparées. Dans une prochaine version d’évaluation, nous prévoyons d’introduire une nouvelle extension de fichier pour les composants de Razor (*.razor*). Composants, les pages et les vues seront hébergées *dans le même projet*.

Quand l’application est exécutée, plusieurs pages sont disponibles à partir des onglets dans la barre latérale :

* Accueil
* Counter
* Récupérer des données

Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page. L’incrémentation d’un compteur dans une page web requiert normalement l’écriture JavaScript, mais le projet Composants Razor fournit une meilleure approche à l’aide C#.

*WebApplication1.App/Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Une demande de `/counter` dans le navigateur, comme spécifié par le `@page` directive en haut, entraîne le composant de compteur afficher son contenu. Composants restituent dans une représentation en mémoire de l’arborescence de rendu qui peut ensuite être utilisé pour mettre à jour l’interface utilisateur de manière flexible et efficace.

Chaque fois que le **Click me** bouton est sélectionné :

* Le `onclick` événement est déclenché.
* La méthode `IncrementCount` est appelée.
* Le `currentCount` est incrémenté.
* Le composant est une nouvelle fois restitué.

Le runtime compare le nouveau contenu au contenu précédent et s’applique uniquement le contenu est modifié pour le modèle DOM (Document Object).

Ajouter un composant à un autre composant à l’aide d’une syntaxe de type HTML. Paramètres de composant sont spécifiés à l’aide d’attributs ou contenu enfant. Par exemple, un composant de compteur peut être ajouté à la page d’accueil de l’application en ajoutant un `<Counter />` élément pour le composant d’Index.

*WebApplication1.App/Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Exécuter l’application. La page d’accueil a son propre compteur.

Pour ajouter un paramètre au composant de compteur, mettre à jour du composant `@functions` bloc :

* Ajouter une propriété pour `IncrementAmount` décorée avec le `[Parameter]` attribut.
* Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.

*WebApplication1.App/Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut.

*WebApplication1.App/Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Exécuter l’application. La page d’accueil a son propre compteur incrémente par dix chaque fois que le **Click me** bouton est sélectionné.

## <a name="next-steps"></a>Étapes suivantes

<xref:tutorials/first-razor-components-app>
