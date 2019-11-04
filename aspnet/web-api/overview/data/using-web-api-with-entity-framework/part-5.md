---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Créer des objets Transfert de données (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445756"
---
# <a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="c281b-102">Créer des objets de transfert de données (DTO)</span><span class="sxs-lookup"><span data-stu-id="c281b-102">Create Data Transfer Objects (DTOs)</span></span>

<span data-ttu-id="c281b-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c281b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c281b-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="c281b-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c281b-105">Pour l’instant, notre API Web expose les entités de base de données au client.</span><span class="sxs-lookup"><span data-stu-id="c281b-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="c281b-106">Le client reçoit des données qui sont mappées directement à vos tables de base de données.</span><span class="sxs-lookup"><span data-stu-id="c281b-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="c281b-107">Toutefois, cela n’est pas toujours une bonne idée.</span><span class="sxs-lookup"><span data-stu-id="c281b-107">However, that's not always a good idea.</span></span> <span data-ttu-id="c281b-108">Parfois, vous souhaitez modifier la forme des données que vous envoyez au client.</span><span class="sxs-lookup"><span data-stu-id="c281b-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="c281b-109">Vous pouvez, par exemple, décider d'effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="c281b-109">For example, you might want to:</span></span>

- <span data-ttu-id="c281b-110">Supprimez les références circulaires (voir la section précédente).</span><span class="sxs-lookup"><span data-stu-id="c281b-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="c281b-111">Masquer les propriétés particulières que les clients ne sont pas censés afficher.</span><span class="sxs-lookup"><span data-stu-id="c281b-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="c281b-112">Omettez certaines propriétés afin de réduire la taille de la charge utile.</span><span class="sxs-lookup"><span data-stu-id="c281b-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="c281b-113">Aplatir les graphiques d’objets qui contiennent des objets imbriqués, afin de les rendre plus pratiques pour les clients.</span><span class="sxs-lookup"><span data-stu-id="c281b-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="c281b-114">Évitez les vulnérabilités de « dépassement de la publication ».</span><span class="sxs-lookup"><span data-stu-id="c281b-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="c281b-115">(Consultez [validation de modèle](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) pour une discussion sur la survalidation.)</span><span class="sxs-lookup"><span data-stu-id="c281b-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="c281b-116">Découplez votre couche de service de votre couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="c281b-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="c281b-117">Pour ce faire, vous pouvez définir un *objet de transfert de données* (DTO).</span><span class="sxs-lookup"><span data-stu-id="c281b-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="c281b-118">Un DTO est un objet qui définit la façon dont les données sont envoyées sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="c281b-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="c281b-119">Voyons comment cela fonctionne avec l’entité Book.</span><span class="sxs-lookup"><span data-stu-id="c281b-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="c281b-120">Dans le dossier Models, ajoutez deux classes DTO :</span><span class="sxs-lookup"><span data-stu-id="c281b-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="c281b-121">La classe `BookDetailDto` comprend toutes les propriétés du modèle Book, sauf que `AuthorName` est une chaîne qui contiendra le nom de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="c281b-121">The `BookDetailDto` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="c281b-122">La classe `BookDto` contient un sous-ensemble de propriétés de `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="c281b-122">The `BookDto` class contains a subset of properties from `BookDetailDto`.</span></span>

<span data-ttu-id="c281b-123">Ensuite, remplacez les deux méthodes d’extraction de la classe `BooksController`, par les versions qui retournent les objets DTO.</span><span class="sxs-lookup"><span data-stu-id="c281b-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="c281b-124">Nous allons utiliser l’instruction LINQ **Select** pour convertir des entités Book en DTO.</span><span class="sxs-lookup"><span data-stu-id="c281b-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="c281b-125">Voici le SQL généré par la nouvelle méthode `GetBooks`.</span><span class="sxs-lookup"><span data-stu-id="c281b-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="c281b-126">Vous pouvez voir que EF traduit la requête LINQ **Select** en une instruction SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="c281b-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="c281b-127">Enfin, modifiez la méthode `PostBook` pour retourner un DTO.</span><span class="sxs-lookup"><span data-stu-id="c281b-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="c281b-128">Dans ce didacticiel, nous effectuons une conversion manuelle en DTO dans le code.</span><span class="sxs-lookup"><span data-stu-id="c281b-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="c281b-129">Une autre option consiste à utiliser une bibliothèque comme [AutoMapper](http://automapper.org/) qui gère automatiquement la conversion.</span><span class="sxs-lookup"><span data-stu-id="c281b-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="c281b-130">[Précédent](part-4.md)
> [Suivant](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="c281b-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
