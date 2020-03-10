---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Fonctionnement de la localisation ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La localisation est le processus de conception et d’intégration de la prise en charge d’une langue et d’une culture spécifiques dans une application ou un composant d’application. La MIC...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 003e7939accd7a68dab97441b3d999bca835b85a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566219"
---
# <a name="understanding-aspnet-ajax-localization"></a>Présentation de la localisation d’ASP.NET AJAX

par [Scott Cate](https://github.com/scottcate)

[Télécharger PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> La localisation est le processus de conception et d’intégration de la prise en charge d’une langue et d’une culture spécifiques dans une application ou un composant d’application. La plateforme Microsoft ASP.NET fournit une prise en charge étendue de la localisation des applications ASP.NET standard en intégrant le modèle de localisation .NET standard. l’infrastructure Microsoft AJAX utilise le modèle intégré pour prendre en charge les différents scénarios dans lesquels la localisation peut être effectuée.

## <a name="introduction"></a>Introduction

La technologie ASP.NET de Microsoft propose un modèle de programmation orienté objet et piloté par les événements, et l’intègre aux avantages du code compilé. Toutefois, son modèle de traitement côté serveur présente plusieurs inconvénients inhérents à la technologie, dont la plupart peuvent être traités par les nouvelles fonctionnalités incluses dans l’espace de noms System. Web. extensions, qui encapsule les services Microsoft AJAX dans le .NET Framework 3,5. Ces extensions activent de nombreuses fonctionnalités clientes riches, précédemment disponibles dans le cadre des extensions ASP.NET 2,0 AJAX, mais qui font désormais partie de la bibliothèque de classes de base du Framework. Les contrôles et les fonctionnalités de cet espace de noms incluent le rendu partiel des pages sans nécessiter une actualisation complète de la page, la possibilité d’accéder aux services Web via le script client (y compris l’API de profilage ASP.NET) et une API côté client étendue, conçue pour mettre en miroir un grand nombre de les schémas de contrôle affichés dans le jeu de contrôles côté serveur ASP.NET.

Ce livre blanc examine les fonctionnalités de localisation présentes dans Microsoft AJAX Framework et la bibliothèque de scripts Microsoft AJAX, dans le contexte des besoins de l’entreprise en matière de prise en charge de la localisation et en passant en revue la prise en charge déjà intégrée de la localisation dans le Web. applications fournies par le .NET Framework. La bibliothèque de scripts Microsoft AJAX utilise le format de fichier. resx déjà utilisé par les applications .NET, qui fournit une prise en charge intégrée de l’IDE et un type de ressource partageable.

Ce livre blanc est basé sur la version bêta 2 de Microsoft Visual Studio 2008. Ce livre blanc suppose également que vous travaillez avec Visual Studio 2008, et non Visual Web Developer Express, et vous fournira des procédures pas à pas en fonction de l’interface utilisateur de Visual Studio. Certains exemples de code utiliseront des modèles de projet qui peuvent ne pas être disponibles dans Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*Besoin de localisation*

En particulier pour les développeurs d’applications d’entreprise et les développeurs de composants, la possibilité de créer des outils capables de connaître les différences entre les cultures et les langages est devenue de plus en plus nécessaire. La conception de composants avec la possibilité de s’adapter aux paramètres régionaux du client augmente la productivité des développeurs et réduit la quantité de travail nécessaire à l’adaptation d’un composant pour fonctionner globalement.

La localisation est le processus de conception et d’intégration de la prise en charge d’une langue et d’une culture spécifiques dans une application ou un composant d’application. La plateforme Microsoft ASP.NET fournit une prise en charge étendue de la localisation des applications ASP.NET standard en intégrant le modèle de localisation .NET standard. l’infrastructure Microsoft AJAX utilise le modèle intégré pour prendre en charge les différents scénarios dans lesquels la localisation peut être effectuée. Avec Microsoft AJAX Framework, les scripts peuvent être localisés en étant déployés dans des assemblys satellites ou à l’aide d’une structure de système de fichiers statique.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Incorporation de scripts avec des assemblys satellites*

Cohérente avec la stratégie de localisation .NET Framework standard, les ressources peuvent être incluses dans les assemblys satellites. Les assemblys satellites offrent plusieurs avantages par rapport à l’inclusion de ressources traditionnelles dans les binaires : toute localisation donnée peut être mise à jour sans mettre à jour l’image plus grande, et les localisations supplémentaires peuvent être déployées simplement en installant des assemblys satellites dans le dossier du projet et les assemblys satellites peuvent être déployés sans entraîner le rechargement de l’assembly principal du projet. En particulier dans les projets ASP.NET, cela est avantageux, car cela peut réduire considérablement la quantité de ressources système utilisées par les mises à jour incrémentielles et perturber au minimum l’utilisation des sites Web de production.

Les scripts sont incorporés dans des assemblys en les incluant dans des fichiers. resx managés (ou. Resources), qui sont inclus dans l’assembly au moment de la compilation. Leurs ressources sont ensuite mises à la disposition de l’application de script par le biais du code généré par le runtime AJAX, via des attributs au niveau de l’assembly.

*Conventions d’affectation des noms pour les fichiers de script incorporés*

La gestion des scripts de Microsoft AJAX Framework prend en charge diverses options pour le déploiement et le test des scripts, et des instructions sont fournies pour faciliter ces options.

*Pour faciliter le débogage :*

Les scripts de mise en production (production) ne doivent pas inclure le qualificateur `.debug` dans le nom de fichier. Les scripts conçus pour le débogage doivent inclure `.debug` dans le nom de fichier.

*Pour faciliter la localisation :*

Les scripts de culture neutre ne doivent pas inclure d’identificateur de culture dans le nom du fichier. Pour les scripts qui contiennent des ressources localisées, le code de langue ISO doit être spécifié dans le nom de fichier. Par exemple, `es-CO` correspond à espagnol, Colombie.

Le tableau suivant récapitule les conventions d’affectation des noms de fichiers avec des exemples :

| Nom de fichier | Signification |
| --- | --- |
| Script.js | Script indépendant de la culture Release-version. |
| Script.debug.js | Script indépendant de la culture Debug-version. |
| Script.en-US.js | Une version Release en anglais, États-Unis script. |
| Script.debug.es-CO.js | Un script Columbia-version Debug espagnol. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Procédure pas à pas : création d’un script incorporé, localisé

*Remarque : cette procédure pas à pas requiert l’utilisation de Visual Studio 2008, car Visual Web Developer Express n’inclut pas de modèle de projet pour les projets de bibliothèque de classes.*

1. Créez un nouveau projet de site Web avec les extensions ASP.NET AJAX intégrées. Créez un autre projet, un projet de bibliothèque de classes, au sein de la solution appelée LocalizingResources.
2. Ajoutez un fichier JScript appelé VerifyDeletion. js au projet LocalizingResources, ainsi que des fichiers de ressources. resx appelés DeletionResources. resx et DeletionResources. es. resx. Le premier contient des ressources indépendantes de la culture. ce dernier contient les ressources de langue espagnole.
3. Ajoutez le code suivant à VerifyDeletion. js :

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Pour ceux qui ne connaissent pas la syntaxe des expressions régulières JavaScript, le texte dans les barres obliques simples (dans l’exemple précédent,/FILENAME/est un exemple) désigne un objet RegExp. MSDN Library contient une référence JavaScript complète et des ressources sur les objets JavaScript natifs sont accessibles en ligne.

1. Ajoutez les chaînes de ressources suivantes à DeletionResources. resx : 

    **VerifyDelete**: voulez-vous vraiment supprimer le nom de fichier ?

    **Supprimé**: le nom de fichier a été supprimé.

1. Ajoutez les chaînes de ressources suivantes à DeletionResources. es. resx : 

    **VerifyDelete**: est Seguro que desee nom de fichier de sortie ?

    **Supprimé**: nom de la haute disponibilité quitado.
2. Ajoutez les lignes de code suivantes au fichier AssemblyInfo :

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Ajoutez des références à System. Web et System. Web. extensions au projet LocalizingResources.
2. Ajoutez une référence au projet LocalizingResources à partir du projet de site Web.
3. Dans default. aspx, sous le projet de site Web, mettez à jour le contrôle ScriptManager avec la balise supplémentaire suivante :

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Dans default. aspx, n’importe où dans la page, incluez ce balisage :

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Appuyez sur F5. Si vous y êtes invité, activez le débogage. Lorsque la page est chargée, appuyez sur le bouton supprimer. Notez que vous êtes invité en anglais (sauf si votre ordinateur est configuré pour préférer les ressources espagnoles par défaut) pour la confirmation.
2. Fermez la fenêtre du navigateur et revenez à default. aspx. Dans la directive @Page en-tête, remplacez auto pour culture et UICulture par es-ES. Appuyez de nouveau sur F5 pour lancer l’application Web dans le navigateur. Cette fois, Notez que vous êtes invité à supprimer le fichier en espagnol :

[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-localization/_static/image3.png))

[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Cliquez pour afficher l’image en taille réelle](understanding-asp-net-ajax-localization/_static/image6.png))

