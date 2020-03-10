---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Sécuriser les applications à l’aide de l’authentification et de l’autorisation | Microsoft Docs
author: microsoft
description: L’étape 9 montre comment ajouter l’authentification et l’autorisation pour sécuriser notre application NerdDinner, afin que les utilisateurs aient besoin de s’inscrire et de se connecter au site pour créer...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8d509c5f15bb4d5014e53b8dc2a736454238e72c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600981"
---
# <a name="secure-applications-using-authentication-and-authorization"></a>Sécuriser des applications avec l’authentification et l’autorisation

par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit de l’étape 9 d’un [didacticiel d’application « NerdDinner »](introducing-the-nerddinner-tutorial.md) gratuit qui vous guide dans la création d’une application Web de petite taille, mais complète, à l’aide de ASP.NET MVC 1.
> 
> L’étape 9 montre comment ajouter l’authentification et l’autorisation pour sécuriser notre application NerdDinner, de sorte que les utilisateurs doivent s’inscrire et se connecter au site pour créer de nouveaux dîners, et que seul l’utilisateur qui héberge un dîner peut le modifier ultérieurement.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre les [prise en main avec](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) les didacticiels du [magasin de musique](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou Mvc.

## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner étape 9 : authentification et autorisation

À l’heure actuelle, notre application NerdDinner accorde à toute personne visitant le site la possibilité de créer et de modifier les détails d’un dîner. Nous allons modifier cela afin que les utilisateurs aient besoin de s’inscrire et de se connecter au site pour créer de nouveaux dîners, et ajouter une restriction afin que seul l’utilisateur qui héberge un dîner puisse le modifier ultérieurement.

Pour ce faire, nous allons utiliser l’authentification et l’autorisation pour sécuriser notre application.

### <a name="understanding-authentication-and-authorization"></a>Fonctionnement de l’authentification et de l’autorisation

L' *authentification* est le processus d’identification et de validation de l’identité d’un client qui accède à une application. Plus simplement, il s’agit d’identifier l’utilisateur final lorsqu’il visite un site Web. ASP.NET prend en charge plusieurs méthodes d’authentification des utilisateurs du navigateur. Pour les applications Web Internet, l’approche d’authentification la plus courante utilisée est appelée « authentification par formulaire ». L’authentification par formulaire permet à un développeur de créer un formulaire de connexion HTML dans son application, puis de valider le nom d’utilisateur/mot de passe qu’un utilisateur final envoie sur une base de données ou autre magasin d’informations d’identification de mot de passe. Si la combinaison nom d’utilisateur/mot de passe est correcte, le développeur peut demander à ASP.NET d’émettre un cookie HTTP chiffré pour identifier l’utilisateur parmi les demandes ultérieures. Nous allons utiliser l’authentification par formulaire avec notre application NerdDinner.

L' *autorisation* est le processus qui consiste à déterminer si un utilisateur authentifié a l’autorisation d’accéder à une URL/ressource particulière ou d’effectuer une action. Par exemple, dans notre application NerdDinner, nous souhaitons autoriser que seuls les utilisateurs qui sont connectés peuvent accéder à l’URL */dinners/Create* et créer de nouveaux dîners. Nous souhaitons également ajouter une logique d’autorisation afin que seul l’utilisateur qui héberge un dîner puisse le modifier, et refuser l’accès en modification à tous les autres utilisateurs.

### <a name="forms-authentication-and-the-accountcontroller"></a>Authentification par formulaire et AccountController

Le modèle de projet Visual Studio par défaut pour ASP.NET MVC active automatiquement l’authentification par formulaire lorsque de nouvelles applications ASP.NET MVC sont créées. Il ajoute également automatiquement une implémentation de page de connexion de compte prédéfinie au projet, ce qui rend très facile l’intégration de la sécurité dans un site.

La page maître du site par défaut affiche un lien « ouvrir une session » en haut à droite du site lorsque l’utilisateur qui y accède n’est pas authentifié :

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

En cliquant sur le lien « connexion », vous accédez à l’URL */Account/Logon* :

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Les visiteurs qui n’ont pas été inscrits peuvent le faire en cliquant sur le lien « s’inscrire », qui les mettra à l’URL */Account/Register* et leur permettra d’entrer les informations de compte :

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

En cliquant sur le bouton « s’inscrire », vous créez un nouvel utilisateur dans le système d’appartenance ASP.NET et vous authentifiez l’utilisateur sur le site à l’aide de l’authentification par formulaire.

Quand un utilisateur se connecte, le fichier site. Master modifie le coin supérieur droit de la page pour générer un « Welcome [username] ! » message et affiche un lien « fermer la session » au lieu d’un « ouvrir une session ». Cliquer sur le lien « fermer la session » déconnecte l’utilisateur :

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Les fonctionnalités de connexion, de déconnexion et d’inscription ci-dessus sont implémentées dans la classe AccountController qui a été ajoutée à notre projet par Visual Studio lors de la création du projet. L’interface utilisateur pour AccountController est implémentée à l’aide de modèles de vue dans le répertoire \Views\Account :

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La classe AccountController utilise le système d’authentification ASP.NET Forms pour émettre des cookies d’authentification chiffrés et l’API d’appartenance ASP.NET pour stocker et valider les noms d’utilisateur/mots de passe. L’API d’appartenance ASP.NET est extensible et permet l’utilisation de n’importe quel magasin d’informations d’identification de mot de passe. ASP.NET est fourni avec des implémentations de fournisseur d’appartenances intégrées qui stockent les noms d’utilisateur/mots de passe dans une base de données SQL, ou dans Active Directory.

Nous pouvons configurer le fournisseur d’appartenances que notre application NerdDinner doit utiliser en ouvrant le fichier « Web. config » à la racine du projet et en recherchant la section&gt; de l’appartenance au &lt;. Le fichier Web. config par défaut ajouté lors de la création du projet inscrit le fournisseur d’appartenances SQL et le configure pour qu’il utilise une chaîne de connexion nommée « ApplicationServices » pour spécifier l’emplacement de la base de données.

La chaîne de connexion « ApplicationServices » par défaut (qui est spécifiée dans la section &lt;connectionStrings&gt; du fichier Web. config) est configurée pour utiliser SQL Express. Il pointe vers une base de données SQL Express nommée «ASPNETDB. MDF» dans le répertoire « App\_Data » de l’application. Si cette base de données n’existe pas lors de la première utilisation de l’API d’appartenance au sein de l’application, ASP.NET crée automatiquement la base de données et configure le schéma de base de données d’appartenance approprié au sein de celle-ci :

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Si, au lieu d’utiliser SQL Express, nous souhaitons utiliser une instance de SQL Server complète (ou se connecter à une base de données distante), tout ce qu’il faudrait faire est de mettre à jour la chaîne de connexion « ApplicationServices » dans le fichier Web. config et de vérifier que le schéma d’appartenance approprié a été ajouté à la base de données sur laquelle il pointe. Vous pouvez exécuter l’utilitaire « ASPNET\_RegSql. exe » dans le répertoire \Windows\Microsoft.NET\Framework\v2.0.50727\ pour ajouter le schéma approprié pour l’appartenance et les autres services d’application ASP.NET à une base de données.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorisation de l’URL/Dinners/Create à l’aide du filtre [Authorize]

Nous n’avons pas eu à écrire de code pour activer une implémentation sécurisée de l’authentification et de la gestion des comptes pour l’application NerdDinner. Les utilisateurs peuvent enregistrer de nouveaux comptes avec notre application, ainsi que la connexion/déconnexion du site.

À présent, nous pouvons ajouter une logique d’autorisation à l’application et utiliser l’état d’authentification et le nom d’utilisateur des visiteurs pour contrôler ce qu’ils peuvent et ne peuvent pas faire au sein du site. Commençons par ajouter la logique d’autorisation aux méthodes d’action « Create » de notre classe DinnersController. Plus précisément, nous exigeons que les utilisateurs qui accèdent à l’URL */dinners/Create* doivent être connectés. S’ils ne sont pas connectés, nous les redirigeons vers la page de connexion pour qu’ils puissent se connecter.

Il est assez facile d’implémenter cette logique. Tout ce que nous devons faire, c’est d’ajouter un attribut de filtre [Authorize] à nos méthodes d’action de création, comme suit :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC prend en charge la possibilité de créer des « filtres d’action » qui peuvent être utilisés pour implémenter une logique réutilisable qui peut être appliquée de manière déclarative aux méthodes d’action. Le filtre [Authorize] est l’un des filtres d’action intégrés fournis par ASP.NET MVC et permet à un développeur d’appliquer de façon déclarative des règles d’autorisation aux méthodes d’action et aux classes de contrôleur.

Lorsqu’il est appliqué sans aucun paramètre (comme ci-dessus), le filtre [Authorize] impose que l’utilisateur qui effectue la demande de méthode d’action doive être connecté et redirige automatiquement le navigateur vers l’URL de connexion, le cas échéant. Lors de cette redirection, l’URL demandée à l’origine est transmise en tant qu’argument QueryString (par exemple:/Account/LogOn ? ReturnUrl =% 2fDinners% 2fCreate). Le AccountController redirige ensuite l’utilisateur vers l’URL demandée à l’origine une fois qu’il se connecte.

Le filtre [Authorize] prend éventuellement en charge la possibilité de spécifier une propriété « Users » ou « Roles » qui peut être utilisée pour exiger que l’utilisateur soit à la fois connecté et dans une liste d’utilisateurs autorisés ou membre d’un rôle de sécurité autorisé. Par exemple, le code ci-dessous autorise uniquement deux utilisateurs spécifiques, « scottgu » et « billg », à accéder à l’URL/Dinners/Create :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Toutefois, l’incorporation de noms d’utilisateur spécifiques dans le code a tendance à être plutôt impossible à entretenir. Une meilleure approche consiste à définir des « rôles » de niveau supérieur pour la vérification du code, puis à mapper les utilisateurs dans le rôle à l’aide d’une base de données ou d’un système Active Directory (en activant la liste de mappages d’utilisateur réelle à stocker en externe à partir du code). ASP.NET inclut une API de gestion des rôles intégrée, ainsi qu’un ensemble intégré de fournisseurs de rôles (y compris ceux pour SQL et Active Directory) qui peuvent aider à effectuer ce mappage utilisateur/rôle. Nous pourrions ensuite mettre à jour le code pour permettre uniquement aux utilisateurs d’un rôle « administrateur » spécifique d’accéder à l’URL/Dinners/Create :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Utilisation de la propriété User.Identity.Name lors de la création de dîners

Nous pouvons récupérer le nom d’utilisateur de l’utilisateur actuellement connecté à une demande à l’aide de la propriété User.Identity.Name exposée sur la classe de base du contrôleur.

Plus haut, lorsque nous avons implémenté la version HTTP-POSTÉRIEURe de notre méthode d’action Create (), nous avions codé en dur la propriété « HostedBy » du dîner sur une chaîne statique. Nous pouvons maintenant mettre à jour ce code pour utiliser à la place la propriété User.Identity.Name, et ajouter automatiquement une RSVP pour l’hôte qui crée le dîner :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Étant donné que nous avons ajouté un attribut [Authorize] à la méthode Create (), ASP.NET MVC garantit que la méthode d’action s’exécute uniquement si l’utilisateur visitant l’URL/Dinners/Create est connecté au site. Par conséquent, la valeur de la propriété User.Identity.Name contient toujours un nom d’utilisateur valide.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Utilisation de la propriété User.Identity.Name lors de la modification de dîners

Nous allons maintenant ajouter une logique d’autorisation qui restreint les utilisateurs afin qu’ils puissent uniquement modifier les propriétés des dîners qu’ils hébergent eux-mêmes.

Pour vous aider, nous allons d’abord ajouter une méthode d’assistance « IsHostedBy (username) » à notre objet dîner (dans la classe partielle Dinner.cs que nous avons créée précédemment). Cette méthode d’assistance retourne la valeur true ou false selon que le nom d’utilisateur fourni correspond à la propriété Dinner HostedBy et encapsule la logique nécessaire pour effectuer une comparaison de chaînes ne respectant pas la casse :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Nous allons ensuite ajouter un attribut [Authorize] aux méthodes d’action Edit () au sein de notre classe DinnersController. Cela permet de s’assurer que les utilisateurs doivent être connectés pour demander une URL */dinners/Edit/[id]* .

Nous pouvons ensuite ajouter du code à nos méthodes de modification qui utilisent la méthode d’assistance dinner. IsHostedBy (nom d’utilisateur) pour vérifier que l’utilisateur connecté correspond à l’hôte du dîner. Si l’utilisateur n’est pas l’hôte, nous affichons une vue « InvalidOwner » et terminons la demande. Le code permettant d’effectuer cette opération ressemble à ce qui suit :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Nous pouvons ensuite cliquer avec le bouton droit sur le répertoire \Views\Dinners et choisir la commande de menu Ajouter-&gt;afficher pour créer une nouvelle vue « InvalidOwner ». Nous allons le remplir avec le message d’erreur ci-dessous :

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Désormais, lorsqu’un utilisateur tente de modifier un dîner qu’il ne possède pas, un message d’erreur s’affiche :

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Nous pouvons répéter les mêmes étapes pour les méthodes d’action DELETE () au sein de notre contrôleur afin de verrouiller l’autorisation de supprimer les dîners, et vous assurer que seul l’hôte d’un dîner peut le supprimer.

### <a name="showinghiding-edit-and-delete-links"></a>Montrer/masquer les liens modifier et supprimer

Nous créons une liaison avec la méthode d’action Edit et Delete de notre classe DinnersController à partir de notre URL de détails :

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Actuellement, nous affichons les liens d’action modifier et supprimer, que le visiteur de l’URL des détails soit l’hôte du dîner. Nous allons modifier cela afin que les liens ne s’affichent que si l’utilisateur visiteur est le propriétaire du dîner.

La méthode d’action Details () dans notre DinnersController récupère un objet Dinner, puis le passe en tant qu’objet de modèle à notre modèle de vue :

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Nous pouvons mettre à jour notre modèle de vue pour afficher ou masquer de manière conditionnelle les liens modifier et supprimer à l’aide de la méthode d’assistance dinner. IsHostedBy (), comme ci-dessous :

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Étapes suivantes

Voyons maintenant comment nous pouvons permettre aux utilisateurs authentifiés de répondre aux dîners à l’aide d’AJAX.

> [!div class="step-by-step"]
> [Précédent](implement-efficient-data-paging.md)
> [Suivant](use-ajax-to-deliver-dynamic-updates.md)
