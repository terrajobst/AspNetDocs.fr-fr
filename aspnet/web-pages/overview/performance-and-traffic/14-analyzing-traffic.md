---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Suivi des informations du visiteur (analytique) pour un site pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Une fois que vous avez obtenu votre site Web, vous souhaiterez peut-être analyser le trafic de votre site Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525185"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Suivi des informations du visiteur (analytique) pour un site pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment utiliser une application d’assistance pour ajouter des analyses de site Web aux pages d’un site Web pages Web ASP.NET (Razor).
> 
> Ce que vous allez apprendre :
> 
> - Comment envoyer des informations sur le trafic de votre site Web à un fournisseur d’analyse.
> 
> Voici les fonctionnalités de programmation ASP.NET présentées dans l’article :
> 
> - Le programme d’assistance `Analytics`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 2
> - Bibliothèque d’applications auxiliaires Web ASP.NET (package NuGet)

Analytics est un terme général pour les technologies qui mesure le trafic sur votre site Web, ce qui vous permet de comprendre comment les utilisateurs utilisent le site. De nombreux services d’analyse sont disponibles, y compris les services de Google, Yahoo, StatCounter et d’autres.

Le fonctionnement d’Analytics est que vous vous inscrivez à un compte avec le fournisseur Analytics, où vous inscrivez le site dont vous souhaitez effectuer le suivi. Le fournisseur vous envoie un extrait de code JavaScript qui comprend un ID ou un code de suivi pour votre compte. Vous ajoutez l’extrait de code JavaScript aux pages Web sur le site dont vous souhaitez effectuer le suivi. (En général, vous ajoutez l’extrait de code Analytics à un pied de page ou à une page de disposition ou à une autre balise HTML qui apparaît sur chaque page de votre site.) Lorsque les utilisateurs demandent une page contenant l’un de ces extraits de code JavaScript, l’extrait de code envoie des informations sur la page actuelle au fournisseur d’analyse, qui enregistre divers détails sur la page.

Lorsque vous souhaitez consulter les statistiques de votre site, vous vous connectez au site Web du fournisseur d’analyse. Vous pouvez ensuite afficher toutes sortes de rapports sur votre site, par exemple :

- Nombre de consultations de page pour des pages individuelles. Cela vous indique (à peu près) le nombre de personnes visitant le site et les pages de votre site les plus populaires.
- La durée pendant laquelle les utilisateurs consacrent des pages spécifiques. Cela peut vous indiquer si votre page d’hébergement est l’intérêt des gens.
- Les sites sur lesquels les utilisateurs se trouvaient avant la visite de votre site. Cela vous permet de savoir si votre trafic provient de liens, de recherches, etc.
- Lorsque les utilisateurs visitent votre site et leur durée de séjour.
- Les pays d’où proviennent vos visiteurs.
- Les navigateurs et systèmes d’exploitation que vos visiteurs utilisent.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Utilisation d’une application d’assistance pour ajouter Analytics à une page

Pages Web ASP.NET comprend plusieurs applications auxiliaires d’analyse (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`et `Analytics.GetStatCounterHtml`) qui facilitent la gestion des extraits de code JavaScript utilisés pour l’analyse. Au lieu de déterminer comment et où placer le code JavaScript, il vous suffit d’ajouter l’application auxiliaire à une page. Les seules informations que vous devez fournir sont le nom de votre compte, votre ID ou votre code de suivi. (Pour StatCounter, vous devez également fournir quelques valeurs supplémentaires.)

Dans cette procédure, vous allez créer une page de disposition qui utilise le programme d’assistance `GetGoogleHtml`. Si vous disposez déjà d’un compte avec l’un des autres fournisseurs d’analyse, vous pouvez utiliser ce compte à la place et effectuer de légers ajustements en fonction des besoins.

> [!NOTE]
> Lorsque vous créez un compte Analytics, vous enregistrez l’URL du site que vous souhaitez suivre. Si vous testez tout sur votre ordinateur local, vous ne suivez pas le trafic réel (le seul trafic est vous), vous ne pourrez donc pas enregistrer et afficher les statistiques de site. Toutefois, cette procédure montre comment ajouter une application auxiliaire Analytics à une page. Lorsque vous publiez votre site, le site actif envoie des informations à votre fournisseur d’analyse.

1. Ajoutez la bibliothèque d’applications auxiliaires Web ASP.NET à votre site Web, comme décrit dans la rubrique [installation des applications d’assistance d’un Site pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=252372), si vous ne l’avez pas encore fait.
2. Créez un compte avec Google Analytics et enregistrez le nom du compte.
3. Créez une page de disposition nommée *Analytics. cshtml* et ajoutez le balisage suivant :

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Vous devez placer l’appel au programme d’assistance `Analytics` dans le corps de votre page Web (avant la balise `</body>`). Dans le cas contraire, le navigateur n’exécutera pas le script.

    Si vous utilisez un autre fournisseur d’analyse, utilisez plutôt l’une des applications d’assistance suivantes :

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Remplacez `myaccount` par le nom du compte, de l’ID ou du code de suivi que vous avez créé à l’étape 1.
5. Exécutez la page dans le navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.)
6. Dans le navigateur, affichez la page source. Vous pouvez voir le code d’analyse rendu :

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Connectez-vous au site Google Analytics et examinez les statistiques de votre site. Si vous exécutez la page sur un site actif, vous voyez une entrée qui enregistre la visite sur votre page.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Site Google Analytics](https://www.google.com/analytics/)
- [Site Web Analytics Yahoo !](http://help.yahoo.com/l/us/yahoo/ywa/)
- [Site StatCounter](http://statcounter.com/)