Notez qu’il existe plusieurs variantes pour cette procédure pas à pas. Par exemple, les scripts peuvent être enregistrés avec le contrôle ScriptManager par programme pendant le chargement de la page.

## <a name="including-a-static-script-file-structure"></a>*Inclusion d’une structure de fichier de script statique*

Lorsque vous utilisez des fichiers de script statiques pour le déploiement, vous perdez certains des avantages liés à l’utilisation du schéma de localisation .NET inhérent. Principalement visible, c’est que vous perdez le type automatique généré à partir de l’inclusion de fichiers de ressources de script ; dans la procédure pas à pas ci-dessus, par exemple, les ressources étaient exposées par un type généré automatiquement appelé message à partir du contrôle ScriptManager.

Il existe toutefois des avantages à l’utilisation d’une structure de fichier de script statique. Les mises à jour peuvent être effectuées sans recompiler et redéployer les assemblys satellites, et l’utilisation d’une structure de fichiers statique peut également être effectuée pour remplacer le script incorporé, pour intégrer une partie mineure de fonctionnalités qui n’ont peut-être pas été fournies avec un composant.

Microsoft recommande d’éviter un problème de contrôle de version en générant automatiquement vos ressources de script lors de la compilation du projet. Lorsque vous conservez une base de code de script étendue, il peut devenir de plus en plus difficile de s’assurer que les modifications du code sont reflétées dans chaque script localisé. En guise d’alternative, vous pouvez simplement gérer un script logique et plusieurs scripts de localisation, en fusionnant les fichiers lors de la génération du projet.

