---
uid: whitepapers/aspnet-and-iis6
title: Exécution de ASP.NET 1,1 avec IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Bien que Windows Server 2003 inclue à la fois IIS 6,0 et ASP.NET 1,1, ces composants sont désactivés par défaut. Ce livre blanc explique comment activer IIS 6,0...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523309"
---
# <a name="running-aspnet-11-with-iis-60"></a>Exécution d’ASP.NET 1.1 avec IIS 6.0

> Bien que Windows Server 2003 inclue à la fois IIS 6,0 et ASP.NET 1,1, ces composants sont désactivés par défaut. Ce livre blanc explique comment activer IIS 6,0 et ASP.NET 1,1, et recommande plusieurs paramètres de configuration pour optimiser les performances d’IIS et de ASP.NET.
> 
> S’applique à ASP.NET 1,1 et IIS 6,0.

ASP.NET 1,1 est fourni avec Windows Server 2003, qui inclut également la dernière version d’Internet Information Server (IIS) version 6,0. IIS 6,0 et ASP.NET 1,1 sont conçus pour s’intégrer en toute transparence et ASP.NET désormais par défaut au nouveau modèle de processus de travail IIS 6,0.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1,1 n’est pas installé par défaut

Contrairement aux versions précédentes des systèmes d’exploitation serveur de Microsoft, Internet Information Server (IIS) n’est pas activé par défaut. il ne s’agit pas non plus de ASP.NET 1,1. Il existe deux options pour activer IIS :

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Activation d’IIS, option #1-Assistant Configuration de votre serveur

Windows Server 2003 fournit un nouvel Assistant Configurer votre serveur pour vous aider à configurer correctement votre serveur dans le mode souhaité.

Pour démarrer l’Assistant-note, pour exécuter l’Assistant, vous devez être connecté en tant qu’administrateur-aller à : Démarrer | Programmes | Outils d’administration et sélectionnez « configurer votre serveur ».

Une fois l’option sélectionnée, vous devez voir l’écran d’ouverture de l’Assistant Configurer votre serveur :

![](aspnet-and-iis6/_static/image1.jpg)

Cliquez sur « Next &gt;» :

![](aspnet-and-iis6/_static/image2.jpg)

Cliquez sur « Next &gt;»

![](aspnet-and-iis6/_static/image3.jpg)

Sur cet écran, vous devez sélectionner « serveur d’applications (IIS, ASP.NET) » comme options à configurer.

Cliquez sur « Next &gt;».

![](aspnet-and-iis6/_static/image4.jpg)

Une fois que vous avez choisi de configurer le serveur en tant que serveur d’applications, cet écran s’affiche et vous invite à indiquer les fonctionnalités supplémentaires à installer. Aucune option n’est sélectionnée par défaut. Pour activer ASP.NET automatiquement, vous devez sélectionner «Activer ASP. NET'.

Cliquez sur « Next &gt;».

![](aspnet-and-iis6/_static/image5.jpg)

Cet écran affiche les options à installer.

Cliquez sur « Next &gt;».

![](aspnet-and-iis6/_static/image6.jpg)

Cet écran s’affiche lorsque les options que vous avez sélectionnées sont en cours d’installation. Il est normal que d’autres boîtes de dialogue s’affichent lorsque les services sont en cours d’installation. Vous pouvez également être invité à indiquer l’emplacement du CD d’installation du serveur Windows 2003.

Lorsque vous avez terminé, cliquez sur « Next &gt;».

![](aspnet-and-iis6/_static/image7.jpg)

Cliquez sur Finish (terminer)-le serveur Windows Server 2003 est maintenant configuré pour prendre en charge IIS 6,0 et ASP.NET 1,1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Activation d’IIS, option #2-Configuration manuelle d’IIS et de ASP.NET

Si vous ne souhaitez pas utiliser l’Assistant Configurer votre serveur, vous pouvez éventuellement installer IIS 6,0 et ASP.NET 1,1 à l’aide de la commande « Ajout/suppression de programmes » du panneau de configuration.

Commencez par ouvrir le panneau de configuration :

![](aspnet-and-iis6/_static/image8.jpg)

Ensuite, cliquez sur « Ajouter/supprimer des composants Windows » pour ouvrir l’Assistant composants de Windows :

![](aspnet-and-iis6/_static/image9.jpg)

Mettez en surbrillance et cochez « serveur d’applications », puis cliquez sur « détails ». bouton

![](aspnet-and-iis6/_static/image10.jpg)

Pour installer ASP.NET, activez la case à cocher ASP. NET'.

Cliquez sur OK pour revenir à l’Assistant composants de Windows. Cliquez sur « Next &gt;» dans l’Assistant Composants Windows pour commencer l’installation de :

![](aspnet-and-iis6/_static/image11.jpg)

