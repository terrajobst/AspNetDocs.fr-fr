---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: Notes de publication de ASP.NET et Web Tools 2013,2 pour Visual Studio 2013 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 22d4d4afd6963f23d6cfef1745a859c20b69d599
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623192"
---
# <a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET et Web Tools 2013.2 pour Visual Studio 2013 - Notes de publication

par [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notes d'installation

ASP.NET et Web Tools pour Visual Studio 2013,2 sont regroupés dans le programme d’installation principal et peuvent être téléchargés dans le cadre de [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentation

Des didacticiels et d’autres informations sur ASP.NET et Web Tools pour Visual Studio 2013,2 sont disponibles sur le [site Web ASP.net](https://www.asp.net/).

## <a name="software-requirements"></a>Configuration logicielle

ASP.NET et Web Tools pour Visual Studio 2013,2 requiert Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nouvelles fonctionnalités de ASP.NET et Web Tools pour Visual Studio 2013,2

Les sections suivantes décrivent les fonctionnalités qui ont été introduites dans la version.

- [Un modèle de projet ASP.NET](#oneaspnet)
- [Prendre en charge SSL lors du lancement d’applications Web sur IIS Express](#ssl)
- [Améliorations de l’éditeur Web de Visual Studio](#vswebeditor)
- [Lien du navigateur](#browserlink)
- [Prise en charge des Web Apps Azure App Service dans Visual Studio](#waws)
- [Créer des ressources Azure distantes lors de la création d’un projet Web](#AzureResources)
- [Améliorations de la publication Web](#webpublish)
- [Génération de modèles automatique ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web Forms](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [API Web ASP.NET 2.1.2](#webapi)
- [Pages Web ASP.NET 3.1.2](#webpages)
- [Entity Framework 6,1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Composants Microsoft OWIN](#owin)
- [Signaleur ASP.NET 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Un modèle de projet ASP.NET

- Mises à jour des modèles de projet ASP.NET pour prendre en charge la confirmation de compte et la réinitialisation de mot de passe.
- Mettez à jour API Web ASP.NET modèle pour prendre en charge l’authentification à l’aide de comptes professionnels locaux.
- Le modèle SPA ASP.NET contient maintenant l’authentification basée sur les vues MVC et côté serveur. Le modèle a un contrôleur WebAPI qui n’est accessible qu’aux utilisateurs authentifiés.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Prendre en charge SSL lors du lancement d’applications Web sur IIS Express

Pour éliminer l’avertissement de sécurité lors de la navigation et du débogage HTTPs sur localhost, nous avons ajouté une boîte de dialogue permettant à Internet Explorer et chrome d’approuver le certificat SSL Express auto-signé.

Par exemple, une propriété de projet Web peut être définie pour utiliser SSL. Cliquez sur F4 pour afficher la boîte de dialogue Propriétés. Remplacez **SSL activé** par true. Copiez l’URL SSL.

![Propriété SSL Enabled](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Définissez l’onglet Web de la page de propriétés du projet Web pour utiliser l’URL HTTPs (l’URL SSL sera `https://localhost:44300/`, sauf si vous avez déjà créé des sites Web SSL).

![Définir l’URL du projet (HTTPs)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Appuyez sur CTRL+F5 pour exécuter l'application. Suivez les instructions pour approuver le certificat auto-signé que IIS Express a généré.

![AVERTISSEMENT SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lisez la boîte de dialogue **avertissement de sécurité** , puis cliquez sur **Oui** si vous souhaitez installer le certificat représentant localhost.

![Avertissement de sécurité](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Le site s’affiche dans IE ou chrome sans l’avertissement de certificat dans le navigateur.

![Page HTTPs sans avertissements](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox utilise son propre magasin de certificats, donc un avertissement s’affiche.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Améliorations de l’éditeur Web de Visual Studio

- **Nouvel éditeur et élément de projet JSON**: nous avons ajouté un élément et un éditeur de projet JSON à Visual Studio. Les fonctionnalités actuelles de l’éditeur JSON incluent la colorisation, la validation de la syntaxe, la saisie semi-automatique des accolades, le mode plan, le paramètre des options des outils, etc.

    ![Éditeur JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense prend désormais en charge les [schémas JSON](http://json-schema.org/) v3 et v4. Il existe une zone de liste déroulante de schéma qui permet de choisir des schémas existants, de modifier le chemin d’accès du schéma local ou de simplement faire glisser un fichier projet JSON vers ce dernier pour obtenir le chemin d’accès relatif.

    ![IntelliSense de JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Éditeur de schéma JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nouvel éditeur Sass (SCSS)** : nous avons ajouté moins dans VS2013 RTM, et nous disposons désormais d’un élément de projet et d’un éditeur Sass. Les fonctionnalités de l’éditeur Sass sont comparables à celles de l’éditeur, et incluent colorisation, variable et Mixins IntelliSense, commenter/annuler les marques de commentaire, info Express, mise en forme, validation de la syntaxe, mode plan, atteindre la définition, sélecteur de couleurs, paramètre d’option outils, etc.

    ![Ajouter un nouvel élément : feuille de style SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Éditeur de feuille de style](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nouveau sélecteur d’URL dans les documents html, Razor, CSS, Less et Sass :** VS 2013 fourni sans sélecteur d’URL en dehors des pages Web Forms. Le nouveau sélecteur d’URL pour les éditeurs HTML, Razor, CSS, LESS et Sass est un sélecteur de frappe sans boîte de dialogue qui comprend'.. ' et filtre les listes de fichiers en fonction des liens et des balises IMG.

    ![Sélecteur d’URL pour la balise d’image](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Sélecteur d’URL pour les vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Sélecteur d’URL pour CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Met à jour un éditeur plus petit en ajoutant des fonctionnalités**
- **Mise à niveau de Knockout IntelliSense**: nous avons ajouté une syntaxe Knockout non standard pour vs IntelliSense, la syntaxe « Ko-vs-Editor ViewModel : ». Il peut être utilisé pour effectuer une liaison à plusieurs modèles de vue sur une page à l’aide de commentaires sous la forme :

    ![IntelliSense masqué](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Nous avons également ajouté la prise en charge de la fonctionnalité IntelliSense du ViewModel imbriqué. vous pouvez donc affiner les objets profondément imbriqués sur le ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    La fonctionnalité IntelliSense affichée correspond à la fonctionnalité IntelliSense complète de l’objet JavaScript.

    ![IntelliSense présentant l’intégralité de l’objet JavaScript](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nouveau sélecteur d’URL dans les documents html, Razor, CSS, Less et Sass**: vs 2013 fourni sans sélecteur d’URL en dehors des pages Web Forms. Le nouveau sélecteur d’URL pour les éditeurs HTML, Razor, CSS, LESS et Sass est un sélecteur de frappe sans boîte de dialogue qui comprend'.. ' et filtre les listes de fichiers en fonction des liens et des balises IMG.

    ![Sélecteur d’URL pour la balise d’image](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Sélecteur d’URL pour les vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Sélecteur d’URL pour CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Lien du navigateur

- Le lien du navigateur prend désormais en charge les connexions HTTPs et répertorie ce tableau dans le tableau de bord avec d’autres connexions, à condition que le certificat soit approuvé par le navigateur.
- Mappage de source HTML statique
- Prise en charge du SPA pour le mappage des données
- Mettre à jour automatiquement les données de mappage

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Prise en charge des Web Apps Azure App Service dans Visual Studio

- **Prenez en charge la connexion à Azure.**
- **Débogage distant et vue à distance pour les applications Web**: nous prenons désormais en charge le [débogage à distance pour les applications Web dans Azure App service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) et l’affichage à distance des fichiers de contenu d’application Web dans l’Explorateur de serveurs.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Créer des ressources Azure distantes lors de la création d’un projet Web

Nous avons ajouté une case à cocher [« créer des ressources distantes »](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) dans la boîte de dialogue nouvelle application Web. En le sélectionnant, vous serez en mesure d’intégrer l’expérience de création d’une application Web, de configurer le site de publication Azure à des fins de test et de créer un profil de publication en quelques étapes simples.

![Nouveau projet avec les ressources Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publication sur Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Améliorations de la publication Web

- Améliorez l’expérience utilisateur pour la publication.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

- **Prise en charge des énumérations :** Si votre modèle utilise des enums, le générateur de modèles MVC génère la liste déroulante pour Enum. Cela utilise les applications d’assistance d’énumération dans MVC.
- **Prise en charge du démarrage**: mise à jour des modèles EditorFor dans la génération de modèles automatique MVC afin qu’ils utilisent les classes bootstrap.
- **Prise en charge des packages**: les générateurs de modèles d’API Web et MVC ajoutent des packages 5,1 pour MVC et l’API Web

Les captures d’écran suivantes illustrent les modèles de génération de modèles automatique.

- Code du modèle :

     ![Code du modèle](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilez le code du modèle, cliquez avec le bouton droit, puis sélectionnez **Ajouter**, **nouvel élément de génération de modèles**automatique.

     ![Ajouter un nouvel élément de génération de modèles automatique](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Choisissez **contrôleur MVC5 avec vues, à l’aide de Entity Framework**:

     ![Ajouter un nouveau contrôleur MVC5 avec des vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Ajouter un contrôleur** à l’aide du modèle :

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Vérifiez le code généré, par exemple views/WeekdayModels/Edit. cshtml, qui contient `@Html.EnumDropDownListFor`: ![vue contenant EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Exécutez la page pour voir la liste déroulante des énumérations générées. Notez que si une valeur peut être null, une chaîne vide peut être choisie pour la zone de liste déroulante. Par exemple, la page **créer** affiche ce qui suit :

    ![Zone de liste déroulante autorisant une chaîne vide](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM sera publié en avril 2014. Voici les points saillants des notes de publication, mais consultez les [notes de publication complètes](http://docs.nuget.org/docs/release-notes/nuget-2.8) pour plus d’informations sur ces modifications.

- **Applications cibles Windows Phone 8,1**: NuGet 2.8.1 prend désormais en charge le ciblage des applications Windows Phone 8,1 à l’aide des monikers du Framework cible’WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81 'et’WPA81 '.

- **Résolution des correctifs pour les dépendances**: lors de la résolution des dépendances de packages, NuGet a implémenté historiquement une stratégie de sélection de la version de package principale et mineure la plus faible qui satisfait les dépendances sur le package. Toutefois, contrairement à la version majeure et la version mineure, la version du correctif a toujours été résolue à la version la plus élevée. Bien que le comportement ait été correctement intentionnel, il a créé un manque de déterminisme pour l’installation des packages avec des dépendances.
- **Commutateur DependencyVersion**: bien que NuGet 2,8 modifie le comportement *par défaut* pour la résolution des dépendances, il ajoute également un contrôle plus précis sur le processus de résolution de dépendance via le commutateur-DependencyVersion dans la console du gestionnaire de package. Le commutateur permet de résoudre les dépendances sur la version la plus basse possible (comportement par défaut), la version la plus élevée possible, ou la version mineure ou correctif la plus élevée. Ce commutateur fonctionne uniquement pour Install-Package dans la commande PowerShell.
- **Attribut DependencyVersion**: outre le commutateur-DependencyVersion détaillé ci-dessus, NuGet a également autorisé la possibilité de définir un nouvel attribut dans le fichier NuGet. config en définissant la valeur par défaut, si le commutateur-DependencyVersion n’est pas spécifié dans un appel de Install-Package. Cette valeur est également respectée par la boîte de dialogue du gestionnaire de package NuGet pour les opérations de package d’installation. Pour définir cette valeur, ajoutez l’attribut ci-dessous à votre fichier NuGet. config :

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Aperçu des opérations NuGet avec-WhatIf**: certains packages NuGet peuvent avoir des graphiques de dépendance profonde et, par conséquent, ils peuvent être utiles lors d’une opération d’installation, de désinstallation ou de mise à jour pour voir d’abord ce qui se passe. NuGet 2,8 ajoute le commutateur PowerShell-What If par défaut aux commandes install-package, Uninstall-package et Update-Package pour permettre la visualisation de la fermeture complète des packages auxquels la commande sera appliquée.
- Mise à niveau du **package**: il n’est pas rare d’installer une version préliminaire d’un package pour étudier les nouvelles fonctionnalités, puis décider de restaurer la dernière version stable. Avant NuGet 2,8, il s’agissait d’un processus à plusieurs étapes de la désinstallation du package de la version préliminaire et de ses dépendances, puis de l’installation de la version antérieure. Toutefois, avec NuGet 2,8, le package de mise à jour restaurera la fermeture complète du package (par exemple, l’arborescence des dépendances du package) à la version précédente.
- **Dépendances de développement**: de nombreux types différents de fonctionnalités peuvent être fournis sous forme de packages NuGet, y compris les outils utilisés pour optimiser le processus de développement. Ces composants, bien qu’ils puissent être Instrumentals dans le développement d’un nouveau package, ne doivent pas être considérés comme une dépendance du nouveau package lorsqu’il est publié ultérieurement. NuGet 2,8 permet à un package de s’identifier dans le fichier. NuSpec en tant que developmentDependency. Lorsqu’elles sont installées, ces métadonnées sont également ajoutées au fichier Packages. config du projet dans lequel le package a été installé. Quand ce fichier Packages. config est analysé ultérieurement pour les dépendances NuGet au cours du package NuGet. exe, il exclut les dépendances marquées comme dépendances de développement.
- **Fichiers de package. config individuels pour différentes plateformes**: lors du développement d’applications pour plusieurs plateformes cibles, il est courant d’avoir des fichiers projet différents pour chacun des environnements de génération respectifs. Il est également courant de consommer différents packages NuGet dans différents fichiers projet, car les packages ont des niveaux différents de prise en charge pour différentes plateformes. NuGet 2,8 fournit une prise en charge améliorée pour ce scénario en créant différents fichiers Packages. config pour différents fichiers projet spécifiques à la plateforme.
- **Basculement vers le cache local**: bien que les packages NuGet soient généralement consommés à partir d’une galerie distante telle que la [galerie NuGet](http://www.nuget.org) à l’aide d’une connexion réseau, il existe de nombreux scénarios dans lesquels le client n’est pas connecté. Sans connexion réseau, le client NuGet n’a pas pu installer correctement les packages, même si ces packages étaient déjà sur l’ordinateur du client dans le cache NuGet local. NuGet 2,8 ajoute le secours de cache automatique à la console du gestionnaire de package.

    La fonctionnalité de secours du cache ne requiert pas d’arguments de commande spécifiques. En outre, le secours du cache ne fonctionne actuellement que dans la console du gestionnaire de package : le comportement ne fonctionne pas dans la boîte de dialogue du gestionnaire de package.
- **Correctifs de bogues**: l’un des principaux correctifs de bogues apportés était l’amélioration des performances dans la commande Update-Package-REINSTALL.

    Outre ces fonctionnalités et les correctifs de performances susmentionnés, cette version de NuGet comprend également de nombreux autres correctifs de bogues. 181 problèmes ont été résolus dans la version. Pour obtenir la liste complète des éléments de travail corrigés dans NuGet 2,8, consultez le [suivi des problèmes NuGet](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) pour cette version.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web Forms

- Les modèles de Web Forms montrent maintenant comment effectuer une confirmation de compte et une réinitialisation de mot de passe pour ASP.NET Identity.
- Le contrôle de source de données d’entité et le fournisseur de Dynamic Data pour Entity Framework 6. Pour plus d’informations, consultez le blog MSDN suivant : [Dynamic Data fournisseur et EntityDataSource Control pour Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Améliorations du routage des attributs](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Prise en charge des démarrages pour les modèles d’éditeur](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Prise en charge des énumérations dans les vues](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Prise en charge discrète des attributs MinLength/MaxLength](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Prise en charge du contexte « This » dans Ajax discrète](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Plusieurs [correctifs de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>API Web ASP.NET 2.1.2

- [Gestion globale des erreurs](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Améliorations du routage des attributs](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Améliorations de la page d’aide](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Support IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formateur de type de média BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Meilleure prise en charge des filtres asynchrones](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Analyse des requêtes pour la bibliothèque de mise en forme du client](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Plusieurs [correctifs de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>Pages Web ASP.NET 3.1.2

- Plusieurs [correctifs de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6,1

Entity Framework a été mis à jour vers la version 6,1 pour le runtime et les outils. Entity Framework (EF) 6,1 est une mise à jour mineure de Entity Framework 6 et comprend un certain nombre de correctifs de bogues et de nouvelles fonctionnalités. Pour plus d’informations sur EF 6.1, y compris des liens vers la documentation sur les nouvelles fonctionnalités, consultez [Entity Framework l’historique des versions](https://msdn.microsoft.com/data/jj574253). Les nouvelles fonctionnalités de cette version sont les suivantes :

- La **consolidation des outils** offre un moyen cohérent de créer un modèle EF. Cette fonctionnalité étend l’Assistant Entity Data Model ADO.NET pour prendre en charge la création de modèles de Code First, y compris l’ingénierie à rebours à partir d’une base de données existante. Ces fonctionnalités étaient auparavant disponibles en version bêta dans EF Power Tools.
- La **gestion des échecs de validation de transaction** fournit le nouvel [System. Data. Entity. infrastructure. CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) qui utilise la capacité récemment introduite pour intercepter les opérations de transaction. Le **CommitFailureHandler** permet la récupération automatique à partir des échecs de connexion pendant la validation d’une transaction.
- **IndexAttribute** permet de spécifier des index en plaçant un attribut sur une propriété (ou des propriétés) dans votre modèle de code First. Code First crée alors un index correspondant dans la base de données.
- **L’API de mappage public** fournit l’accès aux informations EF sur la manière dont les propriétés et les types sont mappés aux colonnes et aux tables de la base de données. Dans les versions antérieures, cette API était interne.
- **Possibilité de configurer des intercepteurs via le fichier app/Web. config**(ce qui permet d’ajouter des intercepteurs sans recompiler l’application).
- **DatabaseLogger** est un nouvel intercepteur qui facilite l’enregistrement de toutes les opérations de base de données dans un fichier. En association avec la fonctionnalité précédente, cela vous permet de basculer facilement la journalisation des opérations de base de données pour une application déployée, sans avoir à recompiler.
- La **détection des modifications du modèle de migrations** a été améliorée afin que les migrations par génération de modèles automatique soient plus précises. les performances du processus de détection des modifications ont également été améliorées.
- **Améliorations des performances** , y compris les opérations de base de données réduites pendant l’initialisation, les optimisations pour la comparaison d’égalité null dans les requêtes LINQ, la génération d’affichages plus rapide (création de modèle) dans plus de scénarios et la matérialisation plus efficace des entités suivies avec plusieurs associations.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Authentification à deux facteurs**: ASP.net Identity prend désormais en charge l’authentification à deux facteurs. L’authentification à deux facteurs fournit une couche supplémentaire de sécurité à vos comptes d’utilisateur dans le cas où votre mot de passe est compromis. Il existe également une protection contre les attaques par force brute contre les codes à deux facteurs.
- **Verrouillage de compte :** Fournit un moyen de verrouiller l’utilisateur si l’utilisateur entre son mot de passe ou ses codes à deux facteurs de manière incorrecte. Le nombre de tentatives non valides et le délai d’attente pour les utilisateurs peuvent être configurés. Un développeur peut éventuellement désactiver le verrouillage de compte pour certains comptes d’utilisateur, si nécessaire.
- **Confirmation du compte :** Le système ASP.NET Identity prend désormais en charge la confirmation de compte. Il s’agit d’un scénario assez courant dans la plupart des sites Web où, lorsque vous vous inscrivez pour un nouveau compte sur le site Web, vous devez confirmer votre adresse de messagerie pour pouvoir faire quoi que ce soit sur le site Web. La confirmation par courrier électronique est utile, car elle empêche la création de comptes erronés. Cela est très utile si vous utilisez la messagerie électronique comme méthode de communication avec les utilisateurs de votre site Web, tels que les sites de forum, les services bancaires, le commerce électronique ou les sites Web sociaux.
- **Réinitialisation du mot de passe :** La réinitialisation de mot de passe est une fonctionnalité permettant à l’utilisateur de réinitialiser son mot de passe s’il a oublié son mot de passe.
- **Tampon de sécurité (déconnexion Everywhere) :** Prend en charge un moyen de régénérer le jeton de sécurité pour l’utilisateur lorsque l’utilisateur modifie son mot de passe ou toute autre information relative à la sécurité, telle que la suppression d’une connexion associée (par exemple, Facebook, Google, compte Microsoft, etc.). Cela est nécessaire pour garantir que tous les jetons générés avec l’ancien mot de passe sont invalidés. Dans l’exemple de projet, si vous modifiez le mot de passe de l’utilisateur, un nouveau jeton est généré pour l’utilisateur et tous les jetons précédents sont invalidés. Cette fonctionnalité fournit une couche supplémentaire de sécurité à votre application, car lorsque vous modifiez votre mot de passe, vous êtes déconnecté de partout (tous les autres navigateurs) où vous vous êtes connecté à cette application.
- **Rendre le type de clé primaire extensible pour les utilisateurs et les rôles**: dans ASP.net Identity 1,0, le type de clé primaire pour les utilisateurs et les rôles de table était des chaînes. Cela signifie que lorsque le système de ASP.NET Identity a été conservé dans SQL Server à l’aide de Entity Framework, nous utilisons nvarchar. Il y avait de nombreuses discussions autour de cette implémentation par défaut sur Stack Overflow et sur la base des commentaires entrants. Nous avons fourni un crochet d’extensibilité dans lequel vous pouvez spécifier ce qui doit être la clé primaire de la table des utilisateurs et des rôles. Ce raccordement d’extensibilité est particulièrement utile si vous migrez votre application et que l’application stockant les UserIds sont des GUID ou des ints.
- **Prendre en charge IQueryable sur les utilisateurs et les rôles**: ajout de la prise en charge de IQueryable sur UsersStore et RolesStore, vous pouvez facilement obtenir la liste des utilisateurs et des rôles.
- **Prendre en charge l’opération de suppression via UserManager**
- **Indexation sur le nom d’utilisateur**: dans ASP.net Identity Entity Framework implémentation, nous avons ajouté un index unique sur le nom d’utilisateur à l’aide du nouveau INDEXATTRIBUTE dans EF 6.1.0. Cela permet de s’assurer que les noms d’utilisateur sont toujours uniques et qu’il n’y avait pas de condition de concurrence critique dans laquelle vous pourriez vous retrouver avec des noms d’utilisateur en double.
- **Validateur de mot de passe amélioré :** Le validateur de mot de passe qui a été fourni dans ASP.NET Identity 1,0 était un validateur de mot de passe assez basique qui validait uniquement la longueur minimale. Il existe un nouveau validateur de mot de passe qui vous permet de mieux contrôler la complexité du mot de passe. Notez que même si vous activez tous les paramètres de ce mot de passe, nous vous encourageons à activer l’authentification à deux facteurs pour les comptes d’utilisateur.
- **Intergiciel IdentityFactory/CreatePerOwinContext**:

    - **Gestionnaire des utilisateurs**: vous pouvez utiliser l’implémentation de fabrique pour obtenir une instance de usermanager à partir du contexte OWIN. Ce modèle est similaire à ce que nous utilisons pour obtenir AuthenticationManager à partir du contexte OWIN pour la connexion et la déconnexion. Il s’agit d’une méthode recommandée pour obtenir une instance de UserManager par demande pour l’application.
    - **DbContextFactory**: ASP.NET Identity utilise Entity Framework pour rendre le système d’identité persistant dans SQL Server. Pour ce faire, le système d’identité a une référence à ApplicationDbContext. L’intergiciel DbContextFactory retourne une instance du ApplicationDbContext par requête que vous pouvez utiliser dans votre application.
- **Package NuGet exemples ASP.net Identity**: le package NuGet Samples peut faciliter l’installation et l’exécution d’exemples pour ASP.net Identity et suivre les meilleures pratiques. Il s’agit d’un exemple d’application MVC ASP.NET. Modifiez le code pour l’adapter à votre application avant de la déployer en production. L’exemple doit être installé dans une application ASP.NET vide. Pour plus d’informations sur le package, consultez le billet de blog suivant : [annonce de la publication de la version RTM de ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Composants Microsoft OWIN

De nombreux bogues ont été résolus dans cette version. Pour plus d’informations, consultez les [notes de publication de la version 2.1.0](https://katanaproject.codeplex.com/releases/view/113281) .

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>Signaleur ASP.NET 2.0.2

De nombreux bogues ont été résolus dans cette version. Pour obtenir des informations plus détaillées, consultez les [notes de publication de la version 2.0.2](https://github.com/SignalR/SignalR/releases/tag/2.0.2) .
