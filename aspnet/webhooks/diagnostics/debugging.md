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
# <a name="aspnet-webhooks-debugging"></a>Débogage de webhooks ASP.NET  

## <a name="debugging-in-azure"></a>Débogage dans Azure

Pour déboguer votre application Web en cours d’exécution dans Azure, consultez le didacticiel [résolution des problèmes liés à une application Web dans Azure App service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Débogage avec la source et les symboles

Outre le débogage de votre propre code, il est possible de déboguer directement dans Microsoft ASP.NET webhook et en fait tout .NET. Cela fonctionne indépendamment du fait que vous déboguez localement ou à distance. Tout d’abord, configurez Visual Studio pour rechercher la source et les symboles en accédant à **Déboguer** , puis **options et paramètres**. Définissez les options comme suit :

![Options et paramètres](_static/SourceSymbols.png)

Ajoutez ensuite un lien vers [symbolsource.org](http://symbolsource.org) pour télécharger la source et les symboles. Accédez à l’onglet **symboles** du menu ci-dessus et ajoutez ce qui suit en tant qu’emplacement de symbole :

```
http://srv.symbolsource.org/pdb/Public
```

En outre, assurez-vous que le répertoire du cache a un nom abrégé. dans le cas contraire, les noms de fichiers peuvent être trop longs, ce qui entraînera l’impossibilité de charger les symboles. Voici un exemple de chemin d’accès :

```
C:\SymCache
```

Les paramètres doivent ressembler à ceci :

![Exemple d’emplacement de fichier de symboles d’options](_static/SymSource.png)
