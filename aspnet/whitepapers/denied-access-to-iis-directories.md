---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET a refusé l’accès aux répertoires IIS | Microsoft Docs
author: rick-anderson
description: Ce livre blanc décrit ce que vous devez faire si une demande à votre application ASP.NET retourne l’erreur «Accès refusé au répertoire DirectoryName. Échec de la tentative...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638501"
---
# <a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET - Accès refusé aux répertoires IIS

> Ce livre blanc décrit ce que vous devez faire si une demande à votre application ASP.NET retourne l’erreur «Accès refusé au répertoire *DirectoryName* . Échec du démarrage de l’analyse des modifications de répertoire.»
> 
> S’applique à ASP.NET 1,0 et ASP.NET 1,1.

ASP.NET v1 RTM s’exécute désormais à l’aide d’un compte Windows avec moins de privilèges, inscrit en tant que compte « ASPNET » sur un ordinateur local.

Sur certains systèmes verrouillés, ce compte ne peut pas par défaut avoir accès en lecture à la sécurité des répertoires de contenu d’un site Web, du répertoire racine de l’application ou du répertoire racine du site Web. Dans ce cas, vous recevrez l’erreur suivante lors de la demande de pages d’une application Web donnée :

![](denied-access-to-iis-directories/_static/image1.jpg)

Pour résoudre ce problème, vous devez modifier les autorisations de sécurité sur les répertoires appropriés.

Plus précisément, ASP.NET requiert un accès en lecture, en exécution et en liste pour le compte ASPNET pour la racine du site Web (par exemple : c:\inetpub\wwwroot ou tout autre répertoire de site que vous avez peut-être configuré dans IIS), le répertoire de contenu et le répertoire racine de l’application pour surveiller les modifications apportées au fichier de configuration. La racine de l’application correspond au chemin d’accès au dossier associé au répertoire virtuel de l’application dans l’outil d’administration IIS (inetmgr).

Par exemple, considérez la hiérarchie d’application suivante sous le dossier Wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Pour cet exemple, le compte ASPNET a besoin des autorisations de lecture définies ci-dessus pour le contenu des répertoires MyApp et wwwroot. Une seule ACL héritée sur le dossier racine peut également être utilisée pour les deux répertoires s’ils sont imbriqués.

Pour ajouter des autorisations à un répertoire, procédez comme suit :

- À l’aide de l’Explorateur Windows, accédez au répertoire
- Cliquez avec le bouton droit sur le dossier Directory, puis choisissez « Propriétés ».
- Accédez à l’onglet « sécurité » dans la boîte de dialogue des propriétés.
- Cliquez sur le bouton « Ajouter », puis entrez le nom de l’ordinateur suivi du nom du compte ASPNET. Par exemple, sur un ordinateur nommé « WebDev », vous devez entrer webdev\ASPNET et appuyer sur « OK ».
- Vérifiez que les cases à cocher « lire &amp; exécuter », « afficher le contenu du dossier » et « lecture » sont activées dans le compte ASPNET.
- Appuyez sur OK pour fermer la boîte de dialogue et enregistrer les modifications.

![](denied-access-to-iis-directories/_static/image2.jpg)

Si vous le souhaitez, ces modifications peuvent être automatisées à l’aide de scripts ou de l’outil « Cacls. exe » fourni avec Windows. Pour plus d’informations sur le compte ASPNET, consultez le [document FAQ](https://go.microsoft.com/fwlink/?LinkId=5828).

Si une application Web donnée s’appuie sur des autorisations d’écriture ou de modification d’un dossier ou d’un fichier particulier, cette opération peut être accordée en suivant la même procédure et en activant les cases à cocher « écrire » et/ou « modifier ».

Sur les ordinateurs qui autorisent tout le monde ou le groupe d’utilisateurs à accéder en lecture à ces répertoires (qui est la configuration par défaut), aucun problème ne se produit et les étapes ci-dessus ne sont pas requises.
