---
ms.openlocfilehash: eb2f83504129ae2b19ff5d85a2bc7d90293b43d6
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57043396"
---
* Startup.cs : [Classe de démarrage](xref:fundamentals/startup) -classe configure le pipeline de requête qui gère toutes les demandes adressées à l’application.
* Program.cs : [Classe du programme](xref:fundamentals/index) qui contient le point d’entrée principal de l’application.
* firstapp.csproj : [Fichier projet](/dotnet/articles/core/preview3/tools/csproj) le format de fichier projet MSBuild pour les applications ASP.NET Core. Contient des références entre projets, références NuGet et autres éléments connexes du projet.
* appSettings.JSON / appsettings. Development.JSON : Fichier de configuration de paramètres de base de l’application d’environnement. [Consultez Configuration](xref:fundamentals/configuration/index).
* bower.json : Bower des dépendances de package pour le projet.
* .bowerrc : Fichier de configuration bower qui définit où installer les composants lors du télécharge de Bower les ressources.
* bundleconfig.JSON : les fichiers de configuration pour le regroupement et minimisation des ressources frontales de JavaScript et CSS.
* Affichage : Contient les affichages Razor. Les vues sont les composants qui affichent l’interface utilisateur de l’application. En règle générale, cette interface utilisateur affiche les données du modèle.
* Contrôleurs : Contient des contrôleurs MVC, initialement *HomeController.cs*. Les contrôleurs sont des classes qui gèrent les demandes du navigateur.
* wwwroot : Dossier racine d’application Web.

Pour plus d’informations, consultez [le modèle MVC](xref:mvc/overview).
