---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ce document décrit la version de ASP.NET MVC 4 bêta pour Visual Studio 2010.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 17800dfe091bbb7afb25f7f41e3bd885b882edb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523302"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> Ce document décrit la version de ASP.NET MVC 4 bêta pour Visual Studio 2010.
> 
> > [!NOTE]
> > Il ne s’agit pas de la version la plus récente. Les notes de publication de ASP.NET MVC 4 RC sont disponibles [ici](mvc4-release-notes.md).

- [Notes d’installation](#_Toc303253802)
- [Documentation](#_Toc303253803)
- [Support](#_Toc303253804)
- [Configuration logicielle requise](#_Toc303253805)
- [Mise à niveau d’un projet ASP.NET MVC 3 vers ASP.NET MVC 4](#_Toc303253806)
- [Nouvelles fonctionnalités dans ASP.NET MVC 4 bêta](#_Toc303253807)

    - [API Web ASP.NET](#_Toc317096197)
    - [Application à page unique ASP.NET](#_Toc317096198)
    - [Améliorations apportées aux modèles de projet par défaut](#_Toc303253808)
    - [Modèle de projet mobile](#_Toc303253809)
    - [Modes d’affichage](#_Toc303253810)
    - [jQuery mobile, le sélecteur de vue et le remplacement du navigateur](#_Toc303253811)
    - [Recettes pour la génération de code dans Visual Studio](#_Toc303253812)
    - [Prise en charge des tâches pour les contrôleurs asynchrones](#_Toc303253813)
    - [Kit de développement logiciel Azure](#_Toc303253814)
    - [Problèmes connus et modifications avec rupture](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notes d'installation

ASP.NET MVC 4 Beta pour Visual Studio 2010 peut être installé à partir de la [page d’hébergement ASP.NET MVC 4](../mvc/mvc4.md) à l’aide de la Web Platform Installer.

Vous devez désinstaller les versions préliminaires précédemment installées de ASP.NET MVC 4 avant d’installer ASP.NET MVC 4 bêta.

Cette version n’est pas compatible avec la version préliminaire du développeur .NET Framework 4,5. Vous devez désinstaller .NET 4,5 developer preview avant d’installer ASP.NET MVC 4 Beta.

ASP.NET MVC 4 peut être installé et peut s’exécuter côte à côte avec ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentation

La documentation relative à ASP.NET MVC est disponible sur le site Web MSDN à l'adresse suivante :

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Des didacticiels et d’autres informations sur ASP.NET MVC sont disponibles sur la page MVC 4 du site Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Assistance

Il s’agit d’une version préliminaire qui n’est pas officiellement prise en charge. Si vous avez des questions sur l’utilisation de cette version, publiez-les sur le Forum ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), où les membres de la communauté ASP.net sont souvent en mesure de fournir un support informel.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Configuration logicielle

Les composants ASP.NET MVC 4 pour Visual Studio requièrent PowerShell 2,0 et Visual Studio 2010 avec Service Pack 1 ou Visual Web Developer Express 2010 avec Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Mise à niveau d’un projet ASP.NET MVC 3 vers ASP.NET MVC 4

ASP.NET MVC 4 peut être installé côte à côte avec ASP.NET MVC 3 sur le même ordinateur, ce qui vous donne la possibilité de choisir quand mettre à niveau une application ASP.NET MVC 3 vers ASP.NET MVC 4.

La façon la plus simple de procéder à la mise à niveau consiste à créer un nouveau projet ASP.NET MVC 4 et à copier tous les affichages, les contrôleurs, le code et les fichiers de contenu du projet MVC 3 existant vers le nouveau projet, puis à mettre à jour les références d’assembly dans le nouveau projet pour qu’elles correspondent à l’ancien projet. Si vous avez apporté des modifications au fichier Web. config dans le projet MVC 3, vous devez également fusionner ces modifications dans le fichier Web. config dans le projet MVC 4.

Pour mettre à niveau manuellement une application ASP.NET MVC 3 existante vers la version 4, procédez comme suit :

1. Dans tous les fichiers Web. config du projet (il en existe un dans la racine du projet, l’un dans le dossier Views et l’autre dans le dossier views pour chaque zone de votre projet), remplacez chaque instance du texte suivant :

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    avec le texte correspondant suivant :

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. Dans le fichier Web. config racine, mettez à jour l’élément Web *pages : version* sur « 2.0.0.0 » et ajoutez une nouvelle clé *PreserveLoginUrl* avec la valeur « true » :

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. Dans Explorateur de solutions, supprimez la référence à *System. Web. Mvc* (qui pointe vers la dll de la version 3). Ajoutez ensuite une référence à *System. Web. Mvc* (v 4.0.0.0). En particulier, apportez les modifications suivantes pour mettre à jour les références d’assembly. Voici les détails :

    1. Dans Explorateur de solutions, supprimez les références aux assemblys suivants : 

        - *System. Web. Mvc*(v 3.0.0.0)
        - *System. Web. webpages*(v 1.0.0.0)
        - *System. Web. Razor*(v 1.0.0.0)
        - *System. Web. webpages. Deployment*(v 1.0.0.0)
        - *System. Web. webpages. Razor*(v 1.0.0.0)
    2. Ajoutez des références aux assemblys suivants : 

        - *System. Web. Mvc*(v 4.0.0.0)
        - *System. Web. webpages*(v 2.0.0.0)
        - *System. Web. Razor*(v 2.0.0.0)
        - *System. Web. webpages. Deployment*(v 2.0.0.0)
        - *System. Web. webpages. Razor*(v 2.0.0.0)
4. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet, puis sélectionnez décharger le projet. Cliquez ensuite avec le bouton droit sur le nom et sélectionnez Modifier *NomProjet*. csproj.
5. Recherchez l’élément *ProjectTypeGuids* et remplacez {E53F8FEA-EAE0-44A6-8774-FFD645390401} par {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Enregistrez les modifications, fermez le fichier projet (. csproj) que vous avez modifié, cliquez avec le bouton droit sur le projet, puis sélectionnez recharger le projet.
7. Si le projet fait référence à des bibliothèques tierces compilées à l’aide de versions antérieures de ASP.NET MVC, ouvrez le fichier Web. config racine et ajoutez les trois éléments *bindingRedirect* suivants sous la section de *configuration* : 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nouvelles fonctionnalités dans ASP.NET MVC 4 bêta

Cette section décrit les fonctionnalités qui ont été introduites dans la version bêta de ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API web ASP.NET

ASP.NET MVC 4 inclut désormais API Web ASP.NET, une nouvelle infrastructure pour la création de services HTTP pouvant atteindre un large éventail de clients, notamment des navigateurs et des appareils mobiles. API Web ASP.NET est également une plate-forme idéale pour la création de services RESTful.

API Web ASP.NET prend en charge les fonctionnalités suivantes :

- **Modèle de programmation http moderne :** Accédez et manipulez directement les requêtes et les réponses HTTP dans vos API Web à l’aide d’un nouveau modèle d’objet HTTP fortement typé. Le même modèle de programmation et le même pipeline HTTP sont symétriquement disponibles sur le client via le nouveau type HttpClient.
- **Prise en charge complète des itinéraires**: les API Web prennent désormais en charge l’ensemble complet des fonctionnalités de routage qui ont toujours fait partie de la pile Web, y compris les paramètres de routage et les contraintes. En outre, le mappage aux actions prend entièrement en charge les conventions. vous n’avez donc plus besoin d’appliquer des attributs tels que [HttpPost] à vos classes et méthodes.
- **Négociation de contenu**: le client et le serveur peuvent travailler ensemble pour déterminer le format approprié pour les données retournées à partir d’une API. Nous fournissons la prise en charge par défaut pour les formats XML, JSON et les formats d’URL de formulaire. vous pouvez étendre cette prise en charge en ajoutant vos propres formateurs ou même remplacer la stratégie de négociation de contenu par défaut.
- **Liaison et validation du modèle :** Les classeurs de modèles offrent un moyen simple d’extraire des données de différentes parties d’une requête HTTP et de les convertir en objets .NET qui peuvent être utilisés par les actions de l’API Web.
- **Filtres :** Les API Web prennent désormais en charge les filtres, y compris les filtres connus tels que l’attribut [Authorize]. Vous pouvez créer et brancher vos propres filtres pour les actions, l’autorisation et la gestion des exceptions.
- **Composition de la requête :** En renvoyant simplement un IQueryable&lt;T&gt;, votre API Web prend en charge l’interrogation via les conventions d’URL OData.
- **Amélioration de la testabilité des détails http :** Au lieu de définir les détails HTTP dans les objets de contexte statiques, les actions de l’API Web peuvent désormais fonctionner avec des instances de HttpRequestMessage et HttpResponseMessage. Il existe également des versions génériques de ces objets qui vous permettent de travailler avec vos types personnalisés en plus des types HTTP.
- **Amélioration de l’inversion de contrôle (IOC) via DependencyResolver :** L’API Web utilise désormais le modèle de localisateur de service implémenté par le programme de résolution de dépendance de MVC pour obtenir des instances de nombreuses installations différentes.
- **Configuration basée sur le code :** La configuration de l’API Web s’effectue uniquement par code, ce qui a pour effet de nettoyer vos fichiers de configuration.
- **Auto-hébergement :** Les API Web peuvent être hébergées dans votre propre processus en plus d’IIS tout en continuant à utiliser toute la puissance des itinéraires et d’autres fonctionnalités de l’API Web.

Pour plus d’informations sur API Web ASP.NET consultez [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>Application à page unique ASP.NET

ASP.NET MVC 4 comprend désormais une version préliminaire de l’expérience de création d’applications à page unique avec des interactions majeures côté client à l’aide de JavaScript et des API Web. Cette prise en charge comprend les éléments suivants :

- Ensemble de bibliothèques JavaScript pour les interactions locales plus riches avec les données mises en cache
- Composants d’API Web supplémentaires pour la prise en charge des unités de travail et des DAL
- Modèle de projet MVC avec génération de modèles automatique pour commencer rapidement

Pour plus d’informations sur la prise en charge d’une application à page unique dans ASP.NET MVC 4, consultez [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Améliorations apportées aux modèles de projet par défaut

Le modèle utilisé pour créer de nouveaux projets ASP.NET MVC 4 a été mis à jour pour créer un site Web plus moderne :

![](mvc4-beta-release-notes/_static/image1.png)

Outre les améliorations esthétiques, les fonctionnalités du nouveau modèle sont améliorées. Le modèle utilise une technique appelée rendu adaptatif pour paraître efficace dans les navigateurs de bureau et les navigateurs mobiles sans aucune personnalisation.

![](mvc4-beta-release-notes/_static/image2.png)

Pour voir le rendu adaptatif en action, vous pouvez utiliser un émulateur mobile ou simplement essayer de redimensionner la fenêtre du navigateur de bureau pour qu’elle soit plus petite. Lorsque la fenêtre du navigateur est suffisamment petite, la disposition de la page change.

Une autre amélioration du modèle de projet par défaut est l’utilisation de JavaScript pour fournir une interface utilisateur plus riche. Les liens de connexion et de Registre utilisés dans le modèle sont des exemples d’utilisation de la boîte de dialogue de l’interface utilisateur jQuery pour présenter un écran de connexion enrichi :

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modèle de projet mobile

Si vous démarrez un nouveau projet et que vous souhaitez créer un site spécifiquement pour les navigateurs mobiles et tablettes, vous pouvez utiliser le nouveau modèle de projet d’application mobile. Elle est basée sur jQuery mobile, une bibliothèque open source pour la création d’une interface utilisateur optimisée tactile :

![](mvc4-beta-release-notes/_static/image4.png)

Ce modèle contient la même structure d’application que le modèle d’application Internet (et le code du contrôleur est virtuellement identique), mais il est mis en forme à l’aide de jQuery mobile pour paraître correct et se comporter correctement sur les appareils mobiles tactiles. Pour en savoir plus sur la structure et le style de l’interface utilisateur mobile, consultez le [site Web du projet jQuery mobile](http://jquerymobile.com/).

Si vous disposez déjà d’un site orienté bureau auquel vous souhaitez ajouter des vues optimisées pour les appareils mobiles ou si vous souhaitez créer un site unique qui utilise des affichages stylisés différents pour les navigateurs mobiles et de bureau, vous pouvez utiliser la nouvelle fonctionnalité modes d’affichage. (Voir la section suivante.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modes d’affichage

La nouvelle fonctionnalité modes d’affichage permet à une application de sélectionner des vues en fonction du navigateur qui effectue la requête. Par exemple, si un navigateur de bureau demande la page d’hébergement, l’application peut utiliser le modèle Views\Home\Index.cshtml. Si un navigateur mobile demande la page d’hébergement, l’application peut retourner le modèle Views\Home\Index.mobile.cshtml.

Les dispositions et les parties partielles peuvent également être remplacées pour des types de navigateurs particuliers. Exemple :

- Si votre dossier Views\Shared contient à la fois les modèles \_Layout. cshtml et \_Layout. mobile. cshtml, l’application utilise par défaut \_Layout. mobile. cshtml pendant les demandes des navigateurs mobiles et \_Layout. cshtml pendant les autres requêtes.
- Si un dossier contient à la fois \_MyPartial. cshtml et \_MyPartial. mobile. cshtml, l’instruction @Html.Partial(«\_MyPartial ») restitue \_MyPartial. mobile. cshtml pendant les demandes des navigateurs mobiles et \_MyPartial. cshtml pendant les autres requêtes.

Si vous souhaitez créer des vues, des dispositions ou des vues partielles plus spécifiques pour d’autres appareils, vous pouvez inscrire une nouvelle instance *DefaultDisplayMode* pour spécifier le nom à rechercher lorsqu’une demande répond à des conditions particulières. Par exemple, vous pouvez ajouter le code suivant à l' *Application\_méthode Start* dans le fichier global. asax pour inscrire la chaîne « iPhone » en tant que mode d’affichage qui s’applique quand le navigateur iPhone d’Apple effectue une requête :

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Après l’exécution de ce code, quand un navigateur iPhone Apple effectue une requête, votre application utilise la disposition Views\Shared\\_Layout. iPhone. cshtml (si elle existe).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery mobile, le sélecteur de vue et le remplacement du navigateur

jQuery mobile est une bibliothèque open source pour la création d’une interface utilisateur Web optimisée pour les fonctions tactiles. Si vous souhaitez utiliser jQuery mobile avec une application ASP.NET MVC 4, vous pouvez télécharger et installer un package NuGet qui vous permet de commencer. Pour l’installer à partir de la console du gestionnaire de package Visual Studio, tapez la commande suivante :

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Cela installe jQuery mobile et certains fichiers d’assistance, y compris les suivants :

- Views/Shared/\_Layout. mobile. cshtml, qui est une mise en page basée sur mobile jQuery.
- Composant View-Switcher, qui se compose de la vue partielle Views/Shared/\_ViewSwitcher. cshtml et du contrôleur ViewSwitcherController.cs.

Une fois le package installé, exécutez votre application à l’aide d’un navigateur mobile (ou équivalent, comme le module complémentaire du [Sélecteur d’agent utilisateur](http://chrispederick.com/work/user-agent-switcher/) Firefox). Vous verrez que vos pages semblent très différentes, car jQuery mobile gère la disposition et le style. Pour tirer parti de cela, vous pouvez effectuer les opérations suivantes :

- Créez des remplacements d’affichages spécifiques aux appareils mobiles, comme décrit dans les [modes d’affichage](#_Toc303253810) précédents (par exemple, créez des Views\Home\Index.mobile.cshtml pour remplacer des Views\Home\Index.cshtml pour les navigateurs mobiles).
- Lisez la [documentation jQuery mobile](http://jquerymobile.com/) pour en savoir plus sur l’ajout d’éléments d’interface utilisateur à fonctions tactiles dans les vues mobiles.

Une convention pour les pages Web optimisées pour les appareils mobiles consiste à ajouter un lien dont le texte est une vue du bureau ou un mode de site complet qui permet aux utilisateurs de basculer vers une version de bureau de la page. Le package jQuery. mobile. MVC comprend un exemple de composant de sélecteur d’affichage à cet effet. Elle est utilisée dans la vue Views\Shared\\_Layout. mobile. cshtml par défaut et ressemble à ceci lorsque la page est rendue :

![](mvc4-beta-release-notes/_static/image5.png)

Si les visiteurs cliquent sur le lien, ils sont basculés vers la version de bureau de la même page.

Étant donné que la disposition de votre bureau n’inclura pas de commutateur de vue par défaut, les visiteurs n’auront pas la possibilité de passer en mode mobile. Pour ce faire, ajoutez la référence suivante à *\_ViewSwitcher* à la disposition de votre poste de travail, à l’intérieur de l’élément de *&gt;du corps du&lt;* :

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Le sélecteur de vue utilise une nouvelle fonctionnalité appelée remplacement du navigateur. Cette fonctionnalité permet à votre application de traiter les requêtes comme si elles provenaient d’un autre navigateur (agent utilisateur) que celui à partir duquel ils proviennent. Le tableau suivant répertorie les méthodes fournies par le remplacement du navigateur.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Substitue la valeur d'agent utilisateur réelle de la demande à l'aide de l'agent utilisateur spécifié. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Retourne la valeur de remplacement de l’agent utilisateur de la demande, ou la chaîne de l’agent utilisateur réelle si aucune substitution n’a été spécifiée. |
| `HttpContext.GetOverriddenBrowser()` | Retourne une instance *HttpBrowserCapabilitiesBase* qui correspond à l’agent utilisateur actuellement défini pour la demande (réelle ou substituée). Vous pouvez utiliser cette valeur pour récupérer des propriétés telles que *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Supprime tout agent utilisateur substitué pour la demande actuelle. |

La substitution du navigateur est une fonctionnalité de base de ASP.NET MVC 4 et est disponible même si vous n’installez pas le package jQuery. mobile. MVC. Toutefois, elle affecte uniquement l’affichage, la disposition et la sélection partielle, elle n’affecte pas les autres fonctionnalités ASP.NET qui dépendent de l’objet *Request. Browser* .

Par défaut, la substitution de l’agent utilisateur est stockée à l’aide d’un cookie. Si vous voulez stocker le remplacement ailleurs (par exemple, dans une base de données), vous pouvez remplacer le fournisseur par défaut (*BrowserOverrideStores. Current*). La documentation de ce fournisseur sera disponible pour accompagner une version ultérieure de ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Recettes pour la génération de code dans Visual Studio

La nouvelle fonctionnalité Recipes permet à Visual Studio de générer du code spécifique à la solution en fonction des packages que vous pouvez installer à l’aide de NuGet. L’infrastructure Recipes permet aux développeurs d’écrire facilement des plug-ins de génération de code, que vous pouvez également utiliser pour remplacer les générateurs de code intégrés pour l’ajout de zone, l’ajout de contrôleur et l’ajout d’une vue. Étant donné que les recettes sont déployées en tant que packages NuGet, elles peuvent être facilement archivées dans le contrôle de code source et partagées automatiquement avec tous les développeurs du projet. Elles sont également disponibles pour chaque solution.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Prise en charge des tâches pour les contrôleurs asynchrones

Vous pouvez maintenant écrire des méthodes d’action asynchrones en tant que méthodes uniques qui retournent un objet de type *Task* ou *task&lt;ActionResult&gt;* .

Par exemple, si vous utilisez Visual C# 5 (ou avec [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), vous pouvez créer une méthode d’action asynchrone qui ressemble à ceci :

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

Dans la méthode d’action précédente, les appels à *newsService. GetHeadlinesAsync* et *sportsService. GetScoresAsync* sont appelés de manière asynchrone et ne bloquent pas un thread du pool de threads.

Les méthodes d’action asynchrones qui retournent des instances de *tâche* peuvent également prendre en charge les délais d’attente. Pour rendre votre méthode d’action annulable, ajoutez un paramètre de type *CancellationToken* à la signature de la méthode d’action. L’exemple suivant montre une méthode d’action asynchrone qui a un délai d’expiration de 2500 millisecondes et qui affiche une vue *TimedOut* au client si un délai d’attente se produit.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 bêta prend en charge la version 2011 1,5 du kit de développement logiciel (SDK) Windows Azure de septembre.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et modifications avec rupture

- **Après l’installation de la version bêta de ASP.NET MVC 4, l’éditeur CSHTML/VBHTML dans l’éditeur de Visual Studio 2010 Service Pack 1 CSHTML/VBHTML peut s’interrompre longtemps après avoir tapé un extrait ou JavaScript dans des fichiers cshtml ou VBHTML.** Cela se produit uniquement dans les applications ASP.NET MVC 4 qui viennent d’être créées et qui n’ont pas encore été compilées.

    La solution de contournement consiste à compiler le projet pour récupérer les assemblys dans le dossier bin. Notez que si vous nettoyez le projet qui supprime les assemblys du dossier bin, le problème de l’éditeur reviendra.

    Cela sera corrigé dans la prochaine version.
- **C#Les modèles de projet pour Visual Studio 11 Beta contiennent une chaîne de connexion incorrecte dans Global.asax.cs.** La connexion par défaut spécifiée dans la méthode de démarrage de\_d’application pour les projets créés dans Visual Studio 11 Beta contient une chaîne de connexion de base de données locale qui contient une barre oblique inverse (\)) sans séquence d’échappement. Cela provoque une erreur de connexion lors des tentatives d’accès à une Entity Framework DbContext, qui génère une exception SqlException.

    Pour corriger ce problème, échappez la barre oblique inverse dans l’application\_méthode Start de Global.asax.cs pour qu’elle se présente comme suit :

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Les applications ASP.NET MVC 4 qui ciblent .NET 4,5 lèveront une FileLoadException lors de la tentative d’accès à l’assembly System. net. http. dll lorsqu’il est exécuté sous .NET 4,0.** Les applications ASP.NET MVC 4 créées sous .NET 4,5 contiennent une redirection de liaison qui aboutit à un FileLoadException qui indique que « impossible de charger le fichier ou l’assembly’System .net. http’ou l’une de ses dépendances ». Lorsque l’application est exécutée sur un système sur lequel .NET 4,0 est installé. Pour corriger ce problème, supprimez la redirection de liaison suivante à partir de Web. config :

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    L’élément de liaison d’assembly dans le fichier Web. config modifié doit se présenter comme suit :

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Le modèle d’élément « Add Controller » dans les projets Visual Basic génère un espace de noms incorrect lorsqu’il est appelé</strong><strong>à l’intérieur d’une zone.</strong> Quand vous ajoutez un contrôleur à une zone d’un projet ASP.NET MVC qui utilise Visual Basic, le modèle d’élément insère l’espace de noms incorrect dans le contrôleur. Le résultat est une erreur « fichier introuvable » lorsque vous accédez à une action dans le contrôleur.  
  
  L’espace de noms généré omet tout ce qui suit l’espace de noms racine. Par exemple, l’espace de noms généré est *RootNamespace* , mais doit être *RootNamespace. areas. AreaName. Controllers* .
- **Modifications avec rupture dans le moteur d’affichage Razor.** Dans le cadre d’une réécriture de l’analyseur Razor, les types suivants ont été supprimés de *System. Web. Mvc. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Les méthodes suivantes ont également été supprimées : 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
- **Lorsque WebMatrix. WebData. dll est inclus dans le répertoire/bin d’une application ASP.NET MVC 4, il prend le contrôle de l’URL pour l’authentification par formulaire.** L’ajout de l’assembly WebMatrix. WebData. dll à votre application (par exemple, en sélectionnant « pages Web ASP.NET avec la syntaxe Razor » lors de l’utilisation de la boîte de dialogue Ajouter des dépendances pouvant être déployées) remplace la connexion d’authentification redirigée vers/Account/Logon au lieu de/Account/login comme attendu par le contrôleur de compte ASP.NET MVC par défaut. Pour éviter ce comportement et utiliser l’URL spécifiée dans la section Authentication de Web. config, vous pouvez ajouter un appSetting appelé PreserveLoginUrl et lui affecter la valeur true : 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Le gestionnaire de package NuGet ne parvient pas à s’installer lors de la tentative d’installation de ASP.NET MVC 4 pour les installations côte à côte de Visual Studio 2010 et Visual Web Developer 2010.** Pour exécuter Visual Studio 2010 et Visual Web Developer 2010 côte à côte avec ASP.NET MVC 4, vous devez installer ASP.NET MVC 4 après avoir installé les deux versions de Visual Studio.
- **La désinstallation de ASP.NET MVC 4 échoue si les conditions préalables ont déjà été désinstallées.** Pour désinstaller correctement ASP.NET MVC 4you devez désinstaller ASP.NET MVC 4 avant de désinstaller Visual Studio.
- **L’exécution d’un projet d’API Web par défaut affiche des instructions qui indiquent incorrectement à l’utilisateur d’ajouter des itinéraires à l’aide de la méthode RegisterApis, qui n’existe pas.** Les itinéraires doivent être ajoutés dans la méthode RegisterRoutes à l’aide de la table de routage ASP.NET.
- **L’installation de ASP.NET MVC 4 Beta interrompt les applications ASP.NET MVC 3 RTM.** Les applications ASP.NET MVC 3 créées avec la version RTM (et non avec la version de mise à jour des outils ASP.NET MVC 3) requièrent les modifications suivantes pour fonctionner côte à côte avec ASP.NET MVC 4 bêta. La génération du projet sans effectuer ces mises à jour entraîne des erreurs de compilation. 

    **Mises à jour requises**

  1. Dans le fichier Web. config racine, ajoutez une nouvelle entrée *&lt;appSettings&gt;* avec la clé *webpage : version* et la valeur *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet, puis sélectionnez décharger le projet. Cliquez ensuite avec le bouton droit sur le nom et sélectionnez Modifier *NomProjet*. csproj.
  3. Recherchez les références d’assembly suivantes : 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Remplacez-les par les éléments suivants :

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Enregistrez les modifications, fermez le fichier projet (. csproj) que vous avez modifié, puis cliquez avec le bouton droit sur le projet et sélectionnez recharger.
