---
uid: single-page-application/overview/templates/backbonejs-template
title: Modèle de réseau principal | Microsoft Docs
author: madskristensen
description: Modèle d’authentification de réseau principal. js
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 7297db7d5b35a53b40f9d9162960e529a167bd12
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074889"
---
# <a name="backbone-template"></a>Modèle Backbone

par [Mads Kristensen](https://github.com/madskristensen)

> Le modèle principal SPA a été écrit par Kazi Manzur Rashid
> 
> [Télécharger le modèle SPA backbone. js](https://go.microsoft.com/fwlink/?LinkId=293631)

Le modèle SPA backbone. js est conçu pour vous aider à créer rapidement des applications Web interactives côté client à l’aide de [backbone. js.](http://backbonejs.org/)

Le modèle fournit un squelette initial pour le développement d’une application backbone. js dans ASP.NET MVC. Il fournit les fonctionnalités de base de la connexion utilisateur, y compris l’inscription des utilisateurs, la connexion, la réinitialisation du mot de passe et la confirmation de l’utilisateur avec les modèles de messagerie de base.

Conditions requises :

- [ASP.NET et Web Tools mise à jour 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Créer un projet de modèle de réseau principal

Téléchargez et installez le modèle en cliquant sur le bouton de téléchargement ci-dessus. Le modèle est empaqueté sous la forme d’un fichier d’extension Visual Studio (VSIX). Vous devrez peut-être redémarrer Visual Studio.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**. Nommez le projet, puis cliquez sur **OK**.

![](backbonejs-template/_static/image1.png)

Dans l’Assistant **nouveau projet** , sélectionnez le projet backbone. js Spa.

![](backbonejs-template/_static/image2.png)

Appuyez sur CTRL + F5 pour générer et exécuter l’application sans débogage, ou appuyez sur F5 pour exécuter le débogage.

![](backbonejs-template/_static/image3.png)

Le fait de cliquer sur « mon compte » affiche la page de connexion :

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Procédure pas à pas : code client

Commençons par le côté client. Les scripts d’application cliente se trouvent dans le dossier ~/scripts/application L’application est [écrite dans des](http://www.typescriptlang.org/) fichiers. TS, qui sont compilés en fichiers JavaScript (. js).

**Application**

`Application` est défini dans application. TS. Cet objet Initialise l’application et agit comme l’espace de noms racine. Il conserve les informations de configuration et d’État partagées dans l’application, par exemple si l’utilisateur est connecté.

La méthode `application.start` crée les vues modales et attache des gestionnaires d’événements pour les événements de niveau application, tels que la connexion de l’utilisateur. Ensuite, il crée le routeur par défaut et vérifie si une URL côté client est spécifiée. Si ce n’est pas le cas, il redirige vers l’URL par défaut (# !/).

**Événements**

Les événements sont toujours importants lors du développement de composants faiblement couplés. Les applications effectuent souvent plusieurs opérations en réponse à une action de l’utilisateur. Le réseau principal fournit des événements intégrés avec des composants tels que Model, collection et View. Au lieu de créer des interdépendances entre ces composants, le modèle utilise un modèle « Pub/Sub » : l’objet `events`, défini dans Events. TS, agit en tant que Event Hub pour la publication et l’abonnement aux événements d’application. L’objet `events` est un singleton. Le code suivant montre comment s’abonner à un événement, puis déclencher l’événement :

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Routeur**

Dans backbone. js, un routeur fournit des méthodes pour le routage des pages côté client et leur connexion aux actions et aux événements. Le modèle définit un routeur unique, dans Router. TS. Le routeur crée les vues activables et conserve l’état lors du basculement des vues. (Les affichages activables sont décrits dans la section suivante.) Au départ, le projet comporte deux vues factices, à l’origine et à propos de. Elle dispose également d’une vue NotFound, qui s’affiche si l’itinéraire n’est pas connu.

**Views**

Les vues sont définies dans ~/scripts/application/views. Il existe deux types de vues : les vues activables et les vues de boîtes de dialogue modales. Les vues activables sont appelées par le routeur. Lorsqu’une vue activable est affichée, toutes les autres vues activables deviennent inactives. Pour créer une vue activable, étendez la vue avec l’objet `Activable` :

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

L’extension avec `Activable` ajoute deux nouvelles méthodes à la vue, `activate` et `deactivate`. Le routeur appelle ces méthodes pour activer et désactiver la vue.

Les vues modales sont implémentées sous la forme de boîtes de dialogue modales de [démarrage Twitter](https://twitter.github.com/bootstrap/) . Les vues `Membership` et `Profile` sont des vues modales. Les vues de modèle peuvent être appelées par n’importe quel événement d’application. Par exemple, dans la vue `Navigation`, le fait de cliquer sur le lien « mon compte » affiche soit la vue `Membership`, soit la vue `Profile`, selon que l’utilisateur est connecté ou non. Le `Navigation` attache des gestionnaires d’événements Click à tous les éléments enfants qui ont l’attribut `data-command`. Voici le balisage HTML :

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Voici le code dans navigation. TS pour raccorder les événements :

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modèles**

Les modèles sont définis dans ~/scripts/application/Models. Les modèles ont tous trois éléments de base : les attributs par défaut, les règles de validation et un point de terminaison côté serveur. Voici un exemple typique :

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Plug-ins**

Le dossier ~/scripts/application/lib contient quelques plug-ins jQuery pratiques. Le fichier Form. TS définit un plug-in pour l’utilisation des données de formulaire. Vous devez souvent sérialiser ou désérialiser les données de formulaire et afficher les erreurs de validation de modèle. Le plug-in Form. TS possède des méthodes telles que `serializeFields`, `deserializeFields`et `showFieldErrors`. L’exemple suivant montre comment sérialiser un formulaire dans un modèle.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Le plug-in Flashbar. TS fournit différents types de messages de commentaires à l’utilisateur. Les méthodes sont `$.showSuccessbar`, `$.showErrorbar` et `$.showInfobar`. En arrière-plan, elle utilise des alertes d’amorçage Twitter pour afficher des messages animés.

Le plug-in Confirm. TS remplace la boîte de dialogue de confirmation du navigateur, bien que l’API soit quelque peu différente :

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Procédure pas à pas : code serveur

Examinons à présent le côté serveur.

**Contrôleurs**

Dans une application à page unique, le serveur ne joue qu’un petit rôle dans l’interface utilisateur. En règle générale, le serveur affiche la page initiale, puis envoie et reçoit les données JSON.

Le modèle a deux contrôleurs MVC : `HomeController` restitue la page initiale et `SupportsController` est utilisé pour confirmer les nouveaux comptes d’utilisateur et réinitialiser les mots de passe. Tous les autres contrôleurs du modèle sont des contrôleurs API Web ASP.NET, qui envoient et reçoivent des données JSON. Par défaut, les contrôleurs utilisent la nouvelle classe `WebSecurity` pour effectuer des tâches liées à l’utilisateur. Toutefois, ils ont également des constructeurs facultatifs qui vous permettent de transmettre des délégués pour ces tâches. Cela facilite le test et vous permet de remplacer `WebSecurity` par autre chose, à l’aide d’un conteneur IoC. Voici un exemple :

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Les vues

Les vues sont conçues pour être modulaires : chaque section d’une page possède sa propre vue dédiée. Dans une application à page unique, il est courant d’inclure des vues qui n’ont pas de contrôleur correspondant. Vous pouvez inclure une vue en appelant `@Html.Partial('myView')`, mais cela est fastidieux. Pour faciliter cette opération, le modèle définit une méthode d’assistance, `IncludeClientViews`, qui affiche toutes les vues dans un dossier spécifié :

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Si le nom du dossier n’est pas spécifié, le nom du dossier par défaut est « ClientViews ». Si votre vue client utilise également des vues partielles, nommez la vue partielle avec un trait de soulignement (par exemple, `_SignUp`). La méthode `IncludeClientViews` exclut toutes les vues dont le nom commence par un trait de soulignement. Pour inclure une vue partielle dans la vue cliente, appelez `Html.ClientView('SignUp')` au lieu de `Html.Partial('_SignUp')`.

**Envoi de courrier électronique**

Pour envoyer un e-mail, le modèle utilise la valeur [postale](http://aboutcode.net/postal). Toutefois, la postale est extraite du reste du code avec l’interface `IMailer`, ce qui vous permet de la remplacer facilement par une autre implémentation. Les modèles d’e-mail se trouvent dans le dossier views/emails. L’adresse de messagerie de l’expéditeur est spécifiée dans le fichier Web. config, dans la clé `sender.email` de la section **appSettings** . En outre, quand `debug="true"` dans Web. config, l’application ne nécessite pas de confirmation de l’adresse de messagerie de l’utilisateur pour accélérer le développement.

## <a name="github"></a>GitHub

Vous pouvez également trouver le modèle SPA backbone. js sur [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
