---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Création de projets Web ASP.NET dans Visual Studio 2013 | Microsoft Docs
author: tdykstra
description: Cette rubrique explique les options permettant de créer des projets Web ASP.NET dans Visual Studio 2013 avec Update 3 Voici quelques-unes des nouvelles fonctionnalités du développement Web c...
ms.author: riande
ms.date: 12/01/2014
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: fbb4cd7afa2506879d47bce980bf0164aad40c2c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519269"
---
# <a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Création de projets web ASP.NET dans Visual Studio 2013

par [Tom Dykstra](https://github.com/tdykstra)

> Cette rubrique décrit les options de création de projets Web ASP.NET dans Visual Studio 2013 avec Update 3
> 
> Voici quelques-unes des nouvelles fonctionnalités pour le développement Web par rapport aux versions antérieures de Visual Studio :
> 
> - Une interface utilisateur simple pour créer des projets qui offrent une [prise en charge de plusieurs infrastructures ASP.net](#add) (Web Forms, MVC et API Web).
> - [ASP.net Identity](#indauth), un nouveau système d’appartenance ASP.net qui fonctionne de la même façon dans toutes les infrastructures ASP.net et fonctionne avec les logiciels d’hébergement Web autres qu’IIS.
> - Utilisation des [données d’amorçage](#bootstrap) pour fournir des fonctionnalités de conception et de thème réactives.
> - Nouvelles fonctionnalités de Web Forms qui étaient proposées uniquement pour MVC, telles que la [création de projets de test automatique](#testproj) et un [modèle de site intranet](#winauth).
> 
> Pour plus d’informations sur la création de projets Web pour Azure cloud services ou Azure Mobile Services, consultez [prise en main des services Cloud Azure et ASP.net](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) et [création d’une application classement avec Azure Mobile services backend .net](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Prerequisites

Cet article s’applique à [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) avec [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installé.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projets d’application Web et projets de site Web

ASP.NET vous permet de choisir entre deux types de projets Web : les projets d' *application Web* et les *projets de site Web*. Nous recommandons des projets d’application Web pour le nouveau développement, et cet article s’applique uniquement aux projets d’application Web. Pour plus d’informations, consultez [projets d’application Web et projets de site Web dans Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) sur le site MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Vue d’ensemble de la création de projets d’application Web

Les étapes suivantes montrent comment créer un projet Web :

1. Dans la page de **démarrage** ou dans le menu **fichier** , cliquez sur **nouveau projet** .
2. Dans la boîte de dialogue **nouveau projet** , cliquez sur **Web** dans le volet gauche et sur **application Web ASP.net** dans le volet central.

    ![Boîte de dialogue Nouveau projet](creating-web-projects-in-visual-studio/_static/image1.png)

    Vous pouvez choisir **Cloud** dans le volet gauche pour créer un [service Cloud Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)ou [Azure tâche Web](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Cette rubrique ne traite pas de ces modèles.
3. Dans le volet droit, activez la case à cocher **Ajouter des application Insights au projet** si vous souhaitez une surveillance de l’intégrité et de l’utilisation de votre application. Pour plus d’informations, consultez l’article [Analyse des performances dans les applications web](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Spécifiez le **nom**du projet, l' **emplacement**et d’autres options, puis cliquez sur **OK**.

    La boîte **de dialogue Nouveau projet ASP.net** s’affiche.

    ![Boîte de dialogue Nouveau projet](creating-web-projects-in-visual-studio/_static/image2.png)
5. Cliquez sur un modèle.

    ![Sélectionner un modèle](creating-web-projects-in-visual-studio/_static/image3.png)
6. Si vous souhaitez ajouter la prise en charge de frameworks supplémentaires non inclus dans le modèle, cochez la case appropriée. (Dans l’exemple illustré, vous pouvez ajouter MVC et/ou API Web à un projet de Web Forms.)

    ![Ajouter des frameworks](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Si vous souhaitez ajouter un projet de test unitaire, cliquez sur **Ajouter des tests unitaires**.

    ![Ajouter des tests unitaires](creating-web-projects-in-visual-studio/_static/image5.png)
8. Si vous souhaitez utiliser une méthode d’authentification différente de celle fournie par défaut par le modèle, cliquez sur **modifier l’authentification**.

    ![Bouton configurer l’authentification](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Boîte de dialogue Configurer l’authentification](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Créer une application Web ou une machine virtuelle dans Azure

Visual Studio comprend des fonctionnalités qui facilitent l’utilisation des services Azure pour héberger des applications Web. Par exemple, vous pouvez effectuer toutes les opérations suivantes à partir de l’IDE de Visual Studio :

- Créez et gérez des applications Web ou des machines virtuelles qui rendent votre application disponible sur Internet.
- Affichez les journaux créés par l’application pendant qu’elle s’exécute dans le Cloud.
- Exécutez en mode débogage à distance pendant que l’application s’exécute dans le Cloud.
- Affichez et gérez d’autres services Azure, tels que les bases de données SQL.

Vous pouvez [créer un compte Azure](https://www.windowsazure.com/pricing/free-trial/) qui comprend des services de base tels que Web Apps gratuitement et, si vous êtes un abonné MSDN, vous pouvez [activer les avantages](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) qui vous permettent d’obtenir des crédits mensuels sur des services Azure supplémentaires. 

Par défaut, la boîte de dialogue **nouveau projet ASP.net** vous permet de créer une application Web ou une machine virtuelle pour un nouveau projet Web. Si vous ne souhaitez pas créer une nouvelle application Web ou un nouvel ordinateur virtuel, désactivez la case à cocher **hôte dans le Cloud** .

![Créer des ressources distantes](creating-web-projects-in-visual-studio/_static/image8.png)

La légende de la case à cocher peut être un **hôte dans le Cloud** ou **créer des ressources distantes**, et dans les deux cas, l’effet est le même. Si vous laissez la case à cocher activée, Visual Studio crée une application Web dans Azure App Service par défaut. Si vous préférez, vous pouvez utiliser la zone de liste déroulante pour la remplacer par une **machine virtuelle** . Si vous n’êtes pas déjà connecté à Azure, vous êtes invité à fournir des informations d’identification Azure. Une fois que vous êtes connecté, une boîte de dialogue vous permet de configurer les ressources que Visual Studio va créer pour votre projet. L’illustration suivante montre la boîte de dialogue d’une application Web. des options différentes s’affichent si vous choisissez de créer une machine virtuelle.

![Configurer les paramètres de Azure App](creating-web-projects-in-visual-studio/_static/image9.png)

Pour plus d’informations sur l’utilisation de ce processus pour la création de ressources Azure, consultez [prise en main d’Azure et ASP.net](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) et [création d’une machine virtuelle pour un site Web avec Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Le reste de cet article fournit des informations supplémentaires sur les modèles disponibles et leurs options. L’article présente également bootstrap, la disposition et la structure de thèmes utilisés dans les modèles.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 les modèles de projet Web

Visual Studio utilise des modèles pour créer des projets Web. Un modèle de projet peut créer des fichiers et des dossiers dans le nouveau projet, installer des packages NuGet et fournir un exemple de code pour une application de travail rudimentaire. Les modèles implémentent les normes Web les plus récentes et sont destinés à illustrer les meilleures pratiques relatives à l’utilisation des technologies ASP.NET et à vous fournir un point de départ pour créer votre propre application.

Visual Studio 2013 fournit les options suivantes pour les modèles de projet Web pour les projets qui ciblent .NET 4,5 ou des versions ultérieures du .NET Framework :

- [Modèle vide](#empty)
- [Modèle de Web Forms](#wf)
- [Modèle MVC](#mvc)
- [Modèle d’API Web](#webapi)
- [Modèle d’application à page unique](#spa)
- [Modèle Azure Mobile service](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Modèles Visual Studio 2012](#vs2012)

Vous pouvez également installer une extension Visual Studio qui fournit un [modèle Facebook](#facebook).

Pour plus d’informations sur la création de projets ciblant .NET 4, consultez [modèles Visual Studio 2012](#vs2012) plus loin dans cette rubrique.

Pour plus d’informations sur la création d’applications ASP.NET pour les clients mobiles, consultez [prise en charge des appareils mobiles dans ASP.net](../../../mobile/overview.md).

<a id="empty"></a>
### <a name="empty-template"></a>Modèle vide

Le modèle vide fournit le nombre minimal de dossiers et de fichiers pour une application Web ASP.NET, tel qu’un fichier projet ( *. csproj* ou. *vbproj*) et un fichier *Web. config* . Vous pouvez ajouter la prise en charge de Web Forms, MVC et/ou de l’API Web en utilisant les cases à cocher situées sous l’étiquette **Ajouter des dossiers et des références principales pour :** .

Pour le modèle vide, aucune option d’authentification n’est disponible. La fonctionnalité d’authentification est implémentée dans des exemples d’applications, et le modèle vide ne crée pas d’exemple d’application.

<a id="wf"></a>
### <a name="web-forms-template"></a>Modèle de Web Forms

Le Web Forms Framework fournit les fonctionnalités suivantes qui vous permettent de créer rapidement des sites Web riches en fonctionnalités d’interface utilisateur et d’accès aux données :

- Un concepteur WYSIWYG dans Visual Studio.
- Contrôles serveur qui restituent le HTML et que vous pouvez personnaliser en définissant les propriétés et les styles.
- Un large éventail de contrôles pour l’accès aux données et l’affichage des données.
- Modèle d’événement qui expose des événements que vous pouvez programmer comme vous le feriez pour une application cliente telle que WPF.
- Conservation automatique de l’État (données) entre les requêtes HTTP.

En général, la création d’une application Web Forms requiert moins d’effort de programmation que la création de la même application à l’aide de l’infrastructure MVC ASP.NET. Toutefois, Web Forms n’est pas seulement pour le développement rapide d’applications. De nombreuses applications et infrastructures commerciales complexes sont basées sur Web Forms.

Étant donné qu’une page de Web Forms et les contrôles de la page génèrent automatiquement une grande partie du balisage envoyé au navigateur, vous n’avez pas le type de contrôle précis du code HTML proposé par ASP.NET MVC. Le modèle déclaratif pour la configuration des pages et des contrôles réduit la quantité de code que vous devez écrire, mais masque certains des comportements HTML et HTTP. Par exemple, il n’est pas toujours possible de spécifier exactement le balisage qui peut être généré par un contrôle.

L’infrastructure Web Forms ne se prête pas aussi bien que ASP.NET MVC aux pratiques de développement basées sur des modèles, telles que le [développement piloté par test](http://en.wikipedia.org/wiki/Test-driven_development), la [séparation des préoccupations](http://en.wikipedia.org/wiki/Separation_of_concerns), l' [inversion de contrôle](http://en.wikipedia.org/wiki/Inversion_of_control)et l' [injection de dépendances](http://en.wikipedia.org/wiki/Dependency_injection). Si vous souhaitez écrire du code de cette façon, vous pouvez : elle n’est pas aussi automatique que dans l’infrastructure MVC ASP.NET. Le projet [MVP ASP.NET Web Forms](http://webformsmvp.com/) illustre une approche qui facilite la séparation des préoccupations et de la testabilité tout en conservant le développement rapide que Web Forms a été conçu pour fournir. Microsoft SharePoint repose sur Web Forms MVP.

Le modèle Web Forms crée un exemple d’application Web Forms qui utilise [bootstrap](#bootstrap) pour fournir des fonctionnalités de conception et de thème réactives. L’illustration suivante montre la page d’hébergement.

![Page d’hébergement de l’application de modèle Web Forms](creating-web-projects-in-visual-studio/_static/image10.png)

Pour plus d’informations sur Web Forms, consultez [ASP.NET Web Forms](https://asp.net/web-forms). Pour plus d’informations sur ce que fait le modèle Web Forms pour vous, consultez [génération d’une application Web Forms de base à l’aide de Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Modèle MVC

ASP.NET MVC a été conçu pour faciliter les pratiques de développement basées sur des modèles, telles que le [développement piloté par test](http://en.wikipedia.org/wiki/Test-driven_development), la [séparation des préoccupations](http://en.wikipedia.org/wiki/Separation_of_concerns), l' [inversion de contrôle](http://en.wikipedia.org/wiki/Inversion_of_control)et l' [injection de dépendances](http://en.wikipedia.org/wiki/Dependency_injection). Le Framework encourage à séparer la couche de logique métier d’une application Web de sa couche de présentation. En divisant l’application en modèles (M), vues (V) et contrôleurs (C), ASP.NET MVC peut faciliter la gestion de la complexité des applications plus volumineuses.

Avec ASP.NET MVC, vous travaillez plus directement avec HTML et HTTP que dans Web Forms. Par exemple, Web Forms pouvez conserver automatiquement l’état entre les requêtes HTTP, mais vous devez coder explicitement dans MVC. L’avantage du modèle MVC est qu’il vous permet de prendre le contrôle complet sur ce que fait votre application et sur son comportement dans l’environnement Web. L’inconvénient est que vous devez écrire davantage de code.

MVC a été conçu pour être extensible, ce qui permet aux développeurs de pouvoir personnaliser l’infrastructure pour leurs besoins en matière d’applications. En outre, le code source ASP.NET MVC est disponible sous une licence OSI.

Le modèle MVC crée un exemple d’application MVC 5 qui utilise [bootstrap](#bootstrap) pour fournir des fonctionnalités de conception et de thème réactives. L’illustration suivante montre la page d’hébergement.

![Exemple d’application MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Pour plus d’informations sur MVC, consultez [ASP.NET MVC](https://asp.net/mvc). Pour plus d’informations sur la façon de sélectionner le modèle MVC 4, consultez [modèles Visual Studio 2012](#vs2012) plus loin dans cet article.

<a id="webapi"></a>
### <a name="web-api-template"></a>Modèle d’API Web

Le modèle d’API Web crée un exemple de service Web basé sur l’API Web, y compris les pages d’aide d’API basées sur MVC.

ASP.NET Web API est une infrastructure qui facilite le développement de services HTTP disponibles sur un large éventail de clients, tels que des navigateurs et des appareils mobiles. API Web ASP.NET est une plateforme idéale pour la création de services RESTful sur le .NET Framework.

Le modèle d’API Web crée un exemple de service Web. Les illustrations suivantes présentent des exemples de pages d’aide.

![Page d’aide de l’API Web](creating-web-projects-in-visual-studio/_static/image12.png)

![Page d’aide de l’API Web pour obtenir l’API](creating-web-projects-in-visual-studio/_static/image13.png)

Pour plus d’informations sur l’API Web, consultez [API Web ASP.net](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Single Page Application Template (Modèle d’application à page unique)

Le modèle d’application à page unique (SPA) crée un exemple d’application qui utilise JavaScript, HTML 5 et [KnockoutJS](http://knockoutjs.com/) sur le client, et API Web ASP.net sur le serveur.

La seule option d’authentification pour le modèle SPA est un [compte d’utilisateur individuel](#indauth).

L’illustration suivante montre l’état initial de l’exemple d’application généré par le modèle SPA.

![Exemple d’application SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Pour plus d’informations sur la création d’une application à l’aide du modèle SPA, consultez [API Web-services d’authentification externe](../../../web-api/overview/security/external-authentication-services.md).

Pour plus d’informations sur les applications à page unique ASP.NET et sur les modèles SPA supplémentaires qui utilisent des infrastructures JavaScript autres que KnockoutJS, consultez les ressources suivantes :

- [Application à page unique ASP.net](../../../single-page-application/index.md).
- [Comprendre les fonctionnalités de sécurité dans le modèle SPA pour VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Applications à page unique : créez des Web Apps modernes et réactives avec ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Modèle Facebook

Vous pouvez installer une [extension Visual Studio qui fournit un modèle Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Ce modèle crée un exemple d’application conçu pour s’exécuter à l’intérieur du site Web Facebook. Il est basé sur ASP.NET MVC et utilise l’API Web pour la fonctionnalité de mise à jour en temps réel.

Aucune option d’authentification n’est disponible pour le modèle Facebook, car les applications Facebook s’exécutent dans le site Facebook et s’appuient sur l’authentification de Facebook.

Pour plus d’informations sur les applications ASP.NET Facebook, consultez [mise à jour de l’API Facebook de MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Modèles Visual Studio 2012

La boîte de dialogue de création de projet Web Visual Studio 2013 ne permet pas d’accéder à certains modèles qui étaient disponibles dans Visual Studio 2012. Si vous souhaitez utiliser l’un de ces modèles, vous pouvez cliquer sur le nœud Visual Studio 2012 dans le volet gauche de la boîte de dialogue Nouveau projet de Visual Studio.

![Modèles Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

Le nœud **Visual Studio 2012** vous permet de choisir les modèles Web suivants auxquels vous n’avez pas accès dans la liste de modèles par défaut pour Visual Studio 2013 :

- Application Web ASP.NET MVC 4
- Application Web ASP.NET Dynamic Data Entities
- Contrôle serveur ASP.NET AJAX
- Extendeur de contrôle serveur ASP.NET AJAX
- Contrôle serveur ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap dans les modèles de projet Web Visual Studio 2013

Les modèles de projet Visual Studio 2013 utilisent [bootstrap](http://getbootstrap.com/), une disposition et une infrastructure de thèmes créés par Twitter. Bootstrap utilise CSS3 pour fournir une conception réactive, ce qui signifie que les dispositions peuvent s’adapter de manière dynamique à différentes tailles de fenêtre de navigateur. Par exemple, dans une fenêtre de navigateur étendue, la page d’hébergement créée par le modèle Web Forms ressemble à l’illustration suivante :

![Page d’hébergement de l’application de modèle Web Forms](creating-web-projects-in-visual-studio/_static/image16.png)

Rendez la fenêtre plus étroite et les colonnes disposées horizontalement se déplacent dans l’organisation verticale :

![Configuration de colonne verticale d’amorçage](creating-web-projects-in-visual-studio/_static/image17.png)

Affinez la fenêtre un peu plus et le menu supérieur horizontal devient une icône sur laquelle vous pouvez cliquer pour développer dans un menu orienté verticalement :

![Icône du menu de démarrage](creating-web-projects-in-visual-studio/_static/image18.png)

![Menu vertical d’amorçage](creating-web-projects-in-visual-studio/_static/image19.png)

Vous pouvez également utiliser la fonctionnalité de thèmes d’amorçage pour effectuer facilement une modification de l’apparence de l’application. Par exemple, vous pouvez effectuer les étapes suivantes pour modifier le thème.

1. Dans votre navigateur, accédez à [http://Bootswatch.com](http://Bootswatch.com), choisissez un thème, puis cliquez sur **Télécharger**. (Cela télécharge *bootstrap. min. CSS* par défaut ; si vous souhaitez examiner le code CSS, récupérez *bootstrap. CSS* au lieu de la version minimisés.)
2. Copiez le contenu du fichier CSS téléchargé.
3. Dans Visual Studio, créez un nouveau fichier de **feuille de style** nommé *bootstrap-Theme. CSS* dans le dossier *content* et collez-y le code CSS téléchargé.
4. Ouvrez l' *application\_démarrer/bundle. config* et remplacez *bootstrap. CSS* par *bootstrap-Theme. CSS*.

Réexécutez le projet et l’application a une nouvelle apparence. L’illustration suivante montre l’effet du thème Amelia :

![Thème Amelia de démarrage](creating-web-projects-in-visual-studio/_static/image20.png)

De nombreux thèmes d’amorçage sont disponibles, à la fois pour les versions gratuite et Premium. Bootstrap offre également un large éventail de composants d’interface utilisateur, tels que les [listes déroulantes](http://twitter.github.io/bootstrap/components.html#dropdowns), les [groupes de boutons](http://twitter.github.io/bootstrap/components.html#buttonGroups)et les [icônes](http://twitter.github.io/bootstrap/base-css.html#images). Pour plus d’informations sur les données d’amorçage, consultez [le site bootstrap](http://twitter.github.io/bootstrap/).

Si vous utilisez le concepteur de Web Forms dans Visual Studio, Notez que le concepteur ne prend pas en charge CSS3, de sorte qu’il n’affiche pas avec précision tous les effets des thèmes de démarrage ou des modifications de disposition réactives. Toutefois, les pages de Web Forms s’affichent correctement lorsqu’elles sont affichées avec un navigateur.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Ajout de la prise en charge d’infrastructures supplémentaires

Lorsque vous sélectionnez un modèle, la case à cocher correspondant aux frameworks utilisés par le modèle est automatiquement sélectionnée. Par exemple, si vous sélectionnez le modèle **Web Forms** , la case à cocher **Web Forms** est activée et vous ne pouvez pas l’effacer.

![Sélectionner un modèle](creating-web-projects-in-visual-studio/_static/image21.png)

![Ajouter des frameworks](creating-web-projects-in-visual-studio/_static/image22.png)

Vous pouvez activer la case à cocher pour une infrastructure qui n’est pas incluse dans le modèle afin d’ajouter la prise en charge de cette infrastructure lorsque le projet est créé. Par exemple, pour activer l’utilisation des pages Web Forms *. aspx* lorsque vous avez sélectionné le modèle MVC, activez la case à cocher **Web Forms** . Ou pour activer MVC lorsque vous utilisez le modèle Web Forms, activez la case à cocher **MVC** . L’ajout d’une infrastructure permet la prise en charge au moment de la conception et de l’exécution. Par exemple, si vous ajoutez la prise en charge de MVC à un projet de Web Forms, vous serez en mesure de générer des vues et des contrôleurs de structure.

Si vous combinez Web Forms et MVC dans un projet et que vous activez des [URL conviviales](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) dans Web Forms, il peut y avoir des problèmes de routage inattendus où une URL a plusieurs cibles possibles. Les itinéraires qui sont définis en premier sont prioritaires. Par exemple, si vous disposez d’un contrôleur de `Home` et d’une page page d' *hébergement. aspx* , l’url du `http://contoso.com/home` sera dans le dossier de *démarrage. aspx* si vous appelez la méthode `EnableFriendlyUrls` avant d’appeler la méthode `MapRoute` dans *ROUTECONFIG.cs*, ou la même URL passera à l’affichage par défaut de votre contrôleur `Home` si vous appelez `MapRoute` avant `EnableFriendlyUrls`.

L’ajout d’une infrastructure n’ajoute pas d’exemple de fonctionnalité d’application. Par exemple, si vous ajoutez la prise en charge de Web Forms lorsque vous avez sélectionné le modèle MVC, aucun fichier de page d’hébergement *. aspx par défaut* n’est créé. Seuls les dossiers, les fichiers et les références nécessaires à la prise en charge de l’infrastructure sont ajoutés. Pour cette raison, l’ajout de frameworks ne modifie pas les options d’authentification, qui sont implémentées par le code dans les exemples d’applications créées par les modèles. Par exemple, si vous sélectionnez le modèle vide et ajoutez Web Forms ou la prise en charge MVC, le bouton **configurer l’authentification** sera toujours désactivé.

Les sections suivantes décrivent brièvement l’effet de chaque case à cocher.

### <a name="add-web-forms-support"></a>Ajouter la prise en charge de Web Forms

Crée une *application vide\_* dossiers de données et de *modèles* et un fichier *global. asax* . Ces modèles sont déjà créés par tous les modèles autres que le modèle vide. ainsi, si vous activez la case à cocher Web Forms, aucune différence n’est constatée pour les autres modèles.

Le modèle Web Forms permet des URL conviviales par défaut, mais lorsque vous ajoutez la prise en charge de la Web Forms à d’autres modèles en sélectionnant l’Web Forms les URL conviviales ne sont pas automatiquement activées.

### <a name="add-mvc-support"></a>Ajouter la prise en charge MVC

Installe les packages NuGet MVC, Razor et Web pages, crée des dossiers d' *application\_de données*, de *contrôleurs*, de *modèles*et de *vues* vides, crée l' *application\_* dossier de démarrage avec le fichier *RouteConfig.cs* et crée un fichier *global. asax* .

### <a name="add-web-api-support"></a>Ajouter la prise en charge de l’API Web

Installe les packages NuGet WebApi et Newtonsoft. JSON, crée des dossiers d' *application\_de données*, de *contrôleurs*et de *modèles* d’application vides, crée l' *application\_* dossier de démarrage avec le fichier *WebApiConfig.cs* et crée le fichier *global. asax* .

<a id="auth"></a>
## <a name="authentication-methods"></a>Méthodes d'authentification

Visual Studio 2013 propose plusieurs options d’authentification pour les modèles de Web Forms, MVC et d’API Web :

- [Aucune authentification](#noauth)
- [Comptes d’utilisateur individuels](#indauth) (ASP.net Identity, anciennement appelé ASP.NET Membership)
- [Comptes organisationnels](#orgauth) (Windows Server Active Directory ou Azure Active Directory)
- [Authentification Windows](#winauth) (intranet)

![Boîte de dialogue Configurer l’authentification](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Aucune authentification

Si vous sélectionnez **aucune authentification**, l’exemple d’application ne contient aucune page Web pour la connexion, aucune interface utilisateur indiquant qui est connecté, aucune classe d’entité pour une base de données d’appartenance et aucune chaîne de connexion pour une base de données d’appartenance.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Comptes d'utilisateur individuels

Si vous sélectionnez **des comptes d’utilisateur individuels**, l’exemple d’application sera configuré pour utiliser ASP.net Identity (anciennement appelé ASP.NET Membership) pour l’authentification utilisateur. ASP.NET Identity permet à un utilisateur d’inscrire un compte, en créant un nom d’utilisateur et un mot de passe sur le site ou en se connectant avec des fournisseurs de réseaux sociaux tels que Facebook, Google, un compte Microsoft ou Twitter. Le magasin de données par défaut pour les profils utilisateur dans ASP.NET Identity est une base de données de base de données locale SQL Server, que vous pouvez déployer sur SQL Server ou Azure SQL Database pour le site de production.

Dans Visual Studio 2013 ces fonctionnalités sont les mêmes que dans Visual Studio 2012, mais le code sous-jacent pour le système d’appartenance ASP.NET a été réécrit. Les avantages de la nouvelle base de code sont les suivants :

- Le nouveau système d’appartenance est basé sur [OWIN](http://owin.org/) plutôt que sur le module ASP.NET Forms Authentication. Cela signifie que vous pouvez utiliser le même mécanisme d’authentification que vous utilisiez Web Forms ou MVC dans IIS, ou que vous auto-hébergementiez l’API Web ou Signalr.
- La nouvelle base de données d’appartenance est gérée par Entity Framework Code First et toutes les tables sont représentées par des classes d’entité que vous pouvez modifier. Cela signifie que vous pouvez facilement personnaliser le schéma de base de données et l’interface utilisateur Web relative au profil pour répondre à vos besoins, et vous pouvez facilement déployer vos mises à jour à l’aide de Migrations Code First.

Le nouveau système d’appartenance est implémenté automatiquement dans les nouveaux modèles, et il peut être implémenté manuellement dans n’importe quel projet qui cible .NET 4,5 ou une version ultérieure.

ASP.NET Identity est un bon choix si vous créez un site Web Internet principalement destiné à des clients externes. Si votre organisation utilise Active Directory ou Office 365 et que vous souhaitez créer un projet qui autorise l’authentification unique pour les employés et les partenaires commerciaux, l’option **comptes organisationnels** peut être un meilleur choix.

Pour plus d’informations sur l’option comptes d’utilisateur individuels, consultez les ressources suivantes :

- [www.asp.net/identity](../../../identity/index.md). Documentation relative à ASP.NET Identity sur le site Web ASP.NET.
- [Créez une application ASP.NET MVC 5 avec Facebook et Google OAuth2 et l’authentification OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Montre également comment personnaliser les données de profil utilisateur.
- [API Web-services d’authentification externe](../../../web-api/overview/security/external-authentication-services.md)
- [Ajout de connexions externes à votre application ASP.NET dans Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Comptes professionnels

Si vous sélectionnez **comptes**professionnels, l’exemple d’application sera configuré pour utiliser wif (Windows Identity Foundation) pour l’authentification basée sur les comptes d’utilisateur dans Azure Active Directory (Azure ad, qui comprend Office 365) ou Windows Server Active Directory. Pour plus d’informations, consultez [options d’authentification de compte organisationnel](#orgauthoptions) plus loin dans cette rubrique.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Authentification Windows

Si vous sélectionnez **l’authentification Windows**, l’exemple d’application sera configuré pour utiliser le module IIS d’authentification Windows pour l’authentification. L’application affiche l’ID de domaine et d’utilisateur du compte Active Directory ou de l’ordinateur local qui est connecté à Windows, mais n’inclut pas l’inscription de l’utilisateur ou l’interface utilisateur de connexion. Cette option est destinée aux sites Web intranet.

Vous pouvez également créer un site intranet qui utilise l’authentification Active Directory en choisissant l' [option local sous comptes](#orgauthonprem)professionnels. L’option locale utilise Windows Identity Foundation (WIF) au lieu du module d’authentification Windows. Certaines étapes supplémentaires sont nécessaires pour configurer l’option locale, mais WIF active les fonctionnalités qui ne sont pas disponibles avec le module d’authentification Windows. Par exemple, avec WIF, vous pouvez configurer l’accès aux applications dans Active Directory et interroger les données d’annuaire.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Options d’authentification du compte professionnel

La boîte de dialogue **configurer l’authentification** offre plusieurs options pour Azure Active Directory (Azure ad, qui inclut Office 365) ou l’authentification du compte de Active Directory Windows Server (AD) :

- [Cloud-organisation unique](#orgauthsingle) (Azure ad ou ad utilisant l’intégration d’annuaire avec Azure AD)
- [Cloud-plusieurs organisations](#orgauthmulti) (Azure ad ou ad utilisant l’intégration d’annuaire avec Azure AD)
- [Local](#orgauthonprem) (AD)

Si vous souhaitez essayer l’une des options de Azure AD mais que vous n’avez pas encore de compte, [cliquez ici pour vous inscrire à un compte Azure ad](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Si vous choisissez l’une des options de Azure AD, votre projet nécessite une base de données et vous devez vous connecter à un compte d’administrateur général pour votre locataire Azure AD. Entrez le nom et le mot de passe d’un compte professionnel (par exemple, admin@contoso.onmicrosoft.com) disposant des autorisations d’administration pour votre client Azure AD.
> 
> **N’entrez pas les informations d’identification d’un compte Microsoft (par exemple, contoso@hotmail.com) dans la boîte de dialogue de connexion.**

<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud-authentification d’une seule organisation

![Authentification d’une seule organisation](creating-web-projects-in-visual-studio/_static/image24.png)

Choisissez cette option si vous souhaitez activer l’authentification pour les comptes d’utilisateurs définis dans un [locataire](https://technet.microsoft.com/library/jj573650.aspx)Azure ad. Par exemple, le site est contoso.com et il est mis à la disposition des employés de la société Contoso qui se trouvent dans le locataire contoso.onmicrosoft.com. Vous ne pouvez pas configurer Azure AD pour permettre aux utilisateurs d’autres locataires d’accéder à l’application.

#### <a name="domain"></a>Domaine

Entrez le domaine de Azure AD dans lequel vous souhaitez configurer l’application, par exemple : `contoso.onmicrosoft.com`. Si vous avez un [domaine personnalisé](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), tel que `contoso.com` au lieu de `contoso.onmicrosoft.com`, vous pouvez l’entrer ici.

#### <a name="access-level"></a>Niveau d’accès

Si l’application doit interroger ou mettre à jour les informations d’annuaire à l’aide de l’API Graph, choisissez **authentification unique, lire les données d’annuaire** ou **authentification unique, lire et écrire des données d’annuaire**. Dans le cas contraire, choisissez **authentification unique**. Pour plus d’informations, consultez [niveaux d’accès aux applications](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) et [utilisation du API Graph pour interroger des Azure ad](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>URI d’ID d’application

Par défaut, le modèle crée un URI d’ID d’application pour vous en ajoutant le nom du projet au domaine Azure AD. Par exemple, si le nom du projet est `Example` et que le domaine est `contoso.onmicrosoft.com`, l’URI de l’ID d’application devient `https://contoso.onmicrosoft.com/Example`. Si vous souhaitez spécifier manuellement l’URI de l’ID d’application, développez la section **autres options** et entrez l’URI de l’ID d’application dans la zone de texte. L’URI de l’ID d’application doit commencer par `https://`.

Par défaut, si une application déjà approvisionnée dans Azure AD a le même URI d’ID d’application que celui utilisé par Visual Studio pour le projet, le projet sera connecté à l’application existante au lieu d’en approvisionner un nouveau. Si vous souhaitez qu’une nouvelle application soit approvisionnée dans ce cas, désactivez la case à cocher **remplacer l’entrée d’application si une des mêmes ID existe déjà** .

Si la case à cocher **remplacer** est désactivée et que Visual Studio trouve une application existante avec le même URI d’ID d’application, il crée un nouvel URI en ajoutant un nombre à l’URI qu’il devait utiliser. Par exemple, supposons que le nom du projet soit `Example`, que vous ne renseignez pas la zone de texte, que vous désactivez la case à cocher **remplacer** et que le locataire Azure ad a déjà une application avec l’URI `https://contoso.onmicrosoft.com/Example`. Dans ce cas, une nouvelle application sera approvisionnée avec un URI d’ID d’application comme `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Configuration de l’application dans Azure AD

Pour configurer l’application dans Azure AD ou pour connecter le projet à une application existante, Visual Studio a besoin des informations d’identification d’un administrateur global pour le domaine. Lorsque vous cliquez sur **OK** dans la boîte de dialogue **configurer l’authentification** , vous êtes invité à entrer le nom d’utilisateur et le mot de passe d’un administrateur global pour le domaine que vous avez spécifié. Plus tard, lorsque vous cliquerez sur **créer un projet** dans la boîte de dialogue **nouveau projet ASP.net** , Visual Studio configure l’application dans Azure ad. Notez que dans le cadre de ce processus, Visual Studio incorpore les valeurs de clé secrète client dans le fichier Web. config qui expirent un an après la création.

Pour plus d’informations sur la création d’applications qui utilisent l’authentification **de l’organisation unique** dans le Cloud, consultez les ressources suivantes :

- [Authentification Azure](../2012/windows-azure-authentication.md)
- [Ajout de l’authentification à votre application web à l’aide d’Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Développement d’applications ASP.NET avec Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Sécuriser les API Web ASP.NET avec les composants Azure AD et Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Les didacticiels n’ont pas encore été mis à jour pour Visual Studio 2013 ; certains des didacticiels vous permettant d’effectuer manuellement les opérations sont automatisés dans Visual Studio 2013.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud-authentification multi-Organisation

![Authentification de plusieurs organisations](creating-web-projects-in-visual-studio/_static/image25.png)

Choisissez cette option si vous souhaitez activer l’authentification pour les comptes d’utilisateurs définis dans plusieurs [locataires](https://technet.microsoft.com/library/jj573650.aspx)Azure ad. Par exemple, le site est contoso.com et il est mis à la disposition des employés de la société Contoso qui se trouvent dans le locataire contoso.onmicrosoft.com, ainsi que des employés de la société Fabrikam qui se trouvent dans le locataire fabrikam.onmicrosoft.com.

Les paramètres que vous entrez et l’étape de configuration de l’application sont similaires à [l’authentification d’une seule organisation](#orgauthsingle).

Pour plus d’informations sur la création d’applications qui utilisent l’authentification **multiorganisationnelle** dans le Cloud, consultez les ressources suivantes :

- [Intégration facile des applications Web avec Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) sur le blog de l’équipe Active Directory.
- [Développement d’applications Web mutualisées avec Azure ad](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) didacticiel. Le didacticiel n’a pas encore été mis à jour pour Visual Studio 2013 ; une partie de ce que le didacticiel vous demande de faire manuellement est automatisée dans Visual Studio 2013.
- [Vous devez vous inscrire auprès de votre propre application ASP.net plusieurs organisations pour pouvoir vous connecter](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog de Vittorio Bertocci qui explique comment résoudre un problème courant rencontré lors de la création d’un projet qui utilise l’authentification multi-organisation.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Authentification d’organisation locale

![Authentification d’organisation locale](creating-web-projects-in-visual-studio/_static/image26.png)

Choisissez cette option si vous souhaitez activer l’authentification pour les comptes d’utilisateur définis dans Windows Server Active Directory (AD) et que vous ne souhaitez pas utiliser Azure AD. Vous pouvez utiliser cette option pour créer un site intranet ou un site Internet. Pour un site Internet, utilisez Services ADFS (ADFS) pour fournir l’accès à Active Directory. Pour plus d’informations, consultez [utiliser l’option d’authentification organisationnelle locale (ADFS) avec ASP.net dans Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Pour un site intranet, vous pouvez également choisir [authentification Windows](#winauth) au lieu de cette option. Pour l’option d’authentification Windows, vous n’avez pas besoin de fournir une URL de document de métadonnées. Toutefois, l’authentification Windows ne vous donne pas la possibilité de contrôler l’accès aux applications dans Active Directory ou d’interroger les données d’annuaire.

#### <a name="on-premises-authority"></a>Autorité locale

Entrez une URL qui pointe vers le document de métadonnées. Le document de métadonnées contient les coordonnées de l’autorité. L’application utilisera ces coordonnées pour conduire le passage de l’authentification Web.

#### <a name="application-id-uri"></a>URI d’ID d’application

Fournissez un URI unique qu’Active Directory peut utiliser pour identifier cette application ou laissez-le vide pour permettre à Visual Studio d’en créer un.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Étapes suivantes :

Ce document a fourni une aide de base pour la création d’un projet Web ASP.NET dans Visual Studio 2013. Pour plus d’informations sur l’utilisation de pour Visual Studio pour le développement Web, consultez [https://www.asp.net/visual-studio/](../../index.md).
