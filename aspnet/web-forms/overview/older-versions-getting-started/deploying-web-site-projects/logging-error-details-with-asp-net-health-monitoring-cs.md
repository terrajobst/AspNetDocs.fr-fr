---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: Journalisation des détails des erreurs avec ASP.NETC#Health Monitoring () | Microsoft Docs
author: rick-anderson
description: Le système de contrôle d’intégrité de Microsoft offre un moyen facile et personnalisable d’enregistrer différents événements Web, y compris les exceptions non gérées. Ce didacticiel vous guide dans transmet...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: e52ed94f78d053701771690fce432d5a1d465b62
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637321"
---
# <a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Journalisation des détails des erreurs avec la supervision de l’intégrité ASP.NET (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> Le système de contrôle d’intégrité de Microsoft offre un moyen facile et personnalisable d’enregistrer différents événements Web, y compris les exceptions non gérées. Ce didacticiel vous guide dans la configuration du système de contrôle d’intégrité pour enregistrer les exceptions non gérées dans une base de données et pour avertir les développeurs par le biais d’un message électronique.

## <a name="introduction"></a>Introduction

La journalisation est un outil utile pour surveiller l’intégrité d’une application déployée et pour diagnostiquer les problèmes qui peuvent survenir. Il est particulièrement important de consigner les erreurs qui se produisent dans une application déployée afin qu’elles puissent être résolues. L’événement `Error` est déclenché chaque fois qu’une exception non gérée se produit dans une application ASP.NET ; le [didacticiel précédent](processing-unhandled-exceptions-cs.md) a montré comment avertir un développeur d’une erreur et enregistrer ses détails en créant un gestionnaire d’événements pour l’événement `Error`. Toutefois, la création d’un gestionnaire d’événements `Error` pour consigner les détails de l’erreur et notifier un développeur n’est pas nécessaire, car cette tâche peut être effectuée par ASP. Système de *contrôle d’intégrité*du réseau.

Le système de contrôle d’intégrité a été introduit dans ASP.NET 2,0 et est conçu pour surveiller l’intégrité d’une application ASP.NET déployée en enregistrant les événements qui se produisent pendant la durée de vie de l’application ou de la demande. Les événements consignés par le système de contrôle d’intégrité sont appelés *événements de contrôle d’intégrité* ou *événements Web*, et incluent les éléments suivants :

- Événements de durée de vie de l’application, tels que le démarrage ou l’arrêt d’une application
- Événements de sécurité, y compris les échecs de tentatives de connexion et les échecs de demandes d’autorisation d’URL
- Erreurs d’application, y compris les exceptions non gérées, les exceptions d’analyse d’état d’affichage, les exceptions de validation de demande et les erreurs de compilation, parmi d’autres types d’erreurs.

Lorsqu’un événement de contrôle d’intégrité est déclenché, il peut être enregistré dans n’importe quel nombre de *sources de journaux*spécifiées. Le système de contrôle d’intégrité est fourni avec les sources de journaux qui journalisent les événements Web dans une base de données Microsoft SQL Server, dans le journal des événements Windows ou via un message électronique, entre autres. Vous pouvez également créer vos propres sources de journaux.

Les événements journalisés par le système de surveillance de l’intégrité, ainsi que les sources de journal utilisées, sont définis dans `Web.config`. Avec quelques lignes de balisage de configuration, vous pouvez utiliser le contrôle d’intégrité pour consigner toutes les exceptions non gérées dans une base de données et vous avertir de l’exception par courrier électronique.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Exploration de la configuration du système de contrôle d’intégrité

Le comportement du système de contrôle d’intégrité est défini par ses informations de configuration, qui se trouvent dans l' [élément`<healthMonitoring>`](https://msdn.microsoft.com/library/2fwh2ss9.aspx) dans `Web.config`. Cette section de configuration définit, entre autres choses, les trois éléments d’information importants suivants :

1. Événements de contrôle d’intégrité qui, lorsqu’ils sont déclenchés, doivent être journalisés,
2. Sources du journal, et
3. Comment chaque événement de contrôle d’intégrité défini dans (1) est mappé aux sources de journaux définies dans (2).

