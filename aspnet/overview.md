---
uid: overview
title: Présentation de ASP.NET | Microsoft Docs
author: rick-anderson
description: Présentation de ASP.NET, une infrastructure gratuite pour la création de sites Web, d’applications Web et d’API Web.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 08/10/2019
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: aa4f627bca99f0a7ffbbb53ea45ebdcf0850fd89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78537344"
---
# <a name="aspnet-overview"></a>Vue d’ensemble d’ASP.NET

ASP.NET est un Framework Web gratuit permettant de créer de superbes sites Web et applications Web en utilisant HTML, CSS et JavaScript. Vous pouvez également créer des API Web et utiliser des technologies en temps réel telles que Web Sockets.

[ASP.net Core](https://docs.microsoft.com/aspnet/core/) est une alternative à ASP.net.  Consultez les [conseils pour choisir entre ASP.net et ASP.net Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Prise en main

Installez [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2019) Community Edition, un environnement de développement intégré (IDE) gratuit pour ASP.net sur Windows.

## <a name="websites-and-web-applications"></a>Sites Web et applications Web

 ASP.NET offre trois frameworks pour la création d’applications Web : Web Forms, ASP.NET MVC et pages Web ASP.NET. Les trois frameworks sont stables et matures, et vous pouvez créer des applications Web attrayantes avec n’importe lequel d’entre eux. Quel que soit le Framework que vous choisissez, vous obtiendrez tous les avantages et fonctionnalités de ASP.NET Everywhere.

Chaque infrastructure cible un style de développement différent. Celui que vous choisissez dépend de la combinaison de vos ressources de programmation (connaissances, compétences et expérience de développement), du type d’application que vous créez et de l’approche de développement avec laquelle vous êtes familiarisé.

Vous trouverez ci-dessous une vue d’ensemble de chacun des frameworks et des idées de choix entre eux. Si vous préférez une présentation vidéo, consultez la page [création de sites Web avec ASP.net](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) et [qu’est-ce que les outils Web ?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Si vous avez une expérience dans | Style de développement | Pointu |
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Win Forms, WPF, .NET | Développement rapide à l’aide d’une bibliothèque de contrôles enrichie qui encapsulent le balisage HTML | RAD de niveau intermédiaire, avancé |
| MVC       | Ruby on rails, .NET  | Contrôle total sur le balisage HTML, le code et le balisage séparés, et les tests faciles à écrire. Le meilleur choix pour les applications mobiles et les applications à page unique (SPA). | Niveau intermédiaire, avancé |
| Pages web  | ASP classique, PHP     | Balisage HTML et votre code ensemble dans le même fichier | Nouveau, de niveau intermédiaire |

### <a name="web-forms"></a>Web Forms

Avec ASP.NET Web Forms, vous pouvez créer des sites Web dynamiques à l’aide d’un modèle de glisser-déplacer familier, piloté par les événements. Une aire de conception et des centaines de contrôles et de composants vous permettent de créer rapidement des sites sophistiqués et puissants, gérés par interface utilisateur avec accès aux données.

[En savoir plus sur Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC est un outil puissant, basé sur les modèles, qui vous permet de générer des sites web en séparant de manière claire et précise tous les aspects associés, et propose un contrôle total sur le balisage, ce qui permet de bénéficier d'un déploiement flexible et optimal. ASP.NET MVC inclut de nombreuses fonctionnalités qui permettent de bénéficier d'un développement TDD rapide et convivial pour la création d'applications sophistiquées qui utilisent les normes web les plus récentes.

[En savoir plus sur MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>Pages web ASP.NET

Pages Web ASP.NET et le syntaxe Razor offrent une méthode rapide, simple et légère pour combiner du code serveur et du code HTML pour créer du contenu Web dynamique. Connectez-vous à des bases de données, ajoutez de la vidéo, établissez des liens vers des sites de réseaux sociaux et incluez de nombreuses fonctionnalités qui vous aideront à créer de superbes sites conformes aux normes Web les plus récentes.

[En savoir plus sur les pages Web](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Remarques sur Web Forms, MVC et les pages Web

Les trois frameworks ASP.NET sont basés sur le .NET Framework et partagent les fonctionnalités principales de .NET et de ASP.NET. Par exemple, les trois frameworks offrent un modèle de sécurité de connexion basé sur l’appartenance, et les trois partagent les mêmes fonctionnalités de gestion des demandes, de gestion des sessions, etc., qui font partie de la fonctionnalité de base ASP.NET.

En outre, les trois frameworks ne sont pas entièrement indépendants et le choix de l’un n’exclut pas l’utilisation d’un autre. Étant donné que les frameworks peuvent coexister dans la même application Web, il n’est pas rare de voir les composants individuels des applications écrites à l’aide de différentes infrastructures. Par exemple, les parties côté client d’une application peuvent être développées dans MVC pour optimiser le balisage, tandis que les parties d’accès et d’administration des données sont développées dans Web Forms pour tirer parti des contrôles de données et de l’accès simple aux données.

## <a name="web-apis"></a>API web

ASP.NET Web API est une infrastructure qui facilite le développement de services HTTP disponibles sur un large éventail de clients, tels que des navigateurs et des appareils mobiles. L'API Web ASP.NET est une plate-forme idéale pour générer des applications RESTful sur le .NET Framework.

[En savoir plus sur l'API web](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Technologies en temps réel

ASP.NET Signalr est une nouvelle bibliothèque pour les développeurs ASP.NET qui facilite le développement de fonctionnalités Web en temps réel. Signalr permet une communication bidirectionnelle entre le serveur et le client. Les serveurs peuvent transmettre instantanément le contenu aux clients connectés dès qu’il est disponible. Signalr prend en charge les sockets Web et revient à d’autres techniques compatibles pour les navigateurs plus anciens. Signalr comprend des API pour la gestion des connexions (par exemple, les événements de connexion et de déconnexion), le regroupement des connexions et l’autorisation.

[En savoir plus sur Signalr](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Applications et sites mobiles

ASP.NET peut alimenter des applications mobiles natives avec une API Web back end, ainsi que des sites Web mobiles à l’aide d’infrastructures de conception réactives telles que les démarrages Twitter. Si vous générez une application mobile native, il est facile de créer une API Web basée sur JSON pour gérer l’accès aux données, l’authentification et les notifications push pour votre application. Si vous créez un site mobile réactif, vous pouvez utiliser n’importe quel Framework CSS ou système ouvert de grille préféré, ou sélectionner un système mobile puissant comme jQuery mobile ou sencha et des applications mobiles performantes avec PhoneGap.

[En savoir plus sur l’application mobile et le développement de sites](mobile/overview.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Applications monopages

L’application à page unique ASP.NET (SPA) vous aide à créer des applications qui incluent des interactions majeures côté client à l’aide de HTML 5, CSS 3 et JavaScript. Visual Studio comprend un modèle pour générer des applications à page unique à l’aide de Knockout. js et API Web ASP.NET. Outre le modèle SPA intégré, les modèles SPA créés par la Communauté sont également disponibles en téléchargement.

[En savoir plus sur le développement d’applications à page unique](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

Webhook est un modèle HTTP léger qui fournit un modèle Pub/Sub simple pour le câblage des API Web et des services SaaS. Lorsqu’un événement se produit dans un service, une notification est envoyée sous la forme d’une requête HTTP POSTALe aux abonnés inscrits. La demande de publication contient des informations sur l’événement, ce qui permet au récepteur d’agir en conséquence.

Les webhooks sont exposés par un grand nombre de services, notamment Dropbox, GitHub, Instagram, MailChimp, PayPal, marge, Trello et bien plus encore. Par exemple, un webhook peut indiquer qu’un fichier a été modifié dans Dropbox ou qu’une modification du code a été validée dans GitHub, ou qu’un paiement a été initié dans PayPal ou qu’une carte a été créée dans Trello.

[En savoir plus sur les webhooks](webhooks/index.md)

<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
