---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Comprendre le processus d’exécution de ASP.NET MVC | Microsoft Docs
author: microsoft
description: Découvrez comment l’infrastructure MVC ASP.NET traite une demande de navigateur pas à pas.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 28940947253e0af43886cf1231f8aaf4615526cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541516"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Présentation du processus d’exécution d’ASP.NET MVC

par [Microsoft](https://github.com/microsoft)

> Découvrez comment l’infrastructure MVC ASP.NET traite une demande de navigateur pas à pas.

Les demandes à une application Web basée sur ASP.NET MVC passent d’abord par l’objet **UrlRoutingModule** , qui est un module http. Ce module analyse la demande et exécute la sélection d'itinéraire. L’objet **UrlRoutingModule** sélectionne le premier objet route qui correspond à la requête actuelle. (Un objet route est une classe qui implémente **RouteBase**, et est généralement une instance de la classe **route** .) Si aucun itinéraire ne correspond, l’objet **UrlRoutingModule** ne fait rien et permet à la requête de revenir au traitement de la demande ASP.net ou IIS normal.

À partir de l’objet d' **itinéraire** sélectionné, l’objet **UrlRoutingModule** obtient l’objet **IRouteHandler** associé à l’objet **route** . En général, dans une application MVC, il s’agit d’une instance de **MvcRouteHandler**. L’instance **IRouteHandler** crée un objet **IHttpHandler** et lui passe l’objet **IHttpContext** . Par défaut, l’instance **IHttpHandler** pour MVC est l’objet **MvcHandler** . L’objet **MvcHandler** sélectionne ensuite le contrôleur qui traitera finalement la demande.

> [!NOTE]
> Lorsqu'une application Web ASP.NET MVC s'exécute dans IIS 7.0, aucune extension de nom de fichier n'est requise pour les projets MVC. Toutefois, dans IIS 6.0, le gestionnaire requiert que vous mappiez l'extension de nom de fichier .mvc à la DLL ISAPI ASP.NET.

Le module et le gestionnaire sont les points d’entrée de l’infrastructure MVC ASP.NET. Elles exécutent les actions suivantes :

- Sélection du contrôleur approprié dans une application Web MVC.
- Obtention d'une instance de contrôleur spécifique.
- Appelez la méthode **Execute** du contrôleur.

La liste suivante répertorie les étapes d’exécution d’un projet Web MVC :

- Réception de la première demande concernant l'application 

    - Dans le fichier global. asax, les objets **route** sont ajoutés à l’objet **RouteTable** .
- Exécuter le routage 

    - Le **module UrlRoutingModule** utilise le premier objet d' **itinéraire** correspondant dans la collection **RouteTable** pour créer l’objet **RouteData** , qu’il utilise ensuite pour créer un objet **RequestContext** (**IHttpContext**).
- Création d'un gestionnaire de demandes MVC 

    - L’objet **MvcRouteHandler** crée une instance de la classe **MvcHandler** et lui passe l’instance **RequestContext** .
- Création du contrôleur 

    - L’objet **MvcHandler** utilise l’instance **RequestContext** pour identifier l’objet **IControllerFactory** (généralement une instance de la classe **DefaultControllerFactory** ) avec lequel créer l’instance de contrôleur.
- Exécuter le contrôleur-l’instance **MvcHandler** appelle la méthode d' **exécution** du contrôleur. |
- Appel de l'action 

    - La plupart des contrôleurs héritent de la classe de base du **contrôleur** . Pour les contrôleurs qui le font, l’objet **ControllerActionInvoker** associé au contrôleur détermine la méthode d’action de la classe de contrôleur à appeler, puis appelle cette méthode.
- Exécution du résultat 

    - Une méthode d’action classique peut recevoir une entrée d’utilisateur, préparer les données de réponse appropriées, puis exécuter le résultat en retournant un type de résultat. Les types de résultats intégrés qui peuvent être exécutés sont les suivants : **ViewResult** (qui restitue une vue et est le type de résultat le plus souvent utilisé), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**et **EmptyResult**.
