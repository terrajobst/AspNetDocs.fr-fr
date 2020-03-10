---
uid: webhooks/diagnostics/logging
title: Journalisation des webhooks ASP.NET | Microsoft Docs
author: rick-anderson
description: Comment se connecter dans des webhooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: a05b32c4a8f9577bcf6170bd19a9e440b1aeb75b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547879"
---
# <a name="aspnet-webhooks-logging"></a>Journalisation des webhooks ASP.NET

Microsoft ASP.NET webhook utilise la journalisation pour signaler les problèmes et les problèmes. Par défaut, les journaux sont écrits à l’aide de [System. Diagnostics. trace,](https://msdn.microsoft.com/library/system.diagnostics.trace) où ils peuvent être développés à l’aide d' [écouteurs de suivi](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) comme tout autre flux de journal.

Lors du déploiement de votre application Web en tant qu’application Web Azure, les journaux sont automatiquement récupérés et peuvent être gérés avec tout autre journal [System. Diagnostics. trace](https://msdn.microsoft.com/library/system.diagnostics.trace) . Pour plus d’informations, consultez [activer la journalisation des diagnostics pour les applications Web dans Azure App service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

En outre, les journaux peuvent être obtenus directement à partir de Visual Studio, comme décrit dans [résoudre les problèmes d’une application Web dans Azure App service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Redirection des journaux

Au lieu d’écrire des journaux dans [System. Diagnostics. trace](https://msdn.microsoft.com/library/system.diagnostics.trace), il est possible de fournir une autre implémentation de journalisation qui peut se connecter directement à un gestionnaire de journaux comme [log4net](http://logging.apache.org/log4net/) et [nlog](http://nlog-project.org/). Il vous suffit de fournir une implémentation de [ILogger](https://github.com/aspnet/AspNetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) et de l’inscrire auprès d’un moteur d’injection de dépendances de votre choix et il sera récupéré par Microsoft ASP.net webhook. Pour plus d’informations, consultez [injection de dépendances dans API Web ASP.NET 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) .