Il est normal que d’autres boîtes de dialogue s’affichent lorsque les services sont en cours d’installation. Vous pouvez également être invité à indiquer l’emplacement du CD d’installation du serveur Windows 2003.

Une fois l’installation terminée, le dernier écran de l’Assistant Composants Windows s’affiche :

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6,0 et ASP.NET 1,1 sont maintenant configurés et disponibles.

## <a name="recommended-settings"></a>Paramètres recommandés

Lors de l’exécution de ASP.NET 1,1 avec IIS 6,0, il est recommandé d’utiliser plusieurs paramètres de configuration pour bénéficier des performances optimales de ASP.NET :

- Configuration des limites de la mémoire du processus de travail
- Configuration du recyclage du processus de travail

### <a name="configuring-worker-process-memory-limits"></a>Configuration des limites de la mémoire du processus de travail

Par défaut, IIS 6,0 ne définit pas de limite quant à la quantité de mémoire qu’IIS est autorisée à utiliser. ASP. La fonctionnalité de cache de NET s’appuie sur une limitation de mémoire afin que le cache puisse supprimer de manière proactive les éléments inutilisés de la mémoire.

Il est recommandé de configurer la fonctionnalité de recyclage de la mémoire d’IIS 6,0. Pour configurer ce gestionnaire Open Internet Information Services Manager (Démarrer | Programmes | Outils d’administration | Internet Information Services). Une fois ouvert, développez le dossier « pools d’applications » :

Pour chaque pool d’applications :

![](aspnet-and-iis6/_static/image13.jpg)

1. Cliquez avec le bouton droit sur le pool d’applications, par exemple « DefaultAppPool », puis sélectionnez « Propriétés » :

![](aspnet-and-iis6/_static/image14.jpg)

2. Ensuite, activez le recyclage de la mémoire en cliquant sur « mémoire maximale utilisée (en mégaoctets) : ». La valeur ne doit pas être supérieure à la quantité de mémoire physique (non virtuelle) sur le serveur, une bonne approximation est 60% de la mémoire physique, c.-à-d. pour un serveur de 512 Mo de mémoire physique, sélectionnez 310. Il est également recommandé de ne pas dépasser le nombre maximal de 800 Mo lors de l’utilisation d’un espace d’adressage de 2 Go. Si l’espace d’adressage de mémoire du serveur est 3 Go, la limite de mémoire maximale pour le processus de travail peut être aussi élevée que 1 800 Mo :

![](aspnet-and-iis6/_static/image15.jpg)

Cliquez sur appliquer et sur OK pour quitter la boîte de dialogue Propriétés. Répétez cette opération pour tous les pools d’applications disponibles.

### <a name="configuring-worker-recycling"></a>Configuration du recyclage de Worker

Par défaut, IIS 6,0 est configuré pour recycler son processus de travail toutes les 29 heures. C’est un peu agressif pour une application exécutant ASP.NET et il est recommandé de désactiver le recyclage automatique des processus de travail.

Pour désactiver le recyclage automatique des processus de travail, ouvrez d’abord le gestionnaire de Internet Information Services (Démarrer | Programmes | Outils d’administration | Internet Information Services). Une fois ouvert, développez le dossier « pools d’applications » :

![](aspnet-and-iis6/_static/image16.jpg)

Pour chaque pool d’applications :

1. Cliquez avec le bouton droit sur le pool d’applications, par exemple « DefaultAppPool », puis sélectionnez « Propriétés » :

![](aspnet-and-iis6/_static/image17.jpg)

2. Désactivez la case à cocher recycler le processus de travail (en minutes) :

![](aspnet-and-iis6/_static/image18.jpg)

Cliquez sur appliquer et sur OK pour quitter la boîte de dialogue Propriétés. Répétez cette opération pour tous les pools d’applications disponibles.

## <a name="granting-write-access-to-the-file-system"></a>Octroi d’un accès en écriture au système de fichiers

Si votre application nécessite un accès en écriture au système de fichiers et que vous utilisez NTFS, vous devez modifier une liste de Access Control (ACL) sur le dossier ou le fichier auquel accorder l’accès ASP.NET.

Par exemple, pour accorder à ASP.NET l’accès en écriture à c:\inetpub\wwwroot First Explorer Open Explorer et accéder au répertoire :

![](aspnet-and-iis6/_static/image19.jpg)

Ensuite, cliquez avec le bouton droit sur le répertoire, par exemple « wwwroot », puis sélectionnez Propriétés. Une fois que la boîte de dialogue Propriétés s’affiche, sélectionnez l’onglet « sécurité » :

![](aspnet-and-iis6/_static/image20.jpg)

