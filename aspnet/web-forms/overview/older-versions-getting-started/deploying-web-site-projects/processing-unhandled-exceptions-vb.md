---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Traitement des exceptions non gérées (VB) | Microsoft Docs
author: rick-anderson
description: Lorsqu’une erreur d’exécution se produit sur une application Web en production, il est important d’informer un développeur et de consigner l’erreur pour qu’elle puisse être diagnostiquée à la...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 8d2e12af29bd95ce9898fa6a5e81be8b7a761f76
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586064"
---
# <a name="processing-unhandled-exceptions-vb"></a>Traitement des exceptions non gérées (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([procédure de téléchargement](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Lorsqu’une erreur d’exécution se produit sur une application Web en production, il est important d’informer un développeur et de consigner l’erreur pour qu’elle puisse être diagnostiquée ultérieurement. Ce didacticiel fournit une vue d’ensemble de la façon dont ASP.NET traite les erreurs d’exécution et examine un moyen d’exécuter du code personnalisé chaque fois qu’une exception non gérée se propage jusqu’au runtime ASP.NET.

## <a name="introduction"></a>Introduction

Lorsqu’une exception non gérée se produit dans une application ASP.NET, elle se propage jusqu’au runtime ASP.NET, qui déclenche l’événement `Error` et affiche la page d’erreur appropriée. Il existe trois types différents de pages d’erreur : l’erreur d’exécution jaune de décès (YSOD); Détails de l’exception YSOD ; et les pages d’erreurs personnalisées. Dans le [didacticiel précédent](displaying-a-custom-error-page-vb.md) , nous avons configuré l’application pour qu’elle utilise une page d’erreurs personnalisée pour les utilisateurs distants et les détails de l’exception YSOD pour les utilisateurs visitant localement.

L’utilisation d’une page d’erreurs personnalisées conviviale qui correspond à l’apparence du site est préférable à l’erreur d’exécution par défaut YSOD, mais l’affichage d’une page d’erreurs personnalisée n’est qu’une partie d’une solution complète de gestion des erreurs. Lorsqu’une erreur se produit dans une application en production, il est important que les développeurs soient informés de l’erreur afin qu’ils puissent Unearth la cause de l’exception et résoudre le problème. En outre, les détails de l’erreur doivent être enregistrés afin que l’erreur puisse être examinée et diagnostiquée ultérieurement.

Ce didacticiel montre comment accéder aux détails d’une exception non gérée pour qu’ils puissent être journalisés et qu’un développeur en soit informé. Les deux didacticiels suivant celui-ci explorent les bibliothèques de journalisation des erreurs qui, après un peu de configuration, informent automatiquement les développeurs des erreurs d’exécution et consignent leurs détails.

> [!NOTE]
> Les informations examinées dans ce didacticiel sont particulièrement utiles si vous devez traiter des exceptions non gérées de manière unique ou personnalisée. Dans les cas où vous devez uniquement consigner l’exception et avertir un développeur, l’utilisation d’une bibliothèque de journalisation des erreurs est la méthode à suivre. Les deux didacticiels suivants fournissent une vue d’ensemble de deux de ces bibliothèques.

## <a name="executing-code-when-theerrorevent-is-raised"></a>Exécution du code lorsque l’événement`Error`est déclenché

Les événements fournissent à un objet un mécanisme pour signaler qu’un événement intéressant s’est produit et pour qu’un autre objet exécute du code en réponse. En tant que développeur ASP.NET, vous êtes habitué à réfléchir en termes d’événements. Si vous souhaitez exécuter du code lorsque le visiteur clique sur un bouton particulier, vous créez un gestionnaire d’événements pour l’événement `Click` de ce bouton et y Placez votre code. Étant donné que le runtime ASP.NET déclenche son [événement`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) chaque fois qu’une exception non gérée se produit, il suit que le code pour la journalisation des détails de l’erreur se trouve dans un gestionnaire d’événements. Mais comment créer un gestionnaire d’événements pour l’événement `Error` ?

L’événement `Error` est l’un des nombreux événements de la [classe`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) qui sont déclenchés à certaines étapes du pipeline http pendant la durée de vie d’une demande. Par exemple, l' [événement`BeginRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) de la classe `HttpApplication` est déclenché au début de chaque demande. son [`AuthenticateRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) est déclenché lorsqu’un module de sécurité a identifié le demandeur. Ces événements `HttpApplication` donnent au développeur de pages un moyen d’exécuter une logique personnalisée aux différents points de la durée de vie d’une demande.

