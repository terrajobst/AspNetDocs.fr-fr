---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : introduction | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou un fournisseur d’hébergement tiers, à l’aide de la méthode V...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 96dd31d949633e001fc595621bedbf74e98000fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640238"
---
# <a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : introduction

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 avec le kit de développement logiciel (SDK) Azure pour .NET. La plupart des procédures sont similaires pour Visual Studio 2013.
> 
> Vous développez une application Web afin de la mettre à la disposition des personnes sur Internet. Mais les didacticiels de programmation Web s’arrêtent généralement juste après qu’ils vous ont montré comment faire fonctionner votre ordinateur de développement. Cette série de didacticiels commence là où les autres ne quittent pas : vous avez créé une application Web, vous l’avez testée et celle-ci est prête à l’emploi. Étapes suivantes Ces didacticiels vous montrent comment déployer en premier sur IIS sur votre ordinateur de développement local à des fins de test, puis auprès d’Azure ou d’un fournisseur d’hébergement tiers pour la mise en lots et la production. L’exemple d’application que vous allez déployer est un projet d’application Web qui utilise le Entity Framework, SQL Server et le système d’appartenance ASP.NET. L’exemple d’application utilise ASP.NET Web Forms, mais les procédures indiquées s’appliquent également à ASP.NET MVC et à l’API Web.
> 
> Ces didacticiels partent du principe que vous savez utiliser ASP.NET dans Visual Studio. Si ce n’est pas le cas, il est recommandé de commencer par un [didacticiel Web Forms de base ASP.net](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) ou un [didacticiel ASP.NET MVC de base](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [Forum de déploiement ASP.net](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) ou sur [StackOverflow](http://stackoverflow.com).
> 
> Ce contenu est également disponible en tant que livre électronique gratuit dans [la Galerie de livres électroniques TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).

## <a name="overview"></a>Vue d'ensemble de

Ces didacticiels vous guident tout au long du déploiement d’une application Web ASP.NET incluant des bases de données SQL Server. Vous allez d’abord effectuer un déploiement sur IIS sur votre ordinateur de développement local à des fins de test, puis Web Apps dans Azure App Service et Azure SQL Database pour la mise en lots et la production. Vous verrez comment déployer à l’aide de la publication en un clic de Visual Studio, et vous verrez comment déployer à l’aide de la ligne de commande.

Le nombre de didacticiels peut rendre le processus de déploiement décourageant. En fait, les procédures de base sont simples. Toutefois, dans les situations réelles, vous devez souvent effectuer des tâches de déploiement supplémentaires, par exemple, définir des autorisations sur le serveur cible. Nous avons montré quelques-unes de ces tâches supplémentaires, dans l’espoir que les didacticiels n’ignorent pas les informations susceptibles de vous empêcher de déployer avec succès une application réelle.

Les didacticiels sont conçus pour s’exécuter en séquence, et chaque partie repose sur le composant précédent. Vous pouvez ignorer des parties qui ne sont pas pertinentes pour votre situation, mais vous devrez peut-être ajuster les procédures dans les didacticiels ultérieurs.

## <a name="intended-audience"></a>Public visé

Les didacticiels sont destinés aux développeurs ASP.NET qui travaillent dans des environnements où :

- L’environnement de production est Azure App Service Web Apps ou un fournisseur d’hébergement tiers.
- Le déploiement n’est pas limité à un processus d’intégration continue, mais peut être effectué directement à partir de Visual Studio.

Le déploiement à partir du [contrôle de code source](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) à l’aide d’un processus de [livraison continue](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) n’est pas abordé dans ces didacticiels, à l’exception d’un didacticiel qui montre comment déployer à partir de la ligne de commande. Pour plus d’informations sur la livraison continue, consultez les ressources suivantes :

- [Intégration continue et livraison continue (création d’applications Cloud réalistes avec Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Déployer une application Web dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Déploiement d’applications Web dans des scénarios d’entreprise](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (un ancien ensemble de didacticiels écrits pour Visual Studio 2010, qui contient toujours des informations utiles pour les environnements d’entreprise.)

## <a name="using-a-third-party-hosting-provider"></a>Utilisation d’un fournisseur d’hébergement tiers

Les didacticiels vous guident tout au long du processus de configuration d’un compte Azure et du déploiement de l’application sur Web Apps dans Azure App Service pour la mise en lots et la production. Toutefois, vous pouvez utiliser les mêmes procédures de base pour le déploiement sur un fournisseur d’hébergement tiers de votre choix. Là où les didacticiels dépassent les processus propres à Azure, ils expliquent comment et indiquent les différences que vous pouvez attendre auprès d’un fournisseur d’hébergement tiers.

## <a name="deploying-web-app-projects"></a>Déploiement de projets d’application Web

L’exemple d’application que vous téléchargez et déployez pour ces didacticiels est un projet d’application Web Visual Studio. Toutefois, si vous installez la dernière mise à jour de publication Web pour Visual Studio, vous pouvez utiliser les mêmes outils et méthodes de déploiement pour les projets d’application Web.

## <a name="deploying-aspnet-mvc-projects"></a>Déploiement de projets MVC ASP.NET

L’exemple d’application est un projet ASP.NET Web Forms, mais tout ce que vous apprenez à faire s’applique également à ASP.NET MVC. Un projet Visual Studio MVC est simplement une autre forme de projet d’application Web. La seule différence est que si vous effectuez un déploiement sur un fournisseur d’hébergement qui ne prend pas en charge ASP.NET MVC ou votre version cible, vous devez vous assurer que vous avez installé le package NuGet ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)ou [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) approprié dans votre projet.

## <a name="programming-language"></a>Langage de programmation

L’exemple d’application C# utilise, mais les didacticiels ne nécessitent C#pas de connaissance de, et les techniques de déploiement présentées par les didacticiels ne sont pas spécifiques à la langue.

## <a name="database-deployment-methods"></a>Méthodes de déploiement de base de données

Il existe trois façons de déployer une base de données SQL Server en même temps que le déploiement Web dans Visual Studio :

- Migrations Entity Framework Code First
- Fournisseur de Web Deploy dbDacFx
- Fournisseur de Web Deploy dbFullSql

Dans ce didacticiel, vous allez utiliser les deux premières méthodes. Le fournisseur dbFullSql Web Deploy est une méthode héritée qui n’est plus recommandée, à l’exception de certains scénarios spécifiques, tels que la migration de SQL Server Compact vers SQL Server.

Les méthodes présentées dans ce didacticiel concernent SQL Server bases de données, et non SQL Server Compact. Pour plus d’informations sur le déploiement d’une base de données SQL Server Compact, consultez [déploiement Web Visual Studio avec SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Les méthodes présentées dans ce didacticiel requièrent l’utilisation de la méthode de publication Web Deploy. Si vous préférez une méthode de publication différente, telle que FTP, le système de fichiers ou FPSE, consultez [déploiement d’une base de données séparément du déploiement d’applications Web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) dans le plan de contenu de déploiement Web pour Visual Studio et ASP.net.

### <a name="entity-framework-code-first-migrations"></a>Migrations Entity Framework Code First

Dans la version 4,3 de Entity Framework, Microsoft a introduit Migrations Code First. Migrations Code First automatise le processus d’apport de modifications incrémentielles à un modèle de données et de propagation de ces modifications à la base de données. Dans les versions antérieures de Code First, vous laissez généralement le Entity Framework supprimer et recréer la base de données chaque fois que vous modifiez le modèle de données. Il ne s’agit pas d’un problème de développement, car les données de test sont facilement recréées, mais en production, vous souhaitez généralement mettre à jour le schéma de base de données sans supprimer la base de données. La fonctionnalité migrations permet à Code First de mettre à jour la base de données sans la supprimer ni la recréer. Vous pouvez laisser Code First décider automatiquement de la manière de modifier le schéma requis, ou vous pouvez écrire du code qui personnalise les modifications. Pour une présentation de Migrations Code First, consultez [migrations code First](https://msdn.microsoft.com/library/hh770484.aspx).

Lorsque vous déployez un projet Web, Visual Studio peut automatiser le processus de déploiement d’une base de données gérée par Migrations Code First. Lorsque vous créez le profil de publication, vous activez une case à cocher intitulée exécuter Migrations Code First (s’exécute au démarrage de l’application). Ce paramètre fait que le processus de déploiement configure automatiquement le fichier Web. config de l’application sur le serveur de destination afin que Code First utilise la classe d’initialiseur `MigrateDatabaseToLatestVersion`.

Visual Studio ne fait rien avec la base de données pendant le processus de déploiement. Lorsque l’application déployée accède à la base de données pour la première fois après le déploiement, Code First crée automatiquement la base de données ou met à jour le schéma de base de données vers la dernière version. Si l’application implémente une méthode Seed migrations, la méthode s’exécute après la création de la base de données ou le schéma est mis à jour.

Dans ce didacticiel, vous allez utiliser Migrations Code First pour déployer la base de données d’application.

### <a name="the-dbdacfx-web-deploy-provider"></a>Fournisseur de Web Deploy dbDacFx

Pour une base de données SQL Server qui n’est pas gérée par Entity Framework Code First, vous pouvez activer une case à cocher intitulée mettre à jour la base de données lorsque vous configurez le profil de publication. Lors du déploiement initial, le fournisseur dbDacFx crée des tables et d’autres objets de base de données dans la base de données de destination pour qu’ils correspondent à la base de données source. Lors des déploiements suivants, le fournisseur détermine ce qui est différent entre les bases de données source et de destination, et il met à jour le schéma de la base de données de destination pour qu’il corresponde à la base de données source. Par défaut, le fournisseur n’apporte aucune modification entraînant une perte de données, par exemple lorsqu’une table ou une colonne est supprimée.

Cette méthode n’automatise pas le déploiement de données dans les tables de base de données, mais vous pouvez créer des scripts pour ce faire et configurer Visual Studio pour les exécuter pendant le déploiement. Une autre raison pour exécuter des scripts au cours du déploiement consiste à apporter des modifications de schéma qui ne peuvent pas être effectuées automatiquement, car elles entraînent une perte de données.

Dans ce didacticiel, vous allez utiliser le fournisseur dbDacFx pour déployer la base de données d’appartenance ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Résolution des problèmes au cours de ce didacticiel

Lorsqu’une erreur se produit pendant le déploiement, ou si le site déployé ne s’exécute pas correctement, les messages d’erreur ne fournissent pas toujours une solution évidente. Pour vous aider dans certains scénarios de problèmes courants, une page de référence sur la [résolution des problèmes](troubleshooting.md) est disponible. Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez les didacticiels, veillez à consulter la page de résolution des problèmes.

## <a name="comments-welcome"></a>Commentaires Bienvenue

Les commentaires sur les didacticiels sont les bienvenus et, lorsque le didacticiel est mis à jour, chaque effort sera tenu pour prendre en compte les corrections ou les suggestions relatives aux améliorations fournies dans les commentaires du didacticiel.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Configuration requise

Ce didacticiel a été rédigé pour les produits suivants :

- Windows 8 ou Windows 7.
- Visual Studio 2012 ou Visual Studio 2012 Express pour le Web avec [la dernière mise à jour](https://go.microsoft.com/fwlink/?LinkId=272486).
- [Kit de développement logiciel (SDK) Azure pour Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Vous pouvez suivre le didacticiel à l’aide de Visual Studio 2010 SP1 ou Visual Studio 2013, mais certaines captures d’écran seront différentes et certaines fonctionnalités seront différentes.

Si vous utilisez Visual Studio 2013, installez le [Kit de développement logiciel (SDK) Azure pour Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Si vous utilisez Visual Studio 2010 SP1, installez les logiciels suivants :

- [Kit de développement logiciel (SDK) Azure pour Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express de base de données locale](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

En fonction du nombre de dépendances du kit de développement logiciel (SDK) que vous avez déjà sur votre ordinateur, l’installation du kit de développement logiciel (SDK) Azure peut prendre un certain temps, de plusieurs minutes à une demi-heure ou plus. Vous avez besoin du kit de développement logiciel (SDK) Azure même si vous envisagez de publier sur un fournisseur d’hébergement tiers plutôt que sur Azure, car le kit de développement logiciel (SDK) comprend les dernières mises à jour des fonctionnalités de publication Web de Visual Studio.

> [!NOTE]
> Ce didacticiel a été écrit avec la version 1.8.1 du kit de développement logiciel (SDK) Azure. Depuis, les versions plus récentes avec des fonctionnalités supplémentaires ont été publiées. Les didacticiels ont été mis à jour pour mentionner ces fonctionnalités et les liens vers les ressources qui contiennent davantage d’informations à leur sujet.

Les instructions et les captures d’écran sont basées sur Windows 8, mais les didacticiels expliquent les différences pour Windows 7.

D’autres logiciels sont requis pour suivre le didacticiel, mais vous n’avez pas besoin d’installer ce logiciel encore. Ce didacticiel vous guide tout au long des étapes d’installation du service informatique lorsque vous en avez besoin.

## <a name="download-the-sample-application"></a>Télécharger l’exemple d’application

L’application que vous déploierez est nommée Contoso University et a déjà été créée pour vous. Il s’agit d’une version simplifiée d’un site Web universitaire, basée librement sur l’application Contoso University décrite dans les [didacticiels Entity Framework sur le site ASP.net](https://asp.net/entity-framework/tutorials).

Une fois les composants requis installés, téléchargez l' [application Web Contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). Le fichier *. zip* contient plusieurs versions du projet. Pour suivre les étapes du didacticiel, commencez par le projet situé dans le C# dossier. Pour voir à quoi ressemble le projet à la fin des didacticiels, ouvrez le projet dans le dossier ContosoUniversity-end.

Pour préparer le projet en vue de suivre les étapes du didacticiel, procédez comme suit :

1. Enregistrez les fichiers solution ContosoUniversity à partir C# du dossier dans un dossier nommé ContosoUniversity dans le dossier que vous utilisez pour travailler avec les projets Visual Studio.

    Par défaut, il s’agit du dossier suivant pour Visual Studio 2012 :

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (Pour les captures d’écran de ce didacticiel, le dossier du projet se trouve dans le répertoire racine sur le lecteur `C`:).
2. Démarrez Visual Studio et ouvrez le projet.
3. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis cliquez sur **EnableNuGet package Restore**.
4. Générez la solution.
5. Si vous recevez des erreurs de compilation, restaurez manuellement les packages NuGet :

    1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution, puis cliquez sur **gérer les packages NuGet pour la solution**.
    2. Dans la partie supérieure de la boîte de dialogue **gérer les packages NuGet** , vous verrez **des packages NuGet absents de cette solution. Cliquez pour restaurer.** Cliquez sur le bouton **restaurer** .
    3. Régénérez la solution.
6. Appuyez sur CTRL-F5 pour exécuter l'application.

    L’application s’ouvre sur la page d’hébergement de la société Contoso University.

    ![Page d’hébergement dev](introduction/_static/image1.png)

    (Il peut y avoir un délai d’attente pendant que Visual Studio démarre l’instance SQL Server Express de la base de données locale, et vous pouvez obtenir une erreur de délai d’attente si ce processus prend trop de temps. Dans ce cas, redémarrez le projet.)

Les pages du site Web sont accessibles à partir de la barre de menus et vous permettent d’effectuer les fonctions suivantes :

- Affichez les statistiques des élèves (page à propos de).
- Afficher, modifier, supprimer et ajouter des élèves.
- Affichez et modifiez des cours.
- Affichez et modifiez des enseignants.
- Affichez et modifiez les départements.

Voici les captures d’écran de quelques pages représentatives.

![Page des étudiants dev](introduction/_static/image2.png)

![Page Ajouter des élèves-dev](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Examiner les fonctionnalités de l’application qui affectent le déploiement

Les fonctionnalités suivantes de l’application affectent la manière dont vous la déployez, ou ce que vous devez faire pour la déployer. Chacune d’elles est expliquée plus en détail dans les didacticiels suivants de la série.

- Contoso University utilise une base de données SQL Server pour stocker les données d’application, telles que les noms des étudiants et des enseignants. La base de données contient une combinaison de données de test et de données de production, et lorsque vous déployez en production, vous devez exclure les données de test.
- L’application utilise le système d’appartenance ASP.NET, qui stocke les informations de compte d’utilisateur dans une base de données SQL Server. L’application définit un utilisateur administrateur qui a accès à des informations confidentielles. Vous devez déployer la base de données d’appartenance sans comptes de test, mais avec un compte d’administrateur.
- L’application utilise un utilitaire de journalisation des erreurs et de création de rapports tiers. Cet utilitaire est fourni dans un assembly qui doit être déployé avec l’application.
- L’utilitaire de journalisation des erreurs écrit des informations d’erreur dans des fichiers XML dans un dossier de fichiers. Vous devez vous assurer que le compte sous lequel ASP.NET s’exécute dans le site déployé dispose d’une autorisation d’écriture sur ce dossier et que vous devez exclure ce dossier du déploiement. (Dans le cas contraire, les données du journal des erreurs de l’environnement de test peuvent être déployées vers des fichiers journaux des erreurs de production et/ou de production peuvent être supprimées.)
- L’application comprend certains paramètres qui doivent être modifiés dans le fichier *Web. config* déployé en fonction de l’environnement de destination (test, intermédiaire ou de production) et d’autres paramètres qui doivent être modifiés en fonction de la configuration de build (Debug ou Release).
- La solution Visual Studio comprend un projet de bibliothèque de classes. Seul l’assembly généré par ce projet doit être déployé, et non le projet lui-même.

## <a name="summary"></a>Récapitulatif

Dans ce premier didacticiel de la série, vous avez téléchargé l’exemple de projet Visual Studio et les fonctionnalités du site revu qui affectent la façon dont vous déployez l’application. Dans les didacticiels suivants, vous préparez le déploiement en configurant certains de ces éléments à traiter automatiquement. D’autres que vous devez prendre en charge manuellement.

> [!div class="step-by-step"]
> [Suivant](preparing-databases.md)
