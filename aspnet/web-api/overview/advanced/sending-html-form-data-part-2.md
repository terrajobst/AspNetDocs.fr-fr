---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Envoi de données de formulaire HTML dans API Web ASP.NET : chargement de fichier et multipart MIME-ASP.NET 4. x'
author: MikeWasson
description: Ce didacticiel montre comment charger des fichiers sur une API Web. Elle explique également comment traiter les données MIME à parties multiples.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557567"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="74fd6-104">Envoi de données de formulaire HTML dans API Web ASP.NET : Téléchargement de fichiers et MIME à parties multiples</span><span class="sxs-lookup"><span data-stu-id="74fd6-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="74fd6-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="74fd6-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="74fd6-106">Partie 2 : Téléchargement de fichiers et MIME à parties multiples</span><span class="sxs-lookup"><span data-stu-id="74fd6-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="74fd6-107">Ce didacticiel montre comment charger des fichiers sur une API Web.</span><span class="sxs-lookup"><span data-stu-id="74fd6-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="74fd6-108">Elle explique également comment traiter les données MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="74fd6-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="74fd6-109">[Téléchargez le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="74fd6-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>

<span data-ttu-id="74fd6-110">Voici un exemple de formulaire HTML pour le chargement d’un fichier :</span><span class="sxs-lookup"><span data-stu-id="74fd6-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="74fd6-111">Ce formulaire contient un contrôle d’entrée de texte et un contrôle d’entrée de fichier.</span><span class="sxs-lookup"><span data-stu-id="74fd6-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="74fd6-112">Lorsqu’un formulaire contient un contrôle d’entrée de fichier, l’attribut **enctype** doit toujours être &quot;&quot;multipart/form-data, qui spécifie que le formulaire sera envoyé en tant que message MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="74fd6-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="74fd6-113">Le format d’un message MIME à parties multiples est plus facile à comprendre en examinant un exemple de demande :</span><span class="sxs-lookup"><span data-stu-id="74fd6-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="74fd6-114">Ce message est divisé en deux *parties*, une pour chaque contrôle de formulaire.</span><span class="sxs-lookup"><span data-stu-id="74fd6-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="74fd6-115">Les limites de la pièce sont indiquées par les lignes qui commencent par des tirets.</span><span class="sxs-lookup"><span data-stu-id="74fd6-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="74fd6-116">La limite de la partie comprend un composant aléatoire (&quot;41184676334&quot;) pour garantir que la chaîne limite n’apparaît pas accidentellement dans une partie de message.</span><span class="sxs-lookup"><span data-stu-id="74fd6-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>

<span data-ttu-id="74fd6-117">Chaque partie de message contient un ou plusieurs en-têtes, suivis du contenu de la partie.</span><span class="sxs-lookup"><span data-stu-id="74fd6-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="74fd6-118">L’en-tête Content-disposition comprend le nom du contrôle.</span><span class="sxs-lookup"><span data-stu-id="74fd6-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="74fd6-119">Pour les fichiers, il contient également le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="74fd6-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="74fd6-120">L’en-tête Content-type décrit les données de la partie.</span><span class="sxs-lookup"><span data-stu-id="74fd6-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="74fd6-121">Si cet en-tête est omis, la valeur par défaut est text/plain.</span><span class="sxs-lookup"><span data-stu-id="74fd6-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="74fd6-122">Dans l’exemple précédent, l’utilisateur a chargé un fichier nommé GrandCanyon. jpg, avec le type de contenu image/JPEG ; et la valeur de l’entrée de texte était &quot;vacances d’été&quot;.</span><span class="sxs-lookup"><span data-stu-id="74fd6-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="74fd6-123">Chargement de fichiers</span><span class="sxs-lookup"><span data-stu-id="74fd6-123">File Upload</span></span>