Les gestionnaires d’événements pour les événements `HttpApplication` peuvent être placés dans un fichier spécial nommé `Global.asax`. Pour créer ce fichier dans votre site Web, ajoutez un nouvel élément à la racine de votre site Web à l’aide du modèle de classe d’application globale portant le nom `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Figure 1**: ajouter des `Global.asax` à votre application Web  
 ([Cliquez pour afficher l’image en taille réelle](processing-unhandled-exceptions-vb/_static/image3.png))

Le contenu et la structure du fichier `Global.asax` créé par Visual Studio varient légèrement selon que vous utilisez un projet d’application Web (WAP) ou un projet de site Web (WSP). Avec un WAP, le `Global.asax` est implémenté sous la forme de deux fichiers distincts : `Global.asax` et `Global.asax.vb`. Le fichier `Global.asax` ne contient rien, mais une directive `@Application` qui référence le fichier `.vb` ; les gestionnaires d’événements intéressants sont définis dans le fichier `Global.asax.vb`. Pour wsp, un seul fichier est créé, `Global.asax`, et les gestionnaires d’événements sont définis dans un bloc `<script runat="server">`.

Le fichier `Global.asax` créé dans un WAP par le modèle de classe d’application globale de Visual Studio comprend des gestionnaires d’événements nommés `Application_BeginRequest`, `Application_AuthenticateRequest`et `Application_Error`, qui sont des gestionnaires d’événements pour les événements `HttpApplication` `BeginRequest`, `AuthenticateRequest`et `Error`, respectivement. Il existe également des gestionnaires d’événements nommés `Application_Start`, `Session_Start`, `Application_End`et `Session_End`, qui sont des gestionnaires d’événements qui se déclenchent lorsque l’application Web démarre, lorsqu’une nouvelle session démarre, lorsque l’application se termine et lorsqu’une session se termine, respectivement. Le fichier `Global.asax` créé dans un WSP par Visual Studio contient uniquement les gestionnaires d’événements `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`et `Session_End`.

> [!NOTE]
> Lors du déploiement de l’application ASP.NET, vous devez copier le fichier `Global.asax` dans l’environnement de production. Le fichier `Global.asax.vb`, qui est créé dans le WAP, n’a pas besoin d’être copié en production, car ce code est compilé dans l’assembly du projet.

Les gestionnaires d’événements créés par le modèle de classe d’application globale de Visual Studio ne sont pas exhaustifs. Vous pouvez ajouter un gestionnaire d’événements pour n’importe quel événement `HttpApplication` en nommant le gestionnaire d’événements `Application_EventName`. Par exemple, vous pouvez ajouter le code suivant au fichier `Global.asax` pour créer un gestionnaire d’événements pour l' [événement`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

De même, vous pouvez supprimer tous les gestionnaires d’événements créés par le modèle de classe d’application globale qui ne sont pas nécessaires. Pour ce didacticiel, nous avons uniquement besoin d’un gestionnaire d’événements pour l’événement `Error` ; n’hésitez pas à supprimer les autres gestionnaires d’événements du fichier `Global.asax`.

