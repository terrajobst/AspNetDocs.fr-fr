---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio : Introduction-1 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: ea88da1e6d510f706fc7ca370cfa32974c1243f8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587730"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio : Introduction-1 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web.
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Ces didacticiels vous guident tout au long du déploiement d’IIS sur votre ordinateur de développement local à des fins de test, puis à un fournisseur d’hébergement tiers. L’application que vous déploierez utilise une base de données d’application et une base de données d’appartenance ASP.NET. Vous commencez à utiliser SQL Server Compact et le déploiement sur SQL Server Compact, et les didacticiels ultérieurs vous montrent comment déployer des modifications de base de données et comment migrer vers SQL Server.
> 
> Les didacticiels partent du principe que vous savez utiliser ASP.NET dans Visual Studio. Si ce n’est pas le cas, il est recommandé de commencer par un [didacticiel Web Forms de base ASP.net](../tailspin-spyworks/tailspin-spyworks-part-1.md) ou un [didacticiel ASP.NET MVC de base](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [Forum de déploiement ASP.net](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).

## <a name="overview"></a>Vue d'ensemble de

Ces didacticiels vous guident tout au long du déploiement d’IIS sur votre ordinateur de développement local à des fins de test, puis à un fournisseur d’hébergement tiers. L’application que vous déploierez utilise une base de données d’application et une base de données d’appartenance ASP.NET. Vous commencez à utiliser SQL Server Compact et le déploiement sur SQL Server Compact, et les didacticiels ultérieurs vous montrent comment déployer des modifications de base de données et comment migrer vers SQL Server.

Le nombre de didacticiels (11) et une page de dépannage sont susceptibles de compliquer le processus de déploiement. En fait, les procédures de base pour le déploiement d’un site constituent une partie relativement réduite de l’ensemble des didacticiels. Toutefois, dans les situations réelles, vous avez souvent besoin d’informations sur un aspect supplémentaire petit mais important du déploiement, par exemple, la définition des autorisations de dossier sur le serveur cible. Nous avons inclus un grand nombre de ces techniques supplémentaires dans les didacticiels, en espérant que les didacticiels n’ignorent pas les informations susceptibles de vous empêcher de déployer correctement une application réelle.

Les didacticiels sont conçus pour s’exécuter en séquence, et chaque partie repose sur le composant précédent. Toutefois, vous pouvez ignorer des parties qui ne sont pas pertinentes pour votre situation. (Les éléments ignorés peuvent vous obliger à ajuster les procédures dans les didacticiels ultérieurs.)

## <a name="intended-audience"></a>Public visé

Les didacticiels sont destinés aux développeurs ASP.NET qui travaillent dans des organisations de petite taille ou d’autres environnements où :

- Un processus d’intégration continue (builds et déploiement automatisés) n’est pas utilisé.
- L’environnement de production est un fournisseur d’hébergement tiers.
- Une personne remplit généralement plusieurs rôles (la même personne développe, teste et déploie).

Dans les environnements d’entreprise, il est plus courant d’implémenter des processus d’intégration continus, et l’environnement de production est généralement hébergé par les serveurs de l’entreprise. Les différentes personnes exécutent généralement des rôles différents. Pour plus d’informations sur le déploiement d’entreprise, consultez [déploiement d’applications Web dans des scénarios d’entreprise](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Les organisations de toutes tailles peuvent également déployer des applications Web sur Azure, et la plupart des procédures présentées dans ces didacticiels s’appliquent également à Azure App services Web Apps. Pour une présentation d’Azure, consultez [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>Fournisseur d’hébergement présenté dans les didacticiels

Les didacticiels vous guident tout au long du processus de configuration d’un compte avec une société d’hébergement et de déploiement de l’application sur ce fournisseur d’hébergement. Une société d’hébergement spécifique a été choisie afin que les didacticiels illustrent l’expérience complète du déploiement sur un site Web en ligne. Chaque société d’hébergement fournit des fonctionnalités différentes, et l’expérience de déploiement sur leurs serveurs varie quelque peu ; Toutefois, le processus décrit dans ce didacticiel est courant pour le processus global.

Le fournisseur d’hébergement utilisé pour ce didacticiel, Cytanium.com, est l’un des nombreux disponibles, et son utilisation dans ce didacticiel ne constitue pas une approbation ou une recommandation.

## <a name="deploying-web-site-projects"></a>Déploiement de projets de site Web

Contoso University est un projet d’application Web Visual Studio. La plupart des méthodes de déploiement et des outils présentés dans ce didacticiel ne s’appliquent pas aux [projets de site Web](https://msdn.microsoft.com/library/dd547590.aspx). Pour plus d’informations sur le déploiement de projets de site Web, consultez [mappage de contenu de déploiement ASP.net](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Déploiement de projets MVC ASP.NET

Pour ce didacticiel, vous déployez un projet ASP.NET Web Forms, mais tout ce que vous apprenez à faire s’applique également à ASP.NET MVC. Un projet Visual Studio MVC est simplement une autre forme de projet d’application Web. La seule différence est que si vous effectuez un déploiement sur un fournisseur d’hébergement qui ne prend pas en charge ASP.NET MVC ou votre version cible, vous devez vous assurer que vous avez installé le package NuGet ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) ou [MVC 4](http://nuget.org/packages/aspnetmvc)) approprié dans votre projet.

## <a name="programming-language"></a>Langage de programmation

L’exemple d’application C# utilise, mais les didacticiels ne nécessitent C#pas de connaissance de, et les techniques de déploiement présentées par les didacticiels ne sont pas spécifiques à la langue.

## <a name="troubleshooting-during-this-tutorial"></a>Résolution des problèmes au cours de ce didacticiel

Lorsqu’une erreur se produit pendant le déploiement, ou si le site déployé ne s’exécute pas correctement, les messages d’erreur ne fournissent pas toujours une solution. Pour vous aider dans certains scénarios de problèmes courants, une page de référence sur la [résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) est disponible. Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez les didacticiels, veillez à consulter la page de résolution des problèmes.

## <a name="comments-welcome"></a>Commentaires Bienvenue

Les commentaires sur les didacticiels sont les bienvenus et, lorsque le didacticiel est mis à jour, chaque effort sera tenu pour prendre en compte les corrections ou les suggestions relatives aux améliorations fournies dans les commentaires du didacticiel.

## <a name="prerequisites"></a>Configuration requise

Avant de commencer, assurez-vous que vous disposez de Windows 7 ou d’une version ultérieure et que l’un des produits suivants est installé sur votre ordinateur :

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Si vous disposez de Visual Studio 2010 SP1 ou de Visual Web Developer Express 2010 SP1, installez également les produits suivants :

- [Kit de développement logiciel (SDK) Azure pour .net (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (comprend la mise à jour de publication Web)
- [Outils Microsoft Visual Studio 2010 SP1 pour SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

D’autres logiciels sont requis pour suivre le didacticiel, mais vous n’avez pas besoin de le charger. Ce didacticiel vous guide tout au long des étapes d’installation du service informatique lorsque vous en avez besoin.

## <a name="downloading-the-sample-application"></a>Téléchargement de l’exemple d’application

L’application que vous allez déployer est nommée Contoso University et a déjà été créée pour vous. Il s’agit d’une version simplifiée d’un site Web universitaire, basée librement sur l’application Contoso University décrite dans les [didacticiels Entity Framework sur le site ASP.net](https://asp.net/entity-framework/tutorials).

Une fois les composants requis installés, téléchargez l' [application Web Contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). Le fichier *. zip* contient plusieurs versions du projet et un fichier PDF qui contient les 12 didacticiels. Pour suivre les étapes du didacticiel, commencez par ContosoUniversity. Pour voir à quoi ressemble le projet à la fin des didacticiels, ouvrez ContosoUniversity-end. Pour voir à quoi ressemble le projet avant la migration vers la SQL Server complète dans le didacticiel 10, ouvrez ContosoUniversity-AfterTutorial09.

Pour vous préparer à suivre les étapes du didacticiel, enregistrez ContosoUniversity-Begin dans le dossier que vous utilisez pour travailler avec les projets Visual Studio. Par défaut, il s’agit du dossier suivant :

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(Pour les captures d’écran de ce didacticiel, le dossier du projet se trouve dans le répertoire racine sur le lecteur `C`:).

Démarrez Visual Studio, ouvrez le projet, puis appuyez sur CTRL-F5 pour l’exécuter.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Les pages du site Web sont accessibles à partir de la barre de menus et vous permettent d’effectuer les fonctions suivantes :

- Affichez les statistiques des élèves (page à propos de).
- Afficher, modifier, supprimer et ajouter des élèves.
- Affichez et modifiez des cours.
- Affichez et modifiez des enseignants.
- Affichez et modifiez les départements.

Voici les captures d’écran de quelques pages représentatives.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Examen des fonctionnalités de l’application qui affectent le déploiement

Les fonctionnalités suivantes de l’application affectent la manière dont vous la déployez, ou ce que vous devez faire pour la déployer. Chacune d’elles est expliquée plus en détail dans les didacticiels suivants de la série.

- Contoso University utilise une base de données SQL Server Compact pour stocker les données d’application, telles que les noms des étudiants et des enseignants. La base de données contient une combinaison de données de test et de données de production, et lorsque vous déployez en production, vous devez exclure les données de test. Plus loin dans cette série de didacticiels, vous allez migrer de SQL Server Compact vers SQL Server.
- L’application utilise le système d’appartenance ASP.NET, qui stocke les informations de compte d’utilisateur dans une base de données SQL Server Compact. L’application définit un utilisateur administrateur qui a accès à des informations confidentielles. Vous devez déployer la base de données d’appartenance sans comptes de test, mais avec un compte d’administrateur.
- Étant donné que la base de données d’application et la base de données d’appartenance utilisent SQL Server Compact comme moteur de base de données, vous devez déployer le moteur de base de données sur le fournisseur d’hébergement, ainsi que les bases de données elles-mêmes.
- L’application utilise des fournisseurs d’appartenances universels ASP.NET pour que le système d’appartenance puisse stocker ses données dans une base de données SQL Server Compact. L’assembly qui contient les fournisseurs d’appartenances universelles doit être déployé avec l’application.
- L’application utilise la Entity Framework 5,0 pour accéder aux données de la base de données d’application. L’assembly qui contient Entity Framework 5,0 doit être déployé avec l’application.
- L’application utilise un utilitaire de journalisation des erreurs et de création de rapports tiers. Cet utilitaire est fourni dans un assembly qui doit être déployé avec l’application.
- L’utilitaire de journalisation des erreurs écrit des informations d’erreur dans des fichiers XML dans un dossier de fichiers. Vous devez vous assurer que le compte sous lequel ASP.NET s’exécute dans le site déployé dispose d’une autorisation d’écriture sur ce dossier et que vous devez exclure ce dossier du déploiement. (Dans le cas contraire, les données du journal des erreurs de l’environnement de test peuvent être déployées vers des fichiers journaux des erreurs de production et/ou de production peuvent être supprimées.)
- L’application comprend certains paramètres qui doivent être modifiés dans le fichier *Web. config* déployé en fonction de l’environnement de destination (test ou production) et d’autres paramètres qui doivent être modifiés en fonction de la configuration de build (Debug ou Release).
- La solution Visual Studio comprend un projet de bibliothèque de classes. Seul l’assembly généré par ce projet doit être déployé, et non le projet lui-même.

Dans ce premier didacticiel de la série, vous avez téléchargé l’exemple de projet Visual Studio et les fonctionnalités du site revu qui affectent la façon dont vous déployez l’application. Dans les didacticiels suivants, vous préparez le déploiement en configurant certains de ces éléments à traiter automatiquement. D’autres que vous devez prendre en charge manuellement.

> [!div class="step-by-step"]
> [Suivant](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