Étant donné qu’il n’y a pas de ressources à inclure de façon déclarative, les fichiers de script statiques doivent être référencés soit en ajoutant des éléments `<asp:ScriptElement>` en tant qu’enfant de la balise `<Scripts>` du contrôle ScriptManager, soit en ajoutant par programmation des objets `ScriptReference` à la propriété `Scripts` du contrôle `ScriptManager` de la page au moment de l’exécution.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager et son rôle dans la localisation*

Le ScriptManager active plusieurs comportements automatiques pour les applications localisées :

- Il localise automatiquement les fichiers de script en fonction des paramètres et des conventions d’affectation des noms. par exemple, il charge les scripts avec débogage en mode débogage et charge les scripts localisés en fonction de la sélection de l’interface utilisateur du navigateur.
- Il permet la définition de cultures, y compris les cultures personnalisées.
- Il permet la compression des fichiers de script sur HTTP.
- Il met en cache des scripts pour gérer efficacement de nombreuses demandes.
- Elle ajoute une couche d’indirection aux scripts en les canalisant via une URL chiffrée.

Des références de script peuvent être ajoutées au contrôle ScriptManager par programme ou par un balisage déclaratif. Le balisage déclaratif est particulièrement utile lorsque vous utilisez des scripts incorporés dans des assemblys autres que le projet de site Web lui-même, car le nom du script n’est probablement pas modifié à mesure que les révisions sont envoyées.

## <a name="summary"></a>Récapitulatif

À mesure que les applications Web grandissent pour atteindre un public plus large, le besoin de pouvoir atteindre des cultures et des communautés plus larges devient un modèle d’entreprise essentiel. les applications Web de commerce électronique doivent être en mesure de gérer les devises étrangères, les systèmes de gestion de contenu doivent être en mesure de présenter leur contenu, mais également leurs indicateurs de navigation et champs de formulaire dans d’autres langues, et les entreprises doivent savoir que ce besoin est disponibles.

Le .NET Framework prend intrinsèquement en charge un Framework de localisation enrichi, en utilisant des assemblys satellites et des fichiers de ressources XML (. resx) pour présenter un moyen uniforme de rechercher des chaînes et des images de ressources. Les extensions ASP.NET AJAX, y compris Microsoft AJAX Framework et la bibliothèque de scripts Microsoft AJAX, fournissent la prise en charge de ce modèle de programmation dans le code côté client, ce qui facilite les recherches de chaînes de ressources. Les assemblys satellites prennent en charge l’inclusion automatique des ressources de script (fichiers. js réels) à l’aide de ScriptResource. axd tant que les noms de fichiers suivent un modèle d’affectation de noms donné. Grâce à cette prise en charge, les extensions ASP.NET AJAX simplifient la localisation des scripts et la globalisation des applications.

## <a name="bio"></a>*Équivalence*

Scott Cate travaille avec les technologies Web de Microsoft depuis 1997 et est le Président de myKB.com ([www.myKB.com](http://www.myKB.com)), où il se spécialise dans l’écriture d’applications ASP.net axées sur les solutions logicielles de la base de connaissances. Scott peut être contacté par e-mail à [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou son blog à l’adresse [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Précédent](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Suivant](understanding-asp-net-ajax-web-services.md)
