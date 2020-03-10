---
uid: web-forms/what-is-web-forms
title: Qu’est-ce que Web Forms | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: 19be419c499759713971a6c77674c924867d1bbc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636576"
---
# <a name="what-is-web-forms"></a>Qu’est-ce que Web Forms

ASP.NET Web Forms fait partie de l’infrastructure d’application Web ASP.NET et est inclus dans [Visual Studio](https://www.asp.net/downloads). Il s’agit de l’un des quatre modèles de programmation que vous pouvez utiliser pour créer des applications Web ASP.NET, les autres sont ASP.NET MVC, pages Web ASP.NET et ASP.NET des applications à page unique.

Web Forms sont des pages que vos utilisateurs demandent en utilisant leur navigateur. Ces pages peuvent être écrites à l’aide d’une combinaison de code HTML, de script client, de contrôles serveur et de code serveur. Lorsque les utilisateurs demandent une page, ils sont compilés et exécutés sur le serveur par le Framework, puis le Framework génère le balisage HTML que le navigateur peut restituer. Une page de Web Forms ASP.NET fournit des informations à l’utilisateur dans n’importe quel navigateur ou appareil client.

À l’aide de Visual Studio, vous pouvez créer des Web Forms ASP.NET. L’environnement de développement intégré (IDE) de Visual Studio vous permet de glisser-déplacer des contrôles serveur pour disposer votre page Web Forms. Vous pouvez ensuite facilement définir des propriétés, des méthodes et des événements pour les contrôles de la page ou pour la page elle-même. Ces propriétés, méthodes et événements sont utilisés pour définir le comportement, l’apparence et la convivialité de la page Web. Pour écrire du code serveur pour gérer la logique de la page, vous pouvez utiliser un langage .NET comme Visual Basic C#ou.

> [!NOTE] 
> 
> La documentation ASP.NET et Visual Studio s’étend sur plusieurs versions. Les rubriques qui mettent en évidence les fonctionnalités des versions précédentes peuvent être utiles pour vos tâches et scénarios actuels à l’aide des versions les plus récentes.

**ASP.NET Web Forms sont :** 

- Basé sur Microsoft ASP.NET technologie, dans laquelle le code qui s’exécute sur le serveur génère dynamiquement une sortie de page Web sur le navigateur ou le périphérique client.
- Compatible avec n’importe quel navigateur ou appareil mobile. Une page Web ASP.NET affiche automatiquement le code HTML conforme au navigateur pour les fonctionnalités telles que les styles, la disposition, etc.
- Compatible avec tous les langages pris en charge par le common language runtime .NET, tels que Microsoft C#Visual Basic et Microsoft Visual.
- Basé sur l’infrastructure Microsoft .NET. Cela offre tous les avantages de l’infrastructure, notamment un environnement managé, la sécurité de type et l’héritage.
- Flexible, car vous pouvez y ajouter des contrôles créés par des utilisateurs ou des tiers.

**Offre ASP.NET Web Forms :** 

- Séparation du code HTML et d’un autre code d’interface utilisateur à partir de la logique d’application.
- Une suite complète de contrôles serveur pour les tâches courantes, y compris l’accès aux données.
- Une puissante liaison de données, avec une excellente prise en charge des outils.
- Prise en charge des scripts côté client qui s’exécutent dans le navigateur.
- Prise en charge d’une grande variété d’autres fonctionnalités, notamment le routage, la sécurité, les performances, l’internationalisation, le test, le débogage, la gestion des erreurs et la gestion des États.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Forms vous aide à surmonter les défis

La programmation d’applications Web présente des défis qui ne se posent généralement pas lors de la programmation d’applications clientes traditionnelles. Parmi les défis, citons :

- **Implémentation d’une interface utilisateur Web enrichie** : il peut être difficile et fastidieux de concevoir et d’implémenter une interface utilisateur à l’aide d’installations HTML de base, en particulier si la page a une mise en page complexe, une grande quantité de contenu dynamique et des objets interactifs complets.
- **Séparation du client et du serveur** : dans une application Web, le client (navigateur) et le serveur sont des programmes qui s’exécutent souvent sur des ordinateurs différents (et même sur des systèmes d’exploitation différents). Par conséquent, les deux moitiés de l’application partagent très peu d’informations ; ils peuvent communiquer, mais en général, échangent uniquement de petites parties d’informations simples.
- **Exécution sans état** : lorsqu’un serveur Web reçoit une demande pour une page, il trouve la page, la traite, l’envoie au navigateur, puis ignore toutes les informations de la page. Si l’utilisateur demande de nouveau la même page, le serveur répète l’intégralité de la séquence et retraite la page à partir de zéro. En d’autres termes, un serveur n’a pas de mémoire pour les pages qu’il a traitées : les pages sont sans État. Par conséquent, si une application doit conserver des informations sur une page, sa nature sans État peut devenir un problème.
- **Fonctionnalités clientes inconnues** : dans de nombreux cas, les applications Web sont accessibles à de nombreux utilisateurs à l’aide de différents navigateurs. Les navigateurs ont des fonctionnalités différentes, ce qui complique la création d’une application qui s’exécute tout aussi bien sur tous.
- **Les complications avec l’accès aux données** : la lecture et l’écriture dans une source de données dans des applications Web traditionnelles peuvent être complexes et gourmandes en ressources.
- **Complications avec l’évolutivité** : dans de nombreux cas, les applications Web conçues avec des méthodes existantes ne respectent pas les objectifs d’évolutivité en raison de l’absence de compatibilité entre les différents composants de l’application. Il s’agit souvent d’un point de défaillance courant pour les applications dans un cycle de croissance lourd.

Le respect de ces défis pour les applications Web peut nécessiter beaucoup de temps et d’efforts. ASP.NET Web Forms et ASP.NET Framework résolvent ces problèmes des manières suivantes :

- **Modèle d’objet intuitif et cohérent** : l’infrastructure de page ASP.net présente un modèle d’objet qui vous permet de considérer vos formulaires comme une unité, et non comme des éléments de serveur et de client distincts. Dans ce modèle, vous pouvez programmer la page de manière plus intuitive que dans les applications Web traditionnelles, y compris la possibilité de définir des propriétés pour les éléments de page et de répondre aux événements. En outre, les contrôles serveur ASP.NET sont une abstraction du contenu physique d’une page HTML et de l’interaction directe entre le navigateur et le serveur. En général, vous pouvez utiliser les contrôles serveur de la manière dont vous pouvez travailler avec les contrôles dans une application cliente et vous n’avez pas besoin de réfléchir à la façon de créer le code HTML pour présenter et traiter les contrôles et leur contenu.
- **Modèle de programmation piloté** par les événements-ASP.NET Web Forms apporter aux applications Web le modèle familier d’écriture de gestionnaires d’événements pour les événements qui se produisent sur le client ou le serveur. L’infrastructure de page ASP.NET soustrait ce modèle de telle sorte que le mécanisme sous-jacent de capture d’un événement sur le client, sa transmission au serveur et l’appel de la méthode appropriée soit tous automatique et invisible pour vous. Le résultat est une structure de code clair et facilement écrit qui prend en charge le développement piloté par les événements.
- **Gestion d’État intuitive** : l’infrastructure de page ASP.NET gère automatiquement la tâche de maintenance de l’état de votre page et de ses contrôles, et vous offre des moyens explicites de conserver l’état des informations spécifiques à l’application. Cette mise à niveau est effectuée sans utilisation intensive des ressources du serveur et peut être implémentée avec ou sans envoyer de cookies au navigateur.
- **Applications indépendantes du navigateur** : l’infrastructure de page ASP.net vous permet de créer toute la logique d’application sur le serveur, ce qui évite d’avoir à coder explicitement les différences dans les navigateurs. Toutefois, il vous permet toujours de tirer parti des fonctionnalités spécifiques au navigateur en écrivant du code côté client pour améliorer les performances et une expérience client plus riche.
- **.NET Framework Common Language Runtime prise en charge** : l’infrastructure de page ASP.net est basée sur le .NET Framework, de sorte que l’ensemble de l’infrastructure est disponible pour n’importe quelle application ASP.net. Vos applications peuvent être écrites dans n’importe quel langage qui est compatible avec le Runtime. En outre, l’accès aux données est simplifié à l’aide de l’infrastructure d’accès aux données fournie par le .NET Framework, y compris ADO.NET.
- **.NET Framework des performances de serveur évolutives** : l’infrastructure de page ASP.net vous permet de mettre à l’échelle votre application Web d’un ordinateur avec un processeur unique vers une batterie de serveurs Web à plusieurs ordinateurs, sans modification complexe de la logique de l’application.

## <a name="features-of-aspnet-web-forms"></a>Fonctionnalités de ASP.NET Web Forms

- **Contrôles serveur**: les contrôles serveur Web ASP.net sont des objets sur des pages Web ASP.net qui s’exécutent lorsque la page est demandée et qui restituent le balisage dans le navigateur. De nombreux contrôles de serveur Web sont similaires aux éléments HTML familiers, tels que les boutons et les zones de texte. D’autres contrôles englobent un comportement complexe, tel qu’un contrôle de calendrier, et des contrôles que vous pouvez utiliser pour vous connecter à des sources de données et afficher des données.
- **Pages maîtres**-les pages maîtres ASP.net vous permettent de créer une disposition cohérente pour les pages de votre application. Une seule page maître définit la présentation et le comportement standard voulus pour toutes les pages (ou un groupe de pages) dans votre application. Vous pouvez ensuite créer des pages de contenu individuelles avec le contenu que vous souhaitez afficher. Lorsque les utilisateurs demandent les pages de contenu, ils fusionnent avec la page maître pour produire une sortie qui associe la disposition de la page maître au contenu de la page de contenu.
- L' **utilisation de Data**-ASP.NET fournit de nombreuses options pour le stockage, la récupération et l’affichage de données. Dans une application ASP.NET Web Forms, vous utilisez des contrôles liés aux données pour automatiser la présentation ou l’entrée de données dans des éléments d’interface utilisateur de page Web, tels que des tableaux et des zones de texte et des listes déroulantes.
- **Membership**-ASP.net Identity stocke les informations d’identification de vos utilisateurs dans une base de données créée par l’application. Lorsque vos utilisateurs se connectent, l’application valide leurs informations d’identification en lisant la base de données. Le dossier de *compte* de votre projet contient les fichiers qui implémentent les différentes parties de l’appartenance : l’inscription, la connexion, la modification d’un mot de passe et l’autorisation d’accès. En outre, ASP.NET Web Forms prend en charge OAuth et OpenID. Ces améliorations d’authentification permettent aux utilisateurs de se connecter à votre site à l’aide des informations d’identification existantes, à partir de comptes Facebook, Twitter, Windows Live et Google. Par défaut, le modèle crée une base de données d’appartenance à l’aide d’un nom de base de données par défaut sur une instance de SQL Server Express de base de données locale, le serveur de base de données de développement fourni avec Visual Studio Express 2013 pour le Web.
- **Script client et frameworks client**: vous pouvez améliorer les fonctionnalités basées sur le serveur de ASP.net en incluant la fonctionnalité de script client dans les pages Web Form ASP.net. Vous pouvez utiliser le script client pour fournir une interface utilisateur plus riche et plus réactive aux utilisateurs. Vous pouvez également utiliser le script client pour effectuer des appels asynchrones au serveur Web pendant qu’une page est en cours d’exécution dans le navigateur.
- Le routage d’URL de **routage**vous permet de configurer une application pour accepter les URL de requête qui ne sont pas mappées aux fichiers physiques. Une URL de demande est simplement l’URL entrée par un utilisateur dans son navigateur pour rechercher une page sur votre site Web. Vous utilisez le routage pour définir des URL qui sont sémantiquement significatives pour les utilisateurs et qui peuvent aider à l’optimisation du moteur de recherche (SEO).
- **Gestion**de l’état-ASP.NET Web Forms propose plusieurs options qui vous permettent de conserver les données par page et à l’échelle de l’application.
- **Sécurité**: une partie importante du développement d’une application plus sécurisée consiste à comprendre les menaces qui pèsent sur celle-ci. Microsoft a développé un moyen de catégoriser les menaces : l’usurpation, la falsification, la répudiation, la divulgation d’informations, le déni de service, l’élévation de privilèges (STRIDE). Dans ASP.NET Web Forms, vous pouvez ajouter des points d’extensibilité et des options de configuration qui vous permettent de personnaliser différents comportements de sécurité dans ASP.NET Web Forms.
- **Performances**: les performances peuvent être un facteur clé dans un site Web ou un projet réussi. ASP.NET Web Forms vous permet de modifier les performances liées au traitement des pages et des contrôles serveur, à la gestion des États, à l’accès aux données, à la configuration et au chargement des applications, ainsi qu’aux pratiques de codage efficaces.
- **Internationalisation**-ASP.NET Web Forms vous permet de créer des pages Web qui peuvent obtenir du contenu et d’autres données en fonction du paramètre de langue par défaut pour le navigateur ou en fonction du choix explicite de la langue de l’utilisateur. Le contenu et les autres données sont appelés ressources et ces données peuvent être stockées dans des fichiers de ressources ou d’autres sources. Dans une page de Web Forms de ASP.NET, vous configurez des contrôles pour récupérer leurs valeurs de propriété à partir des ressources. Au moment de l’exécution, les expressions de ressource sont remplacées par des ressources à partir du fichier de ressources localisé approprié.
- **Débogage et gestion des erreurs**-ASP.net comprend des fonctionnalités qui vous aident à diagnostiquer les problèmes susceptibles de survenir dans votre application Web Forms. Le débogage et la gestion des erreurs sont bien pris en charge dans ASP.NET Web Forms pour que vos applications se compilent et s’exécutent efficacement.
- **Déploiement et hébergement**: Visual Studio, ASP.net, Azure et IIS fournissent des outils qui vous aident dans le processus de déploiement et d’hébergement de votre application Web Forms.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Décider quand créer une application Web Forms

Vous devez réfléchir soigneusement s’il faut implémenter une application Web à l’aide du modèle de Web Forms ASP.NET ou d’un autre modèle, tel que l’infrastructure ASP.NET MVC. L'infrastructure MVC ne remplace pas le modèle Web Forms ; vous pouvez utiliser l'une ou l'autre des infrastructures pour les applications Web. Avant de décider d’utiliser le modèle Web Forms ou l’infrastructure MVC pour un site Web spécifique, évaluez les avantages de chaque approche.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Avantages d'une application Web basée sur des Web Forms

L'infrastructure basée sur des Web Forms offre les avantages suivants :

- Elle prend en charge un modèle d'événement permettant la conservation de l'état via HTTP, mécanisme qui s'avère particulièrement utile pour le développement d'applications Web métier. L'application basée sur des Web Forms fournit des dizaines d'événements pris en charge dans des centaines de contrôles serveur.
- Elle utilise un modèle de contrôleur de pages qui ajoute des fonctionnalités aux pages individuelles. Pour plus d’informations, consultez [page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Contrôleur de page") sur le site Web MSDN.
- Il utilise l’état d’affichage ou les formulaires serveur, ce qui peut faciliter la gestion des informations d’État.
- Elle est parfaitement adaptée aux petites équipes de concepteurs et de développeurs Web qui souhaitent tirer parti des nombreux composants disponibles pour le développement rapide d'applications.
- En général, il est moins complexe pour le développement d’applications, car les composants (la classe de **page** , les contrôles, etc.) sont étroitement intégrés et nécessitent généralement moins de code que le modèle MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Avantages d'une application Web basée sur MVC

L'infrastructure ASP.NET MVC offre les avantages suivants :

- Elle permet de gérer plus facilement la complexité en décomposant une application en modèle, vue et contrôleur.
- Elle n'utilise pas l'état d'affichage ni les formulaires serveur. L'infrastructure MVC s'avère par conséquent idéale pour les développeurs souhaitant contrôler totalement le comportement d'une application.
- Elle utilise un modèle de contrôleur frontal qui traite les requêtes de l'application Web par le biais d'un contrôleur unique. De cette façon, vous pouvez concevoir une application prenant en charge une infrastructure de routage complète. Pour plus d’informations, consultez [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Contrôleur frontal") sur le site Web MSDN.
- Elle offre une meilleure prise en charge du développement axé sur des tests.
- Elle fonctionne bien pour les applications Web qui sont prises en charge par les grandes équipes de développeurs et de concepteurs Web qui ont besoin d’un niveau élevé de contrôle sur le comportement de l’application.
