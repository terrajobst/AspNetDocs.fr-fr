---
uid: webhooks/diagnostics/debugging
title: Débogage d’un webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Comment déboguer des webhooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640867"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="f5f34-103">Débogage de webhooks ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f5f34-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="f5f34-104">Débogage dans Azure</span><span class="sxs-lookup"><span data-stu-id="f5f34-104">Debugging in Azure</span></span>

<span data-ttu-id="f5f34-105">Pour déboguer votre application Web en cours d’exécution dans Azure, consultez le didacticiel [résolution des problèmes liés à une application Web dans Azure App service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="f5f34-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="f5f34-106">Débogage avec la source et les symboles</span><span class="sxs-lookup"><span data-stu-id="f5f34-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="f5f34-107">Outre le débogage de votre propre code, il est possible de déboguer directement dans Microsoft ASP.NET webhook et en fait tout .NET.</span><span class="sxs-lookup"><span data-stu-id="f5f34-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="f5f34-108">Cela fonctionne indépendamment du fait que vous déboguez localement ou à distance.</span><span class="sxs-lookup"><span data-stu-id="f5f34-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="f5f34-109">Tout d’abord, configurez Visual Studio pour rechercher la source et les symboles en accédant à **Déboguer** , puis **options et paramètres**.</span><span class="sxs-lookup"><span data-stu-id="f5f34-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="f5f34-110">Définissez les options comme suit :</span><span class="sxs-lookup"><span data-stu-id="f5f34-110">Set the options like this:</span></span>

![Options et paramètres](_static/SourceSymbols.png)

<span data-ttu-id="f5f34-112">Ajoutez ensuite un lien vers [symbolsource.org](http://symbolsource.org) pour télécharger la source et les symboles.</span><span class="sxs-lookup"><span data-stu-id="f5f34-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="f5f34-113">Accédez à l’onglet **symboles** du menu ci-dessus et ajoutez ce qui suit en tant qu’emplacement de symbole :</span><span class="sxs-lookup"><span data-stu-id="f5f34-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="f5f34-114">En outre, assurez-vous que le répertoire du cache a un nom abrégé. dans le cas contraire, les noms de fichiers peuvent être trop longs, ce qui entraînera l’impossibilité de charger les symboles.</span><span class="sxs-lookup"><span data-stu-id="f5f34-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="f5f34-115">Voici un exemple de chemin d’accès :</span><span class="sxs-lookup"><span data-stu-id="f5f34-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="f5f34-116">Les paramètres doivent ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="f5f34-116">The settings should look similar to this:</span></span>

![Exemple d’emplacement de fichier de symboles d’options](_static/SymSource.png)
