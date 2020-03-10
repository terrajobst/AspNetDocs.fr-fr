---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installation d’un programme d’assistance sur un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment installer une application auxiliaire sur un site Web pages Web ASP.NET (Razor). Une application auxiliaire est un composant réutilisable qui comprend le code et le balisage par...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638585"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Installation d’un programme d’assistance sur un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment installer une application auxiliaire sur un site Web pages Web ASP.NET (Razor). Une *application auxiliaire* est un composant réutilisable qui comprend du code et des balises pour effectuer une tâche qui peut être fastidieuse ou complexe.
> 
> Ce que vous allez apprendre :
> 
> - Comment installer une application d’assistance dans un site Web créé à l’aide de WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - WebMatrix 3

## <a name="overview-of-helpers"></a>Vue d’ensemble des applications auxiliaires

Certaines tâches que les utilisateurs souhaitent souvent effectuer sur des pages Web nécessitent beaucoup de code ou nécessitent des connaissances supplémentaires. Les exemples incluent l’affichage d’un graphique pour les données ; Ajout d’un bouton « follow » Twitter sur une page ; envoi de courrier électronique à partir de votre site Web ; rognage ou redimensionnement d’images ; utilisation de PayPal pour votre site. Pour faciliter ces types d’opérations, pages Web ASP.NET vous permet d’utiliser des *applications auxiliaires*. Les programmes d’assistance sont des composants que vous installez pour un site et qui vous permettent d’effectuer des tâches courantes à l’aide d’une ou deux lignes de code Razor.

Pages Web ASP.NET dispose de quelques applications d’assistance intégrées. Toutefois, de nombreuses applications d’assistance sont disponibles dans les packages (compléments) fournis à l’aide du gestionnaire de package NuGet. NuGet vous permet de sélectionner un package à installer, puis il prend en charge tous les détails de l’installation.

## <a name="installing-a-helper-in-webmatrix-3"></a>Installation d’une application d’assistance dans WebMatrix 3

1. Dans WebMatrix 3, cliquez sur le bouton **NuGet** .

    ![Boîte de dialogue galerie NuGet dans WebMatrix](installing-helpers/_static/image1.png)
2. Le gestionnaire de package NuGet est lancé et les packages disponibles s’affichent. Dans la zone de recherche, entrez un mot clé pour l’application auxiliaire que vous souhaitez installer.

    ![Boîte de dialogue galerie NuGet dans WebMatrix](installing-helpers/_static/image2.png)
3. Sélectionnez le package, puis cliquez sur **installer**. Cliquez sur **Oui** lorsque vous êtes invité à installer le package et indiquez que vous acceptez les termes du contrat.

     S’il s’agit de la première fois que vous avez installé une application d’assistance, NuGet crée des dossiers dans votre site Web pour le code qui compose l’application d’assistance.
4. Pour désinstaller une application d’assistance, cliquez sur le bouton **Galerie** , cliquez sur l’onglet **installé** , puis sélectionnez le package que vous souhaitez désinstaller.

## <a name="installing-the-twitter-helper"></a>Installation du programme d’assistance Twitter

La dernière version de l’API Twitter n’est pas compatible avec l’application auxiliaire Twitter que vous installez par le biais de NuGet. Pour plus d’informations sur la façon de configurer le programme d’assistance Twitter dans votre projet, consultez plutôt la rubrique [application auxiliaire Twitter avec WebMatrix](twitter-helper.md) .

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Présentation de pages Web ASP.NET 2-notions de base de la programmation](../getting-started/introducing-razor-syntax-c.md)

[Application auxiliaire Twitter avec WebMatrix](twitter-helper.md)