> [!NOTE]
> Les *modules http* offrent une autre façon de définir des gestionnaires d’événements pour les événements de `HttpApplication`. Les modules HTTP sont créés sous la forme d’un fichier de classe qui peut être placé directement dans le projet d’application Web ou séparé dans une bibliothèque de classes distincte. Dans la mesure où elles peuvent être séparées dans une bibliothèque de classes, les modules HTTP offrent un modèle plus flexible et réutilisable pour créer des gestionnaires d’événements `HttpApplication`. Tandis que le fichier `Global.asax` est spécifique à l’application Web où il réside, les modules HTTP peuvent être compilés en assemblys. à partir de là, l’ajout du module HTTP à un site Web est aussi simple que la suppression de l’assembly dans le dossier `Bin` et l’inscription du module dans `Web.config`. Ce didacticiel n’aborde pas la création et l’utilisation de modules HTTP, mais les deux bibliothèques de journalisation des erreurs utilisées dans les deux didacticiels suivants sont implémentées en tant que modules HTTP. Pour plus d’informations sur les avantages des modules HTTP [, consultez Utilisation des modules et gestionnaires HTTP pour créer des composants ASP.net enfichables](https://msdn.microsoft.com/library/aa479332.aspx).

## <a name="retrieving-information-about-the-unhandled-exception"></a>Récupération d’informations sur l’exception non gérée

À ce stade, nous disposons d’un fichier global. asax avec un gestionnaire d’événements `Application_Error`. Lorsque ce gestionnaire d’événements s’exécute, nous devons informer un développeur de l’erreur et enregistrer ses détails. Pour accomplir ces tâches, nous devons d’abord déterminer les détails de l’exception qui a été levée. Utilisez la [méthode`GetLastError`](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) de l’objet serveur pour récupérer les détails de l’exception non gérée qui a provoqué le déclenchement de l’événement `Error`.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

La méthode `GetLastError` retourne un objet de type `Exception`, qui est le type de base pour toutes les exceptions dans le .NET Framework. Toutefois, dans le code ci-dessus, je caste l’objet exception retourné par `GetLastError` dans un objet `HttpException`. Si l’événement `Error` est déclenché parce qu’une exception a été levée pendant le traitement d’une ressource ASP.NET, l’exception levée est encapsulée dans une `HttpException`. Pour récupérer l’exception réelle qui a déclenché l’événement d’erreur, utilisez la propriété `InnerException`. Si l’événement `Error` a été déclenché en raison d’une exception HTTP, telle qu’une demande pour une page inexistante, une `HttpException` est levée, mais elle n’a pas d’exception interne.

Le code suivant utilise la `GetLastErrormessage` pour récupérer des informations sur l’exception qui a déclenché l’événement `Error`, en stockant le `HttpException` dans une variable nommée `lastErrorWrapper`. Il stocke ensuite le type, le message et la trace de la pile de l’exception d’origine dans trois variables de chaîne, en vérifiant si le `lastErrorWrapper` est l’exception réelle qui a déclenché l’événement `Error` (dans le cas d’exceptions basées sur HTTP) ou s’il s’agit simplement d’un wrapper pour une exception qui a été levée lors du traitement de la demande.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

À ce stade, vous disposez de toutes les informations dont vous avez besoin pour écrire du code qui consignera les détails de l’exception dans une table de base de données. Vous pouvez créer une table de base de données avec des colonnes pour chacun des détails d’erreur intéressants : le type, le message, la trace de la pile, etc., ainsi que d’autres informations utiles, telles que l’URL de la page demandée et le nom de l’utilisateur actuellement connecté. Dans le gestionnaire d’événements `Application_Error`, vous vous connectez ensuite à la base de données et insérez un enregistrement dans la table. De même, vous pouvez ajouter du code pour alerter un développeur de l’erreur par courrier électronique.

Les bibliothèques de journalisation des erreurs examinées dans les deux didacticiels ci-dessous fournissent des fonctionnalités prêtes à l’emploi. il n’est donc pas nécessaire de créer vous-même cette erreur de journalisation et de notification. Toutefois, pour illustrer que l’événement `Error` est déclenché et que le gestionnaire d’événements `Application_Error` peut être utilisé pour enregistrer les détails de l’erreur et notifier un développeur, nous allons ajouter du code qui notifie un développeur quand une erreur se produit.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Notification d’un développeur lorsqu’une exception non gérée se produit

Lorsqu’une exception non gérée se produit dans l’environnement de production, il est important d’informer l’équipe de développement afin qu’elle puisse évaluer l’erreur et déterminer les actions à entreprendre. Par exemple, en cas d’erreur lors de la connexion à la base de données, vous devez vérifier votre chaîne de connexion et, éventuellement, ouvrir un ticket de support auprès de votre entreprise d’hébergement Web. Si l’exception s’est produite en raison d’une erreur de programmation, une logique de code ou de validation supplémentaire devra peut-être être ajoutée pour éviter de telles erreurs à l’avenir.

Les classes .NET Framework de l' [espace de noms`System.Net.Mail`](https://msdn.microsoft.com/library/system.net.mail.aspx) facilitent l’envoi d’un message électronique. La [classe`MailMessage`](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) représente un message électronique et possède des propriétés comme `To`, `From`, `Subject`, `Body`et `Attachments`. Le `SmtpClass` est utilisé pour envoyer un objet `MailMessage` à l’aide d’un serveur SMTP spécifié. les paramètres du serveur SMTP peuvent être spécifiés par programmation ou de manière déclarative dans l' [élément`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx) de l' `Web.config file`. Pour plus d’informations sur l’envoi de messages électroniques dans une application ASP.NET, consultez mon article, [envoi de courrier électronique dans ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)et le [FAQ System .net. mail](http://systemnetmail.com/).

> [!NOTE]
> L’élément `<system.net>` contient les paramètres de serveur SMTP utilisés par la classe `SmtpClient` lors de l’envoi d’un message électronique. Votre entreprise d’hébergement Web dispose probablement d’un serveur SMTP que vous pouvez utiliser pour envoyer des courriers électroniques à partir de votre application. Pour plus d’informations sur les paramètres de serveur SMTP que vous devez utiliser dans votre application Web, consultez la section prise en charge de votre hôte Web.

Ajoutez le code suivant au gestionnaire d’événements `Application_Error` pour envoyer un message électronique à un développeur lorsqu’une erreur se produit :

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Tandis que le code ci-dessus est assez long, la majeure partie de l’informatique crée le code HTML qui apparaît dans le message envoyé au développeur. Le code commence par référencer le `HttpException` retourné par la méthode `GetLastError` (`lastErrorWrapper`). L’exception réelle qui a été levée par la requête est récupérée via `lastErrorWrapper.InnerException` et est assignée à la variable `lastError`. Les informations de type, de message et de trace de la pile sont extraites de `lastError` et stockées dans trois variables de chaîne.

Ensuite, un objet `MailMessage` nommé `mm` est créé. Le corps de l’e-mail est au format HTML et affiche l’URL de la page demandée, le nom de l’utilisateur actuellement connecté et les informations sur l’exception (type, message et trace de la pile). L’un des aspects intéressants de la classe `HttpException` est que vous pouvez générer le code HTML utilisé pour créer les détails de l’exception écran jaune de décès (YSOD) en appelant la [méthode GetHtmlErrorMessage](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Cette méthode est utilisée ici pour récupérer les détails de l’exception YSOD le balisage et l’ajouter à l’e-mail en tant que pièce jointe. Un mot de prudence : si l’exception qui a déclenché l’événement `Error` était une exception HTTP (par exemple, une demande pour une page inexistante), la méthode `GetHtmlErrorMessage` renverra `null`.

La dernière étape consiste à envoyer le `MailMessage`. Pour ce faire, vous créez une nouvelle `SmtpClient` méthode et appelez sa méthode `Send`.

> [!NOTE]
> Avant d’utiliser ce code dans votre application Web, vous souhaiterez modifier les valeurs des `ToAddress` et `FromAddress` constantes de support@example.com à l’adresse de messagerie à laquelle l’e-mail de notification d’erreur doit être envoyé et provient. Vous devez également spécifier les paramètres du serveur SMTP dans la section `<system.net>` de `Web.config`. Consultez votre fournisseur d’hébergement Web pour déterminer les paramètres du serveur SMTP à utiliser.

Avec ce code en place à chaque fois qu’une erreur se produit, le développeur reçoit un message électronique qui résume l’erreur et inclut le YSOD. Dans le didacticiel précédent, nous avons présenté une erreur d’exécution en visitant genre. aspx et en transmettant une valeur de `ID` non valide via QueryString, comme `Genre.aspx?ID=foo`. En visitant la page avec le fichier `Global.asax` en place, vous obtenez la même expérience utilisateur que dans le didacticiel précédent. dans l’environnement de développement, vous continuez à voir les détails de l’exception jaune de la mort, tandis que dans l’environnement de production, vous verrez la page d’erreurs personnalisées. En plus de ce comportement existant, le développeur reçoit un message électronique.

La **figure 2** montre l’e-mail reçu lors de la visite `Genre.aspx?ID=foo`. Le corps de l’e-mail résume les informations sur les exceptions, tandis que la `YSOD.htm` pièce jointe affiche le contenu affiché dans les détails de l’exception YSOD (voir **figure 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Figure 2**: envoi d’une notification par courrier électronique par le développeur lorsqu’il existe une exception non gérée  
 ([Cliquez pour afficher l’image en taille réelle](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Figure 3**: la notification par courrier électronique contient les détails de l’exception YSOD en tant que pièce jointe  
 ([Cliquez pour afficher l’image en taille réelle](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Qu’en est-il de l’utilisation de la page d’erreurs personnalisée ?

Ce didacticiel vous a montré comment utiliser `Global.asax` et le gestionnaire d’événements `Application_Error` pour exécuter du code lorsqu’une exception non gérée se produit. Plus précisément, nous avons utilisé ce gestionnaire d’événements pour notifier une erreur au développeur. Nous pourrions l’étendre pour consigner les détails de l’erreur dans une base de données. La présence du gestionnaire d’événements `Application_Error` n’affecte pas l’expérience de l’utilisateur final. Ils voient toujours la page d’erreur configurée, être les détails de l’erreur YSOD, l’erreur d’exécution YSOD ou la page d’erreurs personnalisée.

Il est naturel de se demander si les `Global.asax` fichier et `Application_Error` événement sont nécessaires lors de l’utilisation d’une page d’erreurs personnalisée. Lorsqu’une erreur se produit, l’utilisateur affiche la page d’erreur personnalisée. Pourquoi ne pouvons-nous pas placer le code pour notifier le développeur et enregistrer les détails de l’erreur dans la classe code-behind de la page d’erreurs personnalisée ? Bien que vous puissiez certainement ajouter du code à la classe code-behind de la page d’erreurs personnalisée, vous n’avez pas accès aux détails de l’exception qui a déclenché l’événement `Error` lors de l’utilisation de la technique que nous avons explorée dans le didacticiel précédent. L’appel de la méthode `GetLastError` à partir de la page d’erreurs personnalisée retourne `Nothing`.

La raison de ce comportement est que la page d’erreurs personnalisée est atteinte via une redirection. Lorsqu’une exception non gérée atteint le runtime ASP.NET, le moteur ASP.NET déclenche son événement `Error` (qui exécute le gestionnaire d’événements `Application_Error`), puis *redirige* l’utilisateur vers la page d’erreurs personnalisée en émettant un `Response.Redirect(customErrorPageUrl)`. La méthode `Response.Redirect` envoie une réponse au client avec un code d’état HTTP 302, indiquant au navigateur de demander une nouvelle URL, à savoir la page d’erreurs personnalisées. Le navigateur demande ensuite automatiquement cette nouvelle page. Vous pouvez indiquer que la page d’erreurs personnalisée a été demandée séparément de la page d’origine de l’erreur, car la barre d’adresses du navigateur devient l’URL de la page d’erreurs personnalisée (voir **figure 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Figure 4**: lorsqu’une erreur se produit, le navigateur est redirigé vers l’URL de la page d’erreurs personnalisée  
 ([Cliquez pour afficher l’image en taille réelle](processing-unhandled-exceptions-vb/_static/image12.png))

L’effet net est que la demande dans laquelle l’exception non gérée s’est produite se termine lorsque le serveur répond avec la redirection HTTP 302. La requête suivante à la page d’erreur personnalisée est une toute nouvelle demande ; à ce stade, le moteur ASP.NET a ignoré les informations d’erreur et, en outre, n’a aucun moyen d’associer l’exception non gérée dans la requête précédente à la nouvelle demande de la page d’erreurs personnalisée. C’est pourquoi `GetLastError` retourne `null` quand elle est appelée à partir de la page d’erreurs personnalisée.

Toutefois, il est possible que la page d’erreurs personnalisée soit exécutée au cours de la même demande qui a provoqué l’erreur. La méthode [`Server.Transfer(url)`](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) transfère l’exécution à l’URL spécifiée et la traite dans la même requête. Vous pouvez déplacer le code dans le gestionnaire d’événements `Application_Error` vers la classe code-behind de la page d’erreurs personnalisée, en le remplaçant par `Global.asax` par le code suivant :

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Désormais, lorsqu’une exception non gérée se produit, le gestionnaire d’événements `Application_Error` transfère le contrôle à la page d’erreurs personnalisée appropriée en fonction du code d’état HTTP. Étant donné que le contrôle a été transféré, la page d’erreurs personnalisée a accès aux informations sur les exceptions non gérées par le biais de `Server.GetLastError` et peut informer un développeur de l’erreur et consigner ses détails. L’appel de `Server.Transfer` empêche le moteur ASP.NET de rediriger l’utilisateur vers la page d’erreurs personnalisée. Au lieu de cela, le contenu de la page d’erreurs personnalisée est retourné en tant que réponse à la page qui a généré l’erreur.

## <a name="summary"></a>Récapitulatif

Lorsqu’une exception non gérée se produit dans une application Web ASP.NET, le runtime ASP.NET déclenche l’événement `Error` et affiche la page d’erreur configurée. Nous pouvons informer le développeur de l’erreur, consigner ses détails ou la traiter d’une autre manière, en créant un gestionnaire d’événements pour l’événement d’erreur. Il existe deux façons de créer un gestionnaire d’événements pour `HttpApplication` des événements comme `Error`: dans le fichier `Global.asax` ou à partir d’un module HTTP. Ce didacticiel vous a montré comment créer un gestionnaire d’événements `Error` dans le fichier `Global.asax` qui avertit les développeurs d’une erreur au moyen d’un message électronique.

La création d’un gestionnaire d’événements `Error` est utile si vous devez traiter des exceptions non gérées de manière unique ou personnalisée. Toutefois, la création de votre propre gestionnaire d’événements `Error` pour consigner l’exception ou pour avertir un développeur n’est pas l’utilisation la plus efficace de votre temps, car il existe déjà des bibliothèques de journalisation des erreurs gratuites et faciles à utiliser qui peuvent être configurées en quelques minutes. Les deux didacticiels suivants examinent deux bibliothèques de ce type.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Vue d’ensemble des modules HTTP ASP.NET et des gestionnaires HTTP](https://support.microsoft.com/kb/307985)
- [Réponse correcte aux exceptions non gérées-traitement des exceptions non gérées](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [Classe `HttpApplication` et objet application ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Gestionnaires HTTP et modules HTTP dans ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Envoi de courrier électronique dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Compréhension du fichier `Global.asax`](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Utilisation de modules et de gestionnaires HTTP pour créer des composants ASP.NET enfichables](https://msdn.microsoft.com/library/aa479332.aspx)
- [Utilisation du fichier de `Global.asax` ASP.NET](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Utilisation des instances de `HttpApplication`](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Précédent](displaying-a-custom-error-page-vb.md)
> [Suivant](logging-error-details-with-asp-net-health-monitoring-vb.md)
