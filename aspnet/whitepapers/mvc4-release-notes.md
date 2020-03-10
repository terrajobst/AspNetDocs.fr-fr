---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ce document décrit la version de ASP.NET MVC 4.
ms.author: riande
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: b57480bd0274fbb76c600dfb0dd09037bdcbf1e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563426"
---
# <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

> Ce document décrit la version de ASP.NET MVC 4.

- [Notes d’installation](#_Toc303253802)
- [Documentation](#_Toc303253803)
- [Support](#_Toc303253804)
- [Configuration logicielle requise](#_Toc303253805)
- [Nouvelles fonctionnalités dans ASP.NET MVC 4](#_Toc303253807)

    - [API Web ASP.NET](#_Toc317096197)
    - [Améliorations apportées aux modèles de projet par défaut](#_Toc303253808)
    - [Modèle de projet mobile](#_Toc303253809)
    - [Modes d’affichage](#_Toc303253810)
    - [jQuery mobile, le sélecteur de vue et le remplacement du navigateur](#_Toc303253811)
    - [Prise en charge des tâches pour les contrôleurs asynchrones](#_Toc303253813)
    - [Kit de développement logiciel Azure](#_Toc303253814)
    - [Migrations de bases de données](#_Toc303253818)
    - [Modèle de projet vide](#_Toc303253819)
    - [Ajouter un contrôleur à un dossier de projet](#_Toc303253820)
    - [Bundles et minimisation](#_Toc303253821)
    - [Activation des connexions à partir de Facebook et d’autres sites à l’aide d’OAuth et OpenID](#_Toc303253822)
- [Mise à niveau d’un projet ASP.NET MVC 3 vers ASP.NET MVC 4](#_Toc303253806)
- [Modifications apportées à ASP.NET MVC 4 release candidate](#_Toc303253817)
- [Problèmes connus et modifications avec rupture](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Notes d'installation

ASP.NET MVC 4 pour Visual Studio 2010 peut être installé à partir de la [page d’hébergement ASP.NET MVC 4](../mvc/mvc4.md) à l’aide de l’Web Platform Installer.

Nous vous recommandons de désinstaller les versions préliminaires précédemment installées de ASP.NET MVC 4 avant d’installer ASP.NET MVC 4. Vous pouvez mettre à niveau la version bêta de ASP.NET MVC 4 et la version Release candidate vers ASP.NET MVC 4 sans désinstaller.

Cette version n’est pas compatible avec les versions préliminaires de .NET Framework 4,5. Vous devez mettre à niveau séparément les versions préliminaires installées de .NET Framework 4,5 vers la version finale avant d’installer ASP.NET MVC 4.

ASP.NET MVC 4 peut être installé et exécuté côte à côte avec ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Documentation

La documentation relative à ASP.NET MVC est disponible sur le site Web MSDN à l'adresse suivante :

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Des didacticiels et d’autres informations sur ASP.NET MVC sont disponibles sur la page MVC 4 du site Web ASP.NET ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Assistance

ASP.NET MVC 4 est entièrement pris en charge. Si vous avez des questions sur l’utilisation de cette version, vous pouvez également les publier sur le Forum ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), où les membres de la communauté ASP.net sont souvent en mesure de fournir un support informel.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Configuration logicielle

Les composants ASP.NET MVC 4 pour Visual Studio requièrent PowerShell 2,0 et Visual Studio 2010 avec Service Pack 1 ou Visual Web Developer Express 2010 avec Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nouvelles fonctionnalités dans ASP.NET MVC 4

Cette section décrit les fonctionnalités qui ont été introduites dans la version ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>API web ASP.NET

ASP.NET MVC 4 inclut API Web ASP.NET, une nouvelle infrastructure pour la création de services HTTP pouvant atteindre un large éventail de clients, dont des navigateurs et des appareils mobiles. API Web ASP.NET est également une plate-forme idéale pour la création de services RESTful.

API Web ASP.NET prend en charge les fonctionnalités suivantes :

- **Modèle de programmation http moderne :** Accédez et manipulez directement les requêtes et les réponses HTTP dans vos API Web à l’aide d’un nouveau modèle d’objet HTTP fortement typé. Le même modèle de programmation et le même pipeline HTTP sont symétriquement disponibles sur le client via le nouveau type *httpclient* .
- **Prise en charge complète des itinéraires :** API Web ASP.NET prend en charge l’ensemble complet des fonctionnalités de routage du routage ASP.NET, y compris les paramètres de routage et les contraintes. En outre, utilisez des conventions simples pour mapper des actions à des méthodes HTTP.
- **Négociation de contenu :** Le client et le serveur peuvent travailler ensemble pour déterminer le format approprié pour les données retournées à partir d’une API Web. API Web ASP.NET fournit une prise en charge par défaut pour les formats XML, JSON et les formats d’URL de formulaire. vous pouvez étendre cette prise en charge en ajoutant vos propres formateurs ou même remplacer la stratégie de négociation de contenu par défaut.
- **Liaison et validation du modèle :** Les classeurs de modèles offrent un moyen simple d’extraire des données de différentes parties d’une requête HTTP et de les convertir en objets .NET qui peuvent être utilisés par les actions de l’API Web. La validation est également effectuée sur les paramètres d’action en fonction des annotations de données.
- **Filtres :** API Web ASP.NET prend en charge les filtres, y compris les filtres connus tels que l’attribut *[Authorize]* . Vous pouvez créer et brancher vos propres filtres pour les actions, l’autorisation et la gestion des exceptions.
- **Composition de la requête :** Utilisez l’attribut de filtre *[interrogeable]* sur une action qui retourne *IQueryable* pour permettre la prise en charge de l’interrogation de votre API Web via les conventions de requête OData.
- **Amélioration de la testabilité :** Plutôt que de définir des détails HTTP dans des objets de contexte statiques, les actions de l’API Web fonctionnent avec des instances de *HttpRequestMessage* et *HttpResponseMessage*. Créez un projet de test unitaire avec votre projet d’API Web pour commencer rapidement à écrire des tests unitaires pour la fonctionnalité de votre API Web.
- **Configuration basée sur le code :** API Web ASP.NET Configuration est effectuée uniquement à l’aide de code, ce qui a pour effet de nettoyer vos fichiers de configuration. Utilisez le modèle de localisateur de service fourni pour configurer des points d’extensibilité.
- **Amélioration de la prise en charge des conteneurs d’inversion de contrôle (IOC) :** API Web ASP.NET offre une excellente prise en charge des conteneurs IoC via une abstraction améliorée du programme de résolution des dépendances
- **Auto-hébergement :** Les API Web peuvent être hébergées dans votre propre processus en plus d’IIS tout en continuant à utiliser toute la puissance des itinéraires et d’autres fonctionnalités de l’API Web.
- **Créer des pages d’aide et de test personnalisées :** Vous pouvez désormais facilement créer des pages d’aide et de test personnalisées pour vos API Web à l’aide du nouveau service *IApiExplorer* pour obtenir une description complète de l’exécution de vos API Web.
- **Surveillance et diagnostics :** API Web ASP.NET fournit désormais une infrastructure de suivi légère qui facilite l’intégration avec les solutions de journalisation existantes, telles que System. Diagnostics, ETW et les infrastructures de journalisation tierces. Vous pouvez activer le suivi en fournissant une implémentation de *ITraceWriter* et en l’ajoutant à votre configuration d’API Web.
- **Génération de liens :** Utilisez l’API Web ASP.NET *UrlHelper* pour générer des liens vers les ressources associées dans la même application.
- **Modèle de projet d’API Web :** Sélectionnez le nouveau projet d’API Web dans l’Assistant Nouveau projet MVC 4 pour rapidement devenir opérationnel avec API Web ASP.NET.
- **Génération de modèles automatique :** Utilisez la boîte de dialogue **Ajouter un contrôleur** pour générer rapidement une structure de contrôleur d’API Web basée sur un type de modèle Entity Framework.

Pour plus d’informations sur API Web ASP.NET consultez [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Améliorations apportées aux modèles de projet par défaut

Le modèle utilisé pour créer de nouveaux projets ASP.NET MVC 4 a été mis à jour pour créer un site Web plus moderne :

![](mvc4-release-notes/_static/image1.png)

Outre les améliorations esthétiques, les fonctionnalités du nouveau modèle sont améliorées. Le modèle utilise une technique appelée rendu adaptatif pour paraître efficace dans les navigateurs de bureau et les navigateurs mobiles sans aucune personnalisation.

![](mvc4-release-notes/_static/image2.png)

Pour voir le rendu adaptatif en action, vous pouvez utiliser un émulateur mobile ou simplement essayer de redimensionner la fenêtre du navigateur de bureau pour qu’elle soit plus petite. Lorsque la fenêtre du navigateur est suffisamment petite, la disposition de la page change.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Modèle de projet mobile

Si vous démarrez un nouveau projet et que vous souhaitez créer un site spécifiquement pour les navigateurs mobiles et tablettes, vous pouvez utiliser le nouveau modèle de projet d’application mobile. Elle est basée sur jQuery mobile, une bibliothèque open source pour la création d’une interface utilisateur optimisée tactile :

![](mvc4-release-notes/_static/image3.png)

Ce modèle contient la même structure d’application que le modèle d’application Internet (et le code du contrôleur est virtuellement identique), mais il est mis en forme à l’aide de jQuery mobile pour paraître correct et se comporter correctement sur les appareils mobiles tactiles. Pour en savoir plus sur la structure et le style de l’interface utilisateur mobile, consultez le [site Web du projet jQuery mobile](http://jquerymobile.com/).

Si vous disposez déjà d’un site orienté bureau auquel vous souhaitez ajouter des vues optimisées pour les appareils mobiles ou si vous souhaitez créer un site unique qui utilise des affichages stylisés différents pour les navigateurs mobiles et de bureau, vous pouvez utiliser la nouvelle fonctionnalité modes d’affichage. (Voir la section suivante.)

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Modes d’affichage

La nouvelle fonctionnalité modes d’affichage permet à une application de sélectionner des vues en fonction du navigateur qui effectue la requête. Par exemple, si un navigateur de bureau demande la page d’hébergement, l’application peut utiliser le modèle Views\Home\Index.cshtml. Si un navigateur mobile demande la page d’hébergement, l’application peut retourner le modèle Views\Home\Index.mobile.cshtml.

Les dispositions et les parties partielles peuvent également être remplacées pour des types de navigateurs particuliers. Exemple :

- Si votre dossier Views\Shared contient à la fois les modèles \_Layout. cshtml et \_Layout. mobile. cshtml, l’application utilise par défaut \_Layout. mobile. cshtml pendant les demandes des navigateurs mobiles et \_Layout. cshtml pendant les autres requêtes.
- Si un dossier contient à la fois \_MyPartial. cshtml et \_MyPartial. mobile. cshtml, l’instruction @Html.Partial(«\_MyPartial ») restitue \_MyPartial. mobile. cshtml pendant les demandes des navigateurs mobiles et \_MyPartial. cshtml pendant les autres requêtes.

Si vous souhaitez créer des vues, des dispositions ou des vues partielles plus spécifiques pour d’autres appareils, vous pouvez inscrire une nouvelle instance *DefaultDisplayMode* pour spécifier le nom à rechercher lorsqu’une demande répond à des conditions particulières. Par exemple, vous pouvez ajouter le code suivant à l' *Application\_méthode Start* dans le fichier global. asax pour inscrire la chaîne « iPhone » en tant que mode d’affichage qui s’applique quand le navigateur iPhone d’Apple effectue une requête :

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Après l’exécution de ce code, quand un navigateur iPhone Apple effectue une requête, votre application utilise la disposition Views\Shared\\_Layout. iPhone. cshtml (si elle existe). Pour plus d’informations sur le mode d’affichage, consultez [fonctionnalités mobiles ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Les applications utilisant DisplayModeProvider doivent installer le package NuGet [DisplayModes fixe](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . La [mise à jour 2012 d’automne ASP.net](https://go.microsoft.com/fwlink/?LinkID=271322) comprend le package NuGet [DisplayModes fixe](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) dans les nouveaux modèles de projet. Pour plus d’informations sur le correctif, consultez [bogue ASP.NET MVC 4 mobile Caching corrigé](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) .

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>Fonctionnalités mobiles et mobiles jQuery

Pour plus d’informations sur la création d’applications mobiles avec ASP.NET MVC 4 à l’aide de jQuery mobile, consultez le didacticiel sur les [fonctionnalités mobiles de ASP.NET MVC 4](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Prise en charge des tâches pour les contrôleurs asynchrones

Vous pouvez maintenant écrire des méthodes d’action asynchrones en tant que méthodes uniques qui retournent un objet de type *Task* ou *task&lt;ActionResult&gt;* .

 Pour plus d’informations [, consultez Utilisation de méthodes asynchrones dans ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 prend en charge les versions 1,6 et ultérieures du kit de développement logiciel (SDK) Windows Azure.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migrations de bases de données

Les projets ASP.NET MVC 4 incluent désormais Entity Framework 5. L’une des fonctionnalités intéressantes de Entity Framework 5 est la prise en charge des migrations de base de données. Cette fonctionnalité vous permet de faire évoluer facilement votre schéma de base de données à l’aide d’une migration axée sur le code tout en préservant les données dans la base de données. Pour plus d’informations sur les migrations de base de données, consultez la page [Ajout d’un nouveau champ au modèle de film et à la table](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) dans le [didacticiel Introduction à ASP.NET MVC 4](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Modèle de projet vide

Le modèle de projet vide MVC est maintenant véritablement vide, ce qui vous permet de démarrer à partir d’une ardoise entièrement propre. La version antérieure du modèle de projet vide a été renommée de base.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Ajouter un contrôleur à un dossier de projet

Vous pouvez maintenant cliquer avec le bouton droit et sélectionner **Ajouter un contrôleur** dans n’importe quel dossier de votre projet MVC. Vous bénéficiez ainsi d’une plus grande flexibilité pour organiser vos contrôleurs comme vous le souhaitez, y compris la conservation de vos contrôleurs d’API Web et MVC dans des dossiers distincts.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Bundles et minimisation

L’infrastructure de regroupement et de minimisation vous permet de réduire le nombre de requêtes HTTP qu’une page Web doit effectuer en combinant des fichiers individuels en un seul fichier groupé pour les scripts et les feuilles de style CSS. Il peut ensuite réduire la taille globale de ces demandes en minimisation le contenu du bundle. Minimisation peut inclure des activités telles que l’élimination des espaces blancs pour raccourcir les noms de variables et même réduire les sélecteurs CSS en fonction de leur sémantique. Les lots sont déclarés et configurés dans le code et sont facilement référencés dans les vues via des méthodes d’assistance qui peuvent générer un lien unique vers le bundle ou, lors du débogage, plusieurs liens vers le contenu individuel du bundle. Pour plus d’informations [, consultez Regroupement et minimisation](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Activation des connexions à partir de Facebook et d’autres sites à l’aide d’OAuth et OpenID

Les modèles par défaut dans le modèle de projet Internet ASP.NET MVC 4 incluent désormais la prise en charge de la connexion OAuth et OpenID à l’aide de la bibliothèque DotNetOpenAuth. Pour plus d’informations sur la configuration d’un fournisseur OAuth ou OpenID, consultez [prise en charge OAuth/OpenID pour les WebForms, MVC et les pages Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) , ainsi que la [documentation sur les fonctionnalités OAuth et OpenID dans pages Web ASP.net](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Mise à niveau d’un projet ASP.NET MVC 3 vers ASP.NET MVC 4

ASP.NET MVC 4 peut être installé côte à côte avec ASP.NET MVC 3 sur le même ordinateur, ce qui vous donne la possibilité de choisir quand mettre à niveau une application ASP.NET MVC 3 vers ASP.NET MVC 4.

La façon la plus simple de procéder à la mise à niveau consiste à créer un nouveau projet ASP.NET MVC 4 et à copier tous les affichages, les contrôleurs, le code et les fichiers de contenu du projet MVC 3 existant vers le nouveau projet, puis à mettre à jour les références d’assembly dans le nouveau projet pour qu’elles correspondent à n’importe quel modèle non MVC dans cluded assembiles que vous utilisez. Si vous avez apporté des modifications au fichier Web. config dans le projet MVC 3, vous devez également fusionner ces modifications dans le fichier Web. config dans le projet MVC 4.

Pour mettre à niveau manuellement une application ASP.NET MVC 3 existante vers la version 4, procédez comme suit :

1. Dans tous les fichiers Web. config du projet (il en existe un dans la racine du projet, l’un dans le dossier Views et l’autre dans le dossier views pour chaque zone de votre projet), remplacez chaque instance du texte suivant (Remarque : System. Web. webpages, version = 1.0.0.0 est introuvable dans projets créés avec Visual Studio 2012) : 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    avec le texte correspondant suivant :

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. Dans le fichier Web. config racine, mettez à jour l’élément Web *pages : version* sur « 2.0.0.0 » et ajoutez une nouvelle clé *PreserveLoginUrl* avec la valeur « true » : 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. Dans Explorateur de solutions, cliquez avec le bouton droit sur les références, puis sélectionnez gérer les packages NuGet. Dans le volet gauche, sélectionnez **Online\NuGet Official package source**, puis mettez à jour les éléments suivants :

    - ASP.NET MVC 4
    - (Facultatif) jQuery, la validation jQuery et l’interface utilisateur jQuery
    - Facultatif Entity Framework
    - (Optonal) Modernizr
4. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet, puis sélectionnez décharger le projet. Cliquez ensuite avec le bouton droit sur le nom et sélectionnez Modifier *NomProjet*. csproj.
5. Recherchez l’élément *ProjectTypeGuids* et remplacez {E53F8FEA-EAE0-44A6-8774-FFD645390401} par {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Enregistrez les modifications, fermez le fichier projet (. csproj) que vous avez modifié, cliquez avec le bouton droit sur le projet, puis sélectionnez recharger le projet.
7. Si le projet fait référence à des bibliothèques tierces compilées à l’aide de versions antérieures de ASP.NET MVC, ouvrez le fichier Web. config racine et ajoutez les trois éléments *bindingRedirect* suivants sous la section de *configuration* : 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Modifications apportées à ASP.NET MVC 4 release candidate

Vous trouverez les notes de publication pour ASP.NET MVC 4 release candidate ici :

Les principales modifications apportées à ASP.NET MVC 4 release candidate dans cette version sont résumées ci-dessous :

- **Configuration par contrôleur :** Les contrôleurs de API Web ASP.NET peuvent être attribués avec un attribut personnalisé qui implémente *IControllerConfiguration* pour configurer leurs propres formateurs, sélecteur d’action et classeurs de paramètres. Le *HttpControllerConfigurationAttribute* a été supprimé.
- **Gestionnaires de messages par itinéraire :** Vous pouvez maintenant spécifier le gestionnaire de messages final dans la chaîne de requête pour un itinéraire donné. Cela permet la prise en charge des infrastructures de remplacement pour utiliser le routage pour distribuer à leurs propres points de terminaison (non*IHttpController*).
- **Notifications de progression :** *ProgressMessageHandler* génère une notification de progression pour les deux entités de demande en cours de téléchargement et les entités de réponse en cours de téléchargement. À l’aide de ce gestionnaire, il est possible d’effectuer le suivi de la durée de chargement d’un corps de requête ou de téléchargement d’un corps de réponse.
- **Contenu de transmission :** La classe *PushStreamContent* permet aux scénarios où un producteur de données souhaite écrire directement dans la demande ou la réponse (de façon synchrone ou asynchrone) à l’aide d’un flux. Lorsque le *PushStreamContent* est prêt à accepter des données, il appelle un délégué d’action avec le flux de sortie. Le développeur peut ensuite écrire dans le flux aussi longtemps que nécessaire et fermer le flux lorsque l’écriture est terminée. *PushStreamContent* détecte la fermeture du flux et termine la *tâche* asynchrone sous-jacente pour l’écriture du contenu.
- **Création des réponses d’erreur :** Utilisez le type *erreur http* pour représenter de manière cohérente des informations d’erreur, telles que les erreurs de validation et les exceptions, tout en respectant encore le *IncludeErrorDetailPolicy*. Utilisez les nouvelles méthodes d’extension *CreateErrorResponse* pour créer facilement des réponses d’erreur avec *erreur http* en tant que contenu. Le contenu *erreur http* est entièrement négocié.
- **MediaRangeMapping supprimé :** Les plages de type de média sont désormais gérées par le négociateur de contenu par défaut.
- La **liaison de paramètre par défaut pour les paramètres de type simple est désormais [FromUri] :** Dans les versions précédentes de API Web ASP.NET la liaison de paramètre par défaut pour les paramètres de type simple utilisés pour la liaison de modèle. La liaison de paramètre par défaut pour les paramètres de type simple est désormais *[FromUri]* .
- La sélection de l' **action honore les paramètres requis :** La sélection d’action dans API Web ASP.NET ne sélectionne désormais qu’une action si tous les paramètres requis provenant de l’URI sont fournis. Un paramètre peut être spécifié comme facultatif en fournissant une valeur par défaut pour l’argument dans la signature de la méthode d’action.
- **Personnaliser les liaisons de paramètre http :** Utilisez *ParameterBindingAttribute* pour personnaliser la liaison de paramètre pour un paramètre d’action spécifique ou utilisez *ParameterBindingRules* sur *HttpConfiguration* pour personnaliser les liaisons de paramètres de manière plus large.
- **Améliorations apportées à MediaTypeFormatter :** Les formateurs ont maintenant accès à l’instance *HttpContent* complète.
- **Sélection de la stratégie de mise en mémoire tampon de l’hôte :** Implémentez et configurez le service *IHostBufferPolicySelector* dans API Web ASP.net pour permettre aux hôtes de déterminer la stratégie pour l’utilisation de la mise en mémoire tampon.
- **Accéder aux certificats clients en mode indépendant de l’hôte :** Utilisez la méthode d’extension *GetClientCertificate* pour obtenir le certificat client fourni à partir du message de demande.
- **Extensibilité de la négociation du contenu :** Personnalisez la négociation de contenu en dérivant de *DefaultContentNegotiator* et en remplaçant tous les aspects de la négociation de contenu que vous souhaitez.
- **Prise en charge du retour de réponses 406 non acceptables :** Vous pouvez maintenant retourner 406 réponses non acceptables dans API Web ASP.NET lorsqu’un formateur approprié est introuvable en créant un *DefaultContentNegotiator* avec le paramètre *excludeMatchOnTypeOnly* défini sur *true*.
- **Lire les données de formulaire en tant que NameValueCollection ou JToken :** Vous pouvez lire les données de formulaire dans la chaîne de requête d’URI ou dans le corps de la demande sous forme de *NameValueCollection* en utilisant respectivement les méthodes d’extension *ParseQueryString* et *ReadAsFormDataAsync* . De même, vous pouvez lire les données de formulaire dans la chaîne de requête d’URI ou dans le corps de la demande sous la forme d’un *JToken* à l’aide des méthodes d’extension *TryReadQueryAsJson* et *ReadAsAsync*&lt;t&gt; respectivement.
- **Améliorations à plusieurs parties :** Il est désormais possible d’écrire un *MultipartStreamProvider* entièrement adapté au type de données MIME à parties multiples qu’il peut lire et présenter de manière optimale à l’utilisateur. Vous pouvez également raccorder une étape de validation sur le *MultipartStreamProvider* qui permet à l’implémentation d’effectuer le traitement de publication souhaité sur les parties du corps MIME à parties multiples. Par exemple, l’implémentation de *MultipartFormDataStreamProvider* lit les parties de données de formulaire HTML et les ajoute à une *NameValueCollection* afin qu’elles soient faciles à obtenir de l’appelant.
- **Améliorations de la génération de liens :** Le *UrlHelper* ne dépend plus de *HttpControllerContext*. Vous pouvez maintenant accéder à *UrlHelper* à partir de n’importe quel contexte où le *HttpRequestMessage* est disponible.
- **Modification de l’ordre d’exécution du gestionnaire de messages :** Les gestionnaires de messages sont maintenant exécutés dans l’ordre dans lequel ils sont configurés, et non dans l’ordre inverse.
- **Application auxiliaire pour la connexion des gestionnaires de messages :** Le nouveau *HttpClientFactory* qui peut connecter *DelegatingHandlers* et créer un *httpclient* avec le pipeline souhaité prêt à l’emploi. Il fournit également des fonctionnalités permettant de s’associer à d’autres gestionnaires internes (la valeur par défaut est *HttpClientHandler*), ainsi que d’effectuer le câblage en cas d’utilisation de *HttpMessageInvoker* ou d’un autre *DelegatingHandler* au lieu de *httpclient* comme le premier demandeur.
- **Prise en charge de CDN dans l’optimisation Web ASP.net :** L’optimisation Web ASP.NET fournit désormais la prise en charge des chemins d’accès alternatifs CDN, ce qui vous permet de spécifier pour chaque Bundle une URL supplémentaire qui pointe vers cette même ressource sur un réseau de distribution de contenu. La prise en charge de CDN vous permet de regrouper votre script et vos groupes de styles plus près des consommateurs finaux de vos applications Web. Les applications de production doivent implémenter une solution de secours lorsque le CDN n’est pas disponible. Testez le secours.
- **API Web ASP.NET itinéraires et la configuration ont été déplacés vers *WebApiConfig. Register* , méthode statique qui peut être resuse dans le code de test.** API Web ASP.NET itinéraires ont été ajoutés précédemment dans *RouteConfig. RegisterRoutes* avec les itinéraires MVC standard. Les itinéraires et la configuration par défaut de API Web ASP.NET sont désormais gérés dans une méthode distincte *WebApiConfig. Register* pour faciliter le test.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et modifications avec rupture

- **La version RC et RTM de ASP.NET MVC 4 a renvoyé de manière incorrecte des vues de bureau mises en cache lorsque les vues mobiles doivent être retournées.**

    - Pour plus d’informations sur le correctif, consultez [bogue ASP.NET MVC 4 mobile Caching corrigé](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) . Le correctif peut être installé à partir du package [DisplayModes NuGet fixe](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) .
- **Modifications avec rupture dans le moteur d’affichage Razor**. Les types suivants ont été supprimés de *System. Web. Mvc. Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Les méthodes suivantes ont également été supprimées : 

    - *MvcCSharpRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
    - *MvcWebPageRazorHost. DecorateCodeGenerator (System. Web. Razor. Generator. RazorCodeGenerator)*
    - *MvcVBRazorCodeParser. ParseInheritsStatement (System. Web. Razor. Parser. CodeBlockInfo)*
- **Lorsque WebMatrix. WebData. dll est inclus dans le répertoire/bin d’une application ASP.NET MVC 4, il prend le contrôle de l’URL pour l’authentification par formulaire.** L’ajout de l’assembly WebMatrix. WebData. dll à votre application (par exemple, en sélectionnant « pages Web ASP.NET avec la syntaxe Razor » lors de l’utilisation de la boîte de dialogue Ajouter des dépendances pouvant être déployées) remplace la connexion d’authentification redirigée vers/Account/Logon au lieu de/Account/login comme attendu par le contrôleur de compte ASP.NET MVC par défaut. Pour éviter ce comportement et utiliser l’URL spécifiée dans la section Authentication de Web. config, vous pouvez ajouter un appSetting appelé PreserveLoginUrl et lui affecter la valeur true : 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Le gestionnaire de package NuGet ne parvient pas à s’installer lors de la tentative d’installation de ASP.NET MVC 4 pour les installations côte à côte de Visual Studio 2010 et Visual Web Developer 2010.** Pour exécuter Visual Studio 2010 et Visual Web Developer 2010 côte à côte avec ASP.NET MVC 4, vous devez installer ASP.NET MVC 4 après avoir installé les deux versions de Visual Studio.
- **La désinstallation de ASP.NET MVC 4 échoue si les conditions préalables ont déjà été désinstallées.** Pour désinstaller correctement ASP.NET MVC 4you devez désinstaller ASP.NET MVC 4 avant de désinstaller Visual Studio.
- **L’installation de ASP.NET MVC 4 interrompt les applications ASP.NET MVC 3 RTM.** Les applications ASP.NET MVC 3 créées avec la version RTM (et non avec la version de [mise à jour des outils ASP.NET MVC 3](https://www.microsoft.com/download/details.aspx?id=1491) ) requièrent les modifications suivantes pour fonctionner côte à côte avec ASP.NET MVC 4. La génération du projet sans effectuer ces mises à jour entraîne des erreurs de compilation. 

    **Mises à jour requises**

  1. Dans le fichier Web. config racine, ajoutez une nouvelle entrée *&lt;appSettings&gt;* avec la clé *webpage : version* et la valeur *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. Dans Explorateur de solutions, cliquez avec le bouton droit sur le nom du projet, puis sélectionnez décharger le projet. Cliquez ensuite avec le bouton droit sur le nom et sélectionnez Modifier *NomProjet*. csproj.
  3. Recherchez les références d’assembly suivantes : 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Remplacez-les par les éléments suivants :

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Enregistrez les modifications, fermez le fichier projet (. csproj) que vous avez modifié, puis cliquez avec le bouton droit sur le projet et sélectionnez recharger.

- **La modification d’un projet ASP.NET MVC 4 pour cibler 4,0 à partir de 4,5 ne met pas à jour la référence d’assembly EntityFramework :** Si vous modifiez un projet ASP.NET MVC 4 pour cibler 4,0 après avoir ciblé 4,5, la référence à l’assembly EntityFramework pointera toujours sur la version 4,5. Pour résoudre ce problème, désinstallez et réinstallez le package NuGet EntityFramework.
- **403 interdit lors de l’exécution d’une application ASP.NET MVC 4 sur Azure après avoir modifié la cible 4,0 de 4,5 :** Si vous modifiez un projet ASP.NET MVC 4 pour cibler 4,0 après avoir ciblé 4,5, puis déployé sur Azure, vous pouvez voir une erreur 403 interdit au moment de l’exécution. Pour contourner ce problème, ajoutez ce qui suit à votre fichier Web. config : `<modules runAllManagedModulesForAllRequests="true" />`
- **Visual Studio 2012 se bloque quand vous tapez un'\' dans un littéral de chaîne dans un fichier Razor.** Pour contourner le problème, entrez d’abord le guillemet fermant du littéral de chaîne.
- <strong>L’accès à &quot;compte/gérer des&quot; dans le modèle Internet génère une erreur d’exécution pour les langues CHS, Rev et CHT.</strong> Pour résoudre le problème, modifiez la page pour séparer les <em>@User.Identity.Name</em> en la plaçant en tant que contenu unique dans la balise de <em>&gt;renforcée&lt;</em> .
- **Les fournisseurs Google et LinkedIn ne sont pas pris en charge dans les sites Web Azure.** Utilisez d’autres fournisseurs d’authentification lors du déploiement sur des sites Web Azure.
- **Lors de l’utilisation de UriPathExtensionMapping avec IIS 8 Express/IIS, vous recevrez des erreurs 404 introuvables lorsque vous essaierez d’utiliser l’extension.** Le gestionnaire de fichiers statiques interfère avec les demandes aux API Web qui utilisent *UriPathExtensionMappings*. Définissez *runAllManagedModulesForAllRequests = true* dans Web. config pour contourner le problème.
- **La méthode Controller. Execute n’est plus appelée.** Tous les contrôleurs MVC sont désormais toujours exécutés de façon asynchrone.
