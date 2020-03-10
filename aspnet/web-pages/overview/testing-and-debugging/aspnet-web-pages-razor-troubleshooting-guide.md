---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Guide de résolution des problèmes de pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article décrit les problèmes que vous pouvez rencontrer lors de l’utilisation de pages Web ASP.NET (Razor) et de quelques suggestions de solutions. Versions logicielles ASP.NET Web Pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: fc03767c16f46c1e282d24ee3a7df2409a7c38bb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585763"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Pages (Razor) - Guide de résolution des problèmes

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit les problèmes que vous pouvez rencontrer lors de l’utilisation de pages Web ASP.NET (Razor) et de quelques suggestions de solutions.
> 
> ## <a name="software-versions"></a>Versions de logiciels
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2 et pages Web ASP.NET 1,0.

Cette rubrique contient les sections suivantes :

- [Problèmes avec les pages en cours d’exécution](#Issues_Running_.cshtml_Pages)
- [Problèmes avec le code Razor](#IssuesWithRazorCode)
- [Problèmes liés à la sécurité et à l’appartenance](#membership)
- [Problèmes d’envoi de courrier électronique](#email)
- [Ressources supplémentaires pour MSBuild](#AdditionalResources)

Pour obtenir des questions générales, consultez le [Forum aux questions sur pages Web ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problèmes avec les pages en cours d’exécution

Divers problèmes peuvent empêcher les pages *. cshtml* et *. vbhtml* de s’exécuter correctement. Cette section répertorie les messages d’erreur courants et les causes probables.

### <a name="http-error-403---forbidden-access-is-denied"></a>Erreur HTTP 403-interdit : accès refusé

*Vous n’avez pas l’autorisation d’afficher ce répertoire ou cette page à l’aide des informations d’identification que vous avez fournies.*

Cette erreur peut se produire si le serveur n’exécute pas la version correcte du .NET Framework. Assurez-vous que l’ordinateur qui exécute le serveur (localement ou à distance) a au moins le .NET Framework 4 installé. Assurez-vous également que l’application elle-même est configurée pour exécuter la version appropriée.

Si vous voyez ce problème localement quand vous travaillez dans WebMatrix, cliquez sur l’espace de travail **site** , puis dans l’arborescence, cliquez sur **paramètres**. Dans la liste **Sélectionner la version de .NET Framework** , sélectionnez **.net 4 (intégré)** . Si cette version est déjà définie, essayez d’exécuter WebMatrix en tant qu’administrateur.

Assurez-vous que la racine de votre site Web contient au moins un fichier *. cshtml* .

Si vous voyez cette erreur lorsque le serveur Web se trouve sur un serveur distant, contactez l’administrateur du serveur. Assurez-vous que la .NET Framework 4 ou version ultérieure est installée sur le serveur. Assurez-vous également que l’application s’exécute dans un pool d’applications qui est configuré pour utiliser cette version de the.NET Framework.

Si vous avez le contrôle sur le serveur, assurez-vous qu’il exécute la version correcte du .NET Framework. Vous pouvez également essayer de réparer l’installation en exécutant la commande `aspnet_regiis -iru`. (Par exemple, si vous installez IIS après avoir installé le .NET Framework, IIS n’est pas configuré correctement pour exécuter les pages ASP.NET.) Pour plus d’informations, consultez [ASP.NET IIS Registration Tool (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Erreur HTTP 403,14-interdit

*Le serveur Web est configuré pour ne pas répertorier le contenu de ce répertoire.*

Cette erreur peut se produire si vous demandez une ressource protégée (comme le fichier *Web. config* ) ou dans un dossier qui est protégé (comme l' *application\_des données* ou du *code d’application\_* ).

### <a name="http-error-40417---not-found"></a>Erreur HTTP 404,17-introuvable

*Le contenu demandé semble être un script et ne sera pas pris en charge par le gestionnaire de fichiers statiques.*

Cette erreur peut se produire si le serveur n’est pas configuré correctement pour utiliser le .NET Framework 4 ou version ultérieure, et ne reconnaît donc pas le code dans les blocs `@{ }`. Voir la description plus haut pour l' *erreur HTTP 403-interdit : accès refusé*.

### <a name="http-error-4047---not-found"></a>Erreur HTTP 404,7-introuvable

*Le module de filtrage des demandes est configuré pour refuser l’extension de fichier*

Cette erreur peut se produire si les extensions *. cshtml* ou *. vbhtml* ont été explicitement bloquées sur le serveur. Un symptôme de ce problème est que les URL fonctionnent quand elles n’incluent pas l’extension, mais les URL qui incluent *. cshtml* ou *. vbhtml* ne fonctionnent pas. Une solution possible consiste à réactiver les extensions dans le fichier *Web. config* du site. L’exemple suivant montre comment activer l’extension *. cshtml* .

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Erreur HTTP 404,8-introuvable

*Le module de filtrage des demandes est configuré pour refuser un chemin d’accès dans l’URL qui contient une section hiddenSegment.*

Cette erreur peut se produire si vous demandez une ressource protégée (comme le fichier *Web. config* ) ou dans un dossier qui est protégé (comme l' *application\_des données* ou du *code d’application\_* ).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Ce type de page n’est pas pris en charge (erreur de serveur dans l’application'/')

Consultez la description plus haut pour l’erreur HTTP 404,17.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problèmes avec le code Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Le nom'*classe*'n’existe pas dans le contexte actuel

Souvent, la raison pour laquelle vous voyez cette erreur est que `class` fait référence à une application d’assistance, mais que le programme d’assistance n’est pas installé. Par exemple, si vous essayez d’utiliser une application d’assistance, mais si vous n’avez pas installé le package à partir de NuGet, vous verrez cette erreur. Utilisez la galerie dans WebMatrix pour rechercher et installer l’application d’assistance.

Si le programme d’assistance est installé, mais que la page ne le reconnaît toujours pas, essayez d’ajouter l’instruction Add a `using` au code. Dans l’instruction `using`, référencez l’espace de noms qui contient le programme d’assistance. Par exemple, les applications auxiliaires de base qui se trouvent dans le package d’assistance Web ASP.NET se trouvent dans l’espace de noms `System.Web.Helpers`. En haut de la page où vous souhaitez utiliser le programme d’assistance, ajoutez la ligne suivante :

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problèmes liés à la sécurité et à l’appartenance

Si vous utilisez le système intégré de sécurité (appartenance) dans pages Web ASP.NET (Razor), vous pouvez rencontrer les problèmes suivants.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Pour appeler cette méthode, la propriété « Membership. Provider » doit être une instance de « ExtendedMembershipProvider »

Cette erreur peut indiquer qu’aucune classe `AspNetSqlMembershipProvider` n’est configurée. (Un symptôme est que le site fonctionne correctement en local, mais génère cette erreur quand vous le publiez sur le serveur d’un fournisseur d’hébergement.) L’un des correctifs pour résoudre ce problème consiste à activer explicitement l’appartenance simple en ajoutant le code suivant au fichier *Web. config* du site :

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problèmes d’envoi de courrier électronique

Les problèmes d’envoi de courrier électronique peuvent être difficiles à déboguer. Un problème initial peut être que vous ne pouvez pas vous connecter au serveur SMTP. Si la connexion réussit, ASP.NET remet le message au serveur SMTP. Toutefois, il peut y avoir des problèmes avec le message lui-même, ce qui empêche le serveur SMTP de l’envoyer.

Si votre application ne parvient pas à envoyer de courrier électronique, essayez ce qui suit :

- Le nom du serveur SMTP est souvent semblable à `smtp.provider.com` ou `smtp.provider.net`. Toutefois, si vous publiez votre site sur un fournisseur d’hébergement, le nom du serveur SMTP à ce stade peut être `localhost`. Cette situation se produit car une fois que vous avez publié et que votre site est en cours d’exécution sur le serveur du fournisseur, le serveur SMTP peut être local du point de vue de votre application. Cette modification des noms de serveurs peut signifier que vous devez modifier le nom du serveur SMTP dans le cadre de votre processus de publication.
- Le numéro de port est généralement 25. Toutefois, certains fournisseurs nécessitent que vous utilisiez le port 587 ou un autre port. Vérifiez auprès du propriétaire du serveur SMTP le numéro de port qu’il s’attend à utiliser.
- Veillez à utiliser les informations d’identification appropriées. Si vous avez publié votre site sur un fournisseur d’hébergement, utilisez les informations d’identification que le fournisseur a spécifiquement indiquées pour la messagerie électronique. Ces informations d’identification peuvent être différentes des informations d’identification que vous utilisez pour la publication.
- Parfois, vous n’avez pas besoin d’informations d’identification. Si vous envoyez un e-mail à l’aide de votre fournisseur de services Internet personnel, votre fournisseur de messagerie peut déjà connaître vos informations d’identification. Après la publication, vous devrez peut-être utiliser d’autres informations d’identification que lorsque vous testez sur votre ordinateur local.
- Si votre fournisseur de messagerie utilise le chiffrement, définissez `WebMail.EnableSsl` sur `true`.

En cas d’erreur lors de l’envoi d’un e-mail, vous pouvez voir un message d’erreur ASP.NET standard, qui ressemble à ceci :

![Message d’erreur ASP.NET en cas de problème avec la messagerie](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Vous pouvez également déboguer des problèmes d’envoi de courrier électronique à l’aide d’un bloc `try-catch`, comme dans l’exemple suivant. Lorsque vous utilisez un bloc de `try-catch`, ASP.NET n’affiche pas ses messages d’erreur standard. Au lieu de cela, vous pouvez capturer l’erreur dans la partie `catch` du bloc.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Remplacez les valeurs appropriées pour `your-SMTP-server-name`, et ainsi de suite. Voici quelques-uns des messages d’erreur suivants :

- *Échec de l’envoi du message électronique.*

    \- ou -

    *Une tentative de connexion a échoué car le tiers connecté n’a pas répondu correctement au bout d’un certain temps, ou la connexion établie a échoué car l’hôte connecté n’a pas répondu*

    Cette erreur signifie généralement que l’application n’a pas pu se connecter au serveur SMTP. Vérifiez le nom du serveur et le numéro de port.
- *Boîte aux lettres non disponible. La réponse du serveur était : 5.1.0 &lt;someuser@invaliddomain&gt; expéditeur rejeté : domaine expéditeur non valide*

    Ce message peut indiquer que l’adresse de `From` est incorrecte ou manquante.
- *La chaîne spécifiée n’est pas dans le formulaire requis pour une adresse de messagerie.*

    Cette erreur peut indiquer que la valeur des propriétés `To` ou `From` n’est pas reconnue en tant qu’adresse de messagerie. (ASP.NET ne peut pas vérifier que l’adresse de messagerie est valide, mais uniquement au format approprié, comme *name@domain.com* .)

> [!NOTE]
> Supprimez le balisage qui affiche l’erreur (`@errorMessage`) avant de publier la page sur un site actif. Il n’est pas judicieux de laisser les utilisateurs voir les messages d’erreur que vous obtenez à partir d’un serveur.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Pages web ASP.NET - Questions fréquentes (FAQ) (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix et pages Web ASP.net](https://forums.asp.net/1224.aspx/1?WebMatrix) Forum sur le site Web ASP.net