Le répertoire c:\inetpub\wwwroot\ est un répertoire spécial dans le fait que le groupe IIS 6,0 spécial « IIS\_WPG » reçoit déjà les autorisations lire &amp; exécuter, répertorier le contenu du dossier et lecture. Toutefois, pour accorder l’autorisation d’écriture, vous devez cliquer sur la case à cocher Autoriser pour l’écriture :

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6,0 dispose à présent d’une autorisation en écriture sur ce dossier. Pour accorder des autorisations en écriture sur d’autres dossiers, procédez comme suit : vous devrez peut-être ajouter le groupe IIS\_WPG s’il n’existe pas déjà.

> [!CAUTION]
> L’octroi d’une autorisation d’accès en écriture à IIS\_WPG permettra à toute application ASP.NET d’écrire dans ce répertoire.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Prise en charge de l’authentification intégrée avec SQL Server

L’authentification intégrée permet à SQL Server de tirer parti de l’authentification Windows NT pour valider SQL Server comptes d’ouverture de session. Cela permet à l’utilisateur de contourner le processus d’ouverture de session SQL Server standard. Avec cette approche, un utilisateur réseau peut accéder à une base de données SQL Server sans fournir d’identification ou de mot de passe d’ouverture de session distincte, car SQL Server obtient les informations d’utilisateur et de mot de passe à partir du processus de sécurité réseau de Windows NT.

Le choix de l’authentification intégrée pour les applications ASP.NET est un bon choix, car aucune information d’identification n’est jamais stockée dans votre chaîne de connexion pour votre application. Au lieu de cela, la chaîne de connexion utilisée pour se connecter à SQL se présente comme suit :

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Cette chaîne de connexion indique à SQL Server d’utiliser les informations d’identification Windows de l’application qui tente d’accéder à SQL Server. Dans le cas de ASP.NET/IIS 6, il s’agit d’un compte dans le groupe IIS\_WPG.

Pour activer l’authentification intégrée entre SQL Server et ASP.NET, vous devez d’abord vous assurer que SQL Server est configuré pour l’authentification intégrée ou l’authentification en mode mixte-Vérifiez auprès de votre administrateur de bases de l’administrateur pour déterminer cela. Si SQL Server est dans l’un de ces deux modes, vous pouvez utiliser l’authentification intégrée.

Ouvrez le gestionnaire de SQL Server Entreprise (Démarrer | Programmes | Microsoft SQL Server | Enterprise Manager), sélectionnez le serveur approprié, puis développez le dossier sécurité :

![](aspnet-and-iis6/_static/image22.jpg)

Si le groupe « BUILTINT\IIS\_WPG » n’est pas listé, cliquez avec le bouton droit sur connexions et sélectionnez Nouvelle connexion :

![](aspnet-and-iis6/_static/image23.jpg)

Dans la zone de texte « Nom : », entrez « [nom de serveur/domaine] \IIS\_WPG » ou cliquez sur le bouton de sélection pour ouvrir le sélecteur d’utilisateur/de groupe Windows NT :

![](aspnet-and-iis6/_static/image24.jpg)

Sélectionnez le groupe IIS\_WPG de l’ordinateur actuel, cliquez sur « Ajouter », puis sur OK pour fermer le sélecteur.

Vous devez ensuite également définir la base de données par défaut et les autorisations d’accès à la base de données. Pour définir la base de données par défaut, choisissez dans la liste déroulante, par exemple, sous Northwind est sélectionné :

![](aspnet-and-iis6/_static/image25.jpg)

Ensuite, cliquez sur l’onglet accès à la base de données :

![](aspnet-and-iis6/_static/image26.jpg)

Cliquez sur la case à cocher Autoriser pour chaque base de données à laquelle vous souhaitez autoriser l’accès. Vous devez également sélectionner des rôles de base de données, en vérifiant la base de données\_propriétaire, afin de garantir que votre connexion dispose de toutes les autorisations nécessaires pour gérer et utiliser la base de données sélectionnée.

Cliquez sur OK pour quitter la boîte de dialogue des propriétés. Votre application ASP.NET est maintenant configurée pour prendre en charge l’authentification SQL Server intégrée.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>N’exécutez pas ASP.NET 1,0 en mode natif IIS 6,0

ASP.NET 1,0 sur IIS 6,0 est pris en charge uniquement en mode de compatibilité IIS 5.

Pour configurer ASP.NET 1,0 pour qu’il s’exécute en mode de compatibilité IIS 5,0, ouvrez Gestionnaire des services Internet puis cliquez avec le bouton droit sur sites Web et sélectionnez Propriétés :

![](aspnet-and-iis6/_static/image27.jpg)

Basculer vers l’onglet service et vérifier ? Exécuter le service WWW en mode d’isolation IIS 5,0 ?:

![](aspnet-and-iis6/_static/image28.jpg)
