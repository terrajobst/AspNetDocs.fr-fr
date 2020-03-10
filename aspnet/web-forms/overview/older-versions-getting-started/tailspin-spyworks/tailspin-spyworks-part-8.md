---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Partie 8 : pages finales, gestion des exceptions et conclusion | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 8 ajoute une page de contact, à propos de la page et de l’exception...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586883"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a>Partie 8 : pages finales, gestion des exceptions et conclusion

par [Joe Stagner](https://github.com/JoeStagner)

> Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET. Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.
> 
> Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks. La partie 8 ajoute une page de contact, à propos de la gestion des exceptions et des pages. Il s’agit de la conclusion de la série.

## <a id="_Toc260221680"></a>Page de contact (envoi d’e-mails à partir de ASP.NET)

Créer une nouvelle page nommée contactus. aspx

À l’aide du concepteur, créez le formulaire suivant en tenant compte de l’ToolkitScriptManager et du contrôle de l’éditeur du AjaxControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Double-cliquez sur le bouton « Submit » (envoyer) pour générer un gestionnaire d’événements Click dans le fichier code-behind et implémenter une méthode pour envoyer les informations de contact en tant que courrier électronique.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Ce code requiert que votre fichier Web. config contienne une entrée dans la section de configuration qui spécifie le serveur SMTP à utiliser pour l’envoi de messages électroniques.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Page à propos de

Créez une page nommée aboutus. aspx et ajoutez le contenu de votre choix.

## <a id="_Toc260221682"></a>Gestionnaire d’exceptions global

Enfin, tout au long de l’application, nous avons levé des exceptions et il existe des circonstances imprévues qui, à froid, provoquent également des exceptions non gérées dans notre application Web.

Nous ne voulons pas qu’une exception non gérée soit affichée à un visiteur de site Web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Hormis le fait qu’il s’agit d’une terrible expérience utilisateur, les exceptions non gérées peuvent également constituer un problème de sécurité.

Pour résoudre ce problème, nous allons implémenter un gestionnaire d’exceptions global.

Pour ce faire, ouvrez le fichier global. asax et notez le gestionnaire d’événements prégénérés suivant.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Ajoutez du code pour implémenter l’application\_gestionnaire d’erreurs comme suit.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Ajoutez ensuite une page nommée Error. aspx à la solution et ajoutez cet extrait de balisage.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

À présent, dans la page\_gestionnaire d’événements de chargement, extrayez les messages d’erreur de l’objet de requête.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Achèvement

Nous avons vu que ASP.NET Web Forms vous permet de créer facilement un site Web sophistiqué avec accès aux bases de données, appartenance, AJAX, etc. assez rapidement.

Nous espérons que ce didacticiel vous a fourni les outils dont vous avez besoin pour commencer à créer vos propres applications WebForms ASP.NET.

> [!div class="step-by-step"]
> [Précédent](tailspin-spyworks-part-7.md)