Ces informations sont spécifiées à l’aide de trois éléments de configuration enfants : [`<eventMappings>`](https://msdn.microsoft.com/library/yc5yk01w.aspx), [`<providers>`](https://msdn.microsoft.com/library/zaa41kz1.aspx)et [`<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx), respectivement.

Les informations de configuration du système de contrôle d’intégrité par défaut se trouvent dans le fichier `Web.config` dans `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` dossier. Ces informations de configuration par défaut, avec des balises supprimées par souci de concision, sont présentées ci-dessous :

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

Les événements de contrôle d’État intéressants sont définis dans l’élément `<eventMappings>`, qui donne un nom convivial à une classe d’événements de contrôle d’intégrité. Dans le balisage ci-dessus, l’élément `<eventMappings>` affecte le nom convivial « toutes les erreurs » aux événements de contrôle d’intégrité de type `WebBaseErrorEvent` et le nom « audits des échecs » pour les événements de contrôle d’intégrité de type `WebFailureAuditEvent`.

L’élément `<providers>` définit les sources de journal, en leur donnant un nom convivial et en spécifiant les informations de configuration spécifiques à la source du journal. Le premier élément `<add>` définit le fournisseur « EventLogProvider », qui journalise les événements de contrôle d’intégrité spécifiés à l’aide de la classe `EventLogWebEventProvider`. La classe `EventLogWebEventProvider` enregistre l’événement dans le journal des événements Windows. Le deuxième élément `<add>` définit le fournisseur « SqlWebEventProvider », qui consigne les événements dans une base de données Microsoft SQL Server via la classe `SqlWebEventProvider`. La configuration « SqlWebEventProvider » spécifie la chaîne de connexion de la base de données (`connectionStringName`) parmi d’autres options de configuration.

L’élément `<rules>` mappe les événements spécifiés dans l’élément `<eventMappings>` à des sources de journal dans l’élément `<providers>`. Par défaut, les applications Web ASP.NET consignent toutes les exceptions non gérées et les échecs d’audit dans le journal des événements Windows.

## <a name="logging-events-to-a-database"></a>Journalisation des événements dans une base de données

La configuration par défaut du système de contrôle d’intégrité peut être personnalisée en fonction de l’application Web, par l’ajout d’une `<healthMonitoring>` section au fichier `Web.config` de l’application. Vous pouvez inclure des éléments supplémentaires dans les sections `<eventMappings>`, `<providers>`et `<rules>` à l’aide de l’élément `<add>`. Pour supprimer un paramètre de la configuration par défaut, utilisez l’élément `<remove>` ou `<clear />` pour supprimer toutes les valeurs par défaut de l’une de ces sections. Nous allons configurer l’application Web de révisions de livres pour consigner toutes les exceptions non gérées dans une base de données Microsoft SQL Server à l’aide de la classe `SqlWebEventProvider`.

La classe `SqlWebEventProvider` fait partie du système de contrôle d’intégrité et enregistre un événement de contrôle d’intégrité dans une base de données SQL Server spécifiée. La classe `SqlWebEventProvider` s’attend à ce que la base de données spécifiée comprenne une procédure stockée nommée `aspnet_WebEvent_LogEvent`. Cette procédure stockée reçoit les détails de l’événement et est chargé de stocker les détails de l’événement. La bonne nouvelle, c’est que vous n’avez pas besoin de créer cette procédure stockée ou la table pour stocker les détails de l’événement. Vous pouvez ajouter ces objets à votre base de données à l’aide de l’outil `aspnet_regsql.exe`.

> [!NOTE]
> L’outil `aspnet_regsql.exe` a été abordé dans la page [ *configuration d’un site Web qui utilise services d’application* didacticiel](configuring-a-website-that-uses-application-services-cs.md) lorsque nous avons ajouté la prise en charge d’ASP. Services d’application du réseau. Par conséquent, la base de données du site Web de révisions de livres contient déjà la procédure stockée `aspnet_WebEvent_LogEvent`, qui stocke les informations d’événement dans une table nommée `aspnet_WebEvent_Events`.

Une fois que vous avez ajouté la table et la procédure stockée nécessaires à votre base de données, il ne reste plus qu’à demander à l’analyse du fonctionnement de consigner toutes les exceptions non gérées dans la base de données. Pour ce faire, ajoutez le balisage suivant au fichier `Web.config` de votre site Web :

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

Le balisage de la configuration du contrôle d’intégrité ci-dessus utilise `<clear />` éléments pour effacer les informations de configuration du contrôle d’intégrité prédéfinies à partir des sections `<eventMappings>`, `<providers>`et `<rules>`. Il ajoute ensuite une seule entrée à chacune de ces sections.

- L’élément `<eventMappings>` définit un événement d’analyse d’intégrité unique nommé « toutes les erreurs », qui est déclenché chaque fois qu’une exception non gérée se produit.
- L’élément `<providers>` définit une source de journal unique nommée « SqlWebEventProvider » qui utilise la classe `SqlWebEventProvider`. L’attribut `connectionStringName` a été défini sur « ReviewsConnectionString », qui est le nom de la chaîne de connexion définie dans la section `<connectionStrings>`.
- Enfin, l’élément &lt;Rules&gt; indique qu’en cas d’événement « All Errors » (toutes les erreurs), il doit être journalisé à l’aide du fournisseur « SqlWebEventProvider ».

Ces informations de configuration demandent au système de contrôle d’intégrité d’enregistrer toutes les exceptions non gérées dans la base de données de révisions de livres.

> [!NOTE]
> L’événement `WebBaseErrorEvent` est déclenché uniquement pour les erreurs de serveur ; elle n’est pas déclenchée pour les erreurs HTTP, telles qu’une demande de ressource ASP.NET introuvable. Cela diffère du comportement de l’événement `Error` de la classe `HttpApplication`, qui est déclenché à la fois pour les erreurs serveur et HTTP.

Pour voir le système de contrôle d’intégrité en action, visitez le site Web et générez une erreur d’exécution en visitant `Genre.aspx?ID=foo`. Vous devez voir la page d’erreur appropriée, à savoir les détails de l’exception écran jaune de la mort (lors de la visite locale) ou la page d’erreurs personnalisée (lorsque vous vous rendez sur le site en production). En arrière-plan, le système de contrôle d’intégrité a consigné les informations d’erreur dans la base de données. Il doit y avoir un enregistrement dans la table `aspnet_WebEvent_Events` (voir **figure 1**). Cet enregistrement contient des informations sur l’erreur d’exécution qui vient de se produire.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Figure 1**: les détails de l’erreur ont été enregistrés dans la table `aspnet_WebEvent_Events`  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Affichage du journal des erreurs dans une page Web

Avec la configuration actuelle du site Web, le système de surveillance de l’intégrité enregistre toutes les exceptions non gérées dans la base de données. Toutefois, le contrôle d’intégrité ne fournit aucun mécanisme permettant d’afficher le journal des erreurs via une page Web. Toutefois, vous pouvez créer une page ASP.NET qui affiche ces informations à partir de la base de données. (Comme nous le verrons momentanément, vous pouvez choisir de vous envoyer les détails de l’erreur dans un message électronique.)

Si vous créez une page de ce type, veillez à prendre les mesures nécessaires pour autoriser uniquement les utilisateurs autorisés à afficher les détails de l’erreur. Si votre site utilise déjà des comptes d’utilisateur, vous pouvez utiliser des règles d’autorisation d’URL pour limiter l’accès à la page à certains utilisateurs ou rôles. Pour plus d’informations sur l’octroi ou la restriction de l’accès aux pages Web en fonction de l’utilisateur connecté, consultez les didacticiels sur la sécurité de mon [site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Le didacticiel suivant explore un autre système de journalisation des erreurs et de notification nommé ELMAH. ELMAH comprend un mécanisme intégré permettant d’afficher le journal des erreurs à la fois sur une page Web et en tant que flux RSS.

## <a name="logging-events-to-email"></a>Journalisation des événements dans un message électronique

Le système de contrôle d’intégrité comprend un fournisseur de source de journal qui « enregistre » un événement dans un message électronique. La source du journal comprend les mêmes informations qui sont consignées dans la base de données dans le corps du message électronique. Vous pouvez utiliser cette source de journal pour notifier un développeur lorsqu’un certain événement de contrôle d’intégrité se produit.

Nous allons mettre à jour la configuration du site Web de révisions de livres pour recevoir un e-mail chaque fois qu’une exception se produit. Pour ce faire, nous devons effectuer trois tâches :

1. Configurez l’application Web ASP.NET pour l’envoi de courrier électronique. Pour ce faire, vous devez spécifier la façon dont les messages électroniques sont envoyés par le biais de l’élément de configuration `<system.net>`. Pour plus d’informations sur l’envoi de messages électroniques dans une application ASP.NET, consultez [Envoyer un message électronique dans ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) et le [FAQ System .net. mail](http://systemnetmail.com/).
2. Inscrivez le fournisseur de source de journaux de messagerie dans l’élément `<providers>`, et
3. Ajoutez une entrée à l’élément `<rules>` qui mappe l’événement « All Errors » au fournisseur de source de journal ajouté à l’étape (2).

Le système de contrôle d’intégrité comprend deux classes de fournisseur de sources de journaux de messagerie : `SimpleMailWebEventProvider` et `TemplatedMailWebEventProvider`. La [classe`SimpleMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) envoie un message électronique de texte brut qui inclut les détails de l’événement et fournit peu de personnalisation du corps de l’e-mail. Avec la [classe`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) , vous spécifiez une page ASP.net dont le balisage rendu est utilisé comme corps du message électronique. La [classe`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) vous donne un meilleur contrôle sur le contenu et le format du message électronique, mais nécessite un peu plus de travail avant que vous ne deviez créer la page ASP.net qui génère le corps du message électronique. Ce didacticiel se concentre sur l’utilisation de la classe `SimpleMailWebEventProvider`.

Mettez à jour l’élément `<providers>` du système de contrôle d’intégrité dans le fichier `Web.config` pour inclure une source de journal pour la classe `SimpleMailWebEventProvider` :

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

Le balisage ci-dessus utilise la classe `SimpleMailWebEventProvider` comme fournisseur de source de journal et lui attribue le nom convivial « EmailWebEventProvider ». En outre, l’attribut `<add>` comprend des options de configuration supplémentaires, telles que les adresses à et depuis du message électronique.

Avec la source du journal des e-mails définie, il ne reste plus qu’à demander au système de contrôle d’intégrité d’utiliser cette source pour « journaliser » les exceptions non gérées. Pour ce faire, ajoutez une nouvelle règle dans la section `<rules>` :

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

La section `<rules>` comprend désormais deux règles. Le premier, nommé « toutes les erreurs à envoyer par courrier électronique », envoie toutes les exceptions non gérées à la source du journal « EmailWebEventProvider ». Cette règle a pour effet d’envoyer des détails sur les erreurs sur le site Web à l’adresse spécifiée. La règle « toutes les erreurs dans la base de données » enregistre les détails de l’erreur dans la base de données du site. Par conséquent, chaque fois qu’une exception non gérée se produit sur le site, ses détails sont tous deux enregistrés dans la base de données et envoyés à l’adresse de messagerie spécifiée.

La **figure 2** montre l’e-mail généré par la classe `SimpleMailWebEventProvider` lors de la visite `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Figure 2**: les détails de l’erreur sont envoyés dans un message électronique  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Récapitulatif

Le système de contrôle d’intégrité ASP.NET est conçu pour permettre aux administrateurs de surveiller l’intégrité d’une application Web déployée. Les événements de contrôle d’intégrité sont déclenchés lorsque certaines actions se déroulent, par exemple lorsque l’application s’arrête, lorsqu’un utilisateur se connecte avec succès au site ou lorsqu’une exception non gérée se produit. Ces événements peuvent être enregistrés dans un nombre quelconque de sources de journaux. Ce didacticiel vous a montré comment enregistrer les détails des exceptions non gérées dans une base de données et par le biais d’un message électronique.

Ce didacticiel se concentre sur l’utilisation de la surveillance de l’intégrité pour consigner les exceptions non gérées, mais gardez à l’esprit que le contrôle d’intégrité est conçu pour mesurer l’intégrité globale d’une application ASP.NET déployée et qu’elle comprend une multitude d’événements de contrôle d’intégrité et de sources de journaux non exploré ici. De plus, vous pouvez créer vos propres événements de surveillance de l’intégrité et sources de journaux, en cas de besoin. Si vous souhaitez en savoir plus sur la surveillance de l’intégrité, la première étape consiste à lire le FAQ sur la surveillance de l' [intégrité](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)de [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Ensuite, consultez [la rubrique Comment : utiliser le contrôle d’intégrité dans ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx).

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Vue d’ensemble de la surveillance de l’intégrité ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configuration et personnalisation du système de contrôle d’intégrité de ASP.NET](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [FAQ-surveillance de l’intégrité dans ASP.NET 2,0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Procédure : envoyer un message électronique pour les notifications de surveillance de l’intégrité](https://msdn.microsoft.com/library/ms227553.aspx)
- [Procédure : utiliser le contrôle d’intégrité dans ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Surveillance de l’intégrité dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Précédent](processing-unhandled-exceptions-cs.md)
> [Suivant](logging-error-details-with-elmah-cs.md)
