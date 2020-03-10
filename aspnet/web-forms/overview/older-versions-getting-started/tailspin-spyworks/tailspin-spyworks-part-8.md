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
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="4c0f5-104">Partie 8 : pages finales, gestion des exceptions et conclusion</span><span class="sxs-lookup"><span data-stu-id="4c0f5-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>

<span data-ttu-id="4c0f5-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4c0f5-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4c0f5-106">Tailspin SpyWorks montre combien il est très simple de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4c0f5-107">Il montre comment utiliser les nouvelles fonctionnalités de ASP.NET 4 pour créer un magasin en ligne, y compris l’achat, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4c0f5-108">Cette série de didacticiels détaille toutes les étapes nécessaires à la création de l’exemple d’application Tailspin SpyWorks.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4c0f5-109">La partie 8 ajoute une page de contact, à propos de la gestion des exceptions et des pages.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="4c0f5-110">Il s’agit de la conclusion de la série.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-110">This is the conclusion of the series.</span></span>

## <a id="_Toc260221680"></a><span data-ttu-id="4c0f5-111">Page de contact (envoi d’e-mails à partir de ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="4c0f5-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="4c0f5-112">Créer une nouvelle page nommée contactus. aspx</span><span class="sxs-lookup"><span data-stu-id="4c0f5-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="4c0f5-113">À l’aide du concepteur, créez le formulaire suivant en tenant compte de l’ToolkitScriptManager et du contrôle de l’éditeur du AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxControlToolkit.</span></span> <span data-ttu-id="4c0f5-114">.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="4c0f5-115">Double-cliquez sur le bouton « Submit » (envoyer) pour générer un gestionnaire d’événements Click dans le fichier code-behind et implémenter une méthode pour envoyer les informations de contact en tant que courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="4c0f5-116">Ce code requiert que votre fichier Web. config contienne une entrée dans la section de configuration qui spécifie le serveur SMTP à utiliser pour l’envoi de messages électroniques.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="4c0f5-117">Page à propos de</span><span class="sxs-lookup"><span data-stu-id="4c0f5-117">About Page</span></span>

<span data-ttu-id="4c0f5-118">Créez une page nommée aboutus. aspx et ajoutez le contenu de votre choix.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="4c0f5-119">Gestionnaire d’exceptions global</span><span class="sxs-lookup"><span data-stu-id="4c0f5-119">Global Exception Handler</span></span>

<span data-ttu-id="4c0f5-120">Enfin, tout au long de l’application, nous avons levé des exceptions et il existe des circonstances imprévues qui, à froid, provoquent également des exceptions non gérées dans notre application Web.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="4c0f5-121">Nous ne voulons pas qu’une exception non gérée soit affichée à un visiteur de site Web.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="4c0f5-122">Hormis le fait qu’il s’agit d’une terrible expérience utilisateur, les exceptions non gérées peuvent également constituer un problème de sécurité.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="4c0f5-123">Pour résoudre ce problème, nous allons implémenter un gestionnaire d’exceptions global.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="4c0f5-124">Pour ce faire, ouvrez le fichier global. asax et notez le gestionnaire d’événements prégénérés suivant.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="4c0f5-125">Ajoutez du code pour implémenter l’application\_gestionnaire d’erreurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="4c0f5-126">Ajoutez ensuite une page nommée Error. aspx à la solution et ajoutez cet extrait de balisage.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="4c0f5-127">À présent, dans la page\_gestionnaire d’événements de chargement, extrayez les messages d’erreur de l’objet de requête.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="4c0f5-128">Achèvement</span><span class="sxs-lookup"><span data-stu-id="4c0f5-128">Conclusion</span></span>

<span data-ttu-id="4c0f5-129">Nous avons vu que ASP.NET Web Forms vous permet de créer facilement un site Web sophistiqué avec accès aux bases de données, appartenance, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="4c0f5-130">assez rapidement.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-130">pretty quickly.</span></span>

<span data-ttu-id="4c0f5-131">Nous espérons que ce didacticiel vous a fourni les outils dont vous avez besoin pour commencer à créer vos propres applications WebForms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4c0f5-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4c0f5-132">Précédent</span><span class="sxs-lookup"><span data-stu-id="4c0f5-132">Previous</span></span>](tailspin-spyworks-part-7.md)
