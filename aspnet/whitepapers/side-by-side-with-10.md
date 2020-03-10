---
uid: whitepapers/side-by-side-with-10
title: ASP.NET l’exécution côte à côte de .NET Framework 1,0 et 1,1 | Microsoft Docs
author: rick-anderson
description: Ce livre blanc explique comment installer .NET 1,0 et .NET 1,1 sur votre ordinateur, ce qui permet à une application Web ASP.NET de s’exécuter sur l’une ou l’autre des versions du minutage...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632971"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>ASP.NET - Exécution côte à côte de .NET Framework 1.0 et 1.1

> Ce livre blanc explique comment installer .NET 1,0 et .NET 1,1 sur votre ordinateur, ce qui permet à une application Web ASP.NET de s’exécuter sur l’une ou l’autre des versions du Framework.
> 
> S’applique à ASP.NET 1,0 et ASP.NET 1,1.

Dans ASP.NET, les applications sont dites exécutées côte à côte lorsqu’elles sont installées sur le même ordinateur, mais utilisent des versions différentes du .NET Framework. La rubrique suivante explique comment configurer des applications ASP.NET pour l’exécution côte à côte et fournit des étapes détaillées pour :

- [Mettre à jour le mappage de votre application Web à .NET Framework version 1,0 lors de l’installation](#1)
- [Mapper une application Web à une version spécifique du .NET Framework](#2)
- [Rechercher la version du .NET Framework qu’un site Web utilise](#3)

Traditionnellement, lorsqu’un composant ou une application est mis à jour sur un ordinateur, l’ancienne version est supprimée et remplacée par la version plus récente. Si la nouvelle version n’est pas compatible avec la version précédente, cela interrompt généralement les autres applications qui utilisent le composant ou l’application. L' .NET Framework prend en charge l’exécution côte à côte, ce qui permet l’installation de plusieurs versions d’un assembly ou d’une application sur le même ordinateur en même temps. Étant donné que plusieurs versions peuvent être installées simultanément, les applications gérées peuvent sélectionner la version à utiliser sans affecter les applications qui utilisent une version différente.

Par défaut, lors de l’installation du .NET Framework version 1,1, toutes les applications ASP.NET existantes sont automatiquement reconfigurées pour utiliser la version la plus récente du .NET Framework. Si vous ne souhaitez pas que vos applications ASP.NET soient par défaut en .NET Framework 1,1, cliquez [ici](#1) pour savoir comment éviter cela pendant l’installation.

Si vous mettez à jour votre serveur Web pour .NET Framework 1,1 et que vous souhaitez qu’une ou plusieurs applications Web s’exécutent .NET Framework 1,0, vous devez mettre à jour le mappage de scripts Internet Information Services (IIS). Le mappage de script est le mécanisme permettant de mapper l’extension de fichier. aspx pour une application Web spécifique à une version du .NET Framework. Cliquez [ici](#2) pour apprendre à mapper une application Web à une version spécifique du .NET Framework.

Vous pouvez utiliser Internet Information Manager ou l’outil d’inscription ASP.NET IIS (ASPNET\_regiis. exe) pour rechercher la version de .NET Framework qui exécute une application Web particulière. Cliquez [ici](#3) pour découvrir comment trouver la version de la .NET Framework qu’un site Web utilise.

L’un des points à prendre en compte lors de la migration vers .NET Framework 1,1 est que chaque version du .NET Framework utilise son propre fichier machine. config. Par conséquent, si un administrateur Web a apporté des modifications au fichier machine. config, ces modifications doivent être migrées vers le fichier machine. config .NET Framework 1,1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Maintien du mappage de votre application Web à .NET Framework 1,0 lors de l’installation

Par défaut, toutes les applications ASP.NET existantes sont automatiquement reconfigurées au cours de l’installation pour utiliser la version plus récente du .NET Framework. À l’aide de la version la plus récente du .NET Framework, les applications peuvent tirer pleinement parti des améliorations et des nouvelles fonctionnalités incluses dans la nouvelle version. En même temps, l’administrateur Web, qui peut avoir besoin d’un contrôle granulaire sur les applications mises à jour, peut empêcher le remappage automatique de toutes les applications ASP.NET existantes lors de l’installation du .NET Framework.

Pour empêcher le remappage automatique de la totalité de l’application ASP.NET vers la version la plus récente du .NET Framework, l’administrateur Web peut utiliser l’option de ligne de commande/noaspupgrade avec le programme d’installation de Dotnetfx. exe.

**Pour empêcher le remappage total de l’application ASP.NET vers une version plus récente**

1. Cliquez sur **Démarrer**.
2. Cliquez sur **exécuter**.
3. Tapez **cmd**.
4. Cliquez sur **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. À partir de l’invite de commandes, tapez la ligne suivante pour commencer l’installation du .NET Framework : **Dotnetfx. exe/c : "installer/noaspupgrade ?** .  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Cliquez sur **Oui** dans le programme d’installation de Microsoft .NET Framework 1,1. Cette opération démarre le processus d’installation de la .NET Framework 1,1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Mapper une application Web à une version spécifique du .NET Framework

Chaque version du .NET Framework contient une version de l’outil d’inscription ASP.NET IIS (ASPNET\_regiis. exe). Cet outil permet aux administrateurs de spécifier qu’une application Web doit être exécutée sous une version particulière du .NET Framework. On parle alors de mapper une application Web à une version du .NET Framework. Les administrateurs doivent sélectionner le\_ASPNET regiis. exe qui correspond à la version du .NET Framework qui sera associé à l’application Web. Par exemple, un administrateur qui souhaite spécifier qu’un site Web utilise .NET Framework 1,1 doit utiliser le\_ASPNET regiis. exe fourni avec .NET Framework 1,1.

Le\_ASPNET regiis. exe pour la version 1,0 se trouve à l’emplacement suivant :

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis

Le\_ASPNET regiis. exe pour la version 1, 1 se trouve à l’emplacement suivant :

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis

Le\_ASPNET regiis. exe fournit deux options pour le mappage de script d’une application Web :

- **-s** définit le mappage de scripts dans le chemin d’accès et dans ses répertoires enfants.
- **-sn** définit le mappage de scripts uniquement dans le chemin d’accès.

Le chemin d’accès définit le chemin des métadonnées IIS de l’application Web, qui est défini sous la forme W3SVC/ROOT/{WebSiteNumber}/{application\_Name}. Par exemple, pour une application Web appelée portail située sous le site Web par défaut, le chemin d’accès à la métabase est W3SVC/1/ROOT/Portal.

![](side-by-side-with-10/_static/image4.gif)

Remarque Vous pouvez également utiliser un outil appelé éditeur de métabase pour obtenir le chemin d’accès de la métabase. Vous pouvez télécharger cet outil à partir du site Support Microsoft à l’adresse [https://support.microsoft.com/default.aspx?scid=kb; en-US ; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Exécutez ASPNET\_regiis. exe-s W3SVC/1/ROOT/Portal pour mettre à jour le mappage de script IIS du portail et sa sous-application.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Exécutez ASPNET\_regiis. exe-SN W3SVC/1/ROOT/Portal pour mettre à jour le mappage de script IIS du portail, sans affecter les applications dans les sous-répertoires du portail.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Rechercher la version de .NET Framework qu’une application Web utilise

Un administrateur peut utiliser l’Service Manager Internet pour rechercher la version du .NET Framework qui exécute un site Web. Les différentes versions de système d’exploitation lancent Internet Service Manager différemment. Pour démarrer le gestionnaire de service, suivez les étapes indiquées ci-dessous.

**Pour démarrer le Service Manager Internet**

1. Cliquez sur **Démarrer**.
2. Cliquez sur **exécuter**.
3. Tapez **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. À partir de l’Service Manager Internet, sélectionnez l’application Web dont vous souhaitez connaître la version du .NET Framework.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Cliquez avec le bouton droit sur l’application Web, puis cliquez sur **Propriétés.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Dans la fenêtre des propriétés, sélectionnez **Configuration.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Dans la table de mappage d’application, sélectionnez **. aspx**, puis cliquez sur **modifier**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Dans la zone de texte **exécutable** , examinez le répertoire de version en faisant défiler la liste. Si le répertoire de version est v. 1.1.4322, l’application est mappée à .NET Framework 1,1. À l’inverse, si le répertoire de version est v 1.0.3705, l’application est mappée à .NET Framework 1,0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