<span data-ttu-id="74fd6-124">Examinons à présent un contrôleur d’API Web qui lit les fichiers à partir d’un message MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="74fd6-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="74fd6-125">Le contrôleur lira les fichiers de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="74fd6-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="74fd6-126">L’API Web prend en charge les actions asynchrones à l’aide du [modèle de programmation basé sur les tâches](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="74fd6-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="74fd6-127">Tout d’abord, voici le code si vous ciblez .NET Framework 4,5, qui prend en charge les mots clés **Async** et **await** .</span><span class="sxs-lookup"><span data-stu-id="74fd6-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="74fd6-128">Notez que l’action du contrôleur n’utilise aucun paramètre.</span><span class="sxs-lookup"><span data-stu-id="74fd6-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="74fd6-129">Cela est dû au fait que nous traiterons le corps de la requête à l’intérieur de l’action, sans appeler un formateur de type de média.</span><span class="sxs-lookup"><span data-stu-id="74fd6-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="74fd6-130">La méthode **IsMultipartContent** vérifie si la requête contient un message MIME à parties multiples.</span><span class="sxs-lookup"><span data-stu-id="74fd6-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="74fd6-131">Si ce n’est pas le cas, le contrôleur retourne le code d’état HTTP 415 (type de média non pris en charge).</span><span class="sxs-lookup"><span data-stu-id="74fd6-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="74fd6-132">La classe **MultipartFormDataStreamProvider** est un objet d’assistance qui alloue des flux de fichiers pour les fichiers téléchargés.</span><span class="sxs-lookup"><span data-stu-id="74fd6-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="74fd6-133">Pour lire le message MIME à parties multiples, appelez la méthode **ReadAsMultipartAsync** .</span><span class="sxs-lookup"><span data-stu-id="74fd6-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="74fd6-134">Cette méthode extrait toutes les parties du message et les écrit dans les flux fournis par **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="74fd6-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="74fd6-135">Une fois la méthode terminée, vous pouvez obtenir des informations sur les fichiers à partir de la propriété **FileData** , qui est une collection d’objets **MultipartFileData** .</span><span class="sxs-lookup"><span data-stu-id="74fd6-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="74fd6-136">**MultipartFileData. FileName** est le nom de fichier local sur le serveur où le fichier a été enregistré.</span><span class="sxs-lookup"><span data-stu-id="74fd6-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="74fd6-137">**MultipartFileData. Headers** contient l’en-tête du composant (*pas* l’en-tête de la demande).</span><span class="sxs-lookup"><span data-stu-id="74fd6-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="74fd6-138">Vous pouvez l’utiliser pour accéder aux en-têtes Content\_disposition et Content-type.</span><span class="sxs-lookup"><span data-stu-id="74fd6-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="74fd6-139">Comme son nom l’indique, **ReadAsMultipartAsync** est une méthode asynchrone.</span><span class="sxs-lookup"><span data-stu-id="74fd6-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="74fd6-140">Pour effectuer le travail une fois la méthode terminée, utilisez une [tâche de continuation](https://msdn.microsoft.com/library/ee372288.aspx) (.net 4,0) ou le mot clé **await** (.net 4,5).</span><span class="sxs-lookup"><span data-stu-id="74fd6-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="74fd6-141">Voici la version .NET Framework 4,0 du code précédent :</span><span class="sxs-lookup"><span data-stu-id="74fd6-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="74fd6-142">Lecture des données de contrôle de formulaire</span><span class="sxs-lookup"><span data-stu-id="74fd6-142">Reading Form Control Data</span></span>

<span data-ttu-id="74fd6-143">Le formulaire HTML que j’ai montré précédemment avait un contrôle Text Input.</span><span class="sxs-lookup"><span data-stu-id="74fd6-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="74fd6-144">Vous pouvez récupérer la valeur du contrôle à partir de la propriété **FormData** de **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="74fd6-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="74fd6-145">**FormData** est une **NameValueCollection** qui contient des paires nom/valeur pour les contrôles de formulaire.</span><span class="sxs-lookup"><span data-stu-id="74fd6-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="74fd6-146">La collection peut contenir des clés dupliquées.</span><span class="sxs-lookup"><span data-stu-id="74fd6-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="74fd6-147">Prenons le formulaire suivant :</span><span class="sxs-lookup"><span data-stu-id="74fd6-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="74fd6-148">Le corps de la demande peut se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="74fd6-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="74fd6-149">Dans ce cas, la collection **FormData** contient les paires clé/valeur suivantes :</span><span class="sxs-lookup"><span data-stu-id="74fd6-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="74fd6-150">voyage : aller-retour</span><span class="sxs-lookup"><span data-stu-id="74fd6-150">trip: round-trip</span></span>
- <span data-ttu-id="74fd6-151">Options : ne pas arrêter</span><span class="sxs-lookup"><span data-stu-id="74fd6-151">options: nonstop</span></span>
- <span data-ttu-id="74fd6-152">Options : dates</span><span class="sxs-lookup"><span data-stu-id="74fd6-152">options: dates</span></span>
- <span data-ttu-id="74fd6-153">siège : fenêtre</span><span class="sxs-lookup"><span data-stu-id="74fd6-153">seat: window</span></span>
