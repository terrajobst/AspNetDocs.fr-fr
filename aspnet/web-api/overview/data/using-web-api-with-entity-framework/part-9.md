---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Ajouter un nouvel élément à la base de données | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557287"
---
# <a name="add-a-new-item-to-the-database"></a><span data-ttu-id="23f75-102">Ajouter un nouvel élément à la base de données</span><span class="sxs-lookup"><span data-stu-id="23f75-102">Add a New Item to the Database</span></span>

<span data-ttu-id="23f75-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="23f75-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="23f75-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="23f75-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="23f75-105">Dans cette section, vous allez ajouter la possibilité pour les utilisateurs de créer un nouveau livre.</span><span class="sxs-lookup"><span data-stu-id="23f75-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="23f75-106">Dans App. js, ajoutez le code suivant au modèle de vue :</span><span class="sxs-lookup"><span data-stu-id="23f75-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="23f75-107">Dans index. cshtml, remplacez le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="23f75-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="23f75-108">Par :</span><span class="sxs-lookup"><span data-stu-id="23f75-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="23f75-109">Ce balisage crée un formulaire pour l’envoi d’un nouvel auteur.</span><span class="sxs-lookup"><span data-stu-id="23f75-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="23f75-110">Les valeurs de la liste déroulante auteur sont liées aux données du `authors` observable dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="23f75-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="23f75-111">Pour les autres entrées de formulaire, les valeurs sont liées aux données de la propriété `newBook` du modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="23f75-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="23f75-112">Le gestionnaire d’envoi du formulaire est lié à la fonction `addBook` :</span><span class="sxs-lookup"><span data-stu-id="23f75-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="23f75-113">La fonction `addBook` lit les valeurs actuelles des entrées de formulaire liées aux données pour créer un objet JSON.</span><span class="sxs-lookup"><span data-stu-id="23f75-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="23f75-114">Ensuite, il publie l’objet JSON dans `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="23f75-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="23f75-115">[Précédent](part-8.md)
> [Suivant](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="23f75-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
